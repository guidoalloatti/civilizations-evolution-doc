# Civilization Evolutions — GDD Maestro

**Document version:** 1.0 (editorial consolidation)  
**Status:** Central design reference; scope decisions and several specifications remain pending  
**Canonical language:** Spanish  
**Reference period:** Paleolithic to the beginning of the Industrial Revolution (exact date pending; the documents use approximately 1800 AD)  
**Initial platform:** PC (Windows), per the original GDD  
**Proposed engine:** Unreal Engine 5; the detailed technical architecture is kept in the [Technical plan](../02-technical/TECHNICAL_PLAN_v0.1.md).

> This document establishes the shared vision, scope, vocabulary and authority map. The linked system GDDs are the detail source for their domains. The "confirmed" labels reproduce decisions that the source documents already marked as such; the authorship/approval date is not recorded in the available material.

## 1. Vision

Civilization Evolutions is a real-time historical strategy and simulation game in which an initial community transforms over centuries into a civilization with its own settlements, economy, institutions, culture and history. Real history serves as a plausibility reference, not as a mandatory sequence. The player can guide an alternative trajectory through free construction and decisions across several systems.

The experience prioritizes the visible evolution of the world and the interdependence between population, production, resources, geography, knowledge and institutions. War is relevant, but secondary to the civil and social simulation. The population and the non-player civilizations retain their own agency, balanced against broad player intervention.

Inspirations recorded in the initial GDD: Anno 1800, Farthest Frontier, Age of Empires, Civilization II and Cities: Skylines. They are references for specific aspects; they do not establish copy requirements.

## 2. Design pillars

1. Long-term historical evolution with non-linear paths.
2. Free construction and cities that preserve traces of their history.
3. A connected economy: resources, production, consumption, labor and transport.
4. A dynamic society: population, culture, religion, politics and institutions.
5. Strategic freedom with no single optimal route.
6. Geography with consequences for development.
7. Configurable management complexity.
8. Simulation scalable between individual and aggregate detail.

## 3. Scope and experience

The design horizon begins in the Paleolithic and reaches the beginning of the Industrial Revolution. The source texts alternate between "approximately 1800 AD" and "beginning of the Industrial Revolution"; their exact relationship remains pending (see the [audit](../../AUDIT_REPORT.md)). The progression document contemplates an optional historical limit and the possibility of continuing in free mode. Continuity beyond that point does not imply that the Industrial Revolution is designed as a playable stage.

The main intended mode is sandbox, with optional objectives, scenarios and challenges. War does not by itself define victory nor constitute the main axis. Multiplayer falls outside the initial scope in the original GDD.

### First prototype

The initial GDD proposes testing a small loop: a procedural 3D world with terrain, water, forests and resources; construction of dwellings, storehouses, fields and productive buildings; an individual population of 20–100 inhabitants; jobs, needs and basic movement; agriculture, gathering, storage, production and consumption; first discoveries; day/night cycle, seasons, pause and acceleration. The success criterion is that the player founds a village, assigns jobs, produces and distributes food, grows the population, researches and observes visible improvements. Each system may require its own MVP; any difference in sequence or scope must be recorded as prototype-dependent, not as a change of vision.

## 4. Design decisions confirmed in source documents

They are kept as confirmed because they appear under "Confirmed decisions" sections or equivalents in the system GDDs. No record was found of who approved them or when; this traceability should be added in future revisions.

- **Construction:** free placement with an optional grid; construction work with workers and materials; architectural evolution chosen by the player and the possibility of preserving old buildings.
- **Population:** a hybrid model of individuals, families and groups; the level of detail changes with scale and configuration.
- **Economy and production:** interdependent chains; automatic labor assignment with intervention; mixed property and configurable complexity.
- **Transport and trade:** physical movement of resources from the start, with simplified simulation; trade with basic, intermediate and advanced management levels.
- **History and progress:** flexible, non-linear evolution; periods as references, not as mandatory unlocks.
- **Geography:** configurable world and complexity; geography conditions settlements and opportunities.
- **Other civilizations:** configurable autonomy, with their own relationships and trajectories.
- **War:** broad consequences; significant but secondary importance.
- **Interface:** the 3D world is the main interface, complemented by panels and maps; accessibility is a priority.
- **End of game:** optional historical limit, possible continuation and a final decision left to the player.

The complete table by system remains in each specialized GDD. If one confirmation entry appears to contradict another, the conflict is recorded in the audit until a decision is received from the designer.

## 5. Document authority and the limits of each GDD

| Domain | Detail source |
|---|---|
| Vision, pillars, global scope and cross-cutting prototype | This GDD Maestro |
| Economy, markets, consumption, economic property and taxes | [Economy](../01-systems/GDD_Economy_v0.2.md) |
| Productive buildings, recipes, productivity, employment and quality | [Production and labor](../01-systems/GDD_Production_and_Labor_v0.2.md) |
| Construction work, building materials, urban planning, architecture and heritage | [Construction and architecture](../01-systems/GDD_Construction_and_Architecture_v0.1.md) |
| Individuals, families, health, education and social structure | [Population and society](../01-systems/GDD_Population_and_Society_v0.1.md) |
| Government, laws, institutions and internal politics | [Politics and government](../01-systems/GDD_Politics_and_Government_v0.1.md) |
| Religious beliefs, practices and institutions | [Religion and spirituality](../01-systems/GDD_Religion_and_Spirituality_v0.1.md) |
| Identity, language, traditions and cultural diffusion | [Culture](../01-systems/GDD_Culture_Identity_and_Traditions_v0.1.md) |
| Science, knowledge, innovation and technology | [Technology](../01-systems/GDD_Technology_Science_and_Innovation_v0.1.md) |
| Transport, trade, logistics networks and physical markets | [Transport, trade and logistics](../01-systems/GDD_Transport_Trade_and_Logistics_v0.1.md) |
| International relations, treaties and diplomacy | [Diplomacy](../01-systems/GDD_Diplomacy_and_International_Relations_v0.1.md) |
| Geography, climate, ecosystems and environmental processes | [Geography and environment](../01-systems/GDD_Geography_Climate_and_Environment_v0.1.md) |
| Initial map generation and the world editor | [World generation](../01-systems/GDD_Procedural_World_Generation_v0.1.md) |
| Exploration, cartography, colonization and expansion | [Exploration](../01-systems/GDD_Exploration_and_Territorial_Expansion_v0.1.md) |
| Armed forces, conflicts and war | [Military systems](../01-systems/GDD_War_and_Military_Systems_v0.1.md) |
| Events, memory and chronology | [Events and history](../01-systems/GDD_Events_and_Historical_Evolution_v0.1.md) |
| Regions, administration and territorial autonomy | [Territorial administration](../01-systems/GDD_Territorial_Administration_and_Regions_v0.1.md) |
| Controls, panels, visualization and accessibility | [UI/UX interface](../01-systems/GDD_Interface_UI_UX_v0.1.md) |
| Objectives, difficulty and ending | [Progression](../01-systems/GDD_Progression_and_Ending_v0.1.md) |
| Autonomous decisions of non-player civilizations | [AI and autonomy](../01-systems/GDD_AI_and_Civilization_Autonomy_v0.1.md) |
| Clock, calendar, scales and cycles | [Time and simulation](../01-systems/GDD_Time_and_Simulation_v0.1.md) |
| Catalog, identity and accounting of materials | [Resources](../01-systems/GDD_Resources_and_Materials_v0.1.md) |
| Identity, relationships and shared memory of world objects | [Entities and relationships](../01-systems/GDD_Entities_and_Relationships_v0.1.md) |
| Existing technical architecture and sequence | [Technical plan v0.1](../02-technical/TECHNICAL_PLAN_v0.1.md), separate from game design |

**Editing rule:** this Maestro summarizes the framework decisions and defers to the owning system. The complete details and tables live in the domain GDD. The other systems describe their input/output contracts, but do not redefine concepts whose authority belongs to another GDD. No proposal becomes a confirmed decision merely by appearing in this summary.

## 6. Integrations between systems

| Interaction | Document contract that must be maintained |
|---|---|
| Geography ↔ world generation ↔ exploration | Geography describes the world and its processes; generation instantiates its initial state; exploration controls knowledge and cartography. Distinguish the real terrain from the information known to each society. |
| Resources ↔ economy ↔ production ↔ logistics | Resources defines material identities and units; economy defines property, consumption and markets; production transforms inputs into outputs; logistics physically moves inventories. Reuse a shared resource, with no copies per system. |
| Population ↔ entities ↔ labor ↔ economy | Entities maintains identity and relationships; population defines social and demographic dynamics; production assigns people to employment; economy defines compensation and consumption. The aggregate model must stay coherent with the total population. |
| Time ↔ all systems | The shared clock coordinates cycles with different frequencies. The exact calendar and time scale are not yet settled. |
| Technology ↔ production, construction, war, transport and society | Knowledge/technology enables methods and changes capabilities; availability must not be confused with automatic adoption. The transition rules still belong to each domain. |
| Politics ↔ administration ↔ population ↔ economy | Government defines institutions and policies; administration applies them in territories; population contributes interests and social response; economy reflects taxes and allocation. Delimit who sets each policy. |
| Culture ↔ religion ↔ population ↔ architecture | Related systems, each with its own concepts: culture covers identity and practices; religion covers beliefs and spiritual institutions; population covers affiliation and diversity; construction covers physical manifestation. |
| Diplomacy ↔ AI ↔ war ↔ trade ↔ exploration | Diplomacy owns relationships and treaties; AI selects autonomous actions; war resolves armed conflict; economy/logistics executes exchange; exploration records territorial occupation and knowledge. |
| Events ↔ all systems | Occurrences record changes derived from domain causes; the event system must not create gameplay effects without an owning rule. Memory and chronology consume verifiable facts. |
| Interface ↔ all systems | The interface presents available information/orders; it is not a rules authority. Notifications must stem from simulated changes. |
| Progression ↔ time ↔ all systems | Progression offers optional objectives/limits; time keeps the date; no system should impose a mandatory historical chronology. |

## 7. Canonical vocabulary

- **Settlement:** a local inhabited nucleus; use it for a hamlet/town/city when a specific urban scale is not required.
- **City:** an urban settlement. **Region:** a territorial unit for administration or analysis that may group settlements together.
- **Civilization:** the broadest political and historical society; do not use as a synonym for city or settlement.
- **Resource/material:** resource is the shared economic category; material is reserved for physical matter, especially in construction. A single good keeps one shared identifier and inventory.
- **Production:** transformation or extraction carried out by processes/buildings. **Labor:** the assignment, occupation and skills of people.
- **Transport:** physical movement. **Logistics:** planning, storage and supply. **Trade:** economic exchange; it does not by itself imply instantaneous transport.
- **World generation:** the initial procedural creation. **Geography:** the world's natural state and processes. **Exploration:** the acquisition and circulation of spatial knowledge.
- **Confirmed:** explicitly marked as a confirmed decision in a source document. **Proposed:** a suggested resolution, without approval. **Pending:** requires a decision or specification. **Outside the initial scope:** excluded from the initial scope, not necessarily discarded.
- **MVP/prototype:** a minimal test set. **Final vision:** a long-term desired capability; it does not imply inclusion in the MVP.

## 8. Pending cross-cutting decisions

1. Confirm the cutoff date and define the relationship between "approx. 1800" and "beginning of the Industrial Revolution".
2. Specify the player's formal role in the world (external entity, diegetic representation, authority) and its shared limits across politics, diplomacy and interface.
3. Define playable time scales and the correspondence between simulation time, calendar, seasons and aging.
4. Determine limits of scale, maps, population, simultaneous civilizations and simulation budgets.
5. Specify the initial scope of climate and environment: simulated effects versus primarily visual representation in construction.
6. Agree on the default world profile: coherent topography versus the mention of flat initial maps in world generation.
7. Define a shared model of property/legal ownership for entities, land, buildings, inventories and organizations.
8. Specify the interoperability of currencies, prices, markets, taxes and transfers between settlements.
9. Define thresholds and transition rules between individual and aggregate simulation.
10. Resolve approval traceability: owner, date, version and rationale for each confirmed decision.

See the complete log of conflicts and proposals in [AUDIT_REPORT.md](../../AUDIT_REPORT.md). These points were not resolved during this consolidation.

## 9. Art direction and conceptual sequence

### Art direction

The general GDD proposes a realistic, historically recognizable 3D style, with high detail, without pursuing extreme photorealism or a cartoonish aesthetic. The camera contemplates an isometric perspective, rotation, zoom, a panoramic view and close-ups of buildings, inhabitants and units. The world must visually express the passage of time through changes in architecture, materials, clothing, vehicles, infrastructure, cultivated vegetation and urban landscape. The Construction, Culture and Interface GDDs detail their respective expressions. These indications are inherited design direction; visual production decisions remain subject to iteration.

### Inherited conceptual roadmap

The sequence from the initial GDD is kept as a proposed order, not an approved schedule:

1. **Technical foundation:** UE environment, map/terrain generation, resources, camera and basic construction.
2. **First settlement:** dwellings, citizens, jobs, needs, agriculture, gathering and storage.
3. **Economy:** production, consumption, chains, markets, supply/demand and distribution.
4. **Historical evolution:** technologies, unlocks, evolving buildings, productive improvements and architectural changes.
5. **Expansion:** settlements, infrastructure, trade and regional specialization.
6. **Society:** groups, representatives, culture, religion, institutions and politics.
7. **Living world:** autonomous civilizations, independent economy, diplomacy, international trade and conflicts.
8. **Product preparation:** balance, optimization, animation, sound, interface, tutorials, testing and historical content.

The sequence does not replace the per-system MVPs or the technical plan. Changes in priority or scope must be recorded as a product decision.

## 10. Document structure

- `docs/00-project/`: central vision and documentation governance.
- `docs/01-systems/`: specialized GDDs, one detail source per domain.
- `docs/02-technical/`: existing technical documentation, separate from design.
- Root: audit report and complete backup prior to the reorganization.

For the per-file inventory and the history of this consolidation, see [AUDIT_REPORT.md](../../AUDIT_REPORT.md).
