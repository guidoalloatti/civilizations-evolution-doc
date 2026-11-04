# DOCUMENTATION_INDEX — Civilization Evolutions

> Entry point for finding the documentation. For each document: path, version, status, purpose, key confirmed decisions, dependencies, and main sections (for targeted reading).
> Vision authority: [docs/00-project/GDD_MASTER.md](docs/00-project/GDD_MASTER.md). Contradictions and pending items: [AUDIT_REPORT.md](AUDIT_REPORT.md). How to read according to the task: [DOCUMENTATION_READING_GUIDE.md](DOCUMENTATION_READING_GUIDE.md).

**Canonical language:** English. **Status legend:** `Confirmed` (consolidated decision table) · `Partial` (decisions + pending items) · `Incomplete` (no formal table / basis in the body) · `Truncated` (file cut off in the source).

---

## Design blueprint (Phase 2 — analysis derived from the GDDs)

Analysis documents in the project root. **Derived** from the original GDDs (they do not replace them); they distinguish confirmed decisions from proposals.

| Document | Purpose |
|---|---|
| [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) | Classified findings (CC/PI/CX/MD/AR/FC): contradictions, duplications, gaps, risks. |
| [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) | Decisions confirmed per GDD (DC-01..) + DR-00..DR-12 **confirmed by delegation** (2026-11-04, reversible); DR-13 unresolved. |
| [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md) | Shared entities, family≠household, property/authority in layers, aggregation persistence, MVP/future. |
| [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md) | Dependency graph, system classification, authoritative data owner, flows, events and cadences. |
| [CROSS_SYSTEM_VALIDATION_SCENARIOS.md](CROSS_SYSTEM_VALIDATION_SCENARIOS.md) | Scenarios A–G that validate (at the design level) that the architecture supports integrated simulation. |
| [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) | Stages E0–E6 with objectives, dependencies, acceptance, risks and what NOT to do yet. |
| [SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md](SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md) | Readiness spec per system (inputs/outputs/events/rules/MVP/acceptance). |
| [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md) | Historical evolution as a shared framework (reaction to discoveries/conditions, not scripting). |
| [IMPLEMENTATION_RISK_REGISTER.md](IMPLEMENTATION_RISK_REGISTER.md) | Architectural/development risks (RISK-01..19) with severity, does it block the MVP?, mitigation and when to address them. |

Also: [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md) (code vs design) and [SYSTEM_ARCHITECTURE_MAP.md](SYSTEM_ARCHITECTURE_MAP.md) (conceptual map).

---

## Multi-agent development (development organization — PROPOSAL 2026-11-04)

**Development governance** documents (not game design): how several agents work in parallel without stepping on each other or duplicating systems. **Status: proposal, pending approval** (no agents are launched and no worktrees are created until approved). Design authority remains in the GDDs.

| Document | Purpose |
|---|---|
| [AGENT_ORGANIZATION.md](AGENT_ORGANIZATION.md) | The 9 workstreams: scope, responsibilities, exclusions, dependencies, deliverables, allowed/restricted folders/files, acceptance criteria. |
| [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) | Shared interfaces (time, events, identity, resources, inventories, buildings, population, terrain, placement, UI access) with their actual API and their owner. |
| [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) | File/folder → single-owner matrix; protocol for shared files and `.uasset` binaries. |
| [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md) | Task board by milestone (M0–M4 + continuous): dependencies, priority, files, acceptance, status, blockers. |
| [GIT_WORKFLOW.md](GIT_WORKFLOW.md) | Branches and worktrees per workstream (two repos: docs + UE), UE binaries with no merge, tests and delivery. |
| [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md) | Central integration process led by the Technical Director. |

---

## 0. Project and governance

### Master GDD
- **Path:** `docs/00-project/GDD_MASTER.md` · **Version:** 1.0 (consolidation) · **Status:** Central reference (scope decisions pending).
- **Purpose:** Common vision, pillars, scope, authority map by domain, cross-system integrations, canonical vocabulary, pending cross-cutting decisions, art direction and conceptual roadmap.
- **Key:** defines who owns each decision (§5) and the integration contracts (§6). No proposal becomes a decision just by appearing here.
- **Sections:** 1 Vision · 2 Pillars · 3 Scope and prototype · 4 Confirmed decisions (summary) · 5 Documentation authority · 6 Integrations · 7 Vocabulary · 8 Cross-cutting pending items · 9 Art direction and roadmap · 10 Documentation structure.

### Technical plan
- **Path:** `docs/02-technical/TECHNICAL_PLAN_v0.1.md` (identical copy in `civilizations-evolution/Docs/00_TECHNICAL.md`) · **Version:** 0.1 · **Status:** Initial implementation.
- **Purpose:** Technical foundation (UE5, C++). Architecture principles, project structure, simulation core, world, construction, resources, production, population, logistics, UI, testing, Git, milestones 1–6, development rules.
- **Key:** simulation/presentation separation; data-driven; modular; FPS-independent clock; individual→aggregate scalability; historical flexibility. The world is simulated as a coherent system, not loose features.
- **Sections:** 1 Overview · 2 Objectives · 3 Stack · 4 Principles · 5 Structure · 6 Simulation core · 7 World · 8 Construction · 9 Resources · 10 Production · 11 Population · 12 Logistics · 13 UI · 14 Config · 15 Testing · 16 Git · 17 Milestones · 18 Rules · 19 First task · 20 Final technical vision.

### Original general GDD (consolidated by the Master GDD)
- **Path:** `civilizations-evolution/Docs/01_GDD_GENERAL.md` · **Version:** 0.1 · **Status:** Conceptual design (historical; its authority passed to the Master GDD).
- **Purpose:** Complete original vision: historical periods, building evolution by era, hybrid population, economy, technology, politics, religion, time, art direction, 8-phase roadmap, **MVP** and 12 non-negotiable principles, 16 pending decisions.

---

## 1. World, geography and time

### Procedural world generation
- **Path:** `docs/01-systems/GDD_Procedural_World_Generation_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** System that creates coherent, diverse and configurable geographic settings (reproducible seeds, world editor).
- **Confirmed:** configurable world type (initially flat maps with natural borders — see conflict **C-02**); coherent geology/relief; rivers/lakes by topography; biomes by geographic conditions; automatic placement of civilizations with recommendations; scale with presets + seeds; editor as an independent module.
- **Dependencies:** consumes the climate model from Geography and the one from Resources; provides the initial world state to almost all systems.
- **Sections:** 1 Vision · 2 Philosophy · 3 World types · 4 Geology/relief · 5 Hydrology · 6 Climate/biomes · 7 Soils · 8 Resources · 9 Placement of civilizations · 10 Exploration/knowledge · 11 Historical maps · 12 Scale · 13 Seeds · 14 Quality/performance · 15 Editor · 16 Config · 17 Integration · 18 Generation architecture · 19 MVP · 20 Vision · 21 Decisions.

### Geography, climate and environment
- **Path:** `docs/01-systems/GDD_Geography_Climate_and_Environment_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** Natural base (geography, climate, ecosystems, resources) that exists before civilizations and conditions their development.
- **Confirmed:** all the main landforms; realistic and continuous topography (vs the "flat maps" of Generation — **C-02**); seasons adapted by region (does not impose 4); climate = relevant meteorological variables; disasters included but **disabled by default**; differentiated regeneration of renewables/non-renewables; domestication very important in the early stages. Climate "mainly visual at the start" vs seasonal dependencies (**C-03**).
- **Dependencies:** source of the climate model (reused by Generation); provides to Economy/Production/Agriculture/Construction/Population/Transport/Technology/Culture/Religion/War.
- **Sections (H1):** 1 Vision · 2 Geography · 3 Topography · 4 Rivers · 5 Oceans/coasts · 6 Biomes · 7 Climate · 8 Seasons · 9 Climate variability · 10 Human impact · 11 Resources/geology · 12 Regeneration/depletion · 13 Flora/fauna · 14 Domestication · 15 Disasters · 16 Disaster frequency · 17 Conservation · 18 Geography and settlements · 19 Generation/config · 20 Complexity · 21 Integration · 22 MVP · 23 Decisions · 24 Vision.

### Exploration and territorial expansion
- **Path:** `docs/01-systems/GDD_Exploration_and_Territorial_Expansion_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** How civilizations get to know, explore, occupy and control the world (directed expansion + autonomous growth).
- **Confirmed:** configurable exploration with knowledge states; cartography with precision levels; mixed and simplified territorial control; colonies with possible autonomy/independence; directed and autonomous migrations; spontaneous settlements with player intervention.
- **Dependencies:** Geography, Population, Economy, Transport (reuses its logistics), Technology, Diplomacy, Politics, Culture, Religion, Military, Construction.
- **Sections:** 1 Vision · 2 Exploration · 3 Discovery of regions · 4 Cartography · 5 Expansion · 6 Founding of settlements · 7 Migration/colonization · 8 Administration/autonomy · 9 Autonomy of other civilizations · 10 Integration · 11 Complexity · 12 MVP · 13 Vision · 14 Decisions.

### Time and simulation
- **Path:** `docs/01-systems/GDD_Time_and_Simulation_v0.1.md` · **v0.1** · **Incomplete** (no decision table; the least developed).
- **Purpose:** Model of **continuous time with a single global clock** and internal cycles of independent frequency (simulate centuries without processing everything at the same frequency).
- **Firm definitions:** single global clock (systems do not have their own clocks); configurable calendar with consequences; pause/normal/accelerated speeds without altering results; hybrid population; production/buildings preserve state without constant updating; pause with no hidden progress.
- **Dependencies:** cross-cutting — reference clock for ALL systems; consumes seasons/environmental conditions.
- **Sections:** 1 Vision · 2 Global clock · 3 Calendar · 4 Speed · 5 Internal cycles · 6 Population/aging · 7 Production/buildings · 8 Seasons · 9 Events · 10 Pause · 11 Acceleration/performance · 12 Config · 13 Integration · 14 MVP · 15 Guiding principle.

---

## 2. Economy, resources, production, logistics, construction

### Resources and materials
- **Path:** `docs/01-systems/GDD_Resources_and_Materials_v0.1.md` · **v0.1** · **Incomplete** (no formal table; base rules in the body).
- **Purpose:** **Single, unified** resource catalog with its own identity, shared by all systems (without duplicating materials).
- **Base rules:** global system without duplicates; dual classification (nature/category + economic function: raw material/intermediate/final); universal monetary unit from the start; general title held by the State/civilization compatible with private ownership of buildings; abstract units with consistent scale.
- **Dependencies:** cross-cutting — consumed by Economy, Production, Construction, Population, Transport, Trade, Technology, Agriculture, Geography, Administration, Diplomacy, War.
- **Sections:** 1 Vision · 2 Classification · 3 Level of detail · 4 Quality/properties · 5 Natural resources · 6 Storage · 7 Deterioration · 8 Consumption · 9 Geographic distribution · 10 Substitution · 11 Property · 12 Strategic resources · 13 Money/currency · 14 Units/accounting · 15 Integration · 16 MVP · 17 Guiding principle.

### Economy
- **Path:** `docs/01-systems/GDD_Economy_v0.2.md` · **v0.2** · **Partial**.
- **Purpose:** Economic evolution from subsistence to complex economies: network of resources, production, consumption, logistics, trade, property and finance.
- **Confirmed:** mixed and evolving property; automatic labor assignment with intervention; physical transport from the start (simplified); hybrid population; configurable complexity; trade at three levels; flexible historical evolution; detailed (small) / aggregate (large) simulation.
- **Pending:** productivity/wage/price formulas; automatic distribution; building/land ownership; economic integration between settlements; financial institutions; balance (**C-07**).
- **Dependencies:** consumes Resources, Production, Population, Technology, Environment; provides prices/markets/treasury/taxes; impacts social/political/cultural.
- **Sections (top):** 1 Vision · 3 Resources · 4 Production · 5 Population/labor · 6 Consumption · 7 Agriculture/seasons · 8 Storage/logistics · 9 Markets/prices · 10 Trade · 11 Finance/currency · 12 Property · 13 Taxes/policies · 14 Historical stages · 16 Interface · 17 Detailed/aggregate simulation · 18 MVP · 19 Decisions · 20 Pending items.

### Production and labor
- **Path:** `docs/01-systems/GDD_Production_and_Labor_v0.2.md` · **v0.2** · **Partial**.
- **Purpose:** How resources are transformed into goods/services and how labor is organized; emphasis on the **visual and functional evolution of buildings**.
- **Confirmed:** production based on buildings/processes; interdependent chains; automatic labor assignment with intervention; specialization (profession + secondary skills); alternative recipes; the player sets priorities and the building adapts; gradual quality; evolving maintenance; mixed property; modular expansion; building evolution by triggers (technological/historical/cultural…), with visible notification; **non-mandatory** evolution.
- **Pending:** productivity formulas; learning; wages; quality; owner autonomy; upgrade conditions; compatibility of old buildings↔new technologies.
- **Dependencies:** consumes Resources, Population (workers), Technology, Construction, Logistics; regulated by Politics/Religion/Culture/Environment; provides goods and regional specialization to Economy.
- **Sections (top):** 1 Vision · 3 Buildings · 4 Recipes · 5 Chains · 6 Productivity · 7 Labor assignment · 8 Professions/skills · 9 Labor relations · 10 Methods/technology · 11 Building evolution · 12 Modularity · 13 Quality · 14 Tools/maintenance · 15 Sectors · 16 Production scales · 17 Property/control · 18 Integration · 19 Interface · 20 Complexity · 21 Simulation · 22 MVP · 23 Decisions · 24 Pending items.

### Transport, trade and logistics
- **Path:** `docs/01-systems/GDD_Transport_Trade_and_Logistics_v0.1.md` · **v0.1** · **Partial**.
- **Purpose:** Physically connect producers, consumers, warehouses, markets, cities, regions and civilizations; from manual transport to international maritime networks.
- **Confirmed:** every resource is transported physically; own transport + carriers; historical means/infrastructure; roads with consequences (distance/terrain/climate/capacity/maintenance); local/regional/international scales; trade at 3 levels; dynamic prices; adaptable routes; maritime trade (fleets/ports); autonomous merchants + intervention; integrated military logistics (no parallel system).
- **Pending:** costs/times; route algorithms; congestion; markets; fleets; performance on large maps.
- **Dependencies:** Economy, Production, Construction, Population, Technology, Politics, Culture, Religion, Diplomacy, War, Environment; provides the logistics network and regional prices.
- **Sections (top):** 1 Vision · 2 Modalities/assignment · 3 Means · 4 Infrastructure · 5 Transport factors · 6 Urban logistics · 7 Trade scales · 8 Prices · 9 Routes · 10 Maritime trade · 11 Merchants · 12 Storage · 13 Logistics problems · (control levels) · 18 Interface · 19 Integration · 21 MVP · 22 Decisions · 23 Future iterations.

### Construction and architecture
- **Path:** `docs/01-systems/GDD_Construction_and_Architecture_v0.1.md` · **v0.1** · **Partial**.
- **Purpose:** How the player builds, organizes, transforms and physically develops settlements throughout history; the city as a testament to progress.
- **Confirmed:** free construction with optional grid; free/interval rotation; construction work with workers and materials; staged construction; if materials are missing the work stops and looks for alternatives; **architectural evolution chosen by the player** (not automatic); preservation of historical buildings; abandonment→deterioration→ruins; styles by culture/geography/era; climate mainly visual at the start (**C-03**); demolition/relocation with costs; mixed urban growth with organic neighborhoods.
- **Pending:** beauty calculation; heritage assessment; organic growth; architectural evolution by civilization; performance of large cities; generation of ruins.
- **Dependencies:** consumes Resources/materials and Population (labor); Technology enables; Culture/Religion give style; Politics regulates; Environment conditions; provides the physical support to the other systems.
- **Sections (top):** 1 Vision · 2 Free construction · 3 Construction process · 4 Building evolution · 5 Modules · 6 Materials · 7 Vertical architecture · 8 Urban growth · 9 Beauty · 10 Infrastructure · 11 Styles · 12 Terrain/climate · 13 Deterioration/ruins · 14 Demolition/heritage · 15 Tools · 16 Camera · 17 Scale · 18 Complexity · 19 Integration · 20 MVP · 22 Decisions · 23 Pending items.

---

## 3. Population, society, culture, religion, politics, administration

### Population and society
- **Path:** `docs/01-systems/GDD_Population_and_Society_v0.1.md` · **v0.1** · **Partial** (the only one with an explicit pending-items section).
- **Purpose:** How inhabitants live, reproduce, work, learn, relate and evolve; from community to complex society.
- **Confirmed:** complete individual information; high individual prominence at the start, decreasing over time; families as entities, households according to culture/era; multifactorial birth rate/mortality; needs (basic→social→cultural) with historical evolution; individual/aggregate happiness; social classes with conditioned mobility; population health with configurable epidemics; education/literacy; internal and external migration; social representatives; aggregate crime.
- **Pending (§21):** mathematical models of needs/happiness/demographics; lineages; social mobility; institutions; population scalability.
- **Dependencies:** Economy, Production/labor, Construction (housing), Technology, Politics, Religion, Culture, Environment; provides the population/social model.
- **Sections:** 1 Principles · 2 Individuals · 3 Families/demographics · 4 Needs · 5 Happiness/quality of life · 6 Social classes · 7 Health · 8 Education · 9 Culture · 10 Religion · 11 Migration · 12 Representatives/conflicts · 13 Crime/security · 14 Historical evolution · 15 Interface · 16 Complexity · 17 Player vs society · 18 Integration · 20 Decisions · 21 Pending items.

### Culture, identity and traditions
- **Path:** `docs/01-systems/GDD_Culture_Identity_and_Traditions_v0.1.md` · **v0.1** · **Confirmed** (no pending-items section).
- **Purpose:** Personality of the civilization (languages, cuisine, dress, art, architecture, customs, festivities, traditions) that evolves with the civilization.
- **Confirmed:** organic and influenceable cultural evolution; general/regional/local identity; exchange through trade/migrations/wars/diplomacy; progressive non-destructive fusion; languages evolve/split/mix/disappear; fashions; **fundamental and visible visual evolution** in the world.
- **Dependencies:** Population, Religion, Politics, Construction (physical expression), Economy, Education, Technology, Diplomacy, Migration.
- **Sections:** 1 Vision · 2 Components · 3 Identity · 4 Geography · 5 Exchange · 6 Fusion · 7 Language · 8 Fashions · 9 Player · 10 Visual evolution · 11 Traditions/heritage · 12 Social classes · 13 Religion · 14 Config · 15 Integration · 16 MVP · 17 Decisions · 18 Vision.

### Religion and spirituality
- **Path:** `docs/01-systems/GDD_Religion_and_Spirituality_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** Beliefs, practices, institutions and spiritual traditions as living systems that appear, evolve, expand, split and disappear.
- **Confirmed:** organic origin + creation by the player; all historical beliefs; very important social influence in early eras; peaceful or conflictual coexistence depending on context; institutions with property; monumental religious architecture; identifiable religious figures; reforms/divisions; configurable complexity.
- **Dependencies:** Population, Politics, Culture, Construction, Economy, Education, Technology, Diplomacy.
- **Sections:** 1 Vision · 2 Origin · 3 Types of beliefs · 4 Evolution · 5 Social influence · 6 Coexistence · 7 Institutions · 8 Architecture · 9 Figures · 10 Daily life · 11 Reforms/divisions · 12 Secularization · 13 Player · 14 Config · 15 Integration · 16 MVP · 17 Decisions · 18 Vision.

### Politics and government
- **Path:** `docs/01-systems/GDD_Politics_and_Government_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** How societies organize power, decide, legislate and administer. The player is an **external guiding entity** (not a ruler) with always-broad power.
- **Confirmed:** player role = higher guiding entity; always-broad power (see conflict **C-05**); all historical forms of government; political changes with consequences; institutions exist but the player retains control; modifiable laws; identifiable leaders; configurable conflicts; progressive and non-linear evolution.
- **Dependencies:** cross-cutting — Population, Economy, Religion, Culture, Production, Construction, Technology, Military.
- **Sections:** 1 Vision · 2 Player's role · 3 Forms of government · 4 Political evolution · 5 Institutions · 6 Laws/policies · 7 Advisors/power groups · 8 Opposition · 9 Leaders · 10 Conflicts · 11 Config · 12 Integration · 13 MVP · 14 Decisions · 15 Vision.

### Territorial administration and regions
- **Path:** `docs/01-systems/GDD_Territorial_Administration_and_Regions_v0.1.md` · **v0.1** · **Confirmed** (simplified; the shortest).
- **Purpose:** Organize and administer territories as the civilization grows, without controlling each settlement individually (central decisions + autonomous management).
- **Confirmed:** cities grouped into regions; mixed administration; organic regional identity; configurable autonomy; general and differentiated taxes; cooperation/competition relationships; automatic and influenceable evolution; configurable complexity with automation.
- **Dependencies:** uses existing Economy/Production/Social/Construction without duplicating; strongly tied to Politics.
- **Sections:** 1 Vision · 2 Organization · 3 Regional administration · 4 Management · 5 Identity/specialization · 6 Autonomy · 7 Taxes/regional economy · 8 Relations between regions · 9 Historical evolution · 10 Distance · 11 Complexity · 12 MVP · 13 Decisions.

---

## 4. Technology, military, diplomacy, AI, events, progression, interface, entities

### Technology, science and innovation
- **Path:** `docs/01-systems/GDD_Technology_Science_and_Innovation_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** Development of knowledge and inventions as an organic consequence of history (not a traditional tech tree).
- **Confirmed:** hybrid model (organic + directed); the player sets priorities and funds, specialists research; all disciplines; family/artisanal/educational/institutional/international transmission; exceptional loss of knowledge; simultaneous parallel evolution; historical innovators; obsolescence with coexistence; diverse paths; possible stagnation; **fundamental visual evolution** connected to Production and Construction.
- **Dependencies:** Economy, Production, Construction, Population, Culture, Religion, Politics, Environment, Transport, Military, Diplomacy.
- **Sections:** 1 Vision · 2 Progress model · 3 Knowledge levels · 4 Disciplines · 5 Origin of discoveries · 6 Characters · 7 Institutions · 8 Transmission/conservation · 9 Exchange · 10 Evolution of disciplines · 11 Simultaneous evolution · 12 Player control · 13 Science vs technology · 14 Education · 15 Economy and innovation · 16 Environment · 17 Obsolescence · 18 Diversity of paths · 19 Stagnation · 20 Military technology · 21 Visual evolution · 22 Interface · 23 Config · 24 Historical scope · 25 Integration · 26 MVP · 27 Decisions · 28 Vision.

### War and military systems
- **Path:** `docs/01-systems/GDD_War_and_Military_Systems_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** Historical evolution of military organization, war technologies, tactics, fortifications and conflicts; a transformative but **secondary** force.
- **Confirmed:** configurable organization; historical force categories; civil/professional recruitment by era; configurable combat (automatic/tactical/detailed); morale important but not decisive; simplified military logistics and economy; configurable causes/start/end; fortifications with visual evolution; diplomacy very important for prevention; broad consequences; complexity by modules.
- **Dependencies:** uses general Transport/Logistics; Population (recruitment); Construction (fortifications); integrates Diplomacy; provides consequences to economy/population/politics/culture/territory.
- **Sections:** 1 Vision · 2 Organization · 3 Recruitment · 4 Military evolution · 5 Commanders · 6 Combat · 7 Logistics · 8 Types of conflict · 9 Causes/start · 10 Diplomacy/prevention · 11 Ending · 12 Fortifications · 13 Naval war · 14 Consequences · 15 Military autonomy · 16 Intelligence · 17 Config · 18 MVP · 19 Vision · 20 Decisions.

### Diplomacy and international relations ⚠️
- **Path:** `docs/01-systems/GDD_Diplomacy_and_International_Relations_v0.1.md` · **v0.1** · **TRUNCATED** (the source file is cut off midway through section 18 "Personality of leaders"; no MVP or decision table). **Logged for completion.**
- **Purpose:** Civilizations that interact, cooperate, compete, negotiate and enter into conflict through multidimensional relations with historical memory and autonomous evolution.
- **Firm definitions (§1, principles):** multidimensional relations (not a single score); historical memory; civilization autonomy; broad player intervention; cooperation/peaceful resolution; integration with economy/politics/culture/religion/technology; negotiations at 3 levels; configurable espionage and international organizations.
- **Dependencies:** integrates Religion (§13); links with economy/politics/culture/technology; relationship with Military; provides treaties, historical memory, prestige, dependent states.
- **Sections (H1, up to the cutoff):** 1 Vision · 2 Relations model · 3 Historical memory · 4 Historical evolution · 5 Ambassadors · 6 Treaties · 7 Duration/compliance · 8 Negotiations · 9 Objectives/interests · 10 Borders/disputes · 11 Influence/prestige · 12 Espionage · 13 Religion · 14 Culture · 15 International organizations · 16 Dependent states · 17 Diplomacy/war/peace · 18 Personality of leaders (cut off).

### AI and civilization autonomy
- **Path:** `docs/01-systems/GDD_AI_and_Civilization_Autonomy_v0.1.md` · **v0.1** · **Incomplete** (no table; principles + acceptance criteria).
- **Purpose:** For civilizations to function as autonomous societies with their own objectives, personalities, leaders and history; the world evolves without the player.
- **Firm definitions:** configurable autonomy globally and per civilization; strategic identity with non-rigid tendencies; AI **without** undiscovered information or knowledge of the future; three simulation levels by distance/importance; possible but infrequent birth/disappearance; difficulty = quality of decisions + optional advantages, **without an omniscient AI**; uses the **same rules** as the player.
- **Dependencies:** Economy, Population, Construction, Technology, Culture/Religion, Diplomacy, Exploration, Administration, War, Events.
- **Sections:** 1 Vision · 2 Autonomy · 3 Personality/objectives · 4 Decisions · 5 Leaders · 6 Competition/cooperation · 7 Differentiated evolution · 8 Simulation by distance · 9 Birth/disappearance · 10 Player influence · 11 Difficulty · 12 Integration · 13 Memory/knowledge · 14 Config · 15 Interface · 16 MVP · 17 Guiding principle.

### Events and historical evolution
- **Path:** `docs/01-systems/GDD_Events_and_Historical_Evolution_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** Generate, connect, record and represent the events that build an emergent history, unique to each playthrough.
- **Confirmed:** mixed generation (historical/dynamic/random) with moderate randomness; simplified consequence chains; balance with relatively more positives; historical characters with biographies (important); complete record + summary encyclopedia; historical traces in the world; direct player intervention; free historical evolution; other civilizations with autonomous history; complexity by modules.
- **Dependencies:** cross-cutting — receives facts from all systems and records them; reuses the shared architecture from the Technology GDD. **Must not create game effects without an owning rule** (Master GDD §6).
- **Sections:** 1 Vision · 2 Generation · 3 Categories · 4 Balance · 5 Chains · 6 Characters · 7 Historical memory · 8 Traces · 9 Player · 10 Pacing · 11 Presentation · 12 Chronology · 13 Config · 14 MVP · 15 Integration · 16 Vision · 17 Decisions.

### Progression and ending
- **Path:** `docs/01-systems/GDD_Progression_and_Ending_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** How a playthrough progresses, what objectives the player pursues and how it ends; prioritizes a **sandbox** experience with no mandatory sequence.
- **Confirmed:** progression through indicative periods and free evolution; configurable and disableable objectives (authentic sandbox); optional overall score + indicators; transparent adaptive difficulty (not cheating); **optional historical limit** (start of the Industrial Revolution, not developed — **C-01**); possible continuation; final player decision; optional historical legacy.
- **Dependencies:** consumes indicators from Economy/Population/Technology/Culture/Religion/Diplomacy/Territory/Construction; legacy is fed by Events.
- **Sections:** 1 Vision · 2 Historical progression · 3 Objectives · 4 Score/prestige · 5 Custom objectives · 6 Difficulty · 7 Challenges/scenarios · 8 Duration/ending · 9 End of the historical period · 10 Legacy · 11 Complexity · 12 MVP · 13 Decisions.

### UI/UX interface
- **Path:** `docs/01-systems/GDD_Interface_UI_UX_v0.1.md` · **v0.1** · **Confirmed**.
- **Purpose:** Modern, contextual and adaptable interface; the **3D world is the main interface**, with menus/panels serving the experience.
- **Confirmed:** modern/contextual style; interaction with the 3D world first; contextual + global info; thematic maps very important; configurable notifications; initial and contextual tutorials; complete customization; **accessibility high priority**; adaptable complexity (simple/intermediate/advanced). The UI **reads** the simulation state, it does not own it.
- **Dependencies:** presents data from almost all systems; does not define its own simulation.
- **Sections:** 1 Vision · 2 Style · 3 Interaction with the world · 4 Information/statistics · 5 Navigation · 6 Maps/indicators · 7 Notifications · 8 Tutorials · 9 Customization · 10 Detailed info · 11 Accessibility · 12 Adaptation to complexity · 13 MVP · 14 Decisions.

### Entities and relationships
- **Path:** `docs/01-systems/GDD_Entities_and_Relationships_v0.1.md` · **v0.1** · **Incomplete** (no table; 8 principles + acceptance criteria).
- **Purpose:** Fundamental entities (inhabitants, families, buildings, settlements, regions, organizations), attributes, relationships and evolution — **shared database** for all systems.
- **Firm definitions (§2):** own identity; multiple simultaneous relationships that change/disappear/reappear; historical memory in important relationships; entities that evolve/split/merge/disappear; systems share entities without duplicating; persistent identifiers; configurable complexity. Title (§13): natural resources belong to the State/civilization while private parties operate them.
- **Dependencies:** base consumed by all systems; integrates Territorial Administration.
- **Sections:** 1 Objective · 2 Principles · 3 Inhabitants · 4 Families/households · 5 Buildings · 6 Settlements · 7 Regions · 8 Organizations · 9 Relationships · 10 Persistent identity/history · 11 Scalability · 12 Integration · 13 Property/title · 14 Interface/query · 15 Complexity · 16 MVP · 17 Acceptance criteria · 18 Guiding principle.

---

## 5. UE project documents (condensed / operational)

- `civilizations-evolution/Docs/00_TECHNICAL.md` — **identical** to the technical plan (see above).
- `civilizations-evolution/Docs/01_GDD_GENERAL.md` — original general GDD (see section 0).
- `civilizations-evolution/Docs/02_GDD_ECONOMIA.md` — condensed version of the Economy GDD used to guide the first code.
- `civilizations-evolution/Docs/03_GDD_PRODUCCION_TRABAJO.md` — condensed version of Production and Labor.
- `civilizations-evolution/Docs/04_GDD_CONSTRUCCION.md` — condensed version of Construction.
- `civilizations-evolution/HANDOFF.md` — handoff for compiling in UE5 (implementation context).
- `civilizations-evolution/README.md` and `Source/README_UE5.md` — getting the UE project up and running.
- `civilizations-evolution/Docs/ASSETS.md` — research on CC0/Fab/Mixamo assets (maintained by the parallel assets session).

## 6. Contradictions and pending items (summary; detail in AUDIT_REPORT.md)

- **C-01** cutoff date (≈1800 vs "start of the Industrial Revolution").
- **C-02** "flat maps" (Generation) vs "realistic and continuous topography" (Geography).
- **C-03** climate "mainly visual" (Construction) vs seasonal dependencies (Geography/Agriculture).
- **C-04** project name ("Historical Civilization Simulator" vs "Civilization Evolutions").
- **C-05** player role (external non-ruling entity vs ability to legislate/direct).
- **C-06** scale limits (population/map/civilizations) undefined.
- **C-07** property/prices/taxation: direction confirmed but rules open.
- **C-08** "pending" list from the general GDD became obsolete against the specialized GDDs.
- **Diplomacy truncated** in the source (see above).
