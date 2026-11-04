# AGENT_ORGANIZATION — Civilization Evolutions

> **Multi-agent development organization proposal.** Defines the *workstreams* (teams of specialized agents) that can work in parallel without stepping on each other or duplicating systems. This is a **development governance** document, not a game design one: design authority still lives in the GDDs (see [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)) and in the decision register ([DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md)).
>
> **Status: PROPOSAL — pending approval.** No worktrees are created and no implementation agents are launched until the project owner approves this structure.
> Version 1.0 · 2026-11-04 · Canonical language: Spanish (like the rest of the documentation).

## Sibling documents

| Document | What it defines |
|---|---|
| **AGENT_ORGANIZATION.md** (this one) | The 9 workstreams: scope, responsibilities, exclusions, dependencies, deliverables, allowed/restricted folders, acceptance criteria. |
| [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) | Shared interfaces (time, events, identity, resources, inventories, buildings, population, terrain, placement, UI access). Inputs/outputs per system. |
| [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) | File/folder → single-owner matrix. Protocol for shared files and `.uasset` files. |
| [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md) | Task board by milestone, with dependencies, priority, files, criteria and status. |
| [GIT_WORKFLOW.md](GIT_WORKFLOW.md) | Branches, worktrees, commits, integration, conflicts, tests, delivery. Two repos (docs + UE). |
| [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md) | Central integration process led by the Technical Director. |

## Why this organization (and what shapes it)

The project already has a real codebase (it does not start from scratch). Before parallelizing, three facts must be respected:

1. **Two distinct git repositories** (see [GIT_WORKFLOW.md](GIT_WORKFLOW.md)):
   - **Documentation/design repo** (root `Civilization Evolutions/`): GDDs, Phase 2 blueprint, and **these organization documents**.
   - **Unreal project repo** (`civilizations-evolution/`): SimCore + UE layer + assets. It has its own history (up to `HANDOFF`), an `origin/main` remote, **8 unpushed commits** and **uncommitted changes** (RISK-12). `civilizations-evolution/` is in the docs repo's `.gitignore`, so it **does not appear** in the docs repo's worktrees.
2. **The core (`SimCore/`) is small and tightly coupled** (8 header/impl pairs + `data/*.json`). `Simulation.cpp` and `World.cpp` are *hotspots* that today touch economy, construction and population all at once. Parallelizing over them without splitting them first guarantees conflicts → see task **M0** on the task board and the shared-file rule in [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md).
3. **Design decisions already made** that no agent may silently revert: among them **Model A — "the UE world is canonical"** (single stock in `World::colony()` within SimCore; the UE layer currently governs hunger/health/death) and DR-01..DR-12 (see register). Changing them requires going through the Technical Director and updating `DESIGN_DECISION_REGISTER.md` / `AUDIT_REPORT.md`.

## Organization principles (non-negotiable)

1. **One owner per datum and per file.** Each shared concept has **one** owning workstream (source of truth); the rest **read** via contract. Each code file has **one** owner in [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md). No shared ownership.
2. **Contracts before implementation.** No one implements against another system until its interface is in [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md). While the owner has not published the real contract, consumers work against agreed **stubs/mocks**.
3. **Simulation/presentation separation** (technical plan §4.3, RISK-01). Logic lives in SimCore; the UE layer presents and translates orders. A presentation workstream does **not** put simulation rules in.
4. **No duplication** (DC-07/DC-15, RISK-02). No second model is created for a concept that already has an owner (resources, entities, inventory, clock).
5. **Data-driven where appropriate** (technical plan §4.2, RISK-04). New content (resources/recipes/buildings) is added as **data** (`SimCore/data/*.json` / future Data Assets), not hardcoded.
6. **Green and committed.** Every change compiles and passes tests before requesting integration; it is committed at green checkpoints (RISK-12).
7. **Don't break what works.** Existing gameplay is not rewritten without an explicit decision from the Technical Director; it is extended while respecting the state on disk.

## Role model

- **Technical Director (WS-TD):** sole integrator. Reviews contracts and implementations, validates dependencies, integrates branches, runs build/tests, updates board status and records decisions. Does **not** implement domain gameplay.
- **Domain workstreams (WS-CORE, WS-WORLD, WS-BUILD, WS-ECON, WS-POP):** implement their system in SimCore and its *wiring* in the UE layer, within their allowed files.
- **Cross-cutting presentation/quality workstreams (WS-UI, WS-ART, WS-QA):** span several domains but with strict boundaries (UI reads, never writes sim; ART only touches assets/visuals; QA adds tests and instrumentation, does not change rules).

---

# The 9 Workstreams

> Format: **Scope · Responsibilities · Exclusions · Dependencies · Deliverables · Allowed folders/files · Restricted files · Acceptance criteria.**
> The `civilizations-evolution/...` paths live in the **UE repo**; root paths live in the **docs repo**.

## WS-TD — Technical Director and Architecture

- **Scope:** technical governance, shared contracts, integration and architectural coherence across the whole project.
- **Responsibilities:**
  - Maintain [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md), [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md), [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md), [GIT_WORKFLOW.md](GIT_WORKFLOW.md), [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md).
  - Lead the **M0** refactor that splits `SimCore/Simulation.*` and `SimCore/World.*` into domain modules with clear boundaries (enables safe parallelism).
  - Review and integrate branches; run `simcore_tests` and the UE build; resolve contract conflicts.
  - Record architecture decisions in `DESIGN_DECISION_REGISTER.md` and contradictions in `AUDIT_REPORT.md`.
- **Exclusions:** does not implement domain features (economy, population, world, etc.); does not write art or final UI.
- **Dependencies:** nothing blocks it; **everyone** depends on it publishing and versioning the contracts.
- **Deliverables:** versioned contracts; ownership map; updated board; M0 refactor; integration reports per milestone.
- **Allowed:** all organization documents (root); `SimCore/include/simcore/*.h` (**only** signatures/contracts, coordinating with owners); `civilizations-evolution/CivilizationSimulator.Build.cs` and build config **only** for integration.
- **Restricted:** implementation bodies (`.cpp`) of other domains except for integration; original GDDs and `BACKUP_PRE_AUDIT_2026-11-03/` (not edited).
- **Acceptance:** contracts published and stable; green build after each integration; zero duplication of shared concepts; decisions recorded.

## WS-CORE — Engine and Simulation Core

- **Scope:** SimCore foundations (Level 0): clock, event bus, entity identity, simulation orchestration, serialization (save/load) and the UE↔SimCore bridge.
- **Responsibilities:** `SimulationClock` (day/month/year/season, pause/speeds, FPS-independent, DR-09); `EventBus` (typed events, DC-14); entity types and **persistent identity** (`Types.h`, stable ids, DC-15); `Simulation` orchestrator (registration and tick of subsystems after M0); `World` as root container; serialization/versioning (RISK-08/16); `CivSimSubsystem` as the sole bridge.
- **Exclusions:** does not define economy/construction/population rules (only coordinates them); does no render or UI.
- **Dependencies:** none (Level 0). It is a prerequisite for all.
- **Deliverables:** stable clock + events + identity; subsystem registration API after M0; save/load with schema version; `CivSimSubsystem` exposes state reads for UI.
- **Allowed:** `SimCore/include/simcore/{Types,SimulationClock,EventBus,World,Simulation}.h` + their `.cpp`; `SimCore/app/main.cpp`; `civilizations-evolution/Source/CivilizationSimulator/Simulation/CivSimSubsystem.*`.
- **Restricted:** `Inventory.*`, `GameDatabase.*`, `DefaultContent.*`, `data/*.json` (from ECON/BUILD); presentation actors (`View/`, `Units/`, `UI/`).
- **Acceptance:** the clock advances and pauses/accelerates without depending on `Tick()`; a system reacts to an event without coupling to the emitter; an entity keeps its id after save/load; `simcore_tests` green.

## WS-WORLD — World Generation and Geography

- **Scope:** the physical world state: procedural terrain, height, water, biomes, reproducible seed, climate/seasons (visual and, by phase, their effect on production — C-02/C-03).
- **Responsibilities:** deterministic generation by seed (DR-02); terrain queries (height, walkable, biome) via contract; natural resource distribution by geology (coordinated with ECON for the catalog); seasonal tint/environment tied to the WS-CORE clock.
- **Exclusions:** does not define the resource catalog (ECON does); does not place buildings (BUILD); does not implement the clock (CORE).
- **Dependencies:** WS-CORE (clock, events, types). Provides to BUILD (valid placement), ECON/PROD (where resources are), UI (minimap).
- **Deliverables:** `CivTerrain` with a configurable, exposed seed; contract terrain-queries API; natural resource nodes placed by rules (not pure chance).
- **Allowed:** `civilizations-evolution/Source/CivilizationSimulator/View/CivTerrain.*`; geography/world module in SimCore (e.g. `SimCore/.../World`-geo after M0, **coordinated with CORE**); `Content/Maps/` (new).
- **Restricted:** `CivBuildingActor.*` (BUILD), inventory/recipe logic (ECON), `SimulationClock.*` (CORE).
- **Acceptance:** same seed ⇒ same world (RISK-13); terrain queries respond per contract; natural resources follow a plausible geographic distribution.

## WS-BUILD — Buildings and Construction

- **Scope:** logical building representation (separate from the mesh), placement with validation and cost, construction work with progress/workers/materials, demolition, ruins, evolution by technology/era.
- **Responsibilities:** data-driven building defs (`data/buildings.json`); construction-work state (DC-03); resource cost (via ECON's inventory); building evolution (coordinated with Technology in ECON/CORE); `CivBuildingActor` as representation (mesh provided by ART); menu/placement in coordination with UI.
- **Exclusions:** does **not** define production recipes (ECON); does not assign workers to the economy (ECON/POP); does not model the mesh/material (ART); does not implement the construction panel (UI, with BUILD's data).
- **Dependencies:** WS-CORE (entities/events), WS-ECON (resources/inventory for cost and storage), WS-WORLD (valid placement over terrain). DC-03.
- **Deliverables:** placement with valid/invalid feedback + rotation + confirmation; real construction work reflected in UE (today it appears instant — backlog P1.9); ruins/abandonment; contract construction-placement API.
- **Allowed:** construction module in SimCore (defs/construction-work state/evolution after M0); `SimCore/data/buildings.json`; `civilizations-evolution/Source/CivilizationSimulator/View/CivBuildingActor.*`.
- **Restricted:** `Inventory.*`/`GameDatabase.cpp` for recipes (ECON owns them; BUILD **reads**), `CivTerrain.*` (WORLD), meshes/materials (ART), `CivHUD/CivMainHUD` (UI).
- **Acceptance:** placing a building deducts resources from the **single stock**; it cannot be placed without resources or outside valid terrain; demolition and ruins are coherent; no second inventory.

## WS-ECON — Economy, Resources and Production

- **Scope:** single catalog of resources/materials, inventories (single stock `World::colony()`, Model A), recipes and production chains, assigning workers to production, simplified physical logistics, trade.
- **Responsibilities:** **owner of the resource catalog** (DC-07) and of the inventories; recipes (`data/recipes.json`) and their execution in SimCore; "nothing is consumed without delivery" (DC-06); renewable regeneration (backlog P1.7, coordinated with WORLD); trade between settlements; prices/currency **only when the roadmap enables it** (E2+).
- **Exclusions:** does not define buildings (BUILD; ECON provides the production function as data); does not model demographics (POP); does not do resource UI (UI reads).
- **Dependencies:** WS-CORE (types/events/tick), WS-POP (available workers), WS-BUILD (where things are produced/stored), WS-WORLD (natural resources). DC-05/DC-06/DC-07.
- **Deliverables:** **resource definitions** and **inventories** contract; wheat→mill→flour→bread chain working over the single stock; gathering↔production↔consumption speaking the same inventory; resource/inventory access API for UI.
- **Allowed:** `SimCore/include/simcore/{Inventory,GameDatabase,DefaultContent}.h` + `.cpp`; `SimCore/data/{resources,recipes}.json`; economy/production module in SimCore after M0.
- **Restricted:** `SimulationClock/EventBus/World` (container) from CORE; `data/buildings.json` (BUILD; ECON reads required building types); UE presentation actors.
- **Acceptance:** a single inventory per location (no duplicate `ColonyStock` — already satisfied, do not regress); production validates inputs and does not produce without them; adding a resource/recipe is editing data, not code.

## WS-POP — Population and Society

- **Scope:** individuals with persistent identity (Citizen), families and households (family ≠ household), needs, health, aging, demographics (birth rate/mortality/migration), employment as a skill.
- **Responsibilities:** model population in **SimCore** (Task F, invert the authority that today sits in UE: have SimCore drive hunger/health/aging — see the Model A note); map `Citizen`↔`CivUnit` 1:1; individual↔aggregate transition (LOD, DR-10/DR-06) in the future; professions/skills.
- **Exclusions:** does not define recipes or inventories (ECON); does not assign the character's render (ART); does not build housing (BUILD; POP consumes housing capacity).
- **Dependencies:** WS-CORE (identity/time), WS-BUILD (housing/workplaces), WS-ECON (food consumption/employment). DC-04, DR-10.
- **Deliverables:** **population** contract; `Citizen` with name/age/household/occupation/needs/health in SimCore; basic demographics tied to the clock; reflection in `CivUnit` without sim logic in UE.
- **Allowed:** population module in SimCore after M0; `civilizations-evolution/Source/CivilizationSimulator/Units/CivUnit.*` (reflection/state, no sim rules).
- **Restricted:** `Inventory/GameDatabase` (ECON), `CivBuildingActor` (BUILD), character meshes/animations (ART), `SimulationClock` (CORE).
- **Acceptance:** a `Citizen` keeps identity and history after save/load; ages with the clock; consumption goes through the single inventory (no double deduction); the aggregate population is coherent with the total (the Master's integration rule).

## WS-UI — Interface and Interaction

- **Scope:** HUD, information panels (individual/building), construction menu, debug panel, i18n, RTS camera and translation of player orders. **Reads** simulation state; **never** writes rules.
- **Responsibilities:** `CivHUD`/`CivMainHUD`/`CivUIStyle`; `CivText` (EN by default + ES); `CivPlayerController` (selection/orders/menus); `CivCameraPawn`; minimap, notifications, tooltips; debug panel (Task H). DC-12.
- **Exclusions:** does not implement simulation; does not define assets/materials (ART); does not invent data (consumes contracts from CORE/ECON/BUILD/POP/WORLD).
- **Dependencies:** **UI data access** contracts from all domains; WS-CORE (`CivSimSubsystem` bridge). Can proceed **now** with **mock data** from the contract.
- **Deliverables:** HUD with date/season/resources/population; info panels with real data; construction menu and panel (BUILD's data); debug panel; full i18n.
- **Allowed:** `civilizations-evolution/Source/CivilizationSimulator/UI/*`; `Core/CivHUD.*`, `Core/CivText.*`, `Core/CivPlayerController.*`; `Camera/CivCameraPawn.*`; `Content/UI/` (new).
- **Restricted:** any SimCore `.cpp`; `CivSimSubsystem` except for **reading** (coordinate with CORE if a getter is missing); domain actors except to **display** their state.
- **Acceptance:** the UI works against mocks and then against real data without changing its structure; it does not write simulation state; i18n with no hardcoded text.

## WS-ART — Art, Environment and Visual Quality

- **Scope:** asset pipeline (headless import from `ArtStaging/` to `Content/`), meshes and materials per building/era/character/nature, lighting (Lumen/SM6), water, VFX. **Visual, no logic.**
- **Responsibilities:** replace placeholders with real models (KayKit, Quaternius, Stylized Nature — backlog P0); PBR materials; real water; scale/`MeshYawOffset`; lighting/post-process. Only changes **which mesh/material** an actor uses, not its behavior.
- **Exclusions:** does not touch simulation rules or the structure of logical actors; does not modify contracts.
- **Dependencies:** mesh names/slots exposed by BUILD (`CivBuildingActor`), POP (`CivUnit`) and WORLD (`CivTerrain`). Can work **now** on the existing slots.
- **Deliverables:** assets imported to `Content/` with a convention; PBR materials applied; pipeline document (`Docs/ASSETS.md` already exists — extend it).
- **Allowed:** `civilizations-evolution/Content/**` (meshes/materials/textures), `civilizations-evolution/ArtStaging/**` (raw, **outside git**), `Docs/ASSETS.md`, `Docs/UI_STYLE_GUIDE.md` (coordinating with UI). Editing **asset assignment** (not logic) in `CivBuildingActor`/`CivUnit`/`CivTerrain` **only** via exposed properties and coordinated with the file's owner.
- **Restricted:** all of SimCore; the logic of any actor; contracts. **`.uasset` rule:** no other workstream edits the same `.uasset` files that ART has assigned on the board (see the binary protocol in [GIT_WORKFLOW.md](GIT_WORKFLOW.md)).
- **Acceptance:** the game looks better without changing behavior or tests; `.uasset` files have a single owner per task; imports are reproducible by script.

## WS-QA — QA, Performance and Integration

- **Scope:** testing strategy (SimCore suite + UE automation where practical), performance, instrumentation to debug emergent behavior (RISK-19), and support to the Technical Director in validating integration.
- **Responsibilities:** maintain and extend `SimCore/tests/`; add placement/cost/recipe/consumption/assignment/pause/speed/production tests; measure performance (RISK-03/07); deterministic debugging tools (RISK-13/19); per-milestone acceptance checklist.
- **Exclusions:** does not change domain rules to "fix" a test (reports to the owner); is not the final integrator (that is WS-TD) but assists it.
- **Dependencies:** reads all contracts; coordinates with WS-TD on each integration.
- **Deliverables:** reproducible green suite; new tests per system; performance and reproducibility report; debugging tools/logs.
- **Allowed:** `SimCore/tests/**`, `SimCore/app/main.cpp` (test scenarios, coordinating with CORE), UE automation tests (new folder `Source/.../Tests` if enabled), CI/measurement scripts.
- **Restricted:** domain implementation (only proposes/adds tests); contracts (it **uses** them, does not define them).
- **Acceptance:** `simcore_tests` green on every integration; growing coverage of the critical rules; an emergent bug can be reproduced with seed + save.

---

## Non-overlap matrix (summary)

| Shared concept | Single owner | Readers (via contract) |
|---|---|---|
| Clock / date / season | WS-CORE | all |
| Events (typed bus) | WS-CORE | all |
| Entity identity/relationships | WS-CORE | all |
| Save/load and schema version | WS-CORE | — |
| Resource/material catalog | WS-ECON | BUILD, POP, UI, WORLD |
| Inventories (single stock) | WS-ECON | BUILD, POP, UI |
| Recipes/production | WS-ECON | BUILD, UI |
| Building definitions and state | WS-BUILD | ECON, POP, UI, ART |
| Placement/validation on terrain | WS-BUILD (+WORLD for terrain) | UI |
| Terrain / biomes / seed / queries | WS-WORLD | BUILD, ECON, UI |
| Individuals/families/households/demographics | WS-POP | ECON, UI |
| Mesh/material/asset of any actor | WS-ART | — |
| State reads for display | WS-UI | — (consumes from all) |
| Tests and instrumentation | WS-QA | — |

Golden rule: if two workstreams need the same datum, **one owns it and the other reads it by contract**; the datum is never copied nor the logic duplicated.

## What NOT to do (explicit brief constraints)

- Do not modify existing gameplay without a Technical Director decision, nor overwrite uncommitted work (read the state on disk first).
- Do not change project configuration (`Config/`, `*.Build.cs`, `.uproject`, UE version) without approval: WS-TD manages it.
- Do not duplicate systems or create premature empty abstractions.
- Do not assume everyone works on the same branch (see [GIT_WORKFLOW.md](GIT_WORKFLOW.md)).
- Do not silently change confirmed GDD decisions (DC-*/DR-*): document and ask.
