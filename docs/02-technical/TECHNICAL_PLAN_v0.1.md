> **Document control:** version 0.1 | Original status: Initial implementation. Relocated without altering the content. Technical document separate from game design authority; any architectural change requires authorization. See the GDD Maestro and AUDIT_REPORT.md.

---

# TECHNICAL IMPLEMENTATION PLAN
## Historical Civilization Simulator

**Document:** Technical Setup & Architecture Foundation  
**Version:** 0.1  
**Status:** Initial implementation  
**Engine:** Unreal Engine 5  
**Primary language:** C++  
**Supporting tools:** Blueprints, Blender, Git

---

# 1. PROJECT OVERVIEW

We are developing a historical civilization simulation and city-building game.

The game will simulate the evolution of civilizations from the Paleolithic period until the beginning of the Industrial Revolution (approximately late 18th century).

The main references are:

- Anno 1800
- Farthest Frontier
- Age of Empires
- Civilization II

The game will combine:

- Free-form 3D city construction.
- Individual and aggregated population simulation.
- Complex economic and production systems.
- Historical technological evolution.
- Culture, religion and political systems.
- Logistics, transportation and trade.
- Diplomacy and military systems.
- Procedural world generation.
- Autonomous civilizations and historical events.

The experience should allow players to observe their civilization evolving over hundreds or thousands of simulated years.

**Important:** This project is intended to become a large, modular simulation. The initial implementation must prioritize architecture, maintainability, performance and extensibility over feature quantity.

# 2. TECHNICAL OBJECTIVES

The initial technical implementation must establish:

1. A working Unreal Engine project.
2. A clean C++ architecture.
3. A modular simulation framework.
4. A configurable simulation clock.
5. A basic procedural world.
6. A data-driven resource system.
7. A basic building placement system.
8. A minimal population simulation.
9. A basic production and logistics loop.
10. A reliable development workflow with Git.

We should avoid implementing complex gameplay before the underlying architecture is established.

# 3. TECHNOLOGY STACK

## 3.1. Core engine

**Unreal Engine 5**

Use a stable UE5 release compatible with the available development environment.

Before creating the project, inspect the installed Unreal Engine versions and recommend the most appropriate version.

Do not assume a specific engine version without checking the environment.

## 3.2. Programming

- C++ for core simulation systems.
- Blueprints for visual prototyping, configuration and presentation.
- Unreal Engine reflection system (UCLASS, USTRUCT, UPROPERTY, UFUNCTION) where appropriate.
- Data Assets and Data Tables for configurable game content.

Core simulation logic should not depend unnecessarily on Blueprint execution.

## 3.3. Visual development

- Blender for 3D modeling.
- Substance Painter for materials and texturing.
- Unreal Engine materials and lighting.
- Git for version control.

## 3.4. Development tools

Use the appropriate IDE and C++ toolchain for the detected operating system.

Possible environments:

- Windows: Visual Studio with the required C++ and Unreal development workloads.
- macOS: Xcode and the supported Unreal Engine toolchain.

Claude Code should inspect the existing environment before recommending installations.

# 4. ARCHITECTURAL PRINCIPLES

The architecture must follow these principles.

### 4.1. Modular architecture

Each major gameplay domain should be independent and communicate through clearly defined interfaces and events.

Planned modules:

- Core
- Simulation
- World
- Building
- Population
- Economy
- Production
- Logistics
- Technology
- Society
- Politics
- Culture
- Religion
- Diplomacy
- Military
- UI

Not all modules need to be implemented immediately.

The initial architecture should establish the foundation for these future systems without creating unnecessary empty abstractions.

### 4.2. Data-driven design

Game content should be defined through data whenever practical.

Examples:

- Resources.
- Buildings.
- Production recipes.
- Professions.
- Technologies.
- Population attributes.
- Construction materials.
- Historical periods.

Avoid hardcoding content definitions directly into simulation logic.

Prefer Unreal Data Assets, Data Tables and appropriate C++ structures.

### 4.3. Separation of simulation and presentation

Simulation logic must be independent from visual representation.

For example:

- A building's production should not depend on its 3D mesh.
- A citizen's needs should not depend on an actor being rendered.
- Resource transportation should not require a visible vehicle for every individual operation.
- The UI should read simulation state rather than own it.

The simulation must eventually support large populations and cities without requiring every entity to exist as a fully simulated Actor.

### 4.4. Scalability

The game must support a progression from small settlements with individually simulated inhabitants to large civilizations using aggregated population groups.

The architecture must allow different simulation detail levels depending on population size and system configuration.

### 4.5. Configurable complexity

Simulation systems should support configurable levels of detail.

Initial design:

- Basic.
- Intermediate.
- Advanced.

These settings may eventually be configurable independently for each system.

### 4.6. Historical flexibility

Do not implement a rigid historical progression.

Technology, buildings, production methods and social systems must be driven by conditions, knowledge and available resources rather than simply by the current historical year.

# 5. PROPOSED PROJECT STRUCTURE

Start with a clean project structure.

```text
CivilizationSimulator/
│
├── Config/
│
├── Content/
│   ├── Core/
│   ├── Maps/
│   ├── Buildings/
│   ├── Resources/
│   ├── Population/
│   ├── UI/
│   ├── Materials/
│   ├── Meshes/
│   └── Data/
│
├── Source/
│   └── CivilizationSimulator/
│       │
│       ├── Core/
│       │   ├── GameInstance/
│       │   ├── GameMode/
│       │   └── Configuration/
│       │
│       ├── Simulation/
│       │   ├── SimulationManager/
│       │   ├── SimulationClock/
│       │   ├── SimulationEvents/
│       │   └── SimulationSubsystems/
│       │
│       ├── World/
│       │   ├── WorldGenerator/
│       │   ├── Terrain/
│       │   └── WorldData/
│       │
│       ├── Buildings/
│       │   ├── BuildingBase/
│       │   ├── BuildingData/
│       │   └── BuildingPlacement/
│       │
│       ├── Economy/
│       │   ├── Resources/
│       │   ├── Production/
│       │   └── Storage/
│       │
│       ├── Population/
│       │   ├── PopulationManager/
│       │   ├── Citizen/
│       │   └── PopulationGroups/
│       │
│       ├── Logistics/
│       │   ├── Transportation/
│       │   ├── Inventory/
│       │   └── ResourceDistribution/
│       │
│       └── UI/
│
├── Plugins/
│
├── Tests/
│
├── Docs/
│
├── .gitignore
├── README.md
└── CivilizationSimulator.uproject
```

This structure is a starting point, not a requirement to create every directory immediately.

Use Unreal Engine module conventions and avoid unnecessary fragmentation into multiple Unreal modules until there is a concrete reason.

# 6. CORE SIMULATION ARCHITECTURE

The simulation should be the central foundation of the game.

## 6.1. Simulation Manager

Create a central simulation coordinator responsible for:

- Registering simulation systems.
- Managing simulation updates.
- Controlling simulation speed.
- Pausing and resuming simulation.
- Coordinating simulation phases.
- Broadcasting relevant events.

Avoid placing all gameplay logic inside one enormous manager class.

The manager should coordinate systems, not own every system's internal logic.

Use Unreal subsystems where appropriate.

## 6.2. Simulation Clock

Create a configurable simulation clock.

Required features:

- Pause.
- Resume.
- Normal speed.
- Increased speed.
- Multiple acceleration levels.
- Configurable simulation tick interval.
- Day, month and year progression.
- Season changes.
- Historical year tracking.

The clock should be independent from the rendering frame rate.

Gameplay systems should not rely on Tick() for every simulation operation.

Use controlled simulation intervals.

The initial time scale can be configurable. Do not assume a fixed number of real-world seconds per simulated day.

## 6.3. Simulation events

Implement a lightweight event architecture.

Examples:

- ResourceProduced.
- ResourceConsumed.
- BuildingConstructed.
- BuildingUpdated.
- PopulationChanged.
- TechnologyDiscovered.
- SeasonChanged.
- YearChanged.

Systems should be able to react to relevant events without tightly coupling themselves to one another.

Use typed events or delegates where appropriate.

Avoid an unnecessarily generic event framework.

# 7. WORLD FOUNDATION

Create a basic procedural world foundation.

The initial version should support:

- Configurable map dimensions.
- Terrain generation.
- Height variation.
- Basic terrain types.
- Water areas.
- Walkable and non-walkable areas.

The first implementation does not need sophisticated procedural generation.

It must, however, be designed to accommodate future additions:

- Continents.
- Islands.
- Archipelagos.
- Rivers.
- Lakes.
- Mountains.
- Biomes.
- Natural resources.
- Climate.
- Historical maps.

World generation must be reproducible using a configurable random seed.

The same seed and configuration should generate the same initial world.

# 8. BUILDING SYSTEM

Create a modular building foundation.

Buildings should have a logical representation separate from their visual Actor.

A building should eventually support:

- Unique identifier.
- Building type.
- Position.
- Orientation.
- Ownership.
- Construction state.
- Construction materials.
- Workers.
- Storage.
- Production configuration.
- Maintenance.
- Condition.
- Historical evolution.

For the initial implementation, only the essential fields are required.

Create a basic building placement system with:

- Grid or configurable placement rules.
- Valid/invalid placement feedback.
- Rotation.
- Construction confirmation.
- Basic resource validation.

The architecture should eventually support free-form modular construction and building evolution.

# 9. RESOURCE SYSTEM

Implement a data-driven resource system.

A resource definition should support:

- Unique ID.
- Display name.
- Category.
- Description.
- Icon.
- Stack or quantity rules.
- Optional storage properties.

Initial resources:

- Wood.
- Stone.
- Berries.
- Meat.
- Wheat.
- Flour.
- Bread.

Resources should be referenced by stable identifiers rather than relying on display names.

Inventory and resource quantities should use appropriate numeric types and validation.

# 10. BASIC PRODUCTION SYSTEM

Implement a minimal recipe-based production framework.

A production recipe should define:

- Input resources.
- Output resources.
- Production duration.
- Required building type.
- Optional workforce requirements.

Initial production chain:

```text
Wheat
  |
  v
Mill
  |
  v
Flour
  |
  v
Bakery
  |
  v
Bread
```

The production system must consume inputs and generate outputs through simulation updates.

Production should not happen if required resources are unavailable.

Do not implement complex worker skills, quality, maintenance or historical production evolution in this first iteration.

However, avoid architectural decisions that prevent these features from being added later.

# 11. BASIC POPULATION SYSTEM

Implement a minimal population foundation.

The initial scenario should contain approximately 30 inhabitants.

For the first prototype, citizens may be represented by lightweight data structures.

Do not create a fully detailed Actor for every citizen.

A citizen data structure should eventually support:

- Unique ID.
- Age.
- Family.
- Profession.
- Workplace.
- Basic needs.
- Health.
- Skills.

Only implement the fields needed for the initial prototype.

The architecture must support future aggregation into population groups and social classes.

Visual representation of individual citizens can be introduced progressively.

# 12. BASIC LOGISTICS

Implement a simplified but functional physical resource distribution system.

Example:

1. A resource is produced at a building.
2. It enters that building's inventory.
3. A transport request is created.
4. The logistics system identifies a destination.
5. Resources are transferred.
6. The destination inventory is updated.

For the first prototype, transportation can be abstracted.

Do not attempt to simulate individual carts, animals or transport workers yet.

The important requirement is that resources cannot be consumed at a destination unless they have actually been delivered there.

The architecture must later support distance, transport costs, routes, vehicles, capacity and logistics workers.

# 13. USER INTERFACE FOUNDATION

Create a minimal debug-oriented UI.

It should display:

- Current simulated date.
- Simulation speed.
- Pause/resume controls.
- Population.
- Available resources.
- Building information.
- Basic production status.

The UI should be simple and functional.

Do not invest heavily in final visual design during the initial setup.

Use the UI to validate that simulation systems work correctly.

# 14. DEVELOPMENT CONFIGURATION

Create a development configuration that allows:

- Changing initial population.
- Changing map size.
- Setting the world generation seed.
- Configuring simulation speed.
- Enabling or disabling basic simulation systems.
- Configuring initial resources.

Prefer configuration assets or settings objects rather than hardcoded values.

# 15. TESTING AND QUALITY

Create a basic testing strategy from the beginning.

Tests should cover:

- Simulation clock progression.
- Pause and resume.
- Resource creation.
- Resource consumption.
- Inventory limits.
- Production input validation.
- Production output generation.
- Basic logistics transfers.
- Building placement validation.

Use Unreal's automation testing framework where appropriate.

Keep simulation logic testable without requiring complex visual scenes.

# 16. VERSION CONTROL

Initialize Git before implementing substantial gameplay.

Use a suitable Unreal Engine .gitignore.

Important considerations:

- Do not commit Intermediate/.
- Do not commit Saved/.
- Do not commit DerivedDataCache/.
- Do not commit local IDE files.
- Do commit project source, configuration and required assets.
- Do commit project documentation.

Git LFS should be considered for large binary assets.

Do not introduce unnecessary external dependencies.

# 17. INITIAL DEVELOPMENT MILESTONES

Implement the project incrementally.

## Milestone 1 — Environment and project setup

- Inspect local environment.
- Verify Unreal Engine installation.
- Verify C++ toolchain.
- Create the Unreal project.
- Configure source control.
- Create README.
- Verify that the project compiles and launches.

**Acceptance criteria:** A clean Unreal project opens, builds and runs locally.

## Milestone 2 — Core simulation foundation

- Simulation Manager.
- Simulation Clock.
- Pause and speed controls.
- Simulation events.
- Basic system registration.

**Acceptance criteria:** The simulation clock advances independently of rendering FPS and can be paused or accelerated.

## Milestone 3 — World foundation

- Procedural terrain.
- Configurable map size.
- Random seed.
- Basic terrain types.
- Water and land.

**Acceptance criteria:** The game can generate and display a reproducible basic world.

## Milestone 4 — Building foundation

- Building data.
- Building placement.
- Basic construction validation.
- Building selection.
- Simple visual representation.

**Acceptance criteria:** The player can place and inspect basic buildings.

## Milestone 5 — Economy prototype

- Resource definitions.
- Inventories.
- Production recipes.
- Resource consumption.
- Resource creation.

**Acceptance criteria:** A production chain can function correctly.

## Milestone 6 — Population and logistics prototype

- Initial population.
- Basic workforce allocation.
- Resource delivery.
- Building supply.

**Acceptance criteria:** A small settlement can produce, transport, store and consume resources.

# 18. IMPORTANT DEVELOPMENT RULES FOR CLAUDE CODE

Follow these rules throughout implementation.

1. Inspect the existing repository and local environment before making changes.
2. Do not overwrite existing work without understanding it.
3. Do not assume Unreal Engine, IDE or compiler versions.
4. Prefer native Unreal Engine capabilities over unnecessary third-party dependencies.
5. Use C++ for core simulation logic.
6. Keep presentation separate from simulation.
7. Prefer data-driven definitions.
8. Avoid monolithic classes.
9. Avoid premature abstractions and overengineering.
10. Do not implement systems that have not been requested.
11. Do not invent gameplay rules that have not been defined.
12. Document architectural decisions.
13. Compile and test after each meaningful milestone.
14. Keep the project buildable throughout development.
15. Ask for approval before installing software, modifying system-level configuration or performing destructive operations.

When a technical decision has multiple viable options, explain the tradeoffs and recommend one before introducing unnecessary complexity.

# 19. FIRST TASK FOR CLAUDE CODE

Start by inspecting the local development environment.

Determine:

- Operating system.
- Available disk space.
- Installed Unreal Engine versions.
- Available C++ compiler and toolchain.
- Available IDE.
- Git installation and configuration.
- Existing project or repository state.

Then provide a concise environment assessment.

Identify missing prerequisites and recommend the exact setup steps.

Do not install software or make system-level changes without approval.

After the environment assessment, propose the initial project creation and Git configuration.

Once approved, implement Milestone 1.

Do not proceed automatically through all milestones.

# 20. FINAL TECHNICAL VISION

The project should evolve into a large-scale historical civilization simulation.

Its architecture must allow independent systems to interact through shared data, simulation events and well-defined interfaces.

The most important technical principle is:

**The game world must be simulated as a coherent system, not as a collection of disconnected gameplay features.**

A technological discovery should eventually influence production, construction, transportation, economy, society and other relevant systems.

A drought should affect agriculture, food availability, prices, population and potentially political stability.

A new transportation technology should change the efficiency of trade and the development of cities.

The architecture should make these interactions possible without requiring every system to know the internal implementation of every other system.

The first objective is not to build the entire game.

It is to establish a reliable technical foundation on which the entire game can be built.
