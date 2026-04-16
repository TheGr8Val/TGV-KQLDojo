---
id: KD-002
category: fix-this
difficulty: Apprentice
tags: [PowerShell, encoded-command, DeviceProcessEvents, defense-evasion]
mitre: T1027, T1059.001
author: thegr8val
language: EN/ES
---

# Challenge KD-002 — Broken Encoder

## 🧩 Scenario / Escenario

A junior analyst wrote a detection for PowerShell encoded command execution and pushed it to production. It hasn't fired once in three weeks. The environment is active — endpoint telemetry is flowing. Something is wrong with the query.

Your job: **find and fix the 3 bugs** without rewriting the logic.

[ES] Un analista junior escribió una detección para la ejecución de comandos codificados de PowerShell y la desplegó en producción. No ha disparado ni una vez en tres semanas. El entorno está activo — la telemetría de endpoints fluye. Algo está mal en la consulta.

Tu trabajo: **encontrar y corregir los 3 errores** sin reescribir la lógica.

---

## 🐛 Broken Query / Consulta rota

```kql
DeviceProcessEvent
| where Timestamp > ago(7d)
| where FileName =~ "powershell.exe"
| where ProcessCommandLine contain "-EncodedCommand"
| where ProcessCommandLine !contains "-NonInteractive"
| project DeviceName, Timestamp, ProcessCommandLine, AccountName
```

---

## 📋 What it should do / Qué debería hacer

Detect PowerShell processes launched with the `-EncodedCommand` (or `-enc`) flag in the last 7 days. Exclude runs that are explicitly non-interactive (common in legitimate automation). Return device name, timestamp, the full command line, and the account that ran it.

[ES] Detectar procesos de PowerShell lanzados con el flag `-EncodedCommand` (o `-enc`) en los últimos 7 días. Excluir ejecuciones explícitamente no interactivas (común en automatización legítima). Retornar nombre del dispositivo, timestamp, la línea de comando completa y la cuenta que lo ejecutó.

---

## 💡 Hint / Pista

One bug is a typo in a table name. One is a wrong operator for substring matching. One is a missing column that breaks the `project` output silently.

[ES] Un error es un typo en el nombre de la tabla. Uno es un operador incorrecto para búsqueda de subcadenas. Uno es una columna que no existe y rompe el `project` silenciosamente.

---

## 📊 Difficulty / Dificultad

- **Apprentice — Single table, common operator** ← This one
- Hunter — Multi-table join or time-based logic
- Archmage — Complex correlation, performance optimization, or behavioral chaining

---
🔍 Solution in: [solutions/fix-this/solution-001.md](../../solutions/fix-this/solution-001.md)
