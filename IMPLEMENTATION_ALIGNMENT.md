# IMPLEMENTATION_ALIGNMENT — Civilization Evolutions

> Relationship between the **existing code** and the **GDDs**. Analysis, **no** code was modified. The code is under active evolution (there is a parallel session implementing the map resource system), so this is a **snapshot** of the state; before touching a file, read its current state on disk. Design: see [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md) and [SYSTEM_ARCHITECTURE_MAP.md](SYSTEM_ARCHITECTURE_MAP.md).

## Where the code lives

```
civilizations-evolution/
├── SimCore/                         Engine-agnostic C++17 simulation core (compiles and tests outside UE)
│   ├── include/simcore/ + src/      Types, SimulationClock, Inventory, GameDatabase, World, Simulation, DefaultContent, EventBus
│   ├── app/main.cpp                 Console runner (runs the village for years)
│   └── tests/                       Own suite (~20 tests / 87 checks)
└── Source/CivilizationSimulator/    Unreal layer (presentation + game)
    ├── SimCoreUnity.cpp             Compiles SimCore inside the UE module
    ├── Core/                        CivGameModeBase, CivPlayerController, CivHUD, CivText
    ├── Camera/                      CivCameraPawn
    ├── View/                        CivViewManager, CivBuildingActor, CivTerrain
    ├── Simulation/                  CivSimSubsystem (bridge to SimCore + UE-side state)
    └── Units/                       CivUnit, CivResourceNode
Content/PBR (materials), Content/Chars (KayKit character).
```

## GDD → implementation map

| System (GDD) | Status | Where | Notes / divergences |
|---|---|---|---|
| **Time and Simulation** | Partial | `SimCore/SimulationClock`, `CivSimSubsystem` (tick, pause, speed, +month) | Clock with day/month/year/seasons, pause/speeds. Missing: seasons per region, configurable calendar. Aligned with the principle of an FPS-independent clock. |
| **Resources and Materials** | Partial | `SimCore` (Types/Inventory/GameDatabase) + `CivResourceNode` + UE stock in `CivSimSubsystem` | Catalog with stable ids (wood/stone/berries/meat/wheat/flour/bread + water/fruit UE). **Divergence:** there are **two** accountings — the SimCore stock and a UE-side `ColonyStock`. Risk of duplication (the GDD requires a single inventory). No quality/decay/currency. |
| **Economy** | Minimal | `SimCore/Simulation` (stocks, trade between settlements, consumption/health) | No markets, prices, supply/demand, currency, taxes or property. |
| **Production and Labor** | Partial | `SimCore` (recipes, automatic worker assignment) + `CivBuildingActor` (UE production per interval) | Chain wheat→mill→flour→bakery→bread in SimCore. **Divergence:** production of the buildings placed via the UE menu (gatherer→berries, hunter→meat) runs in `CivBuildingActor` (UE), not in SimCore. No skills/quality/wages. |
| **Transport and Logistics** | Partial | `SimCore` (simplified logistics) + UE hauling of settlers to the town hall | "Nothing is consumed without being delivered" is respected conceptually. The hauling of `CivUnit` is UE-side. No routes/vehicles/infra. |
| **Construction and Architecture** | Partial | `SimCore` (building defs + evolution + cost + demolition) + `CivBuildingActor` + menu in `CivPlayerController`/`CivHUD` | Placement with cost (wood/stone), evolution by technology in SimCore. Placeholder meshes (cube/cone/cylinder). Missing: real free construction, materials per era, ruins, heritage, optional grid. |
| **Population and Society** | Partial / **divergent** | `SimCore` (**aggregate** population, workers, consumption) + `CivUnit` (**individual** UE settlers) | Two models coexist: the SimCore aggregate and the 20 `CivUnit` actors. The GDD calls for a **unified hybrid** model; today they are separate. No families, classes, needs, individual health, demographics. |
| **World Generation** | Minimal | `CivTerrain` (deterministic noise, flat center + hills, water, trees) | No exposed configurable seed, biomes, map types or editor. |
| **Geography/Climate/Environment** | Minimal | `CivTerrain` (seasonal tint), clock seasons | Hills + water + tint per season. No simulated climate, geological resources, flora/fauna, disasters. (The relief with hills is closer to "realistic" Geography, not to the "flat maps" decision of Generation — see **C-02**.) |
| **Technology** | Partial | `SimCore` (technology tree + auto-research + building evolution) | Simple and lineal-ish, not the organic model by disciplines/characters of the GDD. |
| **Interface UI/UX** | Partial (MVP) | `CivHUD` + `CivViewManager` HUD (date/season, settlers, resources, top bar, construction menu, tooltips, selection box) | Matches the "minimal debug UI" of the technical plan. Missing: final UI, thematic maps, accessibility. |
| **Entities and Relationships** | No (implicit) | loose ids in SimCore / UE | There is no shared model of persistent entities/relationships. |
| **Events** | Minimal | `SimCore/EventBus` (internal events) | No emergent history, memory or chronology. |
| **Culture · Religion · Politics · Administration · Exploration · Diplomacy · Civilization AI · War · Progression** | Not implemented | — | Outside the current prototype. (Townhall with "shelter + arrows + enemies" was proposed, not implemented.) |

## What works today (playable loop)

Start with a **town hall** + **20 animated settlers (KayKit)**. The player **selects** (click / box), **moves** (right-click), and orders **gathering** of trees/rocks/water; the settler collects and (in progress) **hauls to the town hall**. With resources, they **build** a hut/hunter/gatherer from the menu; the buildings **produce** over time. HUD with date/season, resource bar and active/maximum population. RTS camera with tilt/rotation/panning. Visual: grass/rock/bark **PBR** + **Lumen/SM6**. i18n **English by default + Spanish**.

## Alignment with the architecture principles (technical plan §4)

- ✅ **Simulation/presentation separation:** strong in the original core — `SimCore` is pure C++, tested, engine-agnostic; the UE layer wraps it.
- ⚠️ **Erosion of that separation (main risk):** the new colony functionality (`CivUnit` settlers, `CivResourceNode` nodes, `ColonyStock` in `CivSimSubsystem`, production in `CivBuildingActor`) lives **in the UE layer**, not in SimCore. This puts **simulation logic into presentation**, leaves it **outside the SimCore tests** and creates a **second model** of population/economy in parallel to SimCore's.
- ⚠️ **Scalability:** `CivUnit` is one **Actor per inhabitant**. The plan explicitly calls for **not** creating one Actor per citizen and for supporting aggregation. At 20 it works fine; it does not scale to the vision of large populations.
- ◑ **Data-driven:** the content (resources/buildings/recipes/technologies) is in **C++** (`DefaultContent`), not in UE Data Tables/Assets. It is "data-ish" but not editable as data from the editor.
- ✅ **Modularity / FPS-independent clock / configuration:** respected in SimCore.

## Architectural risks (to decide before continuing)

1. **Double population/economy model** (SimCore aggregate ↔ UE-side individual). It duplicates entities/resources, against the Maestro ("no copies per system"). To decide: does the colony/settlers move down to SimCore (recommended for testability/scale) or does SimCore become legacy?
2. **Gameplay in the UE layer** reduces test coverage and couples simulation to rendered actors.
3. **One Actor per settler** does not scale; an aggregate level (simulation LOD) as the GDD requires will be missing.
4. **Uncommitted work** (not a lack of Git). The UE project **does** have its own git repo (history up to `HANDOFF`) and the documentation now has a separate repo. The real risk: **everything after the HANDOFF** (this session's build fixes + the new gameplay, ~27 changes) is **uncommitted**, with two sessions editing the same tree. Mitigation: commit at each green checkpoint.
5. **No shared Entities/Relationships model:** the ad-hoc ids will make future property, history and relationships harder.
6. **Hardcoded content** in C++: migrating to data (Data Tables/Assets) will facilitate the data-driven design the plan calls for.

## Pending decisions that condition the implementation

Before going deeper it is worth resolving (see `AUDIT_REPORT.md`): **C-02** (flat vs realistic terrain — the code already uses hills), **C-03** (scope of climate/seasons affecting agriculture/production), **C-05** (player role/power for the future political system), **C-06** (scale limits, which determine whether actors per settler are kept or aggregated), and the **common property/entities model** (C-07 + GDD Entities).

## Constraints of this analysis

No source code or GDDs were modified (this is an understanding task). The refactor recommendations were **not** applied; they are inputs for a design/architecture decision by the project owner.
