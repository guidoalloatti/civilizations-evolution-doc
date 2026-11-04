# CLAUDE.md — Civilization Evolutions

> Project instructions for Claude Code. **Read this file at the start of every session.**
> It is deliberately concise: the authority on detail lives in the original GDDs. Don't copy GDDs here; reference them.

## What Civilization Evolutions is

A **real-time historical strategy and simulation** game: an initial community transforms, over centuries, into a civilization with its own settlements, economy, institutions, culture and history. It spans from the **Paleolithic to the beginning of the Industrial Revolution** (cutoff approx. 1750–1800, pending confirmation — see `AUDIT_REPORT.md` C-01). Real history is a **plausibility reference, not a mandatory script**: evolution is free and non-linear. The player is a **guiding entity** for the civilization (not a ruler), with broad influence but respecting the autonomy of the population and non-player civilizations. Engine: **Unreal Engine 5**, C++ as the primary language.

Vision document: [docs/00-project/GDD_MAESTRO.md](docs/00-project/GDD_MAESTRO.md). Canonical documentation language: **English**.

## Design principles (non-negotiable)

1. **Integrated simulation, not loose features.** Systems share data and respond to common events. A technology changes production, construction, transport, culture, etc.
2. **Free historical evolution.** Periods = references, not mandatory unlocks. Driven by conditions/knowledge/resources, **not by the year**.
3. **Visible world evolution.** Buildings, cities, landscape and culture preserve the imprint of history.
4. **Geography with consequences.** It conditions resources, settlements, transport and development.
5. **Configurable complexity** (basic / intermediate / advanced) and **automation with player intervention**.
6. **Hybrid scale:** individuals in small settlements → aggregated groups in large civilizations, keeping coherence.
7. **Strategic freedom:** no single optimal path. War matters but is **secondary** to the civil/social dimension.

## Architecture constraints (from the technical plan)

- **Separate simulation from presentation.** Logic does NOT depend on rendered meshes/actors or on the render `Tick()`. Today this is realized in **`civilizations-evolution/SimCore/`** (an engine-agnostic C++17 core, with tests) wrapped by the UE layer.
- **Data-driven:** resources, buildings, recipes, professions, technologies, materials and periods defined by data (Data Assets / Data Tables / structs), not hardcoded.
- **Modular:** independent domains that communicate through interfaces/events. Avoid monolithic classes and premature empty abstractions.
- **Simulation clock** independent of FPS, with pause/speeds and day/month/year/season progression.
- **Scalability and performance** as a priority from the design; don't simplify the architecture just to "fit" the prototype.
- **Shared identity:** a resource/entity has **one** identifier and a common inventory; never duplicate resources/relationships per system.

## Where the documentation lives

```
Civilization Evolutions/            (work root = this repo; there is a docs-only copy on the Desktop)
├── CLAUDE.md                        ← this file
├── DOCUMENTATION_INDEX.md           ← navigable index of all GDDs (entry point)
├── SYSTEM_ARCHITECTURE_MAP.md       ← conceptual map of systems and relationships
├── IMPLEMENTATION_ALIGNMENT.md      ← what the design says vs what is implemented
├── DOCUMENTATION_READING_GUIDE.md   ← what to read before each type of task
├── (Blueprint Phase 2) DESIGN_CONSISTENCY_AUDIT · DESIGN_DECISION_REGISTER · SHARED_DATA_MODEL ·
│       SYSTEM_DEPENDENCY_MAP · CROSS_SYSTEM_VALIDATION_SCENARIOS · DEVELOPMENT_ROADMAP ·
│       SYSTEM_IMPLEMENTATION_SPECIFICATIONS · HISTORICAL_EVOLUTION_FRAMEWORK ·
│       IMPLEMENTATION_RISK_REGISTER   (analysis derived from the GDDs; DR-00..DR-12 confirmed by delegation 2026-11-04)
├── (Multi-agent development) AGENT_ORGANIZATION · AGENT_OWNERSHIP_MAP · SYSTEM_CONTRACTS ·
│       AGENT_TASK_BOARD · GIT_WORKFLOW · INTEGRATION_STRATEGY   (parallel agent organization; PROPOSAL 2026-11-04)
├── AUDIT_REPORT.md                  ← contradictions (C-01..C-08) and cross-cutting open items
├── docs/
│   ├── 00-project/GDD_MAESTRO.md    ← vision, pillars, authority map, integrations, vocabulary
│   ├── 01-systems/                  ← 22 system GDDs (source of detail per domain)
│   └── 02-technical/PLAN_TECNICO_v0.1.md  ← technical architecture (separate from design)
├── BACKUP_PRE_AUDIT_2026-11-03/     ← full backup; DO NOT edit (kept in the original language)
└── civilizations-evolution/         ← Unreal project + SimCore + condensed Docs/
    ├── SimCore/                     ← C++ simulation core (engine-agnostic, with tests)
    ├── Source/CivilizationSimulator/← UE layer (presentation/game)
    └── Docs/                        ← condensed GDDs that guided the first code
```

## How to navigate the documentation (flow)

1. Open **`DOCUMENTATION_INDEX.md`** to locate the domain's GDD.
2. Read the corresponding **system GDD** (authority on detail).
3. Check **`GDD_MAESTRO.md` §5 (authority) and §6 (integrations)** for cross-system dependencies.
4. Use **`DOCUMENTATION_READING_GUIDE.md`** to know which GDDs to read together for a task.
5. To see what is done, look at **`IMPLEMENTATION_ALIGNMENT.md`**.
6. For open decisions/contradictions, **`AUDIT_REPORT.md`**.

Read the **original** when the decision matters; the summaries do not replace the source GDD.

### Which GDDs to consult before touching each system (summary; detail in the reading guide)

- **Production/economy:** `GDD_Produccion_y_Trabajo_v0.2` + `GDD_Economia_v0.2` + `GDD_Recursos_y_Materiales` + `GDD_Transporte_Comercio_y_Logistica` + `GDD_Poblacion_y_Sociedad`.
- **Construction:** `GDD_Construccion_y_Arquitectura` + `GDD_Recursos_y_Materiales` + `GDD_Tecnologia_Ciencia_e_Innovacion` + `GDD_Poblacion_y_Sociedad`.
- **World/terrain:** `GDD_Generacion_Procedural_del_Mundo` + `GDD_Geografia_Clima_y_Medioambiente` + `GDD_Exploracion_y_Expansion_Territorial`.
- **Population/society:** `GDD_Poblacion_y_Sociedad` + `GDD_Entidades_y_Relaciones` + `GDD_Cultura...` + `GDD_Religion...`.
- **Time:** `GDD_Tiempo_y_Simulacion` (clock common to all).

> Note: GDD file names are kept in their original Spanish to preserve the hundreds of internal links; their **content** is in English.

## Rules for future development

1. **Do not contradict confirmed decisions** of the GDDs. If something is ambiguous or clashes, **document it as unresolved and ask**; don't invent the answer or change it silently (update `AUDIT_REPORT.md` and the Master when it is decided).
2. **Do not duplicate** entities, resources, relationships or logic shared across systems.
3. **Keep simulation separate from presentation** and prefer data-driven definitions.
4. **Modular, extensible architecture**; no monoliths.
5. **Staged development**, not the whole game at once. Respect the per-system MVPs and the cross-cutting prototype.
6. **Preserve historical flexibility and emergent behavior**; don't hardwire mandatory chronologies.
7. **Do not sacrifice simulation depth** just to simplify the prototype.
8. **Build and test** after each significant change; keep the project buildable. Add tests (SimCore already ships a suite).
9. When the documentation changes, **update the index and the reading guide**.

## Implementation status (summary; detail in IMPLEMENTATION_ALIGNMENT.md)

- **SimCore** (pure C++, with tests): clock with seasons, resources/inventories, recipes and production, worker assignment, simplified logistics, consumption/hunger/health, construction with cost, demolition, technology tree, building evolution, trade between settlements, save/load.
- **UE layer** (`Source/CivilizationSimulator/`): GameMode, PlayerController (selection/orders/build menu), CameraPawn (RTS), ViewManager (environment + HUD), BuildingActor, procedural Terrain, SimSubsystem (bridge to SimCore), **CivUnit** (animated settlers), **CivResourceNode** (gathering), **CivHUD**, **CivText** (i18n English by default + Spanish). PBR materials in `/Game/PBR`, character in `/Game/Chars`. Lumen/SM6 enabled.
- This code **is being actively extended** (map resource system, hunting, hauling). Before touching a file, read its current state on disk.

## Multi-agent development (PROPOSAL — pending approval)

To scale development with several agents in parallel **without breaking architectural consistency**, there is an organization proposal (no agents are launched and no worktrees are created until the project owner approves):

- **[AGENT_ORGANIZATION.md](AGENT_ORGANIZATION.md)** — 9 workstreams (Technical Director, Core/Simulation, World/Geography, Construction, Economy/Resources/Production, Population, UI/UX, Art, QA/Integration): scope, exclusions, dependencies, deliverables, allowed/restricted files and acceptance criteria.
- **[SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)** — shared interfaces (time, events, identity, resources, inventories, buildings, population, terrain, placement, UI access), with their real API today and their owner.
- **[AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md)** — file/folder → single owner; shared-file and `.uasset` protocols.
- **[AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md)** — tasks by milestone (M0–M4 + continuous), with dependencies, priority, files, acceptance, status and blockers.
- **[GIT_WORKFLOW.md](GIT_WORKFLOW.md)** and **[INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md)** — branches/worktrees per workstream (two repos: docs + UE), unmergeable UE binaries, and central integration by the Technical Director.

Base rule: **one owner per datum and per file**; everyone else **reads by contract**. Before parallelizing, do **M0** (publish contracts + split `SimCore/Simulation.*`/`World.*` by domain + commit the pending work + configure Git/LFS).

## Operational notes

- **Build:** `SimCore` with CMake; the UE project with `Build.bat CivilizationSimulatorEditor Win64 Development` (editor closed).
- **Git:** the UE project (`civilizations-evolution/`) has its **own** git repository (history up to the `HANDOFF` commit); the documentation/design is versioned in a **separate** repo at the root. The work **after HANDOFF** (this session's fixes + new gameplay) is **uncommitted**: commit at each green checkpoint. With two sessions on the same tree, beware of overwrites; read before editing.
- The `BACKUP_PRE_AUDIT_2026-11-03/` folder and the original GDDs **are not edited**.
