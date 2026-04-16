---
id: KD-001
category: write-this
difficulty: Hunter
tags: [impossible-travel, SigninLogs, geolocation, identity]
mitre: T1078.004
author: thegr8val
language: EN/ES
---

# Challenge KD-001 — No Passport Required

## 🧩 Scenario / Escenario

Your SOC has received an alert from the identity team: a senior engineer's account has been flagged by HR after they reported being in Madrid all week. The SIEM shows successful sign-ins from that account — but from multiple locations.

You need to write a KQL query that detects **impossible travel**: a user authenticating from two different countries within a 60-minute window. Any pair of successful logins that crosses country borders inside that window should surface.

[ES] Tu SOC recibió una alerta del equipo de identidad: la cuenta de un ingeniero senior fue marcada porque HR informó que estuvo en Madrid toda la semana. El SIEM muestra inicios de sesión exitosos desde esa cuenta, pero desde múltiples ubicaciones.

Debes escribir una consulta KQL que detecte **viaje imposible**: un usuario que se autentica desde dos países distintos dentro de una ventana de 60 minutos. Cualquier par de inicios de sesión exitosos que cruce fronteras dentro de esa ventana debe aparecer.

---

## 📋 Requirements / Requisitos

- Use the `SigninLogs` table
- Only consider **successful** sign-ins (`ResultType == 0`)
- Compare sign-ins from the **same user** across a **60-minute window**
- Output should include: `UserPrincipalName`, both timestamps, both countries, and the time delta in minutes
- Only flag pairs where the **countries differ**

[ES]

- Usa la tabla `SigninLogs`
- Solo considerar inicios de sesión **exitosos** (`ResultType == 0`)
- Comparar inicios de sesión del **mismo usuario** dentro de una **ventana de 60 minutos**
- La salida debe incluir: `UserPrincipalName`, ambos timestamps, ambos países y el delta de tiempo en minutos
- Solo marcar pares donde los **países difieran**

---

## 🗂️ Relevant Schema / Esquema relevante

```
SigninLogs
| TimeGenerated: datetime
| UserPrincipalName: string
| ResultType: int          // 0 = success
| Location: string         // country name
| IPAddress: string
| AppDisplayName: string
| ConditionalAccessStatus: string
```

---

## 💡 Hint / Pista

You'll need to self-join the table. Think about how to pair each login with every other login from the same user and filter by time proximity.

[ES] Necesitarás hacer un self-join de la tabla. Piensa cómo emparejar cada inicio de sesión con todos los demás del mismo usuario y filtrar por proximidad temporal.

---

## 📊 Difficulty / Dificultad

- Apprentice — Single table, common operator
- **Hunter — Multi-table join or time-based logic** ← This one
- Archmage — Complex correlation, performance optimization, or behavioral chaining

---
🔍 Solution in: [solutions/write-this/solution-001.md](../../solutions/write-this/solution-001.md)
