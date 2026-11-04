# DESIGN_DECISION_REGISTER — Civilization Evolutions

> **Phase 2 — Central design decision register.** Three classes of entry: **Confirmed by GDD** (DC-, transcribed from the GDDs with a source), **Confirmed by delegation** (DR-, resolved by the assistant because the user explicitly delegated the decision — "make the decisions needed to move forward, I trust your judgment", 2026-11-04), and **Unresolved** (missing material, not a choice).
> **Traceability:** the DR- entries are NOT from the GDD; they are operational decisions made by the assistant under the user's delegation, **open and reversible**. When applying them to an owner GDD, record owner/date/version and update the Master's summary. If in the future they contradict a source, what the user decides prevails.
> Version 1.1 · Last updated: 2026-11-04 · Sources: [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md). Findings: [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md).

**Statuses:** `confirmed` (by GDD) · `confirmed` ✓ (delegated) (adopted under delegation, reversible) · `confirmed` ✓✓ (ratified by the user, with date) · `unresolved` · `deprecated`.

## Part 1 — Confirmed decisions (transcribed from the GDDs)

| ID | Topic | Decision | Source | Affected systems |
|---|---|---|---|---|
| DC-01 | Historical evolution | Free and non-linear; periods are reference, not mandatory unlocks. | Master §4; general GDD §19; Technology | All |
| DC-02 | Player role | Guiding entity (not ruler) with broad influence; society and other civilizations retain autonomy. | Master §4; Politics §14 | Politics, AI, Interface |
| DC-03 | Construction | Free placement with optional grid; construction work with workers and materials; architectural evolution chosen by the player; preservation of old buildings. | Construction §22 | Construction, Production, Population |
| DC-04 | Population | Hybrid model of individuals/families/groups; detail according to scale and configuration. | Master §4; Population §20 | Population, Entities, Economy |
| DC-05 | Economy/production | Interdependent chains; automatic labor assignment with intervention; **mixed** ownership; configurable complexity. | Economy §19; Production §23 | Economy, Production, Logistics |
| DC-06 | Transport/trade | Physical movement of resources from the start (simplified); trade in 3 levels (basic/intermediate/advanced). | Master §4; Logistics §22 | Logistics, Economy |
| DC-07 | Single resource | One resource = one identifier and a common inventory; no per-system copies. | Resources §17; Master §6 | All economic ones |
| DC-08 | Common clock | Continuous time with a single global clock and internal cycles of different frequency; speeds do not alter results. | Time §2/§15 | All |
| DC-09 | Technology | Enables methods/capabilities by conditions and knowledge, **not by the year**; availability ≠ automatic adoption. | Technology §27; Master §6 | Technology, Production, Construction, Military |
| DC-10 | War | Broad consequences; important but **secondary**; reuses logistics/resources. | War §20; Master §4 | War, Logistics, Population |
| DC-11 | Civilization AI | Autonomous, with the **same rules** as the player; no omniscience or knowledge of the future. | AI §4/§11 | AI, Diplomacy, Economy |
| DC-12 | Interface | The 3D world is the main interface; the UI **reads** the state, it does not own it; accessibility is a priority. | Interface §14; Master §6 | Interface, all |
| DC-13 | Ending | **Optional** historical limit; continuation possible; final decision by the player; genuine sandbox. | Progression §13 | Progression, Time |
| DC-14 | Events | Record facts from the owner systems; do not create effects without an owning rule. | Events §17; Master §6 | Events, all |
| DC-15 | Entities | Persistent identity and shared relationships; systems share entities without duplicating. | Entities §2 | All |

## Part 2 — Decisions made by delegation (conflicts and pending items)

> Each one responds to a finding from the audit. **Confirmed by the user's delegation (2026-11-04)**, with recommendation, justification, and consequences. **Reversible:** if you want to change any, it is adjusted and propagated to the owner GDD. The "Proposal" of each one is the adopted decision.

### DR-00 — Decision traceability `confirmed` ✓ (delegated)
- **Topic/finding:** F-21. The tables say "confirmed" without owner/date.
- **Proposal:** adopt this register as the single source; when confirming something, note owner+date+version and update the owner GDD → Master.
- **Affects:** documentary governance. **Consequence:** avoids treating assumptions as confirmed.

### DR-01 — Historical cutoff date `confirmed` ✓✓ (ratified by the user 2026-11-04)
- **Finding:** F-01 (CX). **Affects:** Progression, Time, Technology, Economy, content.
- **Proposal (recommended):** define the cutoff as **"beginning of the Industrial Revolution", ~1760–1780**, used as an **optional limit** (not a playable stage); afterward, sandbox with no new industrial stages. Do not fix an exact year in the UI until your OK.
- **Consequence:** bounds chronology, content, and ending without closing off the sandbox continuation.

### DR-02 — Initial world profile (flat vs relief) `confirmed` ✓✓ (ratified by the user 2026-11-04)
- **Finding:** F-02 (CX). **Affects:** Generation, Geography, Construction, Logistics.
- **Proposal:** interpret "flat maps" as an **optional preset** (world without marked relief / simple projection), and use **coherent relief by default** (as the current code already does, with hills). "Flat" = a generation option, not the default value.
- **Consequence:** eliminates the contradiction; both GDDs become compatible via presets.

### DR-03 — Scope of climate/seasons in the MVP `confirmed` ✓ (delegated)
- **Finding:** F-03 (PI). **Affects:** Geography, Economy, Production, Construction.
- **Proposal:** **Phase 1** = seasons with a **minimal** effect on agriculture/production (what already exists) + **visual** climate in construction; advanced climate (weather, environmental impact, disasters) **post-MVP**. "Mainly visual" in Construction = no structural effects at the start, but seasons do modulate production.
- **Consequence:** reconciles the MVP; does not block seasonal agriculture.

### DR-04 — Project name `confirmed` ✓ (delegated)
- **Finding:** F-04 (PI). **Affects:** docs/UI.
- **Proposal:** use **"Civilization Evolutions"** as the official working name and unify documentation/UI; the technical module name `CivilizationSimulator` can remain as an internal identifier.
- **Consequence:** brand coherence; no mandatory code changes.

### DR-05 — Player role and power `confirmed` ✓✓ (ratified by the user 2026-11-04)
- **Finding:** F-05 (CX). **Affects:** Politics, Economy, Diplomacy, Interface, AI.
- **Proposal:** the player is a **direction/abstraction** with broad intervention; their actions are classified: **direct** (construction, assignment, own economy), **policy** (laws/taxes via the government, with resistance/consequences), and **advisory** (where there is social/private/external autonomy). It is not a diegetic office.
- **Consequence:** defines limits for future Politics/Diplomacy without reducing the confirmed agency.

### DR-06 — Scale limits (prototype) `confirmed` ✓✓ (ratified by the user 2026-11-04)
- **Finding:** F-15 (MD). **Affects:** all (performance).
- **Proposal (prototype targets only, revisable):** 1 map; **20–100** inhabitants with individual simulation; move to aggregate above ~hundreds; **2–3** autonomous civilizations in the first living world. These are product goals, not architecture changes.
- **Consequence:** makes performance verifiable and conditions DR-10 (LOD).

### DR-07 — Common property/ownership model `confirmed` ✓✓ (ratified by the user 2026-11-04)
- **Finding:** F-07/F-17 (MD). **Affects:** Economy, Entities, Resources, Population, Politics.
- **Proposal (layers, conceptual; detail in SHARED_DATA_MODEL):** separate **(a) ownership of the natural resource** (State/civilization), **(b) exploitation rights**, **(c) ownership of the building**, **(d) operational control** (who operates it), **(e) inventory location**. Keep "mixed ownership" (DC-05) as confirmed.
- **Consequence:** avoids duplicating ownership per system; a base for future taxation/inheritance.

### DR-08 — Authority of the specialized GDDs `confirmed` ✓ (delegated)
- **Finding:** F-06 (PI). **Affects:** governance.
- **Proposal:** treat the specialized GDDs as the **current revision**; the general GDD's "pending" list becomes **deprecated**.
- **Consequence:** removes obsolescence; without reclassifying anything as a new decision.

### DR-09 — Time scale `confirmed` ✓ (delegated)
- **Finding:** F-14 (MD). **Affects:** Time, Population, Economy, Production, Events.
- **Proposal:** **configurable** scale (no fixed number of real seconds per day); fix a default value during gameplay testing; aging/seasons derive from the clock, not from scattered constants.
- **Consequence:** respects the Time GDD; enables tuning without rewiring.

### DR-10 — Individual↔aggregate thresholds (simulation LOD) `confirmed` ✓ (delegated)
- **Finding:** F-16 (MD). **Affects:** Population, Entities, AI, Time.
- **Proposal:** LOD by **settlement size** (individual below a threshold; aggregate above) and by **distance/relevance** for other civilizations (3 levels, as in AI §8), preserving the conceptual identity of the important entities.
- **Consequence:** a condition for scaling; linked to risk RISK-03 (Actor per inhabitant).

### DR-11 — Technological availability/adoption/diffusion `confirmed` ✓ (delegated)
- **Finding:** F-18 (MD). **Affects:** Technology, Production, Construction.
- **Proposal:** enablement by **conditions** (knowledge + resources + context), not by year; **adoption** separate from availability (the building/method transforms if there is technology+materials+decision); diffusion between civilizations through the GDD's mechanisms. Implement via a **shared** evolution framework (see HISTORICAL_EVOLUTION_FRAMEWORK).
- **Consequence:** avoids isolated per-system technological effects (RISK-05).

### DR-12 — Scope of the cross-cutting prototype `confirmed` ✓ (delegated)
- **Finding:** F-20 (MD). **Affects:** all.
- **Proposal:** the first playable integrates **Time, minimal Generation/Geography, Resources, Entities, Construction, Population (small individual), Production, Logistics, basic Economy, basic Technology, minimal UI**; it leaves out Culture/Religion/Politics/Diplomacy/AI/War/advanced Progression. Detail in [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md).
- **Consequence:** aligns the cross-cutting MVP with the per-system MVPs.

### DR-13 — Complete the Diplomacy GDD `proposed` (draft to validate, 2026-11-04)
- **Finding:** F-22. The source file is **truncated** (it cuts off at §18). **Affects:** Diplomacy, AI, War.
- **Status:** the user authorized (2026-11-04) **proposing** a draft of the missing part. Written in [DIPLOMACY_PROPOSAL_COMPLETED.md](DIPLOMACY_PROPOSAL_COMPLETED.md), **marked as proposed, to be validated** — it is NOT canonical nor was it incorporated into the original GDD (which is not edited). Derived from what the GDD itself already sets out (up to §18), AI (DC-11), and War (DC-10); each assumption is flagged.
- **Pending:** your validation to promote it to confirmed and only then transfer it to the owner GDD. It does not block the prototype (Diplomacy is stage E5).

### DR-14 — UI visual identity and implementation coordination `confirmed` ✓✓ (ratified by the user 2026-11-04)
- **Topic:** the Global UI Visual Design spec vs the warm-brown palette already implemented (see [UI_DESIGN_REVIEW.md](UI_DESIGN_REVIEW.md) §1). **Affects:** Interface (UI/UX), Art, and the coordination between the docs repo and the active UI session.
- **Proposal (adopted):**
  1. **Official visual identity = the slate-blue palette** of the spec, with the **exact color tokens** from §2 of the spec (dark slate-blue backgrounds, gold as the primary accent, cool text greys). The feel is **modern, premium, elegant — not medieval/fantasy**. The warm historical character comes from **illustrations, materials, maps, documents and environmental elements**, not from tinting the whole UI brown. Exact drop-in tokens: [UI_DESIGN_REVIEW.md](UI_DESIGN_REVIEW.md) §1.
  2. **Preserve the existing implementation** — do not rewrite the UI architecture: keep the C++ UMG architecture, the centralized style tokens (`CivUIStyle.h`), the current button implementation, the simulation/presentation separation and all existing functionality. The palette migration goes **through the centralized style system** wherever possible.
  3. **Ownership/coordination:** the session implementing UI Stages 1–4 **owns** the Unreal UI files (`CivUIStyle.h`, `CivMainHUD.*`, `CivViewManager.cpp`, `Docs/UI_*`). They are **not edited from the documentation worktree** (one writer per file — `GIT_WORKFLOW.md`). No competing implementations.
  4. **Verification gate:** the visual system is **not "complete"** until there are **actual Unreal screenshots** checked at **at least 1080p and 1440p** (acceptance #9/#10 of the spec).
- **Consequence:** settles the palette divergence without discarding the implemented work; the concrete tasks for the owning session are in [UI_IMPLEMENTATION_TASKS.md](UI_IMPLEMENTATION_TASKS.md). Reversible like the other DR entries.

---

## Status index

- **Confirmed by GDD:** DC-01…DC-15.
- **Ratified by the user (2026-11-04):** DR-01, DR-02, DR-05, DR-06, DR-07, DR-14 (the vision/irreversible ones that required explicit human confirmation).
- **Confirmed by delegation (2026-11-04):** DR-00, DR-03, DR-04, DR-08, DR-09, DR-10, DR-11, DR-12 (in effect to move forward; reversible at your request).
- **Proposal to validate:** DR-13 → draft in [DIPLOMACY_PROPOSAL_COMPLETED.md](DIPLOMACY_PROPOSAL_COMPLETED.md) (not canonical until your OK).

> The DR- entries with the greatest implementation impact: **DR-02** (relief by default), **DR-03** (seasons affect agriculture; advanced climate post-MVP), **DR-06** (prototype scale), **DR-07** (layered property), **DR-10** (individual↔aggregate LOD), **DR-12** (scope of the first playable).
