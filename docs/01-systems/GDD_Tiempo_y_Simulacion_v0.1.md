> **Document control:** 0.1 | Status: conceptual design; decisions marked as confirmed are preserved from the source. **Scope:** this GDD is the detail source for its domain; the Master GDD establishes the common vision. Cross-system conflicts are recorded in AUDIT_REPORT.md and are not resolved implicitly.

---

# GDD — Time and Simulation System

**Version:** 0.1  
**Status:** Conceptual design

## 1. Overview

The game will use a continuous time model combined with internal simulation cycles.

There will be a global clock that determines the passage of the world's time. The different systems will use that clock to run their processes at independent frequencies and precisions.

The goal is to simulate a civilization over centuries without needing to process all its elements at the same frequency.

Time will be a fundamental dimension of the experience: it will affect population, economy, production, agriculture, climate, technology, culture, politics and historical events.

## 2. Global clock

The game will have a central clock that represents the time elapsed.

This clock must allow:
- Advancing continuously.
- Pausing.
- Speeding up and slowing down.
- Keeping all systems synchronized.
- Recording historical dates and events.
- Running periodic processes.
- Managing configurable calendars.

The systems will not have independent clocks that could generate inconsistencies.

## 3. Calendar

The calendar will be configurable.

It may use:
- Simulation seconds and minutes.
- Hours and days.
- Weeks.
- Months.
- Seasons.
- Years.
- Decades and centuries.

The precision and presentation will depend on the era and the playthrough configuration.

The calendar will have functional consequences for:
- Agriculture and harvests.
- Climate and seasons.
- Seasonal production.
- Migrations.
- Festivities.
- Religion.
- Trade.
- Taxes and administration.
- Birth rate and mortality.
- Historical events.

Not all regions will necessarily have the same seasons or environmental cycles.

## 4. Game speed

The player will be able to control the speed through:
- Pause.
- Normal speed.
- Accelerated speeds.
- Very accelerated speed.

The number of speeds will be configurable.

There may also be automatic acceleration in the face of relevant events, according to the player's preferences.

Acceleration must not artificially modify the results of the simulation: it will only change the speed at which time passes.

## 5. Internal simulation cycles

Each system will have an appropriate update frequency.

Examples:

- Population: individual processes and aggregate demographic updates.
- Production: work cycles and recipe processing.
- Economy: updating of inventories, supply, demand and prices.
- Agriculture: daily, seasonal and harvest processes.
- Climate: simplified continuous evolution and environmental cycles.
- Technology: research and periodic reviews of opportunities.
- Diplomacy: periodic evaluation and reaction to events.
- Culture: gradual transformations and specific events.
- Politics: institutional processes and changes according to conditions.
- Events: continuous detection of conditions and periodic evaluation.

The frequencies may vary according to the complexity configured and the relevance of each element.

## 6. Population and aging

The population will use a hybrid simulation.

Relevant individuals may have detailed processes:
- Aging.
- Birth.
- Needs.
- Work.
- Relationships.
- Health.
- Migration.
- Death.

As the population grows, certain processes may be aggregated by families, groups, professions or regions.

The transition between individual and aggregate simulation must preserve demographic coherence.

## 7. Production and buildings

Buildings will not need to be updated constantly.

Production processes will run according to:
- Production time.
- Availability of workers.
- Inputs.
- Capacity.
- Transport.
- Building status.
- Environmental conditions.
- Technology.

Relevant events may immediately modify production conditions.

Buildings will retain their state and production progress where applicable, avoiding artificial resets.

## 8. Seasons and natural cycles

Seasons will have configurable effects according to region.

They may influence:
- Temperature and precipitation.
- Crops and harvests.
- Livestock.
- Food availability.
- Transport.
- Navigation.
- Labor activities.
- Diseases.
- Festivities and customs.

Tropical, polar, temperate, Mediterranean and other regions will have different cycles.

The intensity of these effects will depend on the complexity selected.

## 9. Historical events

Events will combine spontaneous processes and periodic evaluations.

They may occur when certain conditions are met or when a system review detects relevant opportunities and situations.

Examples:
- Founding of a city.
- Technological discovery.
- Outbreak of an epidemic.
- Change of government.
- Migration.
- Economic crisis.
- Religious reform.
- War.
- Diplomatic treaty.

Dates will not be predetermined except in specific historical scenarios.

Events must be recorded with their date and consequences.

## 10. Pause and planning

When the player pauses:
- The advance of the world will stop.
- The simulation cycles will stop.
- The current states will be preserved.
- The player will be able to build, plan, consult information and administer.
- Orders may be prepared to execute upon resuming, where applicable.

Pausing must not cause hidden advances in other systems.

## 11. Acceleration and performance

During high speeds the simulation may be simplified.

Two mechanisms are considered:

1. **Simplification of secondary processes:** some calculations may be grouped or run less frequently.
2. **Simulation by relevance:** distant elements, elements of low relevance or without immediate interaction may use aggregate models.

Processes important to the player must retain coherent results.

Acceleration must not cause unexplained demographic, economic or production jumps.

## 12. Configuration

The player will be able to configure:
- Normal speed and accelerations.
- Calendar and time presentation.
- Season precision.
- Update frequencies.
- Individual or aggregate simulation.
- Level of detail during accelerations.
- Complexity per system.

The time and simulation configuration must be separate from the general difficulty.

## 13. Integration

This system will be cross-cutting and connected with all the game's systems.

The global clock will be the common temporal reference for:
- Economy.
- Production.
- Population.
- Construction.
- Agriculture.
- Technology.
- Culture.
- Religion.
- Politics.
- Diplomacy.
- Exploration.
- War.
- Events.
- Territorial administration.
- Civilization AI.

## 14. MVP

First implementation:

- Global clock.
- Basic calendar.
- Pause and three speeds.
- Internal simulation cycles.
- Periodic production updates.
- Basic aging and demographic growth.
- Seasons.
- Event logging.
- Construction and planning during pause.

The goal will be to verify that the systems can advance synchronously over prolonged periods.

## 15. Guiding principle

**Time must feel continuous to the player, but the simulation must be intelligent about when and at what level of detail it needs to update each process.**
