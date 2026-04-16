---
id: KD-003
category: optimize-this
difficulty: Archmage
tags: [DeviceFileEvents, mass-deletion, ransomware, performance]
mitre: T1485
author: thegr8val
language: EN/ES
---

# Challenge KD-003 — The Costly Deleter

## 🧩 Scenario / Escenario

This query was written during an incident response at 2am. It works — barely — and it catches mass file deletion activity that could indicate ransomware pre-staging or a destructive insider. It made it to production as-is.

Your job: **optimize it**. The query currently times out on busy tenants. Identify the performance problems and rewrite it to be production-grade without losing detection fidelity.

[ES] Esta consulta fue escrita durante una respuesta a incidentes a las 2am. Funciona — a duras penas — y detecta actividad de eliminación masiva de archivos que podría indicar pre-staging de ransomware o un insider destructivo. Llegó a producción tal como está.

Tu trabajo: **optimizarla**. La consulta actualmente expira en tenants con alta carga. Identifica los problemas de rendimiento y reescríbela para que sea de nivel producción sin perder fidelidad de detección.

---

## 🐢 Slow Query / Consulta lenta

```kql
DeviceFileEvents
| join kind=leftouter (
    DeviceProcessEvents
    | project DeviceId, InitiatingProcessFileName, InitiatingProcessId, Timestamp
) on DeviceId, $left.InitiatingProcessId == $right.InitiatingProcessId
| where ActionType == "FileDeleted"
| where FileName !endswith ".tmp"
| where FileName !endswith ".log"
| summarize DeleteCount = count(), FileList = make_set(FileName) by DeviceName, InitiatingProcessFileName, bin(Timestamp, 5m)
| where DeleteCount > 50
| order by DeleteCount desc
```

---

## 📋 What it should do / Qué debería hacer

Detect devices where a single process deleted more than 50 non-temporary files within any 5-minute window. Include the device name, the initiating process, the count of deleted files, and a sample of filenames.

[ES] Detectar dispositivos donde un único proceso eliminó más de 50 archivos no temporales dentro de cualquier ventana de 5 minutos. Incluir el nombre del dispositivo, el proceso iniciador, el conteo de archivos eliminados y una muestra de nombres de archivos.

---

## 🔍 Known Performance Issues / Problemas de rendimiento conocidos

There are at least **3 identifiable problems** with the query above:

1. The `join` runs before any filtering — it ingests the full `DeviceProcessEvents` table
2. No time bounds — the query scans the entire retention window
3. String filters run on every row before aggregation reduces the dataset

[ES] Hay al menos **3 problemas identificables** con la consulta anterior:

1. El `join` se ejecuta antes de cualquier filtrado — ingiere la tabla `DeviceProcessEvents` completa
2. Sin límites de tiempo — la consulta escanea toda la ventana de retención
3. Los filtros de cadena se ejecutan en cada fila antes de que la agregación reduzca el conjunto de datos

---

## 💡 Hint / Pista

`summarize` before `join`, `where` before everything else, and always bound your time. KQL executes left-to-right — order is performance.

[ES] `summarize` antes de `join`, `where` antes que todo, y siempre limita el tiempo. KQL se ejecuta de izquierda a derecha — el orden es rendimiento.

---

## 📊 Difficulty / Dificultad

- Apprentice — Single table, common operator
- Hunter — Multi-table join or time-based logic
- **Archmage — Complex correlation, performance optimization, or behavioral chaining** ← This one

---
🔍 Solution in: [solutions/optimize-this/solution-001.md](../../solutions/optimize-this/solution-001.md)
