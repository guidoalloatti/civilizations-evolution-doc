> **Document control:** 0.1 | Status: conceptual design; decisions marked as confirmed are preserved from the source. **Scope:** this GDD is the detail source for its domain; the Master GDD establishes the shared vision. Cross-system conflicts are recorded in AUDIT_REPORT.md and are not resolved implicitly.

---

# GDD — PROCEDURAL WORLD GENERATION
**Version:** 0.1  
**Status:** Consolidated conceptual definition  
**Project:** Historical civilization simulator  
**Playable period:** From the Paleolithic to the beginning of the Industrial Revolution.

---

## 1. Overview

The Procedural World Generation system will be responsible for creating the geographic settings where civilizations develop.

The goal is to generate worlds that are visually appealing, geographically coherent, and diverse enough that each playthrough has its own identity.

Generation must respect geological, climatic, hydrological, and ecological principles. However, realism must not limit the player's freedom: the generation parameters will be widely configurable.

The system must make it possible to create anything from relatively small maps with several continents to enormous worlds with archipelagos, supercontinents, vast oceans, and extreme regions.

It must also allow the use of real historical geographies and the modification of the world before or during a playthrough.

**Fundamental principle:**

The world will not be a decorative backdrop. It will be a dynamic system that conditions the history, economy, culture, technology, and development possibilities of each civilization.

## 2. Generation philosophy

The system will have four principles:

1. **Natural coherence:** geography must respond to logical relationships among topography, water, climate, soils, biomes, and resources.
2. **Configurability:** virtually all relevant parameters will be modifiable.
3. **Diversity:** not all worlds need to be balanced or offer the same opportunities.
4. **Reproducibility:** the same configuration and seed must make it possible to generate the same world.

Generation must not follow a purely random procedure. Each stage will use information from the previous ones to produce coherent results.

## 3. World types and geographic distribution

### 3.1. World format

A configurable system will be used that initially prioritizes flat maps with natural borders.

Different possibilities are considered:

- Maps with natural boundaries and irregular geography.
- Maps with continents and oceans.
- Island and archipelago maps.
- Fragmented maps.
- Maps with large continental landmasses.
- Maps with a high proportion of oceans.
- Other formats that may be incorporated later.

The architecture must allow the evaluation of future alternatives, including the representation of spherical worlds, without this being a requirement of the MVP.

### 3.2. Landmass distribution

All parameters will be configurable:

- Number of continents.
- Size and shape of each continent.
- Number of islands.
- Size of archipelagos.
- Distribution of landmasses.
- Distance between continents.
- Land-to-water ratio.
- Existence of supercontinents.
- Geographic fragmentation.
- Distribution of large oceans and inland seas.

Generation must produce natural, varied configurations, avoiding artificial patterns that are too obvious.

## 4. Geology and relief formation

### 4.1. Geological principles

The relief will be generated through geological processes simulated in a simplified but coherent manner.

The following are considered:

- Formation of mountain ranges.
- Elevations and depressions.
- Basins.
- Plateaus.
- Valleys.
- Plains.
- Hills.
- Volcanoes.
- Faults and geological zones.
- Coastal mountainous regions.

Simulating plate tectonics in real time is not required. The goal is to reproduce plausible geographic results.

The intensity and variety of these elements will be configurable.

### 4.2. Relief and topography

The world will have a continuous topography, not a collection of isolated zones.

Each region may present different elevations and slopes.

Generation must consider:

- Altitude.
- Slope.
- Terrain orientation.
- Valleys and natural passes.
- Mountain barriers.
- Drainage basins.
- Plains and plateaus.
- Transitions between different types of relief.

The relief will have direct consequences for construction, agriculture, transport, settlements, defense, and territorial expansion.

## 5. Hydrology: rivers, lakes, and oceans

### 5.1. Rivers

Rivers will be generated from the topography and climatic conditions.

The system will consider:

- Headwaters in mountains, springs, and wetlands.
- Tributaries.
- Drainage basins.
- Main and secondary courses.
- River mouths.
- Deltas.
- Estuaries.
- Seasonal variations in flow.
- Floods.
- Navigability depending on depth, flow, and course characteristics.

Rivers must follow geographically coherent paths, avoiding courses that ignore elevations and slopes.

### 5.2. Lakes and wetlands

Lakes of different sizes and characteristics will be generated:

- Mountain lakes.
- Glacial lakes represented as landforms.
- Basin lakes.
- Volcanic lakes.
- Lakes connected to rivers.
- Lakes with no outlet to the sea.
- Marshes and wetlands.

Lakes may connect with river systems and have consequences for settlements, fishing, agriculture, and transport.

### 5.3. Seas and coasts

The following are considered:

- Oceans.
- Inland seas.
- Gulfs.
- Bays.
- Peninsulas.
- Estuaries.
- Fjords.
- Deltas.
- Reefs.
- Rocky coasts.
- Beaches.
- Coastal islands.

The underwater relief will also be represented, including variable depths and continental shelves.

This will allow coasts to have different characteristics and affect navigation, fishing, ports, and maritime trade.

## 6. Climate and biomes

### 6.1. Climate generation

Climate will be determined primarily by:

- Latitude.
- Altitude.
- Temperature.
- Precipitation.
- Humidity.
- Proximity to the sea.
- Relief.
- Winds and regional conditions.

Seasonal and regional variations are considered.

Not all regions will have four seasons. For example, tropical regions may present wet and dry seasons, while polar regions will have their own dynamics.

The system will leverage the climate model already defined in the Geography, Climate, and Environment GDD.

### 6.2. Biomes

Biomes will be distributed taking climatic and geographic conditions into account.

They will include:

- Temperate forests.
- Boreal forests and taigas.
- Tropical rainforests.
- Savannas.
- Grasslands.
- Steppes.
- Hot deserts.
- Cold deserts.
- Tundras.
- Wetlands.
- Mediterranean regions.
- Alpine regions.

Biomes will have natural transitions. They must not appear as perfectly delimited geometric blocks.

The relationship between climate and biomes will be realistic up to a point, allowing special configurations and controlled exceptions.

### 6.3. Flora and fauna

Each biome may include characteristic species and natural resources:

- Trees and vegetation.
- Food plants.
- Medicinal plants.
- Herbivorous animals.
- Predators.
- Migratory fauna.
- Domesticable species.
- Particular natural resources.

Fauna and flora must integrate with the environment, economy, and domestication systems.

## 7. Soils and fertility

Soils will have their own characteristics that influence productive possibilities.

The following are considered:

- Alluvial soils.
- Volcanic soils.
- Fertile soils.
- Poor soils.
- Sandy soils.
- Clay soils.
- Mountain soils.
- Wet soils.
- Arid soils.

Each region may have different levels of fertility, water retention capacity, and suitability for certain crops.

Fertility will directly influence:

- Agricultural yield.
- Livestock farming.
- Crop selection.
- Need for irrigation.
- Exploitation techniques.
- Economic value of territories.

Soil degradation and recovery will be linked to the environmental system.

## 8. Natural resources

### 8.1. Geological and environmental distribution

Resources will be distributed according to geological, climatic, and ecological conditions.

The following are considered:

- Metallic minerals.
- Stone and building materials.
- Coal and other pre-industrial energy resources.
- Wood.
- Forest resources.
- Water.
- Fishing resources.
- Fauna.
- Wild plants and products.
- Potential agricultural resources.
- Other natural resources that may be incorporated.

The distribution will be neither uniform nor completely random.

### 8.2. Abundance and scarcity

The player will be able to configure:

- General abundance.
- Mineral abundance.
- Forest resources.
- Soil fertility.
- Fishing resources.
- Water availability.
- Rare resources.

Resources may be concentrated in certain regions, creating economic opportunities and differences between civilizations.

The already established resource model will be respected: minerals are limited, and renewable resources can also be depleted through overexploitation.

## 9. Initial placement of civilizations

### 9.1. Automatic generation

Initial civilizations will appear automatically in appropriate geographic locations.

The system will evaluate:

- Water availability.
- Access to food.
- Soil fertility.
- Climate.
- Natural resources.
- Geographic protection.
- Expansion possibilities.
- Access to land and sea routes.

The initial location will be a recommendation generated by the system, not a guarantee of success.

### 9.2. Configuration

The player will be able to choose:

- Random distribution.
- Distribution across habitable regions.
- Appropriate distribution with geographic diversity.
- Manual placement.
- Combinations of automatic and manual placement.

The system will be able to recommend initial locations and explain their advantages and disadvantages.

### 9.3. Geographic balance

Balance between civilizations will be optional.

The player will be able to create worlds where all civilizations have relatively similar initial conditions, or highly unequal worlds.

An equitable distribution of resources, fertility, climate, or accessibility must not be forced.

Geographic inequalities may generate different stories and particular challenges.

## 10. Exploration and knowledge of the world

The system will have three distinct states:

1. **Unknown:** the civilization does not have sufficient information about the region.
2. **Known:** general information exists about the territory, obtained through exploration, accounts, contacts, or cartography.
3. **Explored and mapped:** the region has been investigated and more precise geographic information exists.

The level of knowledge will depend on the game setup options and the available technology.

The player will be able to configure:

- Fully visible world.
- Only initial territory known.
- Initial territory and nearby regions.
- Progressive exploration.

Exploration will integrate with the population, technology, transport, and diplomacy systems.

## 11. Historical maps

The system will allow the use of geographies based on real regions.

Maps will be considered for:

- Europe.
- The Mediterranean.
- The Middle East.
- North Africa.
- Asia.
- Africa.
- The Americas.
- Other historical regions.

These maps must represent recognizable geographies and allow different configurations of initial civilizations.

Historical maps will not require reproducing historical events. They will be geographic settings for developing alternative histories.

Geographic accuracy and level of detail may vary by scenario.

## 12. World scale and dimensions

The size will be configurable.

The following are considered:

- Total surface area.
- Geographic resolution.
- Number of continents.
- Number of islands.
- Distance between landmasses.
- Land-to-ocean ratio.
- Number of regions.
- Number of civilizations.
- Resource density.
- Level of detail.

Preset configurations and advanced options will be offered.

The architecture must accommodate large-scale maps without requiring the entire geography to have the same level of detail or simulation.

## 13. Seeds and reproducibility

Each generation will be able to use a seed (*seed*).

This will allow:

- Exactly reproducing a world.
- Sharing worlds between players.
- Comparing different configurations.
- Regenerating scenarios.
- Running balance tests.
- Facilitating technical testing.

The same seed and configuration must produce reproducible results.

## 14. Visual quality and performance

Visual quality will be a high priority, without losing sight of the possibility of generating large maps.

The aim will be to represent:

- Natural relief.
- Recognizable mountains and valleys.
- Varied coasts.
- Rivers and lakes integrated into the terrain.
- Transitions between biomes.
- Characteristic vegetation.
- Visual differences between regions.

The geography must be visually convincing from different heights and scales.

The system will use levels of detail and optimization strategies to prevent the size of the world from compromising performance.

Visual representation and geographic simulation must be decoupled: a region may retain a simplified simulation when it is not within the player's visual area.

## 15. World editor

The world editor will be an independent module.

It will allow modifying the world before starting a playthrough.

Among the planned tools:

- Create or modify mountains.
- Raise or lower terrain.
- Modify valleys and plateaus.
- Create and remove lakes.
- Modify rivers.
- Create islands and archipelagos.
- Modify coasts.
- Change biomes.
- Modify resources.
- Change the initial placement of civilizations.

The editor must allow regenerating certain layers when possible, without having to rebuild the entire world.

For example, modifying a river might require updating the hydrology and certain related elements, but not necessarily regenerating the distribution of continents.

### 15.1. Modification during the playthrough

The player will also have some tools to modify the terrain during a playthrough.

These options will not replace the full editor.

They must integrate with the construction, economy, and environment systems, taking into account the consequences of each intervention.

Some examples:

- Modify terrain for construction.
- Create canals.
- Drain certain wetlands.
- Modify watercourses through engineering works.
- Build hydraulic infrastructure.
- Carry out engineering interventions on the relief.

The modifications will have technological requirements, materials, costs, and environmental consequences where applicable.

Availability will depend on the era and the knowledge of the civilization.

## 16. Configuration and complexity

Three main levels will be offered:

**Simple**
- Automatic generation.
- Preset configurations.
- Basic parameters.
- Simplified exploration.
- Few intervention options.

**Intermediate**
- Configuration of continents, resources, biomes, and civilizations.
- Control of abundance and fertility.
- Exploration configuration.
- Additional generation options.

**Advanced**
- Detailed control of geographic parameters.
- Configuration of resources and distribution.
- Manual placement of civilizations.
- Advanced editing tools.
- Greater control over the generation processes.

The modules may also be enabled or disabled individually when possible.

## 17. Integration with other systems

Procedural generation will not be an isolated system.

The geographic information will be used by the other systems in the game.

| System | Information received |
|---|---|
| Construction and architecture | Relief, slopes, soils, water, and materials |
| Economy | Resources, fertility, accessibility, and distances |
| Production and labor | Available resources and regional conditions |
| Transport and logistics | Relief, rivers, coasts, roads, and obstacles |
| Population and society | Climate, food, water, and habitability conditions |
| Technology | Resources, needs, and development opportunities |
| Culture and identity | Biomes, environment, resources, and geographic isolation |
| Religion | Landscapes, natural places, and regional characteristics |
| Politics and government | Territories, natural borders, and resources |
| Diplomacy | Accessibility, proximity, and connections between civilizations |
| Environment | Ecosystems, species, resources, and climatic conditions |
| War | Terrain, passes, rivers, coasts, and strategic positions |

Geography will also condition the historical development of each civilization.

For example, a civilization located in a mountainous region will have different opportunities and limitations than one settled on a large fertile plain or on an archipelago.

## 18. Conceptual generation architecture

Generation will be organized into dependent layers.

**Conceptual order:**

1. Distribution of landmasses and oceans.
2. Geology and relief.
3. Hydrology.
4. Climate.
5. Biomes.
6. Soils and fertility.
7. Natural resources.
8. Initial placement of civilizations.
9. Exploration and initial knowledge.

Each layer will use information from the previous ones.

This will make it possible to maintain coherence and facilitate partial modification of the world.

For example, changing the distribution of continents will require regenerating much of the later layers, whereas modifying mineral abundance should not require rebuilding the topography.

The concrete technical implementation remains pending the architecture and performance decisions.

## 19. MVP — First playable version

The MVP does not need to implement all of the geographic complexity.

It must make it possible to generate a functional world on which the first civilization systems can develop.

### Initial functionality

- Procedural generation of flat maps with natural borders.
- Distribution of landmasses and oceans.
- Elevation and basic relief.
- Plains, hills, and mountains.
- Basic lakes and rivers.
- Initial biomes.
- Temperature and seasons.
- Resource distribution.
- Basic terrain fertility.
- Initial placement of civilizations.
- Seed configuration.
- Basic size and distribution parameters.

### Outside the MVP

- Detailed historical maps.
- Advanced geology.
- Complex underwater relief.
- Advanced hydrological simulation.
- A wide variety of species.
- Full world editor.
- Advanced tools for modification during the playthrough.
- Extremely detailed geographic configuration.

These elements must be taken into account from the design stage to avoid having to rebuild the system later.

## 20. Final vision

The ultimate goal is for each world to have its own geography, with opportunities, limitations, and characteristics that deeply influence the development of civilizations.

A world with great rivers, fertile lands, and inland seas should produce different opportunities than one made up of mountains, deserts, and archipelagos.

The aim is not for the player to simply receive a random map. The aim is to generate a coherent geographic environment, capable of sustaining thousands of years of alternative history.

**Design principle:**

*Geography will not inevitably determine the fate of a civilization, but it will deeply condition the opportunities it has to build that fate.*

---

## 21. Confirmed decisions

| Area | Decision |
|---|---|
| World type | Configurable; initially flat maps with natural borders |
| Continents and islands | Fully configurable distribution |
| Geology | Realistic and configurable |
| Relief | Geographically coherent |
| Rivers and lakes | Generation based on topography and hydrology |
| Coasts and oceans | Great diversity of formations |
| Biomes | Determined by geographic conditions, with some flexibility |
| Resources | Realistic distribution |
| Soils | Differentiated by origin and characteristics |
| Abundance | Fully configurable |
| Civilizations | Automatic placement with recommendations |
| Initial distribution | Configurable, including manual placement |
| Balance | Optional |
| Exploration | Configurable, with knowledge states |
| Historical maps | Yes |
| Scale | Configurable via presets and advanced options |
| Seeds | Yes |
| Visual quality | High priority with optimization |
| World editor | Independent module |
| Modification during the playthrough | Limited integrated tools |
| Complexity | Simple, intermediate, and advanced |

**Status:** Conceptual GDD v0.1 consolidated. Ready to integrate with the overall game design and evolve into a technical specification when appropriate.
