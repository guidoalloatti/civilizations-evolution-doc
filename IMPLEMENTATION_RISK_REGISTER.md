# IMPLEMENTATION_RISK_REGISTER — Civilization Evolutions

> **Phase 2 — Architecture and development risk register.** For each risk: why it matters, affected systems, consequences, mitigation and when to address it. This is not implementation. Related to [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) (section E) and [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md).
> Version 1.1 · Last updated 2026-11-04 · Severity: 🔴 high · 🟠 medium · 🟡 low.
> **Blocks MVP?** = whether not mitigating it **prevents** reaching the first playable (Scenarios A–D / roadmap E1–E2). *Yes* = it must be tackled before or during the MVP; *No* = it can be deferred without blocking the MVP (but it remains debt). This is orthogonal to severity: a risk can be 🔴 and **not** block the MVP if it only appears at large scale.

| ID | Risk | Sev | Blocks MVP? | When to address |
|---|---|---|---|---|
| RISK-01 | Simulation logic in the presentation layer (UE) | 🔴 | **Yes** | Now (before expanding gameplay) |
| RISK-02 | Data duplication (double population/economy model) | 🔴 | **Yes** | Now |
| RISK-03 | Performance with thousands of entities (one Actor per inhabitant) | 🔴 | No (MVP ~20–30) | Before scaling population |
| RISK-04 | Hardcoded content instead of data-driven | 🟠 | No | As the catalog grows |
| RISK-05 | "Scripted" historical evolution / isolated technological effects | 🟠 | Partial | Before implementing Technology in depth |
| RISK-06 | Strong coupling between systems | 🟠 | **Yes** (sets the pattern from the start) | Continuous |
| RISK-07 | Excessive update frequency (everything per Tick) | 🟠 | No | Continuous |
| RISK-08 | Save/load complexity as the state grows | 🟠 | No | Before persisting new systems |
| RISK-09 | Scalability of procedural generation | 🟡 | No | When moving from small to large worlds |
| RISK-10 | Migration of persistent entities (individual↔aggregate, versions) | 🟠 | No | When introducing LOD and evolution |
| RISK-11 | Simulation too complex for the prototype (over-engineering) | 🟠 | **Yes** | Continuous |
| RISK-12 | Uncommitted work / two sessions on the same tree | 🟠 | **Yes** | Now |
| RISK-13 | **Determinism / reproducibility** of the simulation | 🟠 | Partial | When defining the clock and serialization |
| RISK-14 | **Memory footprint** of the persistent state (individuals/history) | 🟠 | No | When scaling population/centuries |
| RISK-15 | **Streaming of large maps** (world not resident in memory) | 🟡 | No | When moving to large worlds |
| RISK-16 | **Long-term save compatibility** (schema versions) | 🟠 | No | Before publishing builds with saves that matter |
| RISK-17 | **AI cost** of non-player civilizations | 🟠 | No | When introducing strategic AI |
| RISK-18 | **Cascading updates** / feedback storms between systems | 🟠 | Partial | When connecting economy↔population↔politics |
| RISK-19 | **Debugging emergent behavior** (reproducing/understanding systemic bugs) | 🟠 | **Yes** (without this, the MVP is undebuggable) | Now (tooling from the start) |

**MVP blockers (tackle now or during the MVP):** RISK-01, RISK-02, RISK-06, RISK-11, RISK-12, RISK-19 (+ partially RISK-05/13/18). The rest is deferrable debt that does **not** prevent the first playable.

---

### RISK-01 — Simulation coupled to presentation 🔴
- **Why it matters:** the technical plan requires separating simulation from render; that is what allows scaling and testing. The new gameplay (settlers, nodes, colony stock, building production) lives in the **UE layer**, not in SimCore.
- **Affects:** Population, Economy, Production, Logistics.
- **Consequences:** it stays outside the SimCore tests; hard to scale; hard to simulate without render.
- **Mitigation:** define the **home** of the colony logic (recommended: move down to SimCore — DR pending) and leave only presentation/input in UE.
- **When:** before expanding gameplay (each new feature grows the debt).

### RISK-02 — Data duplication 🔴
- **Why it matters:** two population/economy models coexist (aggregate in SimCore ↔ individual/colony in UE), against "one identity per system" (DC-07/DC-15).
- **Affects:** Population, Resources, Economy, Entities.
- **Consequences:** divergent states, consistency bugs, repeated logic.
- **Mitigation:** a single source of truth per entity/resource; unify the two models (see SHARED_DATA_MODEL + DR-07/DR-10).
- **When:** now.

### RISK-03 — Performance with thousands of entities 🔴
- **Why it matters:** `CivUnit` is one **Actor per inhabitant**; the plan explicitly calls for not doing that and for supporting aggregation.
- **Affects:** Population, Time, render.
- **Consequences:** at 20 it works fine; at hundreds/thousands performance drops.
- **Mitigation:** an aggregate simulation level (LOD) by size/distance/relevance (DR-10); represent individuals as data, with actors only for the visible/active ones.
- **When:** before raising the target population.

### RISK-04 — Hardcoded content 🟠
- **Why it matters:** resources/buildings/recipes/technologies live in C++ (`DefaultContent`), not in Data Tables/Assets.
- **Affects:** Resources, Production, Construction, Technology.
- **Consequences:** balancing/adding content forces a recompile; hard to iterate by design.
- **Mitigation:** gradually migrate to Data Tables/Data Assets (plan §4.2).
- **When:** when the catalog grows.

### RISK-05 — Scripted historical evolution / isolated effects 🟠
- **Why it matters:** if each system implements a technology's effects on its own, the result is rigid and hard to extend (against DC-01/DC-09).
- **Affects:** Technology, Production, Construction, Military, Culture.
- **Consequences:** rigid technology tree; rewrites when adding systems.
- **Mitigation:** a **shared** framework of discoveries/conditions/events that systems **react** to (see [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md)).
- **When:** before implementing Technology in depth.

### RISK-06 — Strong coupling between systems 🟠
- **Why it matters:** the vision calls for systems to interact without knowing each other's internal implementation.
- **Affects:** all.
- **Consequences:** local changes break other systems; hard to test in isolation.
- **Mitigation:** communication via typed events + interfaces; respect the authority map (Maestro §5).
- **When:** continuous.

### RISK-07 — Excessive update frequency 🟠
- **Why it matters:** updating everything every frame does not scale.
- **Affects:** all.
- **Consequences:** wasted CPU; FPS drops.
- **Mitigation:** per-system cadences + event-driven (see SYSTEM_DEPENDENCY_MAP §E); the FPS-independent clock already exists.
- **When:** continuous.

### RISK-08 — Save/load complexity 🟠
- **Why it matters:** SimCore already serializes; as entities/relationships/history are added, persistence gets more complex.
- **Affects:** Entities, Events, everything that persists.
- **Consequences:** fragile saves or saves incompatible across versions.
- **Mitigation:** stable identity + save versioning; serialize the shared model, not copies per system.
- **When:** before persisting new systems.

### RISK-09 — Scalability of procedural generation 🟡
- **Why it matters:** the GDD contemplates large and varied worlds; the current terrain is a small patch.
- **Affects:** Generation, Geography, render.
- **Consequences:** slow or heavy generation/meshes in large worlds.
- **Mitigation:** generation by layers/chunks, reproducible seed, terrain LOD.
- **When:** when moving from small to large worlds.

### RISK-10 — Migration of persistent entities 🟠
- **Why it matters:** moving individuals into groups (and vice versa) and evolving buildings while preserving identity is complex.
- **Affects:** Population, Entities, Construction.
- **Consequences:** loss of identity/history; inconsistencies when aggregating/disaggregating.
- **Mitigation:** stable conceptual identity independent of the simulation level (DR-10; Entities §11).
- **When:** when introducing LOD and historical evolution.

### RISK-11 — Over-engineering / over-simplification 🟠
- **Why it matters:** the plan calls for avoiding empty abstractions and, at the same time, for not simplifying the architecture just to "fit" the prototype.
- **Affects:** all.
- **Consequences:** useless complexity or costly redesign.
- **Mitigation:** implement what is required per stage; keep extensibility without building unrequested systems.
- **When:** continuous.

### RISK-12 — Uncommitted work / two sessions on the same tree 🟠
- **Why it matters:** everything after the HANDOFF commit is uncommitted; two sessions edit the same tree (last-to-save wins).
- **Affects:** all the code.
- **Consequences:** loss of work, collisions.
- **Mitigation:** commit at green checkpoints; a single session writes gameplay at a time; the docs repo is already versioned separately.
- **When:** now.

### RISK-13 — Determinism / reproducibility 🟠 · Blocks MVP? Partial
- **Why it matters:** a historical simulation that saves/loads and that could one day replay or be debugged needs that, given the same seed and inputs, it produces the same result. Floating point, container iteration order and a poorly seeded RNG break this.
- **Affects:** Time, all systems with randomness (demographics, events, AI), save/load.
- **Consequences:** irreproducible bugs; saves that diverge; impossible to compare "the same game" across builds.
- **Mitigation:** an RNG with an explicit, per-subsystem seed; avoid depending on `TMap`/pointer ordering; fix the update order; isolate the non-deterministic (render) from the simulation.
- **When:** when defining the clock and serialization (cheap now, expensive later).

### RISK-14 — Memory footprint of the persistent state 🟠 · Blocks MVP? No
- **Why it matters:** individuals with history + a growing historical record + several civilizations can consume a lot of RAM across centuries.
- **Affects:** Population, Entities, Events/Historical record.
- **Consequences:** unbounded memory growth; long games that degrade.
- **Mitigation:** aggregation (DR-10) frees detail; summarize/compact old history; compact structures (SoA, ids instead of pointers); memory budgets per subsystem.
- **When:** when scaling population/duration (not in the ~20–30 MVP).

### RISK-15 — Streaming of large maps 🟡 · Blocks MVP? No
- **Why it matters:** a large world does not fit resident in memory nor is it worth simulating in full detail.
- **Affects:** Generation, Geography, Logistics, render.
- **Consequences:** memory/load spikes; hitches when moving around the world.
- **Mitigation:** chunks with load/unload; aggregate simulation of distant zones (consistent with LOD); separate simulation state from render meshes.
- **When:** when moving from small to large worlds (related to RISK-09).

### RISK-16 — Long-term save compatibility 🟠 · Blocks MVP? No
- **Why it matters:** the state schema will change many times; without versioning, each change invalidates existing saves.
- **Affects:** Entities, Events, everything that persists.
- **Consequences:** loss of games on update; fragile ad-hoc migrations.
- **Mitigation:** a schema version number + migrators per version; optional fields with defaults; tests loading old saves. Different from RISK-08 (which is the *structural complexity*; this is *schema evolution over time*).
- **When:** before publishing builds whose saves matter to users.

### RISK-17 — AI cost of civilizations 🟠 · Blocks MVP? No
- **Why it matters:** each non-player civilization decides (economy, diplomacy, war, expansion); doing it every tick and for many does not scale.
- **Affects:** AI, Diplomacy, Economy, War.
- **Consequences:** CPU saturated with few civilizations; or AI so cheap that it is dumb.
- **Mitigation:** AI at a low cadence/per milestone (see SYSTEM_DEPENDENCY_MAP §E); depth by relevance/proximity (AI §8); a per-frame time budget split among civilizations.
- **When:** when introducing strategic AI (post-MVP).

### RISK-18 — Cascading updates / feedback storms 🟠 · Blocks MVP? Partial
- **Why it matters:** economy↔population↔politics feed back on each other; an event can trigger a cascade that oscillates or explodes (bouncing prices, looping migrations).
- **Affects:** Economy, Population, Politics, Events.
- **Consequences:** numerical instability, infinite event loops, implausible behavior.
- **Mitigation:** damping/inertia in the models; process cascades by cadence (not immediate recursion); limits and cycle detection; resolve in discrete clock steps.
- **When:** when connecting the three cores (it starts in the MVP with economy↔population).

### RISK-19 — Debugging emergent behavior 🟠 · Blocks MVP? Yes
- **Why it matters:** in an integrated simulation, bugs arise from the interaction of many systems and often hours in; without tooling, they are nearly impossible to reproduce and understand.
- **Affects:** all.
- **Consequences:** lost development time; bugs that cannot be isolated; loss of confidence in the simulation.
- **Mitigation:** per-entity state inspection; a queryable timeline/event log (provided by the Events system); reproducibility (RISK-13); logs by cadence; a step-by-step clock mode; validation scenarios ([CROSS_SYSTEM_VALIDATION_SCENARIOS.md](CROSS_SYSTEM_VALIDATION_SCENARIOS.md)) as repeatable tests.
- **When:** now — debugging tooling must be born with the simulation, not after.
