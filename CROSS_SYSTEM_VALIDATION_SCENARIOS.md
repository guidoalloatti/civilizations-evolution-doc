# CROSS_SYSTEM_VALIDATION_SCENARIOS — Civilization Evolutions

> **Phase 2 — Cross-system validation scenarios (design level).** They conceptually check whether the architecture supports the intended simulation. They are **not** automated tests yet. For each scenario: the systems that interact, the shared data, and what counts as correct behavior. Sources: GDDs ([DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)); entities: [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md); flows/events: [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md).
> Version 1.0 · Last updated 2026-11-04 · Status: conceptual.

## How to use
Each scenario is a "path" that the architecture must be able to traverse without duplicating data, without coupling simulation to rendering, and respecting the owner of each decision. They serve to validate the design now and, later, as a basis for integration tests.

---

## Scenario A — Founding a settlement (30 inhabitants)
- **Systems:** Entities, Population, Construction, Production/Labor, Resources, Logistics, Economy, Time, UI.
- **Shared data:** Settlement, Individual×30 (persistent id), Building (housing/productive), Inventory, Resource.
- **Path:** the Settlement is created (Entities) → 30 Individuals with identity → housing and productive buildings are built (Construction, with Resource cost + labor) → workers are assigned (Production) → they gather (wood/stone/berries) → they store (Inventory) → they produce/consume.
- **Correct if:** each individual has id/house/job; the construction work respects costs; gathering fills inventories by location; consumption deducts real stock; everything advances with the clock. **Validates:** housing, population identity, labor, gathering, storage, consumption, production.
- **Risks it stresses:** RISK-01/02/03 (logic in SimCore vs UE; single model; cost of 30 actors).

## Scenario B — Scarcity of a resource
- **Systems:** Resources (natural), Geography, Production, Logistics, Economy, Construction (maintenance), Trade.
- **Shared data:** natural Resource (wood) with quantity/depletion, Inventory, Route, price.
- **Path:** nearby wood runs out (node→0, destroyed/regenerated depending on type) → local gathering fails → Logistics must bring it from farther away (higher cost/time) → Economy raises the price → Production/Construction that depend on wood slow down or look for an **alternative material** → possible trade.
- **Correct if:** depletion propagates to production/price/maintenance; nothing is consumed without delivery; the scarcity signal appears; there is an alternative (another material/import). **Validates:** natural-resource behavior, gathering, transport, trade, prices, maintenance, alternative materials.
- **Risks:** RISK-04 (data-driven materials), RISK-07 (price-recalculation cadence). Note: the destroyed-node bug was already fixed (capture id before `Extract`).

## Scenario C — Technological innovation (metallurgy)
- **Systems:** Technology, Production, Construction, Economy, Military, Trade, Culture, Events. Framework: [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md).
- **Shared data:** Technology/Discovery (id, conditions), `TechnologyDiscovered` event, Recipes, Buildings.
- **Path:** conditions are met (knowledge+ore+specialization) → the discovery emerges → the **shared framework** publishes it → Production enables recipes/tools, Construction new materials, Military new weapons, Economy new goods, Culture visual evolution; Events records it. **Adoption** requires materials+labor+decision (≠ availability).
- **Correct if:** a single discovery triggers effects across several systems **without** Technology implementing the logic of each one; the old coexists; it can go unadopted. **Validates:** discovery, adoption, recipes, tools, construction, trade, social consequences.
- **Risks:** RISK-05 (no scripting; reaction to a common fact).

## Scenario D — Population growth (several generations)
- **Systems:** Population, Entities, Time, Construction, Economy, Production.
- **Shared data:** Individual, Family (lineage) vs Household (residence), Housing, population aggregate.
- **Path:** births/deaths over years → families and households form → housing demand grows → once a threshold is exceeded, part of the population moves from **individual to aggregate** (LOD, DR-10) preserving the identity of what matters → urban growth.
- **Correct if:** demographics are coherent; **family ≠ household** (an individual can belong to a family without sharing a household); aggregating does not lose key historical information; housing/labor/food scale. **Validates:** births, deaths, families, households, housing, labor, food, urban growth, aggregation.
- **Risks:** RISK-03/10 (scale; individual↔aggregate migration without losing identity).

## Scenario E — Regional specialization
- **Systems:** Territorial Administration, Economy, Production, Logistics/Trade, Exploration, Geography.
- **Shared data:** Region, Settlement×2, trade Route, Resource, regional prices.
- **Path:** two settlements with different resources (one agricultural, one mining) → each specializes (Production) → they are connected by a Route (Logistics) → they trade surpluses (Economy) → Administration groups them into a region and sets policies/taxes.
- **Correct if:** the geographic distribution determines the specialization; trade balances supply/demand between settlements; the region administers without micromanagement. **Validates:** resource distribution, production, transport, trade, regional administration, economic relationships.
- **Risks:** RISK-07/09 (cadence; world/market scale).

## Scenario F — Cultural and religious change
- **Systems:** Religion, Culture, Population, Construction, Politics, Diplomacy, Events.
- **Shared data:** Religion/Culture (affiliation), Individual (belief), religious Organization, Building (temple), event.
- **Path:** a religious movement arises → it spreads among individuals/families (affiliation) → cultural identity changes → temples are built → it influences politics (legitimacy) and diplomacy (religious relations) → Events records reforms/schisms.
- **Correct if:** affiliation lives in the individuals (not duplicated), religion leaves a physical (temples) and social (politics/diplomacy) footprint; coexistence can be peaceful or conflictive depending on context. **Validates:** individuals, families, cultural identity, religious institutions, architecture, politics, diplomacy.
- **Risks:** RISK-06 (coupling of Culture/Religion/Population/Politics via events, not direct references).

## Scenario G — Long-term historical evolution (centuries)
- **Systems:** Time, Technology, Construction, Population, Culture, Events, Progression, Economy.
- **Shared data:** Building (historical version + persistent identity), Civilization, historical Record, Technology.
- **Path:** a settlement survives for centuries → the buildings **evolve** (hut→stone house→…) preserving their identity/history → infrastructure and technology change → cultural identity transforms while maintaining continuity → the historical Record accumulates facts and biographies → consequences persist.
- **Correct if:** the same entity (building/settlement/civilization) keeps its id and history through evolutions and the individual↔aggregate change; the chronicle is coherent; the visual evolution of the world is visible. **Validates:** building evolution, infrastructure, technology, population, cultural identity, historical record, persistent consequences.
- **Risks:** RISK-08/10 (long-term persistence/saving; entity migration).

---

## Coverage by system (which scenario exercises it)

| System | A | B | C | D | E | F | G |
|---|---|---|---|---|---|---|---|
| Time | ✓ | | | ✓ | | | ✓ |
| Entities | ✓ | | | ✓ | | ✓ | ✓ |
| Resources | ✓ | ✓ | ✓ | | ✓ | | |
| Geography | | ✓ | | | ✓ | | |
| Construction | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ |
| Population | ✓ | | ✓ | ✓ | | ✓ | ✓ |
| Production | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ |
| Logistics/Trade | | ✓ | ✓ | | ✓ | | |
| Economy | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ |
| Technology | | | ✓ | | | | ✓ |
| Culture/Religion | | | ✓ | | | ✓ | ✓ |
| Politics/Admin | | | | | ✓ | ✓ | |
| Diplomacy | | | | | | ✓ | |
| Events | | | ✓ | | | ✓ | ✓ |
| Progression | | | | | | | ✓ |

**Observation:** scenarios A, B, C, and D cover the heart of the first playable (E1–E2 of the [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md)) and should be able to be "traversed" in design before implementing; E, F, and G validate later stages.
