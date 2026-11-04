# DESIGN_CONSISTENCY_AUDIT — Civilization Evolutions

> **Phase 2 — Design consistency audit.** Joint review of the 22 system GDDs + Master + technical plan. Analysis, not implementation. **Design conflicts are not resolved unilaterally**: they are classified and, where appropriate, referred to a resolution in [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md). As of 2026-11-04, DR-00…DR-12 are **confirmed by delegation** from the designer (reversible and auditable); DR-13 remains **unresolved** (truncated source). The **Severity** column and **User confirmation?** (§ Severity below) indicate which still require an explicit human decision before implementing.
> Version: 1.1 · Last updated: 2026-11-04 · Status: analysis · Sources: see [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md). Extends (does not replace) [AUDIT_REPORT.md](AUDIT_REPORT.md).

## Classification used

| Class | Meaning |
|---|---|
| **CC** Confirmed and consistent | Decision marked as confirmed, coherent across documents. |
| **PI** Potential inconsistency | Wording or scope tension; may not be real. |
| **CX** Confirmed contradiction | Two sources assert incompatible things. |
| **MD** Missing design decision | A definition is needed that does not exist today. |
| **AR** Architectural risk | May cause expensive refactors or coupling. |
| **FC** Future consideration | Valid, but out of the initial scope. |

---

## A. Contradictions and tensions between documents

| ID | Class | Finding | Sources | Referral |
|---|---|---|---|---|
| F-01 | **CX** | Historical cutoff date: "≈1800 AD" vs "beginning of the Industrial Revolution" vs "late 18th century". | General GDD §1/§3; Economy; Progression §8–9; Technical plan §1 | DR-01 |
| F-02 | **CX** | Initial world profile: "flat maps with natural borders" (Generation, confirmed decision) vs "realistic, continuous topography" (Geography, confirmed decision). | Generation §21; Geography §3/§23 | DR-02 |
| F-03 | **PI** | Climate "mainly visual at the start" (Construction) vs climate variables with consequences and seasonal dependence of agriculture/production. | Construction §12; Geography §7–8; Economy §7; Production | DR-03 |
| F-04 | **PI** | Product name: "Historical Civilization Simulator" (technical plan, general GDD) vs "Civilization Evolutions" (folder/UE project). | Technical plan; 01_GDD_GENERAL; `.uproject` | DR-04 |
| F-05 | **CX** | Player role: "external entity that is NOT a ruler" (Politics) vs the ability to dictate laws, foreign policy, and direct decisions (Politics, Economy, Diplomacy). | Politics §2/§14; Economy; Diplomacy §1 | DR-05 |
| F-06 | **PI** | "Pending" status of Diplomacy/Military in the general GDD vs specialized GDDs already with confirmed decisions. | 01_GDD_GENERAL §20; War §20; Diplomacy | DR-08 (resolved as "the specialized one is the current revision") |
| F-07 | **MD** | Property/ownership: Resources and Entities set "State/civilization ownership + private parties operate"; Economy marks it as pending in detail (prices, taxation, inheritance). | Resources §11/§13; Entities §13; Economy §12/§20 | DR-07 |

## B. Duplicated mechanics / overlapping responsibilities

| ID | Class | Finding | Current documentary treatment |
|---|---|---|---|
| F-08 | **PI** | Geography and World Generation both describe terrain, climate, biomes, resources. | Master §5: Geography = natural state/processes; Generation = initial state/editor. Keep that boundary. |
| F-09 | **AR** | "Resources" appears in Economy, Production, Logistics, Construction, Technology. | Single owner = Resources and Materials (catalog/inventory). The others reference, they do not copy. |
| F-10 | **PI** | Population and Entities repeat individual/family/relationships. | Population = social/demographic model; Entities = shared identity/relationships. |
| F-11 | **PI** | Exploration, Administration, and Diplomacy repeat territorial control/autonomy. | Exploration = knowledge/occupation; Administration = internal management; Diplomacy = relationship between civilizations. |
| F-12 | **PI** | Technology, Progression, and Events repeat "historical evolution". | Technology = enablements/knowledge; Progression = objectives/ending; Events = causality/memory. |
| F-13 | **AR** | Logistics: War, Exploration, and Trade could create their own transport. | War/Exploration **reuse** the Transport/Logistics system (as they declare). Do not duplicate. |

## C. Missing definitions / pending design decisions (MD)

| ID | Class | To be defined | Affected systems | Referral |
|---|---|---|---|---|
| F-14 | **MD** | Exact time scale (real seconds ↔ day/month/year) and equivalence with aging/seasons. | Time, Population, Economy, Production, Events | DR-09 |
| F-15 | **MD** | Scale limits: map size, max population, number of civilizations, update budgets. | All (performance) | DR-06 |
| F-16 | **MD** | Thresholds for transition between individual ↔ aggregate simulation (LOD) and identity preservation. | Population, Entities, AI, Time | DR-10 |
| F-17 | **MD** | Common property model (lands, buildings, inventories, organizations), inheritance, and operational monetary unit. | Economy, Entities, Resources, Population, Politics | DR-07 |
| F-18 | **MD** | Rules for technological availability/adoption/diffusion and conditions for transforming buildings. | Technology, Production, Construction | DR-11 |
| F-19 | **MD** | Mathematical models: productivity, wages, prices, needs, happiness, health, demographics, beauty. | Economy, Production, Population, Construction | (pending a stage) |
| F-20 | **MD** | Scope of the cross-cutting prototype vs the per-system MVPs (which systems and at what depth enter the first playable). | All | DR-12 |
| F-21 | **MD** | Approval traceability (owner/date/version) of the "confirmed" decisions. | Documentary governance | DR-00 |

## D. Incomplete / source documents

| ID | Class | Finding |
|---|---|---|
| F-22 | **CX/MD** | **`GDD_Diplomacy...` TRUNCATED** in the source: it cuts off halfway through §18 ("Leader personality"); no MVP or decision table. It must be completed or the source recovered. |
| F-23 | **PI** | `GDD_Resources_and_Materials`, `GDD_Time_and_Simulation`, `GDD_AI...`, and `GDD_Entities...` do **not** carry a formal "Confirmed decisions" table (they use body/principles/criteria). Risk: confusing "asserted in the body" with "formally confirmed". |
| F-24 | **PI** | `00_TECHNICAL.md` (UE project) is **identical** to the technical plan; `02/03/04` are condensations of GDDs. Risk of future divergence between copy and original. |

## E. Architectural risks (summary; detail in IMPLEMENTATION_RISK_REGISTER.md)

| ID | Class | Finding |
|---|---|---|
| F-25 | **AR** | **Dual population/economy model** in the code: aggregate in SimCore ↔ individual/colony in the UE layer. Risk of duplication and of simulation logic outside SimCore. → RISK-01/02. |
| F-26 | **AR** | **One Actor per inhabitant** (`CivUnit`) contradicts "do not create an Actor per citizen" and the scale goal. → RISK-03. |
| F-27 | **AR** | Historical evolution implemented as isolated per-system effects instead of reacting to common discoveries/conditions. → RISK-05, see [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md). |
| F-28 | **AR** | Content (resources/buildings/recipes/technologies) hardcoded in C++ (`DefaultContent`) instead of data (Data Tables/Assets). → RISK-04. |

## F. Future considerations (FC)

- Spherical worlds (Generation): assessable in the future, not MVP.
- Advanced climate, hydrology, ecosystems, disasters (Geography): post-MVP.
- Multiplayer: out of the initial scope.
- International organizations, dependent states, advanced espionage (Diplomacy): advanced levels.

---

## G. Severity and required confirmation (per finding)

> **Severity:** *Critical* = blocks architecture/MVP or forces an expensive refactor if ignored; *High* = affects several systems, decide before E1–E2; *Medium* = containable, decide before the affected system; *Low* = cosmetic/future.
> **User confirmation?** *Yes* = irreversible or vision-level human decision that I should **not** make alone (even if a DR is proposed); *Delegated* = already covered by the 2026-11-04 delegation (reversible); *No* = documentary/architectural rule with no pending vision decision.

| ID | Class | Severity | User confirmation? | Status |
|---|---|---|---|---|
| F-01 | CX | **Critical** | **Yes** (historical cutoff = game scope) | DR-01 delegated, mark as vision to ratify |
| F-02 | CX | **High** | **Yes** (world profile affects generation and geography) | DR-02 delegated |
| F-03 | PI | Medium | Delegated | DR-03 |
| F-04 | PI | Low | Delegated (product name) | DR-04 |
| F-05 | CX | **Critical** | **Yes** (defines the player role = design pillar) | DR-05 delegated, ratify |
| F-06 | PI | Medium | No | DR-08 resolved (the specialized one is current) |
| F-07 | MD | **High** | **Yes** (property/inheritance/taxation model) | DR-07 delegated |
| F-08 | PI | Medium | No | domain rule (Master §5) |
| F-09 | AR | **High** | No | single owner = Resources |
| F-10 | PI | Medium | No | domain rule |
| F-11 | PI | Medium | No | domain rule |
| F-12 | PI | Medium | No | domain rule |
| F-13 | AR | Medium | No | reuse Logistics |
| F-14 | MD | **High** | Delegated | DR-09 (time scale) |
| F-15 | MD | **Critical** | **Yes** (scale limits = performance budget) | DR-06 delegated, ratify |
| F-16 | MD | **High** | Delegated | DR-10 (individual↔aggregate LOD) |
| F-17 | MD | **High** | **Yes** (operational property/currency) | DR-07 delegated |
| F-18 | MD | Medium | Delegated | DR-11 (technological adoption) |
| F-19 | MD | Medium | No (mathematical models, per stage) | pending a stage |
| F-20 | MD | **High** | Delegated | DR-12 (prototype scope) |
| F-21 | MD | Medium | No | DR-00 (governance) |
| F-22 | CX/MD | **Critical** | **Yes** (truncated source; do not invent diplomacy) | DR-13 **unresolved** |
| F-23 | PI | Low | No | documentary note |
| F-24 | PI | Low | No | documentary note |
| F-25 | AR | **Critical** | No (architectural rule) | RISK-01/02 |
| F-26 | AR | **High** | No | RISK-03 |
| F-27 | AR | **High** | No | RISK-05 |
| F-28 | AR | Medium | No | RISK-04 |

**Human confirmation status (updated 2026-11-04):** F-01, F-02, F-05, F-07/F-17, and F-15 were **ratified by the user** (see DR-01/02/05/07/06). F-22 (truncated Diplomacy) has a **proposed draft to validate** ([DIPLOMACY_PROPOSAL_COMPLETED.md](DIPLOMACY_PROPOSAL_COMPLETED.md)); it is still not canonical. The rest are covered by the delegation (reversible) or are documentary/architectural rules.

---

## Summary by class

- **CC** (confirmed and consistent): most of the pillars (see DECISION_REGISTER, `confirmed` status).
- **CX** (contradictions): F-01, F-02, F-05, F-22. → require the designer's decision.
- **PI** (potential): F-03, F-04, F-06, F-08, F-10, F-11, F-12, F-23, F-24.
- **MD** (missing): F-07, F-14..F-21.
- **AR** (risks): F-09, F-13, F-25..F-28.
- **FC** (future): section F.

**Rule applied:** none of these were resolved by unilateral judgment. Those that admit a technical proposal have a **proposed** (not confirmed) resolution in the decision register.
