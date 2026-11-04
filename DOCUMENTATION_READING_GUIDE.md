# DOCUMENTATION_READING_GUIDE — Civilization Evolutions

> Practical guide for consulting the documentation **without re-reading everything on each task**. Always start with [CLAUDE.md](CLAUDE.md) → [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md) → the domain GDD → [GDD_MAESTRO §6](docs/00-project/GDD_MAESTRO.md) for dependencies. Read the **original** when the decision matters.

## Folder structure

```
docs/
├── 00-project/GDD_MAESTRO.md          Vision, authority, integrations, vocabulary, open items
├── 01-systems/                        22 system GDDs (source of detail per domain)
└── 02-technical/PLAN_TECNICO_v0.1.md  Technical architecture (separate from design)
AUDIT_REPORT.md                        Contradictions (C-01..C-08) and cross-cutting open items
CLAUDE.md / DOCUMENTATION_INDEX.md / SYSTEM_ARCHITECTURE_MAP.md / IMPLEMENTATION_ALIGNMENT.md / this file
civilizations-evolution/               UE project + SimCore + condensed Docs/ + code
BACKUP_PRE_AUDIT_2026-11-03/            Immutable backup (do not edit)
```

## Documentation categories

- **Vision/governance:** `GDD_MAESTRO.md` (what the game is, who decides what, how the systems integrate).
- **System GDDs (`docs/01-systems/`):** the detail authority for each domain. Each one carries (depending on the doc) a table of **confirmed decisions**, an **MVP** section, and **complexity** levels (basic/intermediate/advanced).
- **Technical (`docs/02-technical/`):** how to build it in UE5/C++ (architecture, milestones, rules). Separate from design.
- **Operational (`civilizations-evolution/`):** condensed docs that guided the first code, HANDOFF, READMEs and ASSETS.
- **Navigation meta (root):** this guide, the index, the architecture map and the alignment.

## What to read first depending on the task

| If you are going to work on… | Read (in this order) |
|---|---|
| **Production / productive buildings** | `GDD_Produccion_y_Trabajo_v0.2` → `GDD_Economia_v0.2` → `GDD_Recursos_y_Materiales` → `GDD_Transporte_Comercio_y_Logistica` → `GDD_Poblacion_y_Sociedad` → Maestro §6 |
| **Economy / markets / taxes** | `GDD_Economia_v0.2` → `GDD_Recursos_y_Materiales` → `GDD_Produccion_y_Trabajo_v0.2` → `GDD_Poblacion_y_Sociedad` → `GDD_Politica_y_Gobierno` |
| **Resources / inventories / materials** | `GDD_Recursos_y_Materiales` → `GDD_Economia_v0.2` → `GDD_Entidades_y_Relaciones` |
| **Transport / trade / logistics** | `GDD_Transporte_Comercio_y_Logistica` → `GDD_Economia_v0.2` → `GDD_Geografia_Clima_y_Medioambiente` |
| **Construction / urban planning / architecture** | `GDD_Construccion_y_Arquitectura` → `GDD_Recursos_y_Materiales` → `GDD_Tecnologia_Ciencia_e_Innovacion` → `GDD_Poblacion_y_Sociedad` → `GDD_Cultura_Identidad_y_Tradiciones` |
| **World / terrain / biomes** | `GDD_Generacion_Procedural_del_Mundo` → `GDD_Geografia_Clima_y_Medioambiente` → `GDD_Exploracion_y_Expansion_Territorial` |
| **Population / families / classes** | `GDD_Poblacion_y_Sociedad` → `GDD_Entidades_y_Relaciones` → `GDD_Economia_v0.2` → `GDD_Cultura...` → `GDD_Religion...` |
| **Time / seasons / speeds** | `GDD_Tiempo_y_Simulacion` → Maestro §6 (shared clock) |
| **Technology / research** | `GDD_Tecnologia_Ciencia_e_Innovacion` → `GDD_Produccion_y_Trabajo_v0.2` → `GDD_Construccion_y_Arquitectura` |
| **Culture / religion** | `GDD_Cultura_Identidad_y_Tradiciones` + `GDD_Religion_y_Espiritualidad` → `GDD_Poblacion_y_Sociedad` |
| **Politics / territorial administration** | `GDD_Politica_y_Gobierno` → `GDD_Administracion_Territorial_y_Regiones` → `GDD_Poblacion_y_Sociedad` |
| **Diplomacy / AI / other civilizations** | `GDD_Diplomacia...` (⚠️ truncated) + `GDD_IA_y_Autonomia_Civilizaciones` → `GDD_Exploracion...` + `GDD_Guerra...` |
| **War / military** | `GDD_Guerra_y_Sistemas_Militares` → `GDD_Transporte_Comercio_y_Logistica` → `GDD_Diplomacia...` → `GDD_Construccion...` (fortifications) |
| **Events / emergent history** | `GDD_Eventos_y_Evolucion_Historica` → the domain GDD that originates the fact |
| **Interface / HUD / maps** | `GDD_Interfaz_UI_UX` → the GDD of the system whose data is shown |
| **Objectives / ending / difficulty** | `GDD_Progresion_y_Finalizacion` → `GDD_Tiempo_y_Simulacion` |
| **Entity identity/relationships** | `GDD_Entidades_y_Relaciones` → `GDD_Poblacion_y_Sociedad` → `GDD_Administracion_Territorial_y_Regiones` |
| **Anything technical (architecture/UE/C++)** | `docs/02-technical/PLAN_TECNICO_v0.1.md` + `IMPLEMENTATION_ALIGNMENT.md` |

**General rule:** almost everything touches **Resources**, **Entities**, **Time** and **Population**. When in doubt, review those four + Maestro §6.

### Derived analysis (blueprint Phase 2, project root)

For **planning implementation** (it does not replace the GDDs; it is derived analysis):

| If you need… | Read |
|---|---|
| To know which conflicts/decisions are open and their severity | [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) + [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) |
| To model entities without duplication (family≠household, property, aggregation) | [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md) |
| Implementation order, what depends on what, data owner | [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md) |
| To validate that a multi-system flow "closes" before coding | [CROSS_SYSTEM_VALIDATION_SCENARIOS.md](CROSS_SYSTEM_VALIDATION_SCENARIOS.md) (scenarios A–G) |
| What to build at each stage and what NOT yet | [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) + [SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md](SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md) |
| To implement historical/technology evolution without scripting | [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md) |
| To anticipate risks and whether they block the MVP | [IMPLEMENTATION_RISK_REGISTER.md](IMPLEMENTATION_RISK_REGISTER.md) |

> **Before implementing a multi-system feature**, walk through its scenario in CROSS_SYSTEM_VALIDATION_SCENARIOS to confirm that the data and events already exist in the model.

## How to find…

- **Confirmed decisions:** the "Confirmed decisions" section (table) at the end of each GDD. Note: `Recursos`, `Tiempo`, `IA`, `Entidades` and `Diplomacia` do **not** have that table — their rules are stated in the body / under "principles" or "acceptance criteria". Cross-cutting summary in Maestro §4.
- **Unresolved decisions / ambiguities:** `AUDIT_REPORT.md` (C-01..C-08 + §5) and Maestro §8. Several GDDs carry their own list ("Pending topics/to deepen") — with an explicit section only in `Poblacion_y_Sociedad`; the rest defer what is advanced to their levels/MVP.
- **MVP requirements:** the "MVP" section of each GDD + the **cross-cutting MVP** in `01_GDD_GENERAL` §18 and Maestro §3 ("First prototype"). The technical plan defines Milestones 1–6.
- **Technical implementation guide:** `PLAN_TECNICO_v0.1.md` (§4 principles, §5 structure, §6 core, §17 milestones, §18 rules) + `IMPLEMENTATION_ALIGNMENT.md` (what is done).
- **Dependencies between systems:** Maestro §6 (contracts) + the "Integration with other systems" section of each GDD + `SYSTEM_ARCHITECTURE_MAP.md`.
- **Canonical vocabulary:** Maestro §7 (settlement, city, region, civilization, resource/material, production/labor, transport/logistics/trade, confirmed/proposed/pending, MVP/final vision).

## Rules when consulting/editing documentation

1. Do not resolve contradictions by silent edit: record them in `AUDIT_REPORT.md` and, once decided, update the owning GDD first and then the Maestro summary.
2. Label new content as **Confirmed / Proposed / Pending** with version, owner, date and justification.
3. Do not edit the backup originals (`BACKUP_PRE_AUDIT_2026-11-03/`).
4. When the documentation changes, update **this guide** and the **index**.
5. The summaries (index, map, alignment) do **not** replace the source GDD for important decisions.
