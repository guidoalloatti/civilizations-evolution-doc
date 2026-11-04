# AGENT_TASK_BOARD — Civilization Evolutions

> **Multi-agent development task board.** Organized by **milestone**. Each task carries: ID, Agent (workstream), Description, Dependencies, Priority, Allowed files, Restricted files, Deliverables, Acceptance criteria, Status, Blockers. The complete file lists per workstream are in [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md); the contracts in [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md).
>
> Version 1.0 · 2026-11-04 · Status: PROPOSAL (no task starts until approval).
> Priority: **P0** (critical/blocking) → **P3** (deferrable). Status: `todo` · `in-progress` · `blocked` · `review` · `done`.
> Origin: mapped to the UE repo's actual backlog (`Docs/BACKLOG.md`, P0–P7/DT) and to the [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) (E0–E6).

## Milestones

| Milestone | Name | Objective | Depends on |
|---|---|---|---|
| **M0** | Preparation to parallelize | Contracts published + SimCore split by domain + Git/LFS + mocks | — |
| **M1** | Coherent playable base | Close the loop: a single placement path, real construction work, coherent resources | M0 |
| **M2** | Population in SimCore (Task F) | Invert Model A authority: SimCore drives pop; real panels | M0, M1 |
| **M3** | World and data-driven content | Biomes/seed + migrate content to data | M0 |
| **M4** | Real UI + instrumentation | Replace mock data; events→UI; debug tools | M1, M2 |
| **∞** | Continuous cross-cutting | Art and QA throughout all milestones | M0 |

> **Start rule (brief §7):** not all systems are started before their dependencies are defined. The **initial tasks that can start now** (after approving M0) are marked **[START]**.

---

## M0 — Preparation to parallelize (led by WS-TD)

| ID | Agent | Description | Deps | Prio | Status | Blockers |
|---|---|---|---|---|---|---|
| **M0-TD-01** | WS-TD | Publish and version the contracts ([SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)) and the ownership map; ratify Model A and DR-01..12 as the baseline | — | P0 | review | owner approval |
| **M0-TD-02** | WS-TD | **M0 refactor:** split `SimCore/Simulation.cpp` and `World.cpp` into domain steps (Production/Logistics/Trade→ECON, Population→POP, Construction→BUILD, orchestrator/clock/events/save-load→CORE) without adding empty abstractions | M0-TD-01 | P0 | todo | — |
| **M0-TD-03** | WS-TD | Git: branches per workstream + worktrees; create `.gitattributes` with LFS for `*.uasset`/`*.umap` (**requires approval**: it is a config change) and migrate binaries | M0-TD-01 | P0 | todo | config approval |
| **M0-CORE-01** | WS-CORE | Commit the uncommitted post-HANDOFF work (RISK-12) and push `origin/main` of the UE repo; leave a clean tree before branching | — | P0 | todo | — |
| **M0-QA-01** | WS-QA | Reinstall/verify CMake; run `simcore_tests` (green baseline) and document how to run them in CI | — | P0 | todo | — |

**Detail (files / deliverables / acceptance):**
- **M0-TD-02** · *Allowed:* `SimCore/{Simulation,World}.{h,cpp}` (coordinating with CORE/ECON/BUILD/POP), new per-domain step files · *Restricted:* UE presentation · *Deliverable:* SimCore with one file per domain and `simcore_tests` green · *Acceptance:* each workstream can edit its step without touching another's; tests pass the same as before (no behavior change).
- **M0-CORE-01** · *Allowed:* the entire UE repo (it is a state commit, coordinated with TD) · *Deliverable:* clean `git status`, `origin/main` up to date · *Acceptance:* there are no uncommitted changes before opening branches.
- **M0-TD-03** · *Deliverable:* operational `GIT_WORKFLOW.md` + `.gitattributes` + base branches · *Acceptance:* a new `.uasset` goes through LFS; two worktrees do not step on each other.

---

## M1 — Coherent playable base (close the loop)

| ID | Agent | Description | Deps | Prio | Status | Blockers |
|---|---|---|---|---|---|---|
| **M1-BUILD-01** | WS-BUILD | Unify the **two placement paths** (contracts §6/§9): placement via `tryPlaceBuilding` + reactivate `CivViewManager::SyncBuildings`; remove the duplicate direct *spawn* | M0-TD-02 | P0 | todo | M0 |
| **M1-BUILD-02** | WS-BUILD | **Real construction work**: progress + workers carrying materials + it stops if materials are missing (SimCore already models it; reflect it in UE; today it appears instant — backlog P1.9) | M1-BUILD-01 | P1 | todo | M1-BUILD-01 |
| **M1-ECON-01** | WS-ECON | Verify/maintain the **single stock** (Model A) after M0; have gathering↔production↔cost↔consumption use `World::colony()` without regressing to the double inventory | M0-TD-02 | P0 | todo | M0 |
| **M1-ECON-02** | WS-ECON | **Renewable regeneration** (water fast, fruit tree yearly tied to the clock) in the correct model; deposits/storehouses as hauling destinations (P1.7/P1.8) | M1-ECON-01 | P1 | todo | — |
| **M1-ECON-03** | WS-ECON | Clay in recipes/construction (pottery, `masonry`) — today it is only gathered (P1.10) | M1-ECON-01 | P2 | todo | — |
| **M1-WORLD-01** | WS-WORLD | Owner of node hauling: stabilize `CivResourceNode` (new water/clay) and delivery to the town hall against the single stock | M1-ECON-01 | P1 | todo | — |

**Detail:**
- **M1-BUILD-01** · *Allowed:* `View/CivBuildingActor.*`, `View/CivViewManager.*` (`SyncBuildings`, with WORLD), `Core/CivPlayerController.*` (coordinated with UI), construction step in SimCore · *Restricted:* ECON's `Inventory`/recipes, `CivText` except labels · *Deliverable:* a single placement path with a SimCore id · *Acceptance:* placing creates a `buildingId`, deducts from the single stock, and the 3D actor reconciles via `GetBuildingViews()`; no double building model.
- **M1-ECON-02** · *Allowed:* economy step in SimCore, `Inventory.*`, `data/*.json` · *Deliverable:* renewables that come back; storehouse as destination · *Acceptance:* a depleted fruit tree replenishes in ~1 sim year; hauling reaches a storehouse in addition to the town hall; no second stock.
- **M1-WORLD-01** · *Allowed:* `Units/CivResourceNode.*`, `View/CivViewManager.*` (spawns) · *Acceptance:* all 5 node types deliver to the single inventory; renewables are not destroyed.

---

## M2 — Population in SimCore (Task F)

| ID | Agent | Description | Deps | Prio | Status | Blockers |
|---|---|---|---|---|---|---|
| **M2-POP-01** | WS-POP | **Invert authority (Model A→SimCore):** move hunger/health/death from `CivUnit` to SimCore (`nutritionPerCitizenPerDay>0`), `CivUnit` becomes a reflection; coordinate with CORE/ECON | M1-ECON-01 | P0 | blocked | contract decision (WS-TD) |
| **M2-POP-02** | WS-POP | Aging with the clock + basic birth rate/mortality; food consumption per person via the single inventory | M2-POP-01 | P1 | todo | M2-POP-01 |
| **M2-POP-03** | WS-POP | Families and households as entities (family≠household); housing with capacity (DR-07/SHARED_DATA_MODEL) | M2-POP-02 | P2 | todo | — |
| **M2-UI-01** | WS-UI | Real info panels (individual: name/age/occupation/needs/health; building: production/workers/storehouse) reading contracts | M2-POP-01, M1-BUILD-01 | P1 | todo | — |
| **M2-UI-02** | WS-UI | Worker assignment to buildings from the UI (click settler → assign) | M1-BUILD-01 | P2 | todo | — |

**Detail:**
- **M2-POP-01** · *Allowed:* population step in SimCore, `Units/CivUnit.*` (remove sim logic, keep reflection) · *Restricted:* `Inventory`/`GameDatabase` (ECON), `SimulationClock` (CORE) · *Deliverable:* SimCore drives hunger/health/death; UE reflects · *Acceptance:* a `Citizen` ages and can starve **without** double-deducting the stock; aggregate population coherent with the total; save/load preserves identity. · **Blocker:** it is a contract change (§7) → WS-TD validates it first.
- **M2-UI-01** · *Allowed:* `UI/CivMainHUD.*`, `Core/CivHUD.*`, `CivText.*` (labels) · *Restricted:* any SimCore `.cpp` · *Acceptance:* the panels show real subsystem data (not mock); the UI does not compute authoritative state.

---

## M3 — World and data-driven content

| ID | Agent | Description | Deps | Prio | Status | Blockers |
|---|---|---|---|---|---|---|
| **M3-WORLD-01** | WS-WORLD | **[START]** Configurable seed exposed in the UI + reproducible world (RISK-13); terrain-queries API (`HeightAt` already exists; add `IsWalkable`/`BiomeAt`) | M0 | P1 | todo | — |
| **M3-WORLD-02** | WS-WORLD | Biomes and soil types; rivers by topography; resource distribution by geology (not pure chance) — C-02/C-03 pending confirmation | M3-WORLD-01 | P2 | blocked | C-02/C-03 (AUDIT) |
| **M3-ECON-01** | WS-ECON | **Data-driven:** actually load `data/*.json` (or Data Tables/Assets) instead of hardcoding in `DefaultContent.cpp` (RISK-04, DT.27) | M0-TD-02 | P2 | todo | — |

**Detail:**
- **M3-WORLD-01** · *Allowed:* `View/CivTerrain.*`, geo/world in SimCore, `UI/*` only to expose the seed (coordinated with UI) · *Acceptance:* same seed ⇒ same world; terrain queries respond per contract (§8).
- **M3-ECON-01** · *Allowed:* `GameDatabase.*`, `DefaultContent.*`, `data/*.json`, a possible parser without heavy dependencies · *Acceptance:* adding a resource/recipe is editing data, not recompiling logic; `simcore_tests` green. · *Note:* respect "no unnecessary external dependencies" (technical plan §16).

---

## M4 — Real UI + instrumentation

| ID | Agent | Description | Deps | Prio | Status | Blockers |
|---|---|---|---|---|---|---|
| **M4-UI-01** | WS-UI | Replace **mock data** in `CivMainHUD` (Housing/Wellbeing/Weather/notifications) with real data; connect SimCore→UI events ("Stage 4") | M2, §2-bridge | P1 | todo | event bridge (CORE) |
| **M4-CORE-01** | WS-CORE | SimCore→UE event bridge: expose `EventBus` to the subsystem for real notifications (today UE polls) | M0-TD-02 | P1 | todo | — |
| **M4-UI-02** | WS-UI | Minimap + configurable notifications + basic contextual tutorial (P6) | M4-UI-01 | P3 | todo | — |
| **M4-QA-01** | WS-QA | Deterministic debugging tools (RISK-19): reproduce a bug with seed + save; event logging | M0-QA-01 | P1 | todo | — |

---

## ∞ — Continuous cross-cutting (Art and QA)

| ID | Agent | Description | Deps | Prio | Status | Blockers |
|---|---|---|---|---|---|---|
| **ART-01** | WS-ART | **[START]** Headless import of `ArtStaging/`→`Content/`; replace placeholders (settlers KayKit scale/yaw, Quaternius animals, nature/buildings MegaKit) — asset only, no logic (P0) | M0-TD-03 (LFS) | P1 | todo | LFS config |
| **ART-02** | WS-ART | Real PBR materials (grass/rock/bark/clay) + real water (UE Water plugin or material) instead of the blue disc | ART-01 | P2 | todo | — |
| **QA-01** | WS-QA | **[START]** Extend the suite: placement/cost/recipe/consumption/assignment/pause/speed/production-over-time tests (DT.28) | M0-QA-01 | P1 | todo | — |
| **QA-02** | WS-QA | Performance measurement (RISK-03/07): cost of N `CivUnit`/nodes; define the aggregation threshold (DR-06) | M2 | P2 | todo | — |
| **UI-ART-01** | WS-UI+WS-ART | Icons in the resource bar; custom cursor; tooltip extended to animals/buildings (P0.4/P0.5) | — | P3 | todo | UI↔ART coordination |

---

## Immediate-start tasks (brief §7) — after approving M0

Prioritized because they do **not** depend on other systems still to be defined (they operate against stable contracts or mocks):

1. **M0-TD-01/02/03 + M0-CORE-01 + M0-QA-01** — foundations (contracts, refactor, git, test baseline). *Prerequisite for almost everything.*
2. **M3-WORLD-01** — reproducible generation/seed (a system that operates independently early on).
3. **M2-UI-01 / M4-UI-01 with mock data** — the UI can already progress against §10-contract mocks (blocks no one).
4. **ART-01/02** — asset pipeline (visual, without touching logic).
5. **QA-01** — extend tests over what already exists.

**Do not start yet:** M2-POP-01 (blocked by the Model A contract decision), M3-WORLD-02 (blocked by C-02/C-03), and any advanced domain system (culture/politics/diplomacy/AI/war — roadmap E5+).

## Board conventions

- **One owner per task**; if a task touches a 🤝 shared file, the [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) protocol applies.
- **`blocked`** always names the blocker (decision, dependency, approval).
- When closing a task: green + tests + PR to integration (see [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md)); WS-TD moves it to `done` and updates `IMPLEMENTATION_ALIGNMENT.md`.
- Changing a priority or dependency is a WS-TD decision (recorded here).
