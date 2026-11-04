# SYSTEM_IMPLEMENTATION_SPECIFICATIONS — Civilization Evolutions

> **Phase 2 — "Readiness-to-implement" specifications, per system.** No code. Goal: make each future task easy to define and delegate. Fields per system: Purpose · Scope · Responsibilities · Inputs · Outputs · Shared entities · Dependencies · Events · Business rules · MVP · Future extensions · Acceptance.
> Sources: the GDD of each domain ([DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)). Entities: [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md). Order/dep.: [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md). Version 1.0 · conceptual.

---

# Part A — Foundational systems

## 1. Time and Simulation
- **Purpose:** single global clock + internal cycles of different frequency; coordinate the simulation.
- **Scope:** date/calendar, speeds (pause/x1/x2/x3/x5), seasons, cadences; NOT domain rules.
- **Responsibilities:** advance time independently of the FPS; emit ticks/seasons/year; allow pause/acceleration without altering results.
- **Inputs:** scale/speed configuration; seasonal conditions per region (from Geography).
- **Outputs:** time events (DayTick/SeasonChanged/YearChanged), current date.
- **Shared entities:** World (date).
- **Dependencies:** none (Level 0).
- **Events:** produces `SeasonChanged`, `YearChanged`, periodic ticks; reacts to pause/speed.
- **Rules:** acceleration does not change results; systems do not use `Tick()` for everything; states are preserved between updates.
- **MVP:** pause + speeds + day/month/year + seasons (already exists in SimCore/CivSimSubsystem).
- **Future:** fine configurable scale; seasons per region; automatic acceleration.
- **Acceptance:** the clock advances without depending on the FPS; pausing/accelerating does not alter the resulting state.

## 2. World Generation
- **Purpose:** create the initial state of the world (procedural, reproducible) and the editor.
- **Scope:** topography, hydrology, biomes, soils, resources, initial location of civilizations; presets; seed.
- **Responsibilities:** generate a deterministic world from (seed + config); expose the state to Geography.
- **Inputs:** seed, preset, parameters (size, land/water, relief, climate, resources).
- **Outputs:** populated Terrain/Regions, located natural resources.
- **Shared entities:** World, Terrain, Natural resource.
- **Dependencies:** Geography (climate model), Resources (catalog).
- **Events:** `WorldGenerated`.
- **Rules:** same seed+config ⇒ same world; "flat" is a preset, not a default (DR-02).
- **MVP:** terrain with relief, water, resources and a central flat zone (part exists in CivTerrain).
- **Future:** map types, varied biomes, editor, large worlds via chunks.
- **Acceptance:** generates and shows a basic world reproducible by seed.

## 3. Resources and Materials
- **Purpose:** single catalog of goods with identity, units and shared rules.
- **Scope:** resource definition (id, category, economic function, quality, storage, decay, unit, currency).
- **Responsibilities:** be the **single** source of resource identity; provide inventories.
- **Inputs:** data-driven definitions.
- **Outputs:** catalog + rules consumed by all the economic ones.
- **Shared entities:** Resource/Material, Inventory, Natural resource (identity).
- **Dependencies:** none (Level 0); Geography indicates where resources are.
- **Events:** —.
- **Rules:** one id per good; no copies per system (DC-07); universal currency; consistent abstract units.
- **MVP:** initial subset (wood, stone, berries, meat, wheat, flour, bread, +water) with stable ids (exists in SimCore).
- **Future:** quality/decay, strategic resources, catalog per period.
- **Acceptance:** all systems reference the same catalog by id; there are no duplicated resources.

## 4. Entities and Relationships
- **Purpose:** shared base of persistent identity and relationships.
- **Scope:** inhabitants, families, buildings, settlements, regions, organizations; relationships; persistent history.
- **Responsibilities:** stable identifiers; multiple relationships with memory; evolution/merge/split without losing identity.
- **Inputs:** additions/removals/changes from the domain systems.
- **Outputs:** identity/relationship queries for all.
- **Shared entities:** all.
- **Dependencies:** none (Level 0).
- **Events:** `EntityCreated/Changed/Removed/Merged`.
- **Rules:** do not duplicate entities; identity independent of the simulation level (DR-10); layered property (DR-07).
- **MVP:** basic identity for buildings/settlements/individuals (today implicit in SimCore/UE — **unify**, RISK-02).
- **Future:** rich social relationships, organizations, biographies.
- **Acceptance:** an entity keeps its id and history as it evolves/aggregates.

## 5. Construction and Architecture
- **Purpose:** physically build and transform the settlements.
- **Scope:** placement (free + optional grid), construction work with materials/workers, evolution, heritage, infrastructure.
- **Responsibilities:** validate and execute construction work; manage state/condition; support player-chosen evolution.
- **Inputs:** materials (Resources), labor (Population), technology (enablement).
- **Outputs:** Buildings/infrastructure; demand for materials/labor.
- **Shared entities:** Building, Settlement, Inventory.
- **Dependencies:** Entities, Resources, Geography; Production, Technology.
- **Events:** `BuildingConstructed/Upgraded/Abandoned`.
- **Rules:** if materials are missing, the construction work stops; evolution is **not** automatic; the old coexists.
- **MVP:** placement with cost + validation + selection + simple evolution (exists partially in SimCore + CivBuildingActor/UE menu).
- **Future:** real free construction, materials per era, ruins, heritage, organic urban planning.
- **Acceptance:** the player places/inspects buildings and the construction work respects costs.

## 6. Population and Society
- **Purpose:** demographic and social dynamics of the inhabitants.
- **Scope:** individuals/families/classes, needs, health, education, migration, happiness (according to scale).
- **Responsibilities:** birth/aging/death; needs and their satisfaction; classes/representatives; aggregation.
- **Inputs:** housing (Construction), food/consumption (Economy), employment (Production), conditions (Geography).
- **Outputs:** workforce, consumption, social state, happiness.
- **Shared entities:** Individual, Family/Household, Organization.
- **Dependencies:** Time, Entities, Construction.
- **Events:** `PopulationChanged`, `Migration`, `Epidemic`.
- **Rules:** hybrid model; the aggregate stays coherent with the total; individual prominence decreasing with scale.
- **MVP:** population (small individual); basic needs; simple growth. **Divergence to resolve:** unify the SimCore aggregate with the UE settlers (RISK-02, DR-10).
- **Future:** families/lineages, classes, health/education, migration, mathematical models.
- **Acceptance:** a colony grows/consumes coherently over years.

## 7. Production and Labor
- **Purpose:** transform resources into goods/services and organize labor.
- **Scope:** productive buildings, recipes, productivity, employment, building evolution.
- **Responsibilities:** execute recipes (consume inputs/produce outputs); assign workers; adapt production to priorities.
- **Inputs:** inputs (Resources/Inventory), workers (Population), recipes, technology.
- **Outputs:** produced goods; demand for inputs/labor.
- **Shared entities:** Building, Recipe, Profession, Inventory.
- **Dependencies:** Resources, Population, Construction, Technology; Logistics.
- **Events:** `ResourceProduced/Consumed`, `ProductionBlocked`.
- **Rules:** does not produce without inputs; automatic assignment with intervention; visible evolution.
- **MVP:** chain wheat→mill→flour→bakery→bread + automatic assignment (exists in SimCore). (UE building production to be unified, RISK-01.)
- **Future:** skills/quality/wages, modules, maintenance.
- **Acceptance:** a production chain works, consuming and generating resources.

## 8. Storage and Logistics
- **Purpose:** physically move and store resources.
- **Scope:** inventories per location, simplified transport, routes, infrastructure, trade (levels).
- **Responsibilities:** move resources between inventories; prevent consumption without delivery.
- **Inputs:** source/destination inventories, geography/distance, infrastructure.
- **Outputs:** deliveries, costs/times (future), regional prices.
- **Shared entities:** Inventory, Route, Resource.
- **Dependencies:** Resources, Geography; Production, Economy.
- **Events:** `TransportRequested/Delivered`.
- **Rules:** nothing is consumed at the destination without a real delivery (DC-06); physical transport from the start (simplified).
- **MVP:** simplified physical transfer (exists in SimCore). Hauling of settlers to the town hall (UE, to be unified).
- **Future:** routes/vehicles/capacity/costs, maritime trade, congestion.
- **Acceptance:** a produced resource is only consumed at the destination after being delivered.

## 9. Economy
- **Purpose:** property, consumption, markets, currency, taxes.
- **Scope:** economic stocks, prices/supply/demand, mixed property, treasury, taxes, trade.
- **Responsibilities:** a common economic framework; prices; consumption; taxation (future).
- **Inputs:** production, labor, population (demand), logistics (costs).
- **Outputs:** prices, treasury, priority signals.
- **Shared entities:** Inventory, Resource, Civilization (treasury), Organization.
- **Dependencies:** Resources, Population, Production, Logistics.
- **Events:** `PriceChanged`, `TradeCompleted`.
- **Rules:** mixed property (DC-05); configurable complexity; the same economic system in the 3 trade levels.
- **MVP:** stocks + consumption + simple trade between settlements (exists partially in SimCore). No markets/prices yet.
- **Future:** markets with dynamic prices, operational currency, taxes, finance.
- **Acceptance:** basic consumption and trade are coherent; no "magic" resources.

## 10. Technology, Science and Innovation
- **Purpose:** knowledge that enables methods/capabilities by conditions.
- **Scope:** disciplines, discoveries, transmission, diffusion, obsolescence; a shared evolution framework.
- **Responsibilities:** enable (not adopt); publish discoveries to the shared framework.
- **Inputs:** conditions (knowledge+resources+context), funding (Economy), exchange (Diplomacy).
- **Outputs:** enablements for Production/Construction/Logistics/Military; discovery events.
- **Shared entities:** Technology/Knowledge (state per civilization).
- **Dependencies:** Time, Economy.
- **Events:** `TechnologyDiscovered`, `TechnologyDiffused`.
- **Rules:** by conditions, not by year (DC-09); availability ≠ adoption (DR-11); exceptional loss.
- **MVP:** simple tree/conditions + building evolution (exists in SimCore). Migrate to the shared framework (RISK-05).
- **Future:** disciplines, characters, diffusion, obsolescence with coexistence.
- **Acceptance:** a discovery enables effects across several systems without those systems knowing its implementation.

---

# Part B — Remaining systems (appropriate level)

## 11. Culture, Identity and Traditions
- **Purpose/scope:** cultural identity (language, art, traditions, festivities) that is evolving and visible. **Dep.:** Population, Religion, Construction. **Entities:** Culture. **Rules:** organic evolution; non-destructive fusion; visual evolution. **MVP:** basic identity + simple regional differences. **Acceptance:** culture is visible in the world and changes over time.

## 12. Religion and Spirituality
- **Purpose/scope:** beliefs, institutions, architecture and religious figures. **Dep.:** Population, Politics, Construction, Culture. **Entities:** Religion, Organization, Building. **Rules:** organic origin + player creation; coexistence according to context. **MVP:** a basic religion + followers + a building + festivities. **Acceptance:** a religion influences society and leaves a physical imprint.

## 13. Politics and Government
- **Purpose/scope:** power, laws, institutions, stability. **Dep.:** Population, Economy. **Entities:** Political institution, Civilization. **Rules:** the player guides (DC-02/DR-05); changes with consequences. **MVP:** simple structure + stability + a few policies/taxes. **Acceptance:** changing a policy produces coherent consequences.

## 14. Territorial Administration and Regions
- **Purpose/scope:** group/administer settlements into regions with configurable autonomy. **Dep.:** Politics, Entities; uses existing Economy/Population/Construction. **Entities:** Region/Territory. **Rules:** mixed model; does not duplicate functionality. **MVP:** regions + simple budgets/taxes + automatic administrators. **Acceptance:** manage several cities without individual micromanagement.

## 15. Transport, Trade and Logistics (trade)
- See system 8 (storage/logistics). **Trade:** basic/intermediate/advanced levels over the same economic system; dynamic routes; maritime trade (future). **Acceptance:** local/regional trade with prices by conditions.

## 16. Exploration and Territorial Expansion
- **Purpose/scope:** know/occupy/colonize the world (guided + autonomous). **Dep.:** Geography, Logistics. **Entities:** Region, Settlement, Civilization. **Rules:** knowledge ≠ occupation; reuses logistics. **MVP:** exploration with knowledge states + founding of settlements. **Acceptance:** discover territory and found a new settlement.

## 17. Diplomacy and International Relations ⚠️
- **GDD truncated (DR-13)** — spec limited to what is available. **Purpose/scope:** multidimensional relationships with historical memory between civilizations; treaties; negotiation at 3 levels. **Dep.:** Entities, Exploration, AI; integrates Religion. **Entities:** Civilization, Historical record. **Rules:** autonomy + broad intervention; cooperation preferred. **MVP:** to be defined (missing in the source). **Acceptance:** to be defined after completing the GDD.

## 18. AI and Civilization Autonomy
- **Purpose/scope:** autonomous civilizations with objectives/personality/history; a world that evolves without the player. **Dep.:** uses the same rules as the player (all systems). **Entities:** Civilization. **Rules:** no omniscience or knowledge of the future; 3 simulation levels by distance/relevance. **MVP:** 2–3 civilizations with basic economy/technology/diplomacy. **Acceptance:** an AI evolves plausibly without intervention.

## 19. War and Military Systems
- **Purpose/scope:** military organization, configurable combat, fortifications, conflicts; secondary. **Dep.:** Logistics, Population, Construction, Diplomacy. **Entities:** Organization (forces), Building (fortification). **Rules:** reuses logistics/resources; diplomacy to prevent; broad consequences. **MVP:** simple conflict + basic fortification. **Acceptance:** a conflict coherent with the period, without becoming the axis.

## 20. Events and Historical Evolution
- **Purpose/scope:** generate/record/represent occurrences; emergent history. **Dep.:** Time; receives from all. **Entities:** Event, Historical record. **Rules:** the fact is originated by the owner system; Events records (DC-14). **MVP:** simple mixed generation + record + notifications. **Acceptance:** a game produces its own coherent chronicle.

## 21. Interface UI/UX
- **Purpose/scope:** the 3D world as the primary interface + panels/maps; reads the state. **Dep.:** reads all. **Rules:** owns no rules; notifications from simulated changes; accessibility a priority. **MVP:** debug HUD (date, resources, population, construction, selection, tooltips) — exists. **Acceptance:** validates that the simulation works; does not invest in final art yet.

## 22. Progression and Ending
- **Purpose/scope:** objectives, scoring, difficulty, optional ending (sandbox). **Dep.:** Time, Events; indicators from all. **Entities:** — (reads indicators). **Rules:** authentic sandbox (objectives can be disabled); optional historical limit; final decision by the player. **MVP:** progression by periods + simple optional objectives. **Acceptance:** it can be played in sandbox without objectives and, optionally, with goals.
