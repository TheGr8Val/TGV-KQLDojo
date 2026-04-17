<p align="center">
  <img src="assets/Post de Instagram - Master the skills needed for effective threat hunting and defense strategies.png" alt="Train your query mind. Hunt with precision." width="480"/>
</p>

<h1 align="center">TGV-KQLDojo</h1>

<p align="center">
  <strong>Train your query mind. Hunt with precision.</strong><br>
  <em>Entrena tu mente de consulta. Caza con precisión.</em>
</p>

<p align="center">
  <a href="#"><img src="https://img.shields.io/badge/Challenges-4-0078D4?style=for-the-badge&logo=microsoft&logoColor=white"/></a>
  <a href="#"><img src="https://img.shields.io/badge/Language-KQL-00b4d8?style=for-the-badge"/></a>
  <a href="#"><img src="https://img.shields.io/badge/Platform-Microsoft%20Sentinel-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white"/></a>
  <a href="#"><img src="https://img.shields.io/badge/Bilingual-EN%20%7C%20ES-f97316?style=for-the-badge"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge"/></a>
  <a href="https://buymeacoffee.com/thegr8val"><img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-%E2%98%95-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black"/></a>
</p>

---

## 🧠 What is this? / ¿Qué es esto?

**TGV-KQLDojo** is a structured collection of KQL challenges for security analysts working in Microsoft Sentinel and Defender environments. Each challenge sharpens a specific skill: writing detections from scratch, diagnosing broken queries, tuning for production performance, or building detection logic from raw telemetry.

No vendor certifications. No toy datasets. Real table schemas. Real threat techniques.

[ES] **TGV-KQLDojo** es una colección estructurada de desafíos KQL para analistas de seguridad en entornos de Microsoft Sentinel y Defender. Sin certificaciones de vendor. Sin datasets de juguete. Esquemas de tablas reales. Técnicas de amenaza reales.

---

## 🎯 Difficulty System / Sistema de Dificultad

```
  🟢  Apprentice  ──  Single table, common operators
                      Good entry point for analysts building KQL foundations

  🟡  Hunter      ──  Multi-table joins, time-based logic,
                      technique-specific context required

  🔴  Archmage    ──  Complex correlation, performance optimization,
                      behavioral chaining, production-grade constraints
```

| Level | Icon | What It Tests |
|-------|:----:|---|
| **Apprentice** | 🟢 | Single table · common operators · entry-level detection logic |
| **Hunter** | 🟡 | Multi-table joins · time windows · MITRE-specific context |
| **Archmage** | 🔴 | Correlation chains · performance · production-grade constraints |

---

## 📂 Challenge Categories / Categorías de Desafíos

```
  ┌─────────────────┬────────────────────────────────────────────────────┐
  │  ✍️  write-this  │  Blank page. Scenario + schema provided.           │
  │                 │  Your detection logic, your operators.              │
  ├─────────────────┼────────────────────────────────────────────────────┤
  │  🐛 fix-this    │  Broken query provided.                             │
  │                 │  Find the bugs — wrong ops, bad columns, bad logic. │
  ├─────────────────┼────────────────────────────────────────────────────┤
  │  ⚡ optimize-   │  Working but slow query provided.                   │
  │     this        │  Fix the performance problems.                      │
  ├─────────────────┼────────────────────────────────────────────────────┤
  │  🔍 detect-this │  Raw log telemetry provided.                        │
  │                 │  Analyze the behavior. Write the detection.         │
  └─────────────────┴────────────────────────────────────────────────────┘
```

---

## 📋 Challenge Index / Índice de Desafíos

| ID | Category | Difficulty | Title | Table |
|----|----------|:----------:|-------|-------|
| [KD-001](challenges/write-this/challenge-001.md) | ✍️ write-this | 🟡 Hunter | No Passport Required | `SigninLogs` |
| [KD-002](challenges/fix-this/challenge-001.md) | 🐛 fix-this | 🟢 Apprentice | Broken Encoder | `DeviceProcessEvents` |
| [KD-003](challenges/optimize-this/challenge-001.md) | ⚡ optimize-this | 🔴 Archmage | The Costly Deleter | `DeviceFileEvents` |
| [KD-004](challenges/detect-this/challenge-001.md) | 🔍 detect-this | 🟡 Hunter | Ghost in the Memory | `DeviceProcessEvents` |

---

## 🗺️ Learning Path / Ruta de Aprendizaje

```
  ┌────────────────────────────────────────────────────────────────────┐
  │                        KQLDojo Path                               │
  │                                                                    │
  │   [🟢 Apprentice]──────▶[🟡 Hunter]──────────▶[🔴 Archmage]      │
  │    Single table           Joins + time           Complex chains    │
  │    KD-002                 KD-001 · KD-004         KD-003           │
  │                                                                    │
  │   Tip: Start with fix-this (KD-002) to build pattern recognition  │
  │   before writing from scratch.                                     │
  └────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 How to Use / Cómo Usar

```
  1.  🗂️  Pick a challenge from the index above.
  2.  📖  Read the scenario, schema, and requirements carefully.
  3.  🧪  Write or fix your query in one of these environments:
          ├── Microsoft Sentinel  (Log Analytics workspace)
          ├── Defender Advanced Hunting
          └── Log Analytics demo workspace (portal.loganalytics.io/demo)
  4.  ✅  Check your answer in the linked solution file.
```

---

## 📊 Accepted Tables / Tablas Aceptadas

| Table | Platform |
|-------|----------|
| `DeviceProcessEvents` | Microsoft Defender for Endpoint |
| `DeviceNetworkEvents` | Microsoft Defender for Endpoint |
| `DeviceFileEvents` | Microsoft Defender for Endpoint |
| `SecurityEvent` | Microsoft Sentinel (Windows Security Events) |
| `SigninLogs` | Microsoft Sentinel (Entra ID) |
| `AuditLogs` | Microsoft Sentinel (Entra ID) |
| `IdentityLogonEvents` | Microsoft Defender for Identity |
| `EmailEvents` | Microsoft Defender for Office 365 |
| `CloudAppEvents` | Microsoft Defender for Cloud Apps |
| `AlertInfo` | Microsoft Sentinel (unified alerts) |

---

## 🗺️ Roadmap / Lo que viene

| Feature | Status |
|---------|:------:|
| 📦 New challenges across all categories | 🔄 Ongoing |
| 🧩 DNS beaconing detection (`DeviceNetworkEvents`) | 🔜 Planned |
| 🐛 Broken lateral movement query (`SecurityEvent` + `IdentityLogonEvents`) | 🔜 Planned |
| ⚡ Slow email threat hunting query (`EmailEvents`) | 🔜 Planned |
| 🔍 Pass-the-Hash from Windows Security Events | 🔜 Planned |
| 🏆 Difficulty progression tracks | 💭 Considering |
| 🌐 Table quick-reference cheatsheet | 🔜 Planned |
| 🤝 Community challenge submissions | 🔜 Planned |

---

## 🤝 Contributing / Contribuir

Want to submit a challenge? Read [CONTRIBUTING.md](CONTRIBUTING.md).

Community submissions are welcome. Bring your best work — real schemas, real techniques, no toy examples.

---

## 📄 License

[MIT](LICENSE) — thegr8val

---

<p align="center">
  <strong>TGV Toolkit</strong><br><br>
  <a href="https://github.com/TheGr8Val/TGV-Grimoire">
    <img src="https://img.shields.io/badge/TGV-Grimoire-6366f1?style=for-the-badge"/>
  </a>
  <a href="https://github.com/TheGr8Val/TGV-VulnSpotter">
    <img src="https://img.shields.io/badge/TGV-VulnSpotter-ef4444?style=for-the-badge"/>
  </a>
  <a href="https://github.com/TheGr8Val/TGV-ReportForge">
    <img src="https://img.shields.io/badge/TGV-ReportForge-00bcd4?style=for-the-badge"/>
  </a>
  <a href="https://github.com/TheGr8Val/TGV-CareerCompass">
    <img src="https://img.shields.io/badge/TGV-CareerCompass-22c55e?style=for-the-badge"/>
  </a>
  <br><br>
  <a href="https://buymeacoffee.com/thegr8val">
    <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-%E2%98%95%20Support%20thegr8val-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black"/>
  </a>
  <br><br>
  <em>Built by <strong>thegr8val</strong> — Community tooling for the next generation of security practitioners.</em>
</p>
