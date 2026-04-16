---
id: KD-004
category: detect-this
difficulty: Hunter
tags: [LSASS, credential-dumping, DeviceProcessEvents, lateral-movement]
mitre: T1003.001
author: thegr8val
language: EN/ES
---

# Challenge KD-004 — Ghost in the Memory

## 🧩 Scenario / Escenario

Your threat intel team flagged a new campaign where attackers are dumping LSASS memory to harvest credentials. You've been handed raw telemetry from `DeviceProcessEvents`. Study the log sample below and **write a KQL query** that would have detected this behavior.

[ES] Tu equipo de inteligencia de amenazas marcó una nueva campaña en la que atacantes están volcando la memoria de LSASS para recolectar credenciales. Te entregaron telemetría cruda de `DeviceProcessEvents`. Estudia la muestra de logs a continuación y **escribe una consulta KQL** que hubiera detectado este comportamiento.

---

## 📄 Log Sample / Muestra de logs

The following events were extracted from `DeviceProcessEvents` on the compromised endpoint `WKSTN-FINANCE-04`:

```
TimeGenerated: 2024-03-15T14:22:11Z
DeviceName: WKSTN-FINANCE-04
AccountName: jsmith
InitiatingProcessFileName: cmd.exe
InitiatingProcessCommandLine: cmd.exe /c "rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump 624 C:\Users\Public\lsass.dmp full"
FileName: rundll32.exe
ProcessCommandLine: rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump 624 C:\Users\Public\lsass.dmp full
ProcessId: 9832
InitiatingProcessId: 9104
ActionType: ProcessCreated

---

TimeGenerated: 2024-03-15T14:22:14Z
DeviceName: WKSTN-FINANCE-04
AccountName: jsmith
InitiatingProcessFileName: rundll32.exe
FileName: lsass.exe
ProcessCommandLine: lsass.exe
ProcessId: 624
InitiatingProcessId: 9832
ActionType: OpenProcess

---

TimeGenerated: 2024-03-15T14:22:15Z
DeviceName: WKSTN-FINANCE-04
AccountName: jsmith
InitiatingProcessFileName: rundll32.exe
FileName: lsass.dmp
ProcessCommandLine: (none)
ProcessId: 9832
InitiatingProcessId: 9104
ActionType: FileCreated
FolderPath: C:\Users\Public\lsass.dmp
```

---

## 📋 Requirements / Requisitos

Write a KQL query using `DeviceProcessEvents` that:

1. Detects LSASS memory dump attempts via `comsvcs.dll MiniDump` technique
2. Also catches common alternative tools: `procdump`, `mimikatz`, `sqldumper`
3. Scopes to the last 24 hours
4. Returns: `DeviceName`, `AccountName`, `ProcessCommandLine`, `InitiatingProcessFileName`, `TimeGenerated`
5. Is written to minimize false positives (no legitimate admin tools should fire this)

[ES] Escribe una consulta KQL usando `DeviceProcessEvents` que:

1. Detecte intentos de volcado de memoria de LSASS mediante la técnica `comsvcs.dll MiniDump`
2. También capture herramientas alternativas comunes: `procdump`, `mimikatz`, `sqldumper`
3. Limite al alcance de las últimas 24 horas
4. Retorne: `DeviceName`, `AccountName`, `ProcessCommandLine`, `InitiatingProcessFileName`, `TimeGenerated`
5. Esté escrita para minimizar falsos positivos

---

## 💡 Hint / Pista

The technique uses `comsvcs.dll` with the `MiniDump` export. The process being opened (`lsass.exe`) is the victim — look at what's initiating the access, not what's being accessed.

[ES] La técnica usa `comsvcs.dll` con la exportación `MiniDump`. El proceso que se abre (`lsass.exe`) es la víctima — mira qué está iniciando el acceso, no qué está siendo accedido.

---

## 📊 Difficulty / Dificultad

- Apprentice — Single table, common operator
- **Hunter — Multi-table join or time-based logic** ← This one
- Archmage — Complex correlation, performance optimization, or behavioral chaining

The detection requires recognizing multiple technique variants and encoding them without triggering on benign activity.

---
🔍 Solution in: [solutions/detect-this/solution-001.md](../../solutions/detect-this/solution-001.md)
