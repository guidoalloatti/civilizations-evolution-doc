# AGENT_OWNERSHIP_MAP — Civilization Evolutions

> **File and folder ownership map.** Each path has **one** owning workstream (the one that edits it). The rest **read**. Avoids simultaneous writes and conflicts. Complements [AGENT_ORGANIZATION.md](AGENT_ORGANIZATION.md) (who each workstream is) and [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) (what each owner exposes).
>
> Version 1.0 · 2026-11-04 · Status: PROPOSAL.
> `civilizations-evolution/...` paths → **UE repo**. Root paths → **docs repo**. (The UE repo is in the docs repo's `.gitignore`.)

## Legend

- **Owner:** the only workstream that **edits** the file.
- **Readers:** those that depend on it (via contract); they do not edit it.
- 🔒 **Globally restricted:** no one edits it without Technical Director approval.
- 🤝 **Shared (protocol):** several might need to touch it → the **shared-file protocol** (below) applies. Marked here as 🤝.
- 🎨 **`.uasset` binary:** not textually mergeable → **binary protocol** (below).

---

## A. Documentation repo (root)

| Path | Owner | Readers | Note |
|---|---|---|---|
| `AGENT_ORGANIZATION.md`, `AGENT_OWNERSHIP_MAP.md`, `SYSTEM_CONTRACTS.md`, `AGENT_TASK_BOARD.md`, `GIT_WORKFLOW.md`, `INTEGRATION_STRATEGY.md` | **WS-TD** | all | Governance. Only TD edits; others propose via PR/message. |
| `CLAUDE.md`, `DOCUMENTATION_INDEX.md`, `DOCUMENTATION_READING_GUIDE.md` | **WS-TD** | all | Index and entry point. |
| `DESIGN_DECISION_REGISTER.md`, `AUDIT_REPORT.md`, `IMPLEMENTATION_RISK_REGISTER.md` | **WS-TD** | all | Decisions/risks. A workstream that makes a decision **proposes** it; TD records it. |
| `SHARED_DATA_MODEL.md`, `SYSTEM_DEPENDENCY_MAP.md`, `SYSTEM_ARCHITECTURE_MAP.md`, `DEVELOPMENT_ROADMAP.md`, `SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md`, `CROSS_SYSTEM_VALIDATION_SCENARIOS.md`, `DESIGN_CONSISTENCY_AUDIT.md`, `HISTORICAL_EVOLUTION_FRAMEWORK.md` | **WS-TD** | all | Phase 2 blueprint (derived from the GDDs). |
| `IMPLEMENTATION_ALIGNMENT.md` | **WS-TD** (with input from WS-QA) | all | Code↔design snapshot; update after each milestone. |
| `docs/00-project/GDD_MAESTRO.md`, `docs/01-systems/*`, `docs/02-technical/*`, `docs/README.md` | 🔒 **GDD — design authority** | all | **Not edited** by a development decision; design changes are the owner's decision (DR-08). |
| `BACKUP_PRE_AUDIT_2026-11-03/**` | 🔒 **Immutable** | — | Backup. Never edited. |

## B. UE repo — SimCore (`civilizations-evolution/SimCore/`)

> SimCore is small and coupled. Until the **M0** refactor (splitting `Simulation`/`World` into domain modules), several files are **shared** and edited under protocol. Post-M0, the "Owner (post-M0)" column applies.

| Path | Owner (today) | Owner (post-M0) | Readers | Note |
|---|---|---|---|---|
| `include/simcore/Types.h`, `src/Types.cpp` | **WS-CORE** | WS-CORE | all | IDs, enums, names. 🤝 adding a new enum notifies TD. |
| `include/simcore/SimulationClock.h`, `src/SimulationClock.cpp` | **WS-CORE** | WS-CORE | all | Clock. |
| `include/simcore/EventBus.h` | **WS-CORE** | WS-CORE | all | Bus + `EventType`. Adding an event = contract change. |
| `include/simcore/World.h`, `src/World.cpp` | 🤝 **WS-CORE** (coordinates) | WS-CORE (container) | ECON/BUILD/POP | Root container + `colony()` + instance registration/lookup + tech state. **Hotspot**: touched by several domains → shared protocol. |
| `include/simcore/Simulation.h`, `src/Simulation.cpp` | 🤝 **WS-CORE** (coordinates) | **split** into domain steps | ECON/BUILD/POP | Orchestrates production/logistics/population/research/trade + save/load. **Hotspot #1**. M0 splits it (see below). |
| `include/simcore/Inventory.h`, `src/Inventory.cpp` | **WS-ECON** | WS-ECON | BUILD/POP/UI | Inventory. |
| `include/simcore/GameDatabase.h`, `src/GameDatabase.cpp` | **WS-ECON** | WS-ECON (+BUILD for `BuildingDef`) | all | Catalog. `BuildingDef` is consumed by BUILD; ECON owns the container. 🤝 |
| `include/simcore/DefaultContent.h`, `src/DefaultContent.cpp` | **WS-ECON** | WS-ECON/BUILD | CORE (scenarios) | Catalog + scenarios (`makeColonyStart`, `makeDefaultScenario`). 🤝 BUILD adds building defs here → coordinates with ECON. |
| `data/resources.json`, `data/recipes.json` | **WS-ECON** | all | — | Data. |
| `data/buildings.json` | **WS-BUILD** | ECON | — | Building data. |
| `app/main.cpp` | **WS-QA** (with CORE) | — | Console runner / test scenarios. |
| `tests/**` (`test_main.cpp`, `simple_test.h`) | **WS-QA** | all | Suite. QA adds tests per system. |
| `CMakeLists.txt` | 🔒 **WS-TD** | — | SimCore build. |

### M0 refactor (enables parallelism) — proposed, led by WS-TD

Split `Simulation.cpp`/`World.cpp` (today shared *hotspots*) into domain steps with clear boundaries, so each workstream has **its own** file:
- `ProductionStep`/`LogisticsStep`/`TradeStep` → **WS-ECON**
- `PopulationStep` → **WS-POP**
- `ConstructionStep`/evolution → **WS-BUILD**
- Orchestrator + clock + events + save/load + step registration → **WS-CORE**

Until M0 is integrated, **every** edit to `Simulation.cpp`/`World.cpp` goes through the shared-file protocol (serialized, one workstream at a time, coordinated by TD). This respects "do not create premature empty abstractions": M0 does **not** add new layers, it only separates existing responsibilities.

## C. UE repo — Unreal layer (`civilizations-evolution/Source/CivilizationSimulator/`)

| Path | Owner | Readers | Note |
|---|---|---|---|
| `Simulation/CivSimSubsystem.{h,cpp}` | 🤝 **WS-CORE** | **all** (UE) | **Sole SimCore↔UE bridge.** It is the UI access contract. A consumer needing a new getter **asks CORE** for it. |
| `SimCoreUnity.cpp` | 🤝 **WS-CORE** (with SimCore owners) | — | Compiles SimCore within the module. Edited when a SimCore `.cpp` is added/removed. |
| `Core/CivGameModeBase.{h,cpp}` | **WS-UI** (presentation bootstrap) | — | Spawns ViewManager + creates the UMG HUD. |
| `Core/CivPlayerController.{h,cpp}` | 🤝 **WS-UI** (owner) with BUILD interest | — | Input/selection/orders/construction menu/time controls. **Hotspot**: spans input→presentation→sim. BUILD coordinates placement changes here. |
| `Core/CivHUD.{h,cpp}` | **WS-UI** | — | Canvas: selection box, menu, info panel, tooltip, F1 debug. |
| `Core/CivText.{h,cpp}` (`CivLoc`) | 🤝 **WS-UI** (owner) | all (add labels) | **i18n — merge hotspot.** Every UI/building/resource/tech string lives here. Shared protocol: adding a key is a small, isolated change; group into small commits. |
| `UI/CivMainHUD.{h,cpp}`, `UI/CivUIStyle.h` | **WS-UI** | — | Main UMG HUD + style tokens. Replace mock data (Stage 4). |
| `Camera/CivCameraPawn.{h,cpp}` | **WS-UI** | — | RTS camera. **Cleanest boundary**: no SimCore. |
| `View/CivViewManager.{h,cpp}` | 🤝 **WS-WORLD** (environment) with BUILD (`SyncBuildings`) | — | Orchestrates the 3D environment + spawns + seasonal re-tint. `SyncBuildings` (dormant) is reactivated by BUILD. Coordinate WORLD↔BUILD. |
| `View/CivTerrain.{h,cpp}` | **WS-WORLD** | — | Procedural terrain + queries + seasons. |
| `View/CivBuildingActor.{h,cpp}` | 🤝 **WS-BUILD** (logic/state) with ART (mesh) | — | Building actor. BUILD owns behavior; ART only changes mesh/material via exposed properties. |
| `Units/CivUnit.{h,cpp}` | 🤝 **WS-POP** (state/sim) with ART (mesh/anim) | — | Settler. POP owns state/machine; ART only visuals. **Today it writes to the single stock** (hunger/hauling) — Model A. |
| `Units/CivAnimal.{h,cpp}` | 🤝 **WS-WORLD**/**WS-ECON** (fauna/hunting) with ART | — | Wild animal. Decide the owner on the board based on the hunting task (proposed: WS-ECON because it feeds resources). |
| `Units/CivResourceNode.{h,cpp}` | **WS-WORLD** (with ECON for the resource id) | — | Gatherable node. Core of the "map resource system". |
| `CivilizationSimulator.{cpp,h}` | 🔒 **WS-TD** | — | Primary module. Trivial. |
| `CivilizationSimulator.Build.cs`, `*.Target.cs` | 🔒 **WS-TD** | — | **Build config.** Adding a module dependency goes through TD. |

## D. UE repo — Config, Content, Docs

| Path | Owner | Note |
|---|---|---|
| `Config/DefaultInput.ini` | 🤝 **WS-UI** (owner) | **Merge hotspot.** UI edits every new input bind; other workstreams request the bind. |
| `Config/DefaultEngine.ini`, `Config/DefaultGame.ini` | 🔒 **WS-TD** | Project/engine config. |
| `CivilizationSimulator.uproject` | 🔒 **WS-TD** | Plugins/modules. |
| `Content/Chars/**` 🎨 | **WS-ART** | KayKit character + anims. Binary. |
| `Content/PBR/**` 🎨 | **WS-ART** | Materials/textures. Binary. |
| `Content/Maps/**` 🎨 (new) | **WS-WORLD** (if a `.umap` is created) | Today there is **no `.umap`** (procedural environment). If one is introduced, single owner. |
| `Content/UI/**` 🎨 (new) | **WS-UI** | Today the HUD is 100% code; if UI `.uasset` files are added, single owner. |
| `Content/Developers/**` | per-user | Personal development folder; outside integration. |
| `ArtStaging/**` | **WS-ART** | Heavy raw files. **Outside git** (`.gitignore`). |
| `Docs/ASSETS.md`, `Docs/UI_STYLE_GUIDE.md` | **WS-ART** / **WS-UI** | Asset pipeline / style guide. |
| `Docs/BACKLOG.md` | 🤝 **WS-TD** (mirrored on the board) | Historical backlog of the UE repo; the official board is `AGENT_TASK_BOARD.md`. |
| `Docs/00_TECHNICAL.md` (= PLAN_TECNICO) | 🔒 technical authority | Copy of the plan; do not edit without approval. |

---

## Shared-file protocol (🤝)

For the *hotspots* that several workstreams might need (`Simulation.cpp`, `World.cpp` pre-M0, `CivText.cpp`, `DefaultInput.ini`, `CivSimSubsystem.*`, `GameDatabase.cpp`, `CivPlayerController.cpp`):

1. **One writer at a time.** The board ([AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md)) marks who has "taken" the file for the active task.
2. **Small, atomic changes**, committed quickly on green, to minimize the conflict window.
3. **Ask, don't invade:** if you need something in another's file (a getter in the subsystem, a key in `CivText`, a bind in `DefaultInput`), you request it from the owner or open a minimal change coordinated with them; you do not rewrite their file.
4. **Prefer additive extension:** adding a case to a `switch`/dictionary (a new `CivLoc` key, a new category `case`) rarely clashes; **reordering/renaming** does → give notice.
5. **M0 first** where applicable: once `Simulation`/`World` are split, most of these stop being shared.

## `.uasset` binary protocol (🎨)

`.uasset` files are **not** merged textually (RISK-12). There are ~49 today, all in `Content/Chars` and `Content/PBR`; **there is no `.umap` nor UI `.uasset`** (the level and the HUD are procedural/code) → today **there is no large shared asset to clash over**. Rules:

1. **Partition by subfolder:** `Chars/` and `PBR/` belong to **WS-ART**; future `Maps/` to WORLD, `UI/` to UI. One `.uasset` = one owner per task.
2. **Never two agents editing the same `.uasset` at once.** The board assigns the asset to a single task.
3. **Configure Git LFS before adding heavy binaries.** LFS is currently **installed but not configured** (there is no `.gitattributes`). WS-TD must create `.gitattributes` with `*.uasset`/`*.umap` in LFS **with approval** (it is a configuration change) and migrate the existing ones.
4. **Locking recommended:** if LFS is adopted, use LFS *file locking* for `.uasset`/`.umap` (`git lfs lock`) on shared assets.
5. **Art commits separate** from code commits, so you can revert without touching logic.

## Who can touch what (one-line summary)

- **WS-TD:** governance docs + build/config (🔒). No gameplay.
- **WS-CORE:** clock/events/types/World(container)/orchestrator/save-load + `CivSimSubsystem` + `SimCoreUnity`.
- **WS-ECON:** `Inventory`/`GameDatabase`/`DefaultContent`/`data/resources|recipes.json` + economy/production/logistics steps post-M0.
- **WS-BUILD:** `data/buildings.json` + construction step post-M0 + `CivBuildingActor` (logic) + `SyncBuildings`.
- **WS-WORLD:** `CivTerrain` + `CivResourceNode` + geo/world in SimCore + `CivViewManager` (environment).
- **WS-POP:** population step post-M0 + `CivUnit` (state).
- **WS-UI:** `CivHUD`/`CivMainHUD`/`CivUIStyle`/`CivText`/`CivPlayerController`/`CivCameraPawn`/`CivGameModeBase` + `DefaultInput.ini`.
- **WS-ART:** `Content/Chars`+`Content/PBR`+`ArtStaging` + actor mesh/material via properties.
- **WS-QA:** `SimCore/tests` + `app/main.cpp` + UE automation tests + CI.
