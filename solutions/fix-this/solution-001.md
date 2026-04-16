---
id: KD-002
challenge: challenges/fix-this/challenge-001.md
---

# Solution KD-002 — Broken Encoder

## ✅ Answer / Respuesta

```kql
DeviceProcessEvents
| where Timestamp > ago(7d)
| where FileName =~ "powershell.exe"
| where ProcessCommandLine contains "-EncodedCommand"
    or ProcessCommandLine contains "-enc "
| where ProcessCommandLine !contains "-NonInteractive"
| project DeviceName, Timestamp, ProcessCommandLine, InitiatingProcessAccountName
```

---

## 🔎 Explanation / Explicación

Three bugs were introduced. Here they are, identified and fixed:

---

### Bug 1 — Wrong table name: `DeviceProcessEvent` → `DeviceProcessEvents`

**Original:**
```kql
DeviceProcessEvent
```
**Fixed:**
```kql
DeviceProcessEvents
```

The correct Defender for Endpoint (MDE) table name is `DeviceProcessEvents` — plural. The original query fails silently in some Sentinel environments or throws a schema error. This is the kind of typo that's easy to miss after a long shift.

---

### Bug 2 — Wrong operator: `contain` → `contains`

**Original:**
```kql
| where ProcessCommandLine contain "-EncodedCommand"
```
**Fixed:**
```kql
| where ProcessCommandLine contains "-EncodedCommand"
    or ProcessCommandLine contains "-enc "
```

`contain` is not a valid KQL operator. The correct operator is `contains` (case-insensitive substring match). The query with `contain` either fails or is silently ignored depending on the engine version.

The bonus fix: `-enc ` (with a trailing space) is added as an `or` branch. Attackers frequently use the abbreviated form `-enc` to avoid detections that only look for the full `-EncodedCommand` string. A robust detection covers both.

---

### Bug 3 — Wrong column name: `AccountName` → `InitiatingProcessAccountName`

**Original:**
```kql
| project DeviceName, Timestamp, ProcessCommandLine, AccountName
```
**Fixed:**
```kql
| project DeviceName, Timestamp, ProcessCommandLine, InitiatingProcessAccountName
```

`AccountName` does not exist in `DeviceProcessEvents`. The correct column is `InitiatingProcessAccountName` — the account that spawned the process. When `project` references a non-existent column, KQL returns `null` for that column rather than erroring, making this bug invisible at first glance but useless in practice.

[ES]

Se introdujeron tres errores. Aquí están identificados y corregidos:

1. **Nombre de tabla incorrecto**: `DeviceProcessEvent` → `DeviceProcessEvents` (plural)
2. **Operador incorrecto**: `contain` → `contains`. El operador correcto en KQL es `contains`. Además, se agregó `-enc ` como alternativa ya que los atacantes usan la forma abreviada.
3. **Nombre de columna incorrecto**: `AccountName` → `InitiatingProcessAccountName`. La columna correcta en `DeviceProcessEvents` para la cuenta que inició el proceso es `InitiatingProcessAccountName`.

---

## ⚡ Performance Notes / Notas de rendimiento

- The `where Timestamp > ago(7d)` time filter at the top is correct placement — it reduces scan volume before any string operations run.
- `contains` is case-insensitive by default in KQL. Use `contains_cs` only if case-sensitive matching is required.
- For production, consider adding `| where isnotempty(ProcessCommandLine)` before the string filter to skip null rows.

---

## 📚 References / Referencias

- [Microsoft: DeviceProcessEvents schema](https://learn.microsoft.com/en-us/microsoft-365/security/defender/advanced-hunting-deviceprocessevents-table)
- [MITRE ATT&CK T1027: Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/)
- [MITRE ATT&CK T1059.001: PowerShell](https://attack.mitre.org/techniques/T1059/001/)
- [KQL string operators reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)
