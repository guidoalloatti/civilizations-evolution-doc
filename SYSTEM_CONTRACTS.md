# SYSTEM_CONTRACTS — Civilization Evolutions

> **Contracts for the shared systems.** Defines the interfaces that cross workstreams, with their **owner**, their **actual API today** (not invented: taken from the code on disk — `SimCore/` and `Source/CivilizationSimulator/`), their **inputs/outputs**, their **consumers** and the **rules** no one may violate. A consumer **programs against this contract**, not against the owner's implementation.
>
> Design authority: GDD + [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md) + [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md). Organization: [AGENT_ORGANIZATION.md](AGENT_ORGANIZATION.md).
> Version 1.0 · 2026-11-04 · Status: PROPOSAL. Contracts marked *(stable)* reflect current code; *(to be defined)* require design by the owner before consumers implement.

## How these contracts are used

1. **The owner publishes and versions the contract** (signature + semantics) before anyone implements against it.
2. **Consumers use only what is published.** If something is missing (e.g. a *getter*), they **request it from the owner**; they do not add it in others' files.
3. **An incompatible contract change** goes through the Technical Director (WS-TD) and is announced to all consumers (see [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md)).
4. **While the contract is *(to be defined)*,** consumers work against agreed **mocks** (e.g.: the UI already consumes mock data in `UCivMainHUD`).

## Two identity spaces (key to everything else)

- **Definition/content IDs = stable `std::string`** (data-driven, designer-oriented): `ResourceId`, `RecipeId`, `BuildingDefId`, tech id. E.g.: `"wood"`, `"farm"`, `"mill_flour"`, `"masonry"`. **Never** referenced by display name (the localized name lives in `CivText`/`CivLoc`).
- **Live-instance IDs = runtime `int`**, assigned by `World` (monotonic from 1): `buildingId`, `citizenId`. `settlementId` is `int` (0 = colony/shared settlement). "No job/no home" sentinel = `-1`.

> **The SimCore↔UE boundary is a single point:** `UCivSimSubsystem` (`Simulation/CivSimSubsystem.{h,cpp}`) is **the only** UE class that includes SimCore headers. The entire UE layer reads/commands through its `UFUNCTION`s. That is, in fact, the **UI access contract** (§10). Owner of the bridge file: **WS-CORE**.

---

## 1. Simulation time *(stable)*

- **Owner:** WS-CORE. **Source:** `simcore::SimulationClock` (`SimulationClock.h`), orchestrated by `simcore::Simulation`. DC-08, DR-09.
- **Inputs:** `advanceDays(double)`, `update(double realSeconds)→double` (sim days elapsed), `pause()/resume()/setSpeed(double)`; `Simulation::dailyTick()` is **the deterministic unit** of advance (1 sim day).
- **Outputs (read-only):** `year()`, `month()` (1..12), `dayOfMonth()` (1..30), `dayOfYear()`, `season()→Season{Spring,Summer,Autumn,Winter}`, `totalDays()`, `isPaused()`, `speed()`; helpers `Simulation::dayFraction()` (0..1), `Simulation::seasonalFactor(BuildingCategory,Season)`.
- **Calendar:** 30 days/month × 12 = **360 days/year**, 4 seasons of 90 days. `CalendarConfig{daysPerMonth,monthsPerYear,startYear,daysPerRealSecond}`.
- **UE bridge:** `UCivSimSubsystem` exposes `Pause/Resume/IsPaused/SetSpeed/AdvanceDays/GetDateString/GetSeason/GetSeasonName/GetDayFraction`.
- **Rules:** the clock is **FPS-independent** (RISK-07); no system does work on render `Tick()` — it reacts to `dailyTick`/events. Speeds do **not** change the result (determinism, RISK-13). There is **a single clock** (no parallel per-system clocks).
- **Consumers:** all.

## 2. Event communication *(stable, to be extended)*

- **Owner:** WS-CORE. **Source:** `simcore::EventBus` (`EventBus.h`), accessible via `Simulation::events()`. DC-14.
- **API:** `subscribe(std::function<void(const Event&)>)`; `emit(const Event&)` (synchronous, in order, no priorities and no *unsubscribe*).
- **`Event`:** `{ EventType type; int intA; std::string strA; double numA; }` (fields with flexible interpretation).
- **`EventType` today:** `YearChanged, SeasonChanged, ResourceProduced, ProductionBlocked, BuildingConstructed, BuildingEvolved, TechnologyDiscovered, PopulationChanged, CitizenHired, CitizenDied`.
- **Per-event field convention (payload contract):**
  | Event | `intA` | `strA` | `numA` |
  |---|---|---|---|
  | `ResourceProduced` | buildingId | resourceId | amount |
  | `ProductionBlocked` | buildingId | buildingDefId | — |
  | `BuildingConstructed` | buildingId | defId | — |
  | `BuildingEvolved` | buildingId | new defId | — |
  | `TechnologyDiscovered` | — | techId | — |
  | `CitizenHired` | buildingId | buildingDefId | — |
  | `CitizenDied` | citizenId | — | — |
  | `PopulationChanged` | delta (±) | — | — |
  | `SeasonChanged` | — | seasonName | — |
  | `YearChanged` | year | — | — |
- **Rules (DC-14):** the **event is originated by the system that owns the fact**; Events only record/notify; **no game effect is born from the bus**. Adding an `EventType` is a contract change → goes through WS-TD. Do not turn the bus into a generic framework (RISK-11).
- **Consumers:** all. *(To be defined: SimCore→UE event bridge — today UE reads state by polling the subsystem, it does not subscribe to the bus; wiring up real events is UI "Stage 4" + CORE work.)*

## 3. Persistent entity identifiers *(partial)*

- **Owner:** WS-CORE (identity), with the conceptual model in [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md). DC-15.
- **Today:** instance `int`s assigned by `World::addBuilding(...)→int` and `World::addCitizen(...)→int`; lookup `World::building(int)→Building*` (linear search). There is **no** public `World::citizen(int)` (you iterate `citizens()`); an internal `findCitizen` exists in `Simulation.cpp`. Persisted counter: `nextBuildingId()/nextCitizenId()` + setters (for save/load).
- **Inputs:** entity registration (returns id), death marking (`Citizen.alive=false`). **Outputs:** vectors `buildings()`, `citizens()`; aggregates `population()`, `employed()`, `housingCapacity()`.
- **Rules:** id **stable** for the entity's lifetime; it is **referenced by id**, never by name/vector index; identity **survives** save/load (RISK-16). No duplicating identity in another system (RISK-02).
- **To be defined (WS-CORE + WS-POP):** `citizen(int)` accessor; first-class relationships (family/household/lineage) of the data model; id policy when crossing individual↔aggregate (DR-10, RISK-10).
- **Consumers:** all.

## 4. Resource definitions *(stable)*

- **Owner:** WS-ECON (single catalog). **Source:** `GameDatabase` + `ResourceDef` (`GameDatabase.h`); mirror data in `SimCore/data/resources.json`. DC-07.
- **`ResourceDef`:** `{ ResourceId id; std::string displayName; ResourceCategory category; bool perishable; bool isFood; double foodValue; }`. `ResourceCategory{Natural,Food,Processed,Manufactured,Economic}`.
- **API:** `addResource(def)`, `hasResource(id)`, `resource(id)→const ResourceDef*`, `resources()→const map&`. Aliases `ResourceAmount=pair<ResourceId,int>`, `ResourceList=vector<ResourceAmount>`.
- **MVP catalog:** `wood, stone, berries, meat, wheat, flour, bread` (+ `water, clay` on the UE/node side).
- **Rules (DC-07):** **one resource = one id and a common inventory**; it is added via **data** (editing JSON/catalog), not scattered hardcode (RISK-04). Display name only in `CivLoc::Resource(id)` (i18n), never as a key.
- **Consumers:** BUILD (costs), POP (consumption), LOG, UI (bar), WORLD (natural resources).
- **Debt note:** the C++ core **does not parse** the JSON today (the catalog is hardcoded in `DefaultContent.cpp`); migrating to real data loading / Data Assets is an ECON task (RISK-04, backlog DT.27).

## 5. Inventories *(stable — model A)*

- **Owner:** WS-ECON. **Source:** `simcore::Inventory` (`Inventory.h`); the colony's **single stock** is `World::colony()` (settlement 0, unlimited capacity). DC-06/DC-07.
- **`Inventory` API:** `count(id)`, `total()`, `capacity()/setCapacity(c)` (`<0`=unlimited), `freeSpace()`, `has(id,qty)`, `add(id,qty)→int added`, `remove(id,qty)→int removed`, `items()→const map&`.
- **Stock aggregates:** `World::resourceStock(id)` (colony + building inventories), `foodStock()`, variants by `settlementId`.
- **UE bridge:** `GetColonyResource(id)` / `AddColonyResource(id,qty)` in `UCivSimSubsystem` (delegate to `world().colony()`).
- **Rules:** **one physical location per container** (DC-06); **nothing is consumed without delivery**; stock is **not** replicated in another system (the duplicate `ColonyStock` has already been removed — **do not reintroduce**).
- **Contention point (critical for multi-agent):** today **three** UE writers mutate `world().colony()` via `AddColonyResource` — `ACivUnit` (hauling + hunger consumption), `ACivBuildingActor` (production v1), `ACivPlayerController` (construction cost). On parallelizing, each writer belongs to a different workstream (POP/BUILD/UI) → **they all write the same datum through the same contract**; changes to that contract's semantics go through WS-TD.
- **Consumers:** BUILD, POP, UI, LOG.

## 6. Buildings *(stable, two paths to reconcile)*

- **Owner:** WS-BUILD (structure/state) + WS-ECON (production function as data). DC-03.
- **Definition (`BuildingDef`, from ECON/BUILD in `GameDatabase`):** `{ id; displayName; category; workerSlots; recipeId; storageCapacity; housingCapacity; buildCost; requiredTechs; evolvesTo; evolveTechs; evolveCost; }`. `BuildingCategory{Housing,Extraction,PrimaryProduction,Transformation,Storage,Service}`.
- **Instance (`World::Building`):** `{ int id; BuildingDefId defId; int settlementId; double x,y; ProductionPriority priority; Inventory inventory; vector<int> workers; bool batchActive; double batchProgress; bool blocked; string blockReason; }`.
- **API (in `Simulation`):** `canAfford(defId)`, `tryPlaceBuilding(defId,x,y,chargeCost)→int` (−1 if tech/material is missing), `demolish(id,refund)`, `canEvolve(id)`, `evolveBuilding(id,chargeCost)`, `assignWorkers()`.
- **UE bridge / read DTO:** `FCivBuildingView{ Id, DefId, DisplayName, X, Y, Workers, Slots, bBlocked, BlockReason, Category, bCanEvolve }`; `UCivSimSubsystem::GetBuildingViews()`, `Demolish`, `EvolveBuilding`, `PlaceBuildingWithCost`.
- **⚠️ Two placement paths to reconcile (contract to unify — BUILD + UI + CORE):**
  1. **Active:** `ACivPlayerController::TryPlaceAtCursor` charges wood/stone and **spawns `ACivBuildingActor` directly** (no SimCore id).
  2. **Designed (dormant):** `Simulation::tryPlaceBuilding` + `CivViewManager::SyncBuildings()` reconciles actors via `GetBuildingViews()` — **commented out** today.
  "Real" construction (work with progress, backlog P1.9) requires unifying to (2). **In the meantime, the placement contract is defined by BUILD; UI only requests `PlaceBuildingWithCost`.**
- **Rules:** placing deducts from the **single stock**; it cannot be placed without resources or outside valid terrain (§8/§9); evolution by technology (enables, does not force — DC-09).
- **Consumers:** ECON (recipes/storehouse), POP (housing/slots), UI (menu/panel), ART (mesh).

## 7. Population *(partial — Task F in progress)*

- **Owner:** WS-POP. **Source:** `World::Citizen` + population steps in `Simulation`. DC-04, DR-10.
- **`Citizen`:** `{ int id; string name; int settlementId; double ageDays; string profession; int workBuildingId; int homeBuildingId; double hunger; double health; bool alive; }`.
- **API:** `World::addCitizen(name,ageDays,settlementId)→int`, `citizens()`, `population()`, `employed()`, `housingCapacity()`; `Simulation::assignWorkers()`.
- **UE bridge:** `GetAliveCitizenIds()`, `GetCitizenName/GetCitizenAgeYears/GetCitizenProfession(id)`, `KillCitizen(id)`. Each `ACivUnit` maps **1:1** to a `Citizen` by `CitizenId`.
- **⚠️ Split authority (Model A, recorded decision):** today the **UE layer (`ACivUnit`) governs hunger/health/death** and SimCore runs with `nutritionPerCitizenPerDay=0` so as not to double-deduct. **Task F (upcoming slices)** inverts this: have **SimCore** drive hunger/health/aging/families and UE reflect it. That change is a contract one → coordinated WS-POP + WS-CORE + WS-TD; UI does not do it unilaterally.
- **To be defined (WS-POP):** families/households/lineage (family≠household), birth rate/mortality, multiple needs, classes, individual↔aggregate transition (DR-10).
- **Rules:** the aggregate population must be **coherent** with the total; consumption goes through the single inventory (no double deduction); identity persists across save/load.
- **Consumers:** ECON (labor/consumption), UI (panels), Culture/Religion (future).

## 8. Terrain queries *(partial)*

- **Owner:** WS-WORLD. **Source:** `ACivTerrain` (`View/CivTerrain.{h,cpp}`). DR-02, C-02/C-03.
- **Today:** `HeightAt(X,Y)→float` (deterministic sum-of-sines noise, flat center `FlatRadius`, scale `HeightScale`); water plane at `WaterLevel`; vertex color grass→rock by height (raw biome); `ApplySeason(0..3)` re-tints terrain/leaves/water. `Resolution=96`, `Extent=16000`.
- **⚠️ Reality to keep in mind:** actors (units/animals/nodes) *ground-snap* via **line trace**, they do **not** call `HeightAt`. The "terrain query" contract is internal-use today; exposing it as a service (height/walkable/biome per point) is WORLD's work.
- **To be defined (WS-WORLD):** `IsWalkable(x,y)`, `BiomeAt(x,y)`, a configurable seed exposed in the UI, resource distribution by geology. Without this, BUILD validates placement by its own rules/trace.
- **Rules:** same seed ⇒ same world (RISK-13); the season index is **pushed** by `CivViewManager` (WORLD does not call the clock directly, it receives it).
- **Consumers:** BUILD (placement), ECON/PROD (natural resources), UI (minimap), ART (textures by biome).

## 9. Construction placement *(to be unified — see §6)*

- **Owner:** WS-BUILD (placement rules) with WS-WORLD (validity over terrain) and WS-UI (player gesture).
- **Inputs:** `defId`, position `(x,y)`/cursor, rotation, confirmation. **Outputs:** new `buildingId` (or failure with reason), valid/invalid feedback, deducted cost.
- **Target contract:** `UI` captures the gesture → requests `PlaceBuildingWithCost(defId, world-pos)` → `BUILD` validates (resources via §5, terrain via §8, tech via §6) and returns `buildingId`/reason → `CivViewManager::SyncBuildings` materializes the `ACivBuildingActor` (reactivate the dormant *sync*) → `ART` provides the mesh by category/era.
- **Rules:** a single placement path (remove the duplicate direct *spawn* when closing real construction work); do not place without resources/terrain/tech.
- **Consumers:** UI, ART, ECON.

## 10. UI access to state *(stable — it is the subsystem)*

- **Bridge owner:** WS-CORE (`UCivSimSubsystem`). **Main consumer:** WS-UI. DC-12.
- **Contract:** the UI **reads** state by calling `GI->GetSubsystem<UCivSimSubsystem>()` and never writes simulation rules. Current read surface (UFUNCTION): `GetPopulation`, `GetEmployed`, `GetColonyResource(id)`, `GetDateString`, `GetSeason/GetSeasonName`, `IsPaused`, `GetDayFraction`, `GetBuildingViews()`, `GetCitizenName/AgeYears/Profession(id)`, `availableTechs`/research reads. Commands allowed to the UI: `Pause/Resume/SetSpeed/AdvanceDays`, `PlaceBuildingWithCost`, `Demolish`, `EvolveBuilding`, `SaveGame/LoadGame`.
- **Actual HUD path:** `UCivMainHUD` refreshes every 0.2 s (`RefreshData()`); `ACivHUD` (Canvas) and the `CivViewManager` debug are secondary paths.
- **⚠️ Mock data to replace (UI "Stage 4"):** in `UCivMainHUD`, *Housing/Wellbeing/Weather* and the notification cards are **placeholders** marked `*`. Connecting them to real state requires new getters (requested from the owners) and the event bridge (§2), not computation in the UI.
- **Rules:** if the UI needs a datum that does not exist in the subsystem, **a getter is added on the owner's side** (CORE exposes, the domain provides), authoritative state is not computed in the UI (RISK-01).
- **Consumers:** WS-UI (and WS-QA tools for debugging).

---

## Summary: owner → consumers per contract

| # | Contract | Owner | Consumers | Status |
|---|---|---|---|---|
| 1 | Simulation time | WS-CORE | all | stable |
| 2 | Events | WS-CORE | all | stable (UE bridge to be defined) |
| 3 | Entity identity | WS-CORE (+POP) | all | partial |
| 4 | Resource definitions | WS-ECON | BUILD, POP, UI, WORLD | stable |
| 5 | Inventories (single stock) | WS-ECON | BUILD, POP, UI | stable |
| 6 | Buildings | WS-BUILD (+ECON) | ECON, POP, UI, ART | stable (2 paths) |
| 7 | Population | WS-POP | ECON, UI | partial (Task F) |
| 8 | Terrain queries | WS-WORLD | BUILD, ECON, UI, ART | partial |
| 9 | Construction placement | WS-BUILD (+WORLD/UI) | UI, ART, ECON | to be unified |
| 10 | UI access to state | WS-CORE (bridge) | UI, QA | stable |

**Cross-cutting rule:** any **incompatible** change in these signatures or semantics is proposed to the Technical Director, versioned here, and announced to the consumers before integrating.
