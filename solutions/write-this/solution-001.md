---
id: KD-001
challenge: challenges/write-this/challenge-001.md
---

# Solution KD-001 — No Passport Required

## ✅ Answer / Respuesta

```kql
let TimeWindow = 60min;
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType == 0
| where isnotempty(Location)
| project TimeGenerated, UserPrincipalName, Location, IPAddress
| join kind=inner (
    SigninLogs
    | where TimeGenerated > ago(7d)
    | where ResultType == 0
    | where isnotempty(Location)
    | project
        TimeGenerated2 = TimeGenerated,
        UserPrincipalName,
        Location2 = Location,
        IPAddress2 = IPAddress
) on UserPrincipalName
| where TimeGenerated < TimeGenerated2
| where datetime_diff('minute', TimeGenerated2, TimeGenerated) <= 60
| where Location != Location2
| project
    UserPrincipalName,
    FirstSignIn = TimeGenerated,
    FirstCountry = Location,
    FirstIP = IPAddress,
    SecondSignIn = TimeGenerated2,
    SecondCountry = Location2,
    SecondIP = IPAddress2,
    TimeDeltaMinutes = datetime_diff('minute', TimeGenerated2, TimeGenerated)
| order by TimeDeltaMinutes asc
```

---

## 🔎 Explanation / Explicación

**Step 1 — Filter to successful sign-ins.**
`ResultType == 0` is the Entra ID (Azure AD) code for a successful authentication. Any other value indicates a failure or MFA challenge — irrelevant for this detection since impossible travel requires actual access.

**Step 2 — Self-join on `UserPrincipalName`.**
The table is joined against itself so that every login for a user can be compared against every other login from the same user. The `kind=inner` ensures both sides exist.

**Step 3 — Directional filtering with `TimeGenerated < TimeGenerated2`.**
Without this, every pair appears twice (A→B and B→A). This constraint forces a canonical ordering: the left side is always the earlier login.

**Step 4 — 60-minute window.**
`datetime_diff('minute', TimeGenerated2, TimeGenerated) <= 60` limits pairs to those within the detection window. Combined with the directional filter, this is now "any two successful logins from the same user within one hour."

**Step 5 — Country mismatch.**
`Location != Location2` is the trigger. If both logins are from the same country, it's not impossible travel. Only cross-border pairs surface.

**Step 6 — `TimeDeltaMinutes` ascending.**
The tightest pairs (fastest "travel") are the most suspicious. Ordering ascending puts the hardest violations at the top.

[ES]

**Paso 1 — Filtrar inicios de sesión exitosos.**
`ResultType == 0` es el código de Entra ID para autenticación exitosa. Cualquier otro valor indica fallo o desafío MFA — irrelevante aquí porque el viaje imposible requiere acceso real.

**Paso 2 — Self-join en `UserPrincipalName`.**
La tabla se une consigo misma para que cada login de un usuario pueda compararse con todos los demás del mismo usuario.

**Paso 3 — Filtrado direccional con `TimeGenerated < TimeGenerated2`.**
Sin esto, cada par aparece dos veces. Esta restricción fuerza un ordenamiento canónico: el lado izquierdo es siempre el login más temprano.

**Paso 4 — Ventana de 60 minutos.**
Limita los pares a los que están dentro de la ventana de detección.

**Paso 5 — Discrepancia de país.**
`Location != Location2` es el disparador. Solo los pares transfronterizos aparecen.

---

## ⚡ Performance Notes / Notas de rendimiento

- **Always time-bound before the join.** The `where TimeGenerated > ago(7d)` on both sides of the join dramatically reduces the input cardinality before the cross product is computed. Without it, the join expands the full table.
- **`isnotempty(Location)` pre-filter.** Drops rows with no country data early, reducing the join input and avoiding spurious matches on empty strings.
- For production use, consider reducing the lookback from `7d` to `1d` and scheduling the query to run hourly — the join is still the expensive operation and shorter windows keep it manageable.
- If `SigninLogs` volume is very high, materialize the filtered base using `let` before the join to avoid double-scanning.

---

## 📚 References / Referencias

- [Microsoft: SigninLogs schema](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs)
- [MITRE ATT&CK T1078.004: Cloud Accounts](https://attack.mitre.org/techniques/T1078/004/)
- [Microsoft Sentinel: Impossible Travel detection](https://learn.microsoft.com/en-us/azure/sentinel/detect-threats-built-in)
- [KQL datetime_diff function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datetime-difffunction)
