---
id: KD-003
challenge: challenges/optimize-this/challenge-001.md
---

# Solution KD-003 — The Costly Deleter

## ✅ Answer / Respuesta

```kql
let LookbackWindow = 24h;
let DeleteThreshold = 50;
let IgnoredExtensions = dynamic([".tmp", ".log"]);
// Step 1: Filter and aggregate file deletions BEFORE any join
let MassDeletions =
    DeviceFileEvents
    | where TimeGenerated > ago(LookbackWindow)
    | where ActionType == "FileDeleted"
    | where not(FileName has_any (IgnoredExtensions))
    | summarize
        DeleteCount = count(),
        FileList = make_set(FileName, 20),
        EarliestDelete = min(TimeGenerated)
        by DeviceId, DeviceName, InitiatingProcessId, bin(TimeGenerated, 5m)
    | where DeleteCount > DeleteThreshold;
// Step 2: Narrow the process table BEFORE joining
let RelevantProcesses =
    DeviceProcessEvents
    | where TimeGenerated > ago(LookbackWindow)
    | project DeviceId, InitiatingProcessId = ProcessId, InitiatingProcessFileName, AccountName;
// Step 3: Join the two pre-aggregated, pre-filtered sets
MassDeletions
| join kind=leftouter RelevantProcesses on DeviceId, InitiatingProcessId
| project
    DeviceName,
    WindowStart = EarliestDelete,
    InitiatingProcessFileName,
    AccountName,
    DeleteCount,
    FileList
| order by DeleteCount desc
```

---

## 🔎 Explanation / Explicación

Three performance problems were identified and fixed:

---

### Fix 1 — Filter before join

**Original approach:**
```kql
DeviceFileEvents
| join kind=leftouter (DeviceProcessEvents | ...) on DeviceId, ...
| where ActionType == "FileDeleted"
```

The join runs on the **full** `DeviceFileEvents` table — millions of rows — before any filtering narrows the dataset. The fix moves all `where` clauses and the `summarize` step before the join. The `join` now operates on a small set of aggregated deletion windows rather than raw event rows.

---

### Fix 2 — Time-bound every table scan

**Original approach:**
```kql
DeviceFileEvents
| join kind=leftouter (
    DeviceProcessEvents
    | project ...
)
```

Neither table had a time filter. KQL scans the full retention window (often 30–90 days) for both tables. The fix adds `| where TimeGenerated > ago(LookbackWindow)` as the **first operator** on every table reference. This is the single highest-impact optimization in most KQL queries.

---

### Fix 3 — Extension filter using `has_any` instead of multiple `!endswith`

**Original approach:**
```kql
| where FileName !endswith ".tmp"
| where FileName !endswith ".log"
```

Each `!endswith` is evaluated per row independently. Using `has_any` with a `dynamic` list and a `not()` wrapper is semantically equivalent but reduces redundancy and makes the exclusion list maintainable. More importantly, the filter now runs **after** the time filter reduces the row count, and **before** the summarize aggregates it further.

---

### Why `summarize` before `join` matters

The original join cross-referenced every raw file deletion event against every process event for the same device. Even with matching on `InitiatingProcessId`, the join input from `DeviceFileEvents` was millions of rows.

After the fix, `MassDeletions` contains only rows where a device exceeded the deletion threshold — typically a handful of rows per query run. The join now operates on a tiny result set.

[ES]

Se identificaron y corrigieron tres problemas de rendimiento:

1. **Filtrar antes del join**: El join se ejecutaba sobre la tabla completa. La corrección mueve todos los filtros y el `summarize` antes del join.
2. **Limitar el tiempo en cada escaneo**: Ninguna tabla tenía filtro de tiempo. La corrección agrega `| where TimeGenerated > ago(LookbackWindow)` como primer operador en cada referencia de tabla.
3. **`has_any` en lugar de múltiples `!endswith`**: Reduce redundancia y el filtro ahora corre sobre un conjunto de datos ya reducido por el filtro de tiempo.

---

## ⚡ Performance Notes / Notas de rendimiento

- **Time filter placement is rule zero.** Always the first operator on any table reference. KQL uses it to prune partition scans at the storage layer before anything else runs.
- **`summarize` before `join` is the second rule.** Reduce cardinality as aggressively as possible before any cross-table operation.
- Using `let` to define intermediate steps (`MassDeletions`, `RelevantProcesses`) is not just readability — it also helps the query engine recognize shared subexpressions and cache them.
- `make_set(FileName, 20)` caps the array at 20 elements. Without a limit, `make_set` on a high-deletion event can allocate unbounded memory per aggregation group.

---

## 📚 References / Referencias

- [Microsoft: KQL best practices for Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/kusto-overview)
- [Microsoft: DeviceFileEvents schema](https://learn.microsoft.com/en-us/microsoft-365/security/defender/advanced-hunting-devicefileevents-table)
- [MITRE ATT&CK T1485: Data Destruction](https://attack.mitre.org/techniques/T1485/)
- [KQL join operator performance](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/joinoperator)
- [KQL has_any operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/has-anyoperator)
