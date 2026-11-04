# DEVELOPMENT_ROADMAP — Civilization Evolutions

> **Phase 2 — Implementation roadmap.** It separates the project into stages with objectives, required systems, dependencies, expected functionality, acceptance criteria, risks, and **what NOT to implement yet**. It respects the GDDs and the per-system MVPs. It is not an approved schedule. Sources: Technical plan §17, cross-cutting MVP (01_GDD_GENERAL §18, Master §3), GDD sheets.
> Version 1.0 · Status: proposal. Order derived from [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md).

## Stage summary

| Stage | Name | Focus |
|---|---|---|
| E0 | Foundational infrastructure | Time, Entities, Resources, modular skeleton |
| E1 | First playable | Minimal world + village + gathering + production + time |
| E2 | First complete colony | Individual population, needs, logistics, consumption, basic economy |
| E3 | Expanded civilization | Multiple settlements, regional trade, administration |
| E4 | Historical evolution | Technology as a shared framework, building evolution, culture/religion |
| E5 | Advanced systems | Politics, diplomacy, civilization AI, war |
| E6 | Long term | Deep events/history, progression, balance, polish, scale |

> **Current prototype status** (see [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md)): there are pieces of E0–E2 implemented (clock, resources/recipes/production/technology in SimCore; terrain, construction with cost, settlers, and gathering in UE) but with **architectural debt** (gameplay in the UE layer, dual model). E3+ not started.

---

## E0 — Foundational infrastructure
- **Objectives:** modular base with simulation/presentation separation; clock; resource catalog; entity/identity model; typed events.
- **Required systems:** Time, Entities, Resources, simulation/event core.
- **Dependencies:** none (Level 0).
- **Functionality:** advance/pause/accelerate time; define data-driven resources; create entities with a persistent id; emit/listen to events.
- **Acceptance:** the clock advances independently of the FPS; resources/entities are created from data; a system reacts to an event without coupling.
- **Risks:** RISK-01/02/06 (define now where the logic lives and the single source of truth).
- **Do NOT do yet:** market economy, politics, AI, final rendering.

## E1 — First playable (cross-cutting prototype)
- **Objectives:** demonstrate that the base mechanics work together (DR-12).
- **Systems:** minimal Generation/Geography, Construction, Population (small individual), Production, simplified Logistics, basic Economy, basic Technology, minimal UI.
- **Dependencies:** E0.
- **Functionality (GDD goal):** generate a world; start a village; ~30 inhabitants; build housing and productive buildings; assign workers; gather resources; produce and consume; store and transport; advance time; see the village evolve.
- **Acceptance:** a village produces, transports, stores, and consumes; the population grows; something basic is researched and an improvement is seen.
- **Risks:** RISK-01/03 (do not make a heavy Actor per inhabitant; keep the logic testable).
- **Do NOT do yet:** markets with dynamic prices, culture/religion/politics, diplomacy, war, ending.

## E2 — First complete colony
- **Objectives:** social and economic depth of a settlement.
- **Systems:** Population (families, needs, health, basic education), Economy (consumption, mixed ownership, basic currency), physical Logistics, Construction (simple evolution).
- **Dependencies:** E1.
- **Functionality:** families and evolving needs; basic employment/wages; inventories by location; complete production chains; seasons affect agriculture (DR-03).
- **Acceptance:** a stable colony with coherent demographics, labor, and consumption over simulated years.
- **Risks:** RISK-02/08/10 (unify the population model; persistence; LOD transition as it grows).
- **Do NOT do yet:** multiple civilizations, international trade.

## E3 — Expanded civilization
- **Objectives:** several settlements and a regional economy.
- **Systems:** Exploration/Expansion, Territorial Administration, Transport/Trade (regional), Economy (regional markets).
- **Dependencies:** E2.
- **Functionality:** found settlements; group into regions; routes and regional trade; specialization.
- **Acceptance:** two+ settlements connected by trade with regional specialization.
- **Risks:** RISK-07/09 (cadences; world scale).
- **Do NOT do yet:** other autonomous civilizations, war.

## E4 — Historical evolution
- **Objectives:** that the civilization change visibly and functionally over time.
- **Systems:** Technology (shared framework), building evolution, Culture, Religion.
- **Dependencies:** E3 + [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md).
- **Functionality:** discoveries by conditions; buildings that evolve; cultural/religious identity; visual evolution of the world.
- **Acceptance:** the same civilization looks and functions differently after advances; alternative paths are possible.
- **Risks:** RISK-05 (no isolated effects; reaction to common discoveries).
- **Do NOT do yet:** complex diplomacy/war.

## E5 — Advanced systems
- **Objectives:** a living world with other civilizations and institutions.
- **Systems:** Politics, Diplomacy (complete the GDD, DR-13), Civilization AI, War.
- **Dependencies:** E4.
- **Functionality:** government/laws with consequences; autonomous civilizations; diplomatic relations; secondary conflicts.
- **Acceptance:** AI civilizations evolve and relate to one another without the player; war is coherent and secondary.
- **Risks:** RISK-03/06/11 (AI scale; coupling; do not over-simulate).
- **Do NOT do yet:** final balance, multiplayer (out of scope).

## E6 — Long term / product
- **Objectives:** deep emergent history, objectives/ending, polish, scale.
- **Systems:** complete Events/History, Progression, optimization, final animation/sound/UI, tutorials, historical content.
- **Dependencies:** E5.
- **Functionality:** chronicle/legacy; optional objectives and historical limit; large-scale performance.
- **Acceptance:** a long, coherent game with its own history and an optional ending.
- **Risks:** RISK-08/09/10 (persistence, scale, entity migration).
- **Do NOT do yet:** anything new without a product decision.

## Decisions that condition the roadmap (confirm before moving forward)

- **E1:** DR-02 (world profile), DR-03 (climate/seasons), DR-06 (scale), DR-12 (prototype scope).
- **E2:** DR-07 (property), DR-10 (individual↔aggregate LOD), model unification (RISK-02).
- **E4:** DR-01 (historical cutoff), DR-11 (technological adoption/diffusion), shared evolution framework.
- **E5:** DR-05 (player role), DR-13 (complete Diplomacy).
