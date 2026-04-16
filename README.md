<p align="center">
  <img src="assets/Post de Instagram - Master the skills needed for effective threat hunting and defense strategies.png" alt="Train your query mind. Hunt with precision." width="480"/>
</p>

<h1 align="center">TGV-KQLDojo</h1>

<p align="center">
  <strong>Train your query mind. Hunt with precision.</strong><br>
  <em>Entrena tu mente de consulta. Caza con precisión.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/challenges-4-teal"/>
  <img src="https://img.shields.io/badge/language-KQL-blue"/>
  <img src="https://img.shields.io/badge/platform-Microsoft%20Sentinel-0078D4"/>
  <a href="https://buymeacoffee.com/thegr8val">
    <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow"/>
  </a>
</p>

---

## 🧠 What is this? / ¿Qué es esto?

**TGV-KQLDojo** is a structured collection of KQL (Kusto Query Language) challenges designed for security analysts working in Microsoft Sentinel and Defender environments. Each challenge sharpens a specific skill: writing detections from scratch, diagnosing broken queries, tuning for production performance, or building detection logic from raw telemetry.

No vendor certifications. No toy datasets. Real table schemas. Real threat techniques.

[ES] **TGV-KQLDojo** es una colección estructurada de desafíos KQL (Kusto Query Language) diseñada para analistas de seguridad que trabajan en entornos de Microsoft Sentinel y Defender. Cada desafío agudiza una habilidad específica: escribir detecciones desde cero, diagnosticar consultas rotas, optimizar para producción o construir lógica de detección desde telemetría cruda.

Sin certificaciones de vendor. Sin datasets de juguete. Esquemas de tablas reales. Técnicas de amenaza reales.

---

## 🎯 Difficulty System / Sistema de Dificultad

| Level | Icon | Description |
|-------|------|-------------|
| **Apprentice** | 🟢 | Single table, common operators. Good entry point for analysts building their KQL foundation. |
| **Hunter** | 🟡 | Multi-table joins, time-based logic, or technique-specific context required. |
| **Archmage** | 🔴 | Complex correlation, performance optimization, behavioral chaining, or production-grade constraints. |

[ES]

| Nivel | Icono | Descripción |
|-------|-------|-------------|
| **Apprentice** | 🟢 | Tabla única, operadores comunes. Buen punto de entrada para analistas que construyen su base de KQL. |
| **Hunter** | 🟡 | Joins multi-tabla, lógica temporal o contexto específico de técnica requerido. |
| **Archmage** | 🔴 | Correlación compleja, optimización de rendimiento, encadenamiento de comportamientos o restricciones de nivel producción. |

---

## 📂 Challenge Categories / Categorías de Desafíos

| Category | Icon | Description |
|----------|------|-------------|
| `write-this` | ✍️ | Write a detection query from a scenario description and schema. Blank page, your logic. |
| `fix-this` | 🐛 | A broken query is provided. Find the bugs — wrong operators, wrong column names, logic errors. |
| `optimize-this` | ⚡ | A working-but-slow query is provided. Identify the performance problems and fix them. |
| `detect-this` | 🔍 | Raw log telemetry is provided. Analyze the behavior and write the detection. |

[ES]

| Categoría | Icono | Descripción |
|-----------|-------|-------------|
| `write-this` | ✍️ | Escribe una consulta de detección a partir de una descripción de escenario y esquema. |
| `fix-this` | 🐛 | Se proporciona una consulta rota. Encuentra los errores — operadores incorrectos, nombres de columna erróneos, errores de lógica. |
| `optimize-this` | ⚡ | Se proporciona una consulta funcional pero lenta. Identifica los problemas de rendimiento y corrígelos. |
| `detect-this` | 🔍 | Se proporciona telemetría de logs cruda. Analiza el comportamiento y escribe la detección. |

---

## 📋 Challenge Index / Índice de Desafíos

| ID | Category | Difficulty | Title | Table |
|----|----------|------------|-------|-------|
| [KD-001](challenges/write-this/challenge-001.md) | ✍️ write-this | 🟡 Hunter | No Passport Required | `SigninLogs` |
| [KD-002](challenges/fix-this/challenge-001.md) | 🐛 fix-this | 🟢 Apprentice | Broken Encoder | `DeviceProcessEvents` |
| [KD-003](challenges/optimize-this/challenge-001.md) | ⚡ optimize-this | 🔴 Archmage | The Costly Deleter | `DeviceFileEvents` |
| [KD-004](challenges/detect-this/challenge-001.md) | 🔍 detect-this | 🟡 Hunter | Ghost in the Memory | `DeviceProcessEvents` |

---

## 🚀 How to Use / Cómo Usar

1. 🗂️ Pick a challenge from the index above.
2. 📖 Read the scenario, schema, and requirements carefully.
3. 🧪 Write or fix the query in your environment — Microsoft Sentinel, Defender Advanced Hunting, or [Log Analytics demo workspace](https://portal.loganalytics.io/demo).
4. ✅ Check your answer in the linked solution file.

[ES]

1. 🗂️ Elige un desafío del índice.
2. 📖 Lee el escenario, esquema y requisitos con atención.
3. 🧪 Escribe o corrige la consulta en tu entorno — Microsoft Sentinel, Defender Advanced Hunting, o el workspace de demo de Log Analytics.
4. ✅ Verifica tu respuesta en el archivo de solución vinculado.

---

## 🗺️ Roadmap / Lo que viene

This dojo is actively growing. Here's what's planned:

| Feature | Status |
|---------|--------|
| 📦 New challenges across all categories | 🔄 Ongoing |
| 🧩 `write-this` — DNS beaconing detection (DeviceNetworkEvents) | 🔜 Planned |
| 🐛 `fix-this` — Broken lateral movement query (SecurityEvent + IdentityLogonEvents) | 🔜 Planned |
| ⚡ `optimize-this` — Slow email threat hunting query (EmailEvents) | 🔜 Planned |
| 🔍 `detect-this` — Pass-the-Hash from Windows Security Events | 🔜 Planned |
| 🏆 Difficulty progression tracks (Apprentice → Hunter → Archmage paths) | 💭 Considering |
| 🌐 Table quick-reference cheatsheet (schema + common operators) | 🔜 Planned |
| 🤝 Community challenge submissions | 🔜 Planned |

> Have a technique you want covered? Open an issue.

[ES] Este dojo está creciendo activamente. Esto es lo que viene:

- 📦 Nuevos desafíos en todas las categorías
- 🧩 Detección de DNS beaconing (DeviceNetworkEvents)
- 🐛 Consulta rota de movimiento lateral (SecurityEvent + IdentityLogonEvents)
- ⚡ Optimización de consulta de threat hunting de email (EmailEvents)
- 🔍 Detección de Pass-the-Hash desde Windows Security Events
- 🌐 Cheatsheet de referencia de tablas

---

## 🤝 Contributing / Contribuir

Want to submit a challenge? Read [CONTRIBUTING.md](CONTRIBUTING.md).

Community submissions are welcome. Bring your best work — real schemas, real techniques, no toy examples.

[ES] ¿Quieres enviar un desafío? Lee [CONTRIBUTING.md](CONTRIBUTING.md).

---

## ☕ Support / Apoya el Proyecto

If this helps you level up your detection engineering skills, consider buying me a coffee.

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow)](https://buymeacoffee.com/thegr8val)

---

<p align="center">
  Built by <strong><a href="https://github.com/thegr8val">thegr8val</a></strong><br>
  <em>Community tooling for the next generation of security practitioners.</em>
</p>
