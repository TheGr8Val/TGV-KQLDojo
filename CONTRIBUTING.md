# Contributing to TGV-KQLDojo

> The dojo improves because practitioners bring what they've earned on the floor.

---

## How to Submit a Challenge / Cómo enviar un desafío

1. **Fork** this repository.
2. Create a branch: `challenge/<category>-<short-title>` (e.g., `challenge/write-this-dns-beacon`)
3. Add your challenge file to `challenges/<category>/challenge-XXX.md`
4. Add the corresponding solution to `solutions/<category>/solution-XXX.md`
5. Update the **Challenge Index** table in `README.md`
6. Open a Pull Request with a short description of the scenario and technique covered.

[ES]

1. **Haz un fork** de este repositorio.
2. Crea una rama: `challenge/<categoria>-<titulo-corto>`
3. Agrega tu archivo de desafío en `challenges/<categoria>/challenge-XXX.md`
4. Agrega la solución correspondiente en `solutions/<categoria>/solution-XXX.md`
5. Actualiza la tabla del **Índice de Desafíos** en `README.md`
6. Abre un Pull Request con una descripción breve del escenario y la técnica cubierta.

---

## Quality Bar / Estándar de calidad

Every submission must meet all of the following:

- **Real table schema.** Queries must target actual Sentinel or MDE tables. No made-up columns. If in doubt, verify against the Microsoft documentation links below.
- **Working solution.** The solution query must be syntactically correct and logically sound. Test it in a real workspace or the [Log Analytics demo environment](https://portal.loganalytics.io/demo) before submitting.
- **MITRE ATT&CK reference.** Every challenge must map to at least one MITRE technique ID (e.g., `T1078.004`). Include it in the frontmatter.
- **Original content.** Do not submit queries copied from Microsoft documentation, blog posts, or existing detection rules without significant transformation and pedagogical value.
- **Bilingual blocks.** Write primary content in English. Include `[ES]` blocks for Spanish translations of scenario text, requirements, and explanations.

[ES]

Cada envío debe cumplir con todo lo siguiente:

- **Esquema de tabla real.** Las consultas deben apuntar a tablas reales de Sentinel o MDE.
- **Solución funcional.** La consulta de solución debe ser sintácticamente correcta y lógicamente sólida.
- **Referencia MITRE ATT&CK.** Cada desafío debe mapear a al menos un ID de técnica MITRE.
- **Contenido original.** No envíes consultas copiadas sin transformación significativa y valor pedagógico.
- **Bloques bilingües.** Escribe el contenido principal en inglés. Incluye bloques `[ES]` para traducciones al español.

---

## Accepted Tables / Tablas aceptadas

Challenges must use one or more of the following tables:

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

If you believe another table should be on this list, include your reasoning in the PR description.

---

## Challenge Template / Plantilla de desafío

Copy this template to `challenges/<category>/challenge-XXX.md`:

```markdown
---
id: KD-XXX
category: <write-this|fix-this|optimize-this|detect-this>
difficulty: <Apprentice|Hunter|Archmage>
tags: [tag1, tag2]
mitre: T####.###
author: <your-github-handle>
language: EN/ES
---

# Challenge KD-XXX — [Short Title]

## 🧩 Scenario / Escenario

[Scenario in English]

[ES] [Escenario en español]

---

## 📋 Requirements / Requisitos

[Numbered list of what the query must do]

[ES] [Lista de requisitos en español]

---

## 🗂️ Relevant Schema / Esquema relevante

\`\`\`
TableName
| Column: type   // description
\`\`\`

---

## 💡 Hint / Pista (optional)

[Subtle hint — don't give it away]

[ES] [Pista en español]

---

## 📊 Difficulty / Dificultad

- Apprentice — Single table, common operator
- Hunter — Multi-table join or time-based logic
- Archmage — Complex correlation, performance optimization, or behavioral chaining

---
🔍 Solution in: solutions/[category]/solution-XXX.md
```

---

## Solution Template / Plantilla de solución

Copy this template to `solutions/<category>/solution-XXX.md`:

```markdown
---
id: KD-XXX
challenge: challenges/[category]/challenge-XXX.md
---

# Solution KD-XXX — [Short Title]

## ✅ Answer / Respuesta

\`\`\`kql
[complete working KQL query]
\`\`\`

---

## 🔎 Explanation / Explicación

[Walk through each clause — why it works, what it detects]

[ES] [Explicación en español]

---

## ⚡ Performance Notes / Notas de rendimiento

[Tips on efficiency — time filters, summarize before join, etc.]

---

## 📚 References / Referencias

- [Microsoft Sentinel docs link]
- [MITRE ATT&CK link]
```

---

## Schema References / Referencias de esquema

- [Microsoft Sentinel table reference](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/tables-category)
- [MDE Advanced Hunting schema](https://learn.microsoft.com/en-us/microsoft-365/security/defender/advanced-hunting-schema-tables)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [Log Analytics demo workspace](https://portal.loganalytics.io/demo)

---

<p align="center">
  <em>Discipline and craft. Not casual.</em>
</p>
