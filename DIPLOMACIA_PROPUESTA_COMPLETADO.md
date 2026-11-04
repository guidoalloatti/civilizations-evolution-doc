# DIPLOMACY — Completion proposal (DRAFT TO VALIDATE)

> ⚠️ **NOT CANONICAL.** This document is a **proposed draft** to cover the **truncated** part of `GDD_Diplomacia_y_Relaciones_Internacionales_v0.1` (it cuts off halfway through §18). It responds to **DR-13 / F-22** and was authorized by the user on **2026-11-04** only as a *proposal to validate*.
> - The original GDD was **not** edited (the originals are not touched).
> - It is **not** confirmed content: nothing here is canonical until your explicit validation.
> - It is **derived** from what the GDD itself already sets out (up to §18), from AI (DC-11), and War (DC-10). Each point that is **not** backed by the source is marked 🟡 **assumption**.
> On validation, it is promoted to `confirmed` in [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) and only then transferred to the owner GDD.

## What is missing in the source
The file cuts off at **§18 "Leader personality"** after listing traits (Ambition, Pragmatism, Distrust, Charisma) and does **not** include: the rest of §18, an **MVP** section, the **complexity levels** (which other GDDs do carry), or a **confirmed decisions** table. This proposal covers those four gaps.

---

## 1. Complete §18 — Leader personality (proposed)

**Anchored in:** §18 already enumerates traits; §1 (principles) and §17.4 (philosophy: diplomacy is a real tool, war is a possibility, not an objective); AI §4/§11 (DC-11: civilizations with the **same rules** as the player, **without omniscience**).

- **Traits as parameters, not as a script.** Each leader has values across a set of traits (the ones already listed + 🟡 possible ones: loyalty, bellicosity, cultural/religious openness, risk tolerance). They modulate **how** a civilization pursues its objectives (§9), not **what** it can do: they grant no information or actions that the player would not have (consistent with DC-11).
- **Concrete influence** (🟡 proposed, to be calibrated): they bias the willingness to negotiate vs escalate (§17.2), the weight of historical memory (§3) in decisions, the probability of breaking treaties (§7.1), territorial aggressiveness (§10), and the tone of communication.
- **Assignment:** when a leader emerges, traits are derived from the 🟡 culture/religion of the civilization (§13/§14), its recent history (§3), and individual variation. Reproducible by seed (see RISK-13 in the risk register).
- **Evolution:** the traits are **stable but not fixed**; strong events (betrayal, prolonged war, prosperity) can shift them slowly. A change of leader can reorient foreign policy without erasing the civilization's **historical memory** (§3), which persists above the individual.
- **Transparency to the player:** personality is **inferred** from observed behavior and intelligence (§12), it is not read as an omniscient datum (consistent with DC-11 and §12).

---

## 2. Diplomacy MVP (proposed)

**Anchored in:** the "per-system MVP" pattern of the rest of the GDDs and Diplomacy's placement in **stage E5** of the [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) (it does not block the first playable).

Includes (minimum viable):
- Bilateral relationships with a **relationship state** (§2.1) that rises/falls through actions and through basic **historical memory** (§3).
- A handful of essential **treaties**: peace/non-aggression, trade, simple alliance (§6.1/§6.2/§6.3), with **duration and breakage** (§7) and basic consequences.
- **"Basic"-level negotiation** (§8): simple offer/counteroffer, no advanced mechanics.
- Integration with **AI** (an autonomous counterpart with objectives, §9) and with **War** only at the boundary (casus belli → conflict, DC-10).
- Leader personality in a **minimal** form: 2–3 traits that bias the AI.

Left **out of the MVP** (advanced levels, already hinted at in the GDD): espionage (§12), international organizations (§15), dependent states/vassalage (§16), fine religious/cultural diplomacy (§13/§14), multiparty mediation/arbitration (§17.3).

---

## 3. Complexity levels (proposed, in the style of the rest of the GDDs)

| Level | Scope |
|---|---|
| **Basic** | Bilateral relationships, essential treaties, simple negotiation, 2–3 leader traits. |
| **Intermediate** | More treaty types (territorial §6.4, religious/cultural §6.5), influence/prestige (§11), border disputes (§10), full personality. |
| **Advanced** | Espionage (§12), international organizations (§15), dependent states (§16), multiparty diplomacy, deep historical memory. |

---

## 4. Confirmed decisions — PROPOSALS (to validate)

> Marked `proposed`: none is canonical until your OK. Those that only transcribe something already written in the GDD carry 📄 (low risk); the 🟡 ones are new proposals (they require more judgment).

| ID | Proposed decision | Basis |
|---|---|---|
| DP-01 📄 | Diplomacy is a real tool for managing interests; war is a **possibility, not an objective**. | §17.4 + DC-10 |
| DP-02 📄 | Relationships have a **historical memory** that evolves and conditions future decisions. | §3 |
| DP-03 📄 | Civilizations act on their own **objectives and interests** (§9), with the same rules as the player and without omniscience. | §9 + DC-11 |
| DP-04 🟡 | The **leader's personality** is a set of parameters that **modulates** behavior, never grants extra information/actions. | §18 (partial) + DC-11 |
| DP-05 🟡 | Personality is **stable but evolving**; the civilization's memory **persists** above the leader. | 🟡 proposed |
| DP-06 🟡 | The Diplomacy MVP is **bilateral + essential treaties + basic negotiation**, in stage **E5**; the advanced items (espionage, organizations, vassalage) stay post-MVP. | roadmap E5 + MVP pattern |

---

## How to proceed
1. Review §1–§4 of this proposal.
2. Tell me what you accept / change / reject (or whether you have the original text to replace it).
3. What is accepted is marked `confirmed` in the register and transferred to the owner GDD with date/version; the rest stays as a proposal or is discarded.
