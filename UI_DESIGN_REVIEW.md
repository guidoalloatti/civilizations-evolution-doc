# UI_DESIGN_REVIEW — Civilization Evolutions

> **Read-only gap audit** of the current Unreal UI implementation against the "Global UI Visual Design Implementation" spec. Produced **without editing any UE file**, because another session is actively implementing this UI (one writer per file — see `GIT_WORKFLOW.md`). This doc lives in the **docs repo**; the UI code lives in the separate `civilizations-evolution/` UE repo.
>
> Version 1.0 · 2026-11-04 · Status: review / advisory. Author did not build or run Unreal (acceptance criterion #10 cannot be verified from this session).

## 0. Context and method

The UI work is **already ~Stage 4 complete** in a concurrent session. Reviewed artifacts (read-only, as found on disk):
- Code: `Source/CivilizationSimulator/UI/CivUIStyle.h`, `UI/CivMainHUD.{h,cpp}` (1344-line widget).
- Docs: `Docs/UI_STYLE_GUIDE.md`, `Docs/UI_COMPONENT_INVENTORY.md`, `Docs/UI_IMPLEMENTATION.md`, `Docs/UI_KNOWN_ISSUES.md`.

Overall: the implementation is a genuine, coherent system — **100% C++ UMG**, centralized tokens in `CivUIStyle.h`, every button uses an explicit `FButtonStyle` (Normal/Hovered/Pressed — **no default UE styling**), tooltips wired, and the UI only **reads** `UCivSimSubsystem` (sim/UI separation respected). The findings below are gaps/divergences against the pasted spec, not defects in that work.

> **How to use this:** items marked **[DECISION]** need the project owner to choose; **[GAP]** are concrete deltas the owning (WS-UI) session can apply; **[VERIFY]** need the editor/running game.

---

## 1. [DECISION] Base palette — slate-blue (spec) vs warm-brown (implemented)

This is the **biggest divergence**. The implemented identity is a deliberate **warm brown** (documented in their `UI_STYLE_GUIDE.md` as "cálida e histórica"). The pasted spec mandates a specific **cool slate-blue** palette. They disagree on nearly every background/border token (the golds and semantics are close).

| Role | Spec (official) | Implemented now (`CivUIStyle.h`) | Match |
|---|---|---|---|
| Main background | `#111820` (17,24,32) | `BgBase` ≈ (26,23,19) warm | ✗ |
| Primary panel | `#1B252E` (27,37,46) | `Panel` ≈ (38,32,25) warm | ✗ |
| Secondary panel | `#25323D` (37,50,61) | `BgSecondary` ≈ (33,28,22) warm | ✗ |
| Hover surface | `#30414D` (48,65,77) | (uses `Border`/`PanelRaised`) | ✗ |
| Elevated/floating | `#202B35` (32,43,53) | `PanelRaised` ≈ (47,40,32) | ✗ |
| Primary border | `#40515D` (64,81,93) | `Border` ≈ (90,74,51) | ✗ |
| Secondary border | `#35434D` (53,67,77) | — | ✗ |
| Selected border | `#C6A66B` | `BorderStrong` ≈ (138,109,63) | ≈ |
| Primary gold | `#C6A66B` | `Gold` ≈ (201,162,75) | ≈ |
| Light gold | `#E0C58D` | `GoldHi` ≈ (228,192,106) | ≈ |
| Dark gold | `#9B7C49` | — (no dark-gold token) | ✗ |
| Text primary | `#E8E5DC` | `TextPrimary` ≈ (237,228,209) | ≈ |
| Text secondary | `#A5AFB5` (cool grey) | `TextSecondary` ≈ (179,168,143) warm | ✗ |
| Text muted | `#74818A` | `TextMuted` ≈ (128,119,95) | ✗ |
| Text disabled | `#56616A` | — (no disabled token) | ✗ |
| Success | `#79A88B` | `Positive` ≈ (111,174,99) | ≈ |
| Warning | `#D4A45F` | `Warning` ≈ (217,164,65) | ≈ |
| Danger | `#C87570` | `Critical` ≈ (192,90,78) | ≈ |
| Info | `#79A9C1` | `Info` ≈ (94,134,168) | ≈ |

**Decision for the owner:** keep the warm-brown identity already built, or switch to the spec's cool slate-blue? They are different moods (both valid "modern historical strategy"). If you adopt the spec palette, here are drop-in `FColor` sRGB values for `CivUIStyle.h` (to be applied **by the session that owns that file**, not from here):

```cpp
// Official slate-blue palette (sRGB). Apply in CivUIStyle.h.
BgBase        FColor(17, 24, 32)     // #111820 main background
Panel         FColor(27, 37, 46)     // #1B252E primary panel
BgSecondary   FColor(37, 50, 61)     // #25323D secondary panel
HoverSurface  FColor(48, 65, 77)     // #30414D (new token: hover)
PanelRaised   FColor(32, 43, 53)     // #202B35 elevated/floating
Border        FColor(64, 81, 93)     // #40515D primary border
BorderSoft    FColor(53, 67, 77)     // #35434D (new token: secondary border)
BorderStrong  FColor(198,166,107)    // #C6A66B selected border (== gold)
Gold          FColor(198,166,107)    // #C6A66B
GoldHi        FColor(224,197,141)    // #E0C58D light gold
GoldDark      FColor(155,124, 73)    // #9B7C49 (new token: primary-action fill)
TextPrimary   FColor(232,229,220)    // #E8E5DC
TextSecondary FColor(165,175,181)    // #A5AFB5
TextMuted     FColor(116,129,138)    // #74818A
TextDisabled  FColor( 86, 97,106)    // #56616A (new token)
Positive      FColor(121,168,139)    // #79A88B success
Warning       FColor(212,164, 95)    // #D4A45F
Critical      FColor(200,117,112)    // #C87570 danger
Info          FColor(121,169,193)    // #79A9C1
```

Adopting it needs three new tokens (`HoverSurface`, `GoldDark`, `TextDisabled`; optionally `BorderSoft`) and a sweep so buttons use `HoverSurface` for hover and `GoldDark` for the **primary-action** fill (spec §6). Because tokens are centralized, this is a **one-file change** plus verifying nothing hardcodes a warm value elsewhere.

---

## 2. [GAP] Geometry

| Element | Spec radius | Implemented | Note |
|---|---|---|---|
| Main panels | 8 px | 6 px | bump panels to 8 |
| Secondary panels | 6 px | 6 px | ✓ |
| Buttons | 5 px | 4 px | minor (`FillBrush(..., 4.f)`) |
| Inputs | 5 px | — (no input component yet) | n/a |
| Tooltips | 6 px | engine default tooltip | see §4 |
| Cards/windows | (8 px) | 8 px | ✓ |

Padding/spacing largely align: implemented uses a 4px base scale (4/8/12/16/24/32), panel padding 16 (12 compact), list gap 8 — consistent with spec's 12–16 padding / 16–20 sections / 8–12 components. No action beyond the button/panel radii.

## 3. [GAP/DECISION] Typography

- **Fonts:** spec asks **Rajdhani SemiBold** (headings) + **Inter** / **Inter Medium** (UI/numbers). Implemented uses the engine **Roboto**. The spec explicitly allows a fallback when the named fonts aren't present and **forbids adding font dependencies without approval** — so Roboto is spec-compliant today. **[DECISION]** if you want the Rajdhani/Inter look, approve importing those font assets (a new `Content/UI/Fonts/` dependency) and point `CivUI::Font` at them.
- **Sizes @1080p:** mostly aligned, small deltas — window title 22 vs spec 21; section 15 vs spec 16; numeric indicator 16 vs spec's 18–22 for *important* values. Consider a dedicated "important value" size (18–22) for headline numbers (population, treasury).

## 4. [GAP] Interaction states, tooltips, transitions

- **Normal / Hover / Pressed:** ✓ implemented on every button via `FButtonStyle` (verified in `CivMainHUD.cpp`).
- **Selected/Active:** ✓ (speed buttons + nav highlight).
- **Disabled:** ✓ visually (muted), per style guide.
- **Focused (keyboard):** style guide §6 specifies a gold 1.5px focus outline, but **no focus brush is wired** in code — add `FButtonStyle` focus styling / `SetKeyboardFocus` handling for accessibility (spec §7, §15).
- **Loading:** not implemented (spec lists it "when relevant" — low priority).
- **Transitions (spec §7, 120–200 ms):** **not implemented** — no `PlayAnimation`/interpolation found. Hover/selected are instant brush swaps. This is the clearest "feels unfinished" gap vs a commercial UI. Add subtle opacity/scale tweens on panel open/close, tab change, and notification entry.
- **Tooltips:** buttons set `ToolTipText` (engine default tooltip chrome). Spec §8 wants a **canonical styled tooltip component** (dark `panel/scrim`, 6px radius, ~0.4s delay). Not yet a shared component.

## 5. [GAP] Component system (spec §8) coverage

| Canonical component (spec §8) | Status |
|---|---|
| Main panels, floating panels | ✓ (`MakePanel`, overlays) |
| Section headers | ◑ partial (ad-hoc text, no canonical header component) |
| Buttons, icon buttons | ✓ (`MakeIconButton`, nav buttons) |
| Tabs | ◑ partial (management tab buttons, Stage 5 scaffold) |
| Dropdowns | ✗ |
| Input fields | ✗ |
| Checkboxes | ✗ |
| Sliders | ✗ (needed for settings/UI-scale) |
| Progress bars | ✗ (needed for construction progress) |
| Tooltips (shared) | ✗ (engine default only) |
| Notification cards | ✓ (`MakeNotifCard`) |
| Resource indicators | ✓ (chips) |
| Statistical indicators | ◑ partial (text pairs) |
| Separators | ◑ (spacing, no canonical separator) |
| Contextual action bars | ✗ (context panel has no action row yet) |

The spec's rule "one canonical visual implementation per component" is being honored for what exists (shared `Make*` helpers). The missing ones belong to Stage 5/6 and the still-unbuilt panels (settings, management windows).

## 6. [GAP] Iconography (spec §14)

Icons are **text-glyph placeholders** (`OVW`, `BLD`, `RES`, `MAP`, `OVL`, `CAM`, `II`, `>`). Spec §14 requires a consistent **vector icon family**, no emojis, and "no placeholder symbols in the final UI". This is the main reason acceptance #2/#5 ("looks like a finished commercial game") is **not yet met**. Needs a coherent icon set (resource icons + building-category icons) — an **art task** (WS-ART in the org proposal), likely CC0/Fab per `Docs/ASSETS.md`.

## 7. HUD & mock data

- Four HUD zones (top/left/right/bottom) ✓ implemented and the 3D world stays the protagonist (center has no widgets; `Root` is `SelfHitTestInvisible`).
- Still **mock** (correctly marked `*` / "datos demo", per spec's "document it" rule): Housing, Wellbeing, Weather, and the welcome notification. These depend on simulation systems that don't exist yet (housing capacity, happiness, climate) — wiring them is gameplay work, out of this visual task's scope, and must **not** be faked (spec §18).

## 8. [VERIFY] Responsiveness & acceptance #10

- Designed at 1080p with UMG DPI scaling; **1440p / 2160p not tested** (spec §15, acceptance #9) — needs the editor.
- **Acceptance #10 (screenshots of the rendered UI in Unreal): cannot be produced from this session** — no UE build/run here. This must be verified on the machine with the editor (the owning session), closing the editor to build, then Play-in-Editor at each target resolution.

## 9. Deliverable-doc naming (spec §19)

| Spec asks for | Present | Note |
|---|---|---|
| `UI_VISUAL_DESIGN_SYSTEM.md` | ✗ missing | content mostly lives in `UI_STYLE_GUIDE.md`; either add it or fold the two |
| `UI_STYLE_GUIDE.md` | ✓ | good |
| `UI_COMPONENT_INVENTORY.md` | ✓ | good |
| `UI_IMPLEMENTATION_REPORT.md` | ◑ named `UI_IMPLEMENTATION.md` | rename to match spec |
| (extra) `UI_KNOWN_ISSUES.md` | ✓ | useful, keep |

## 10. Acceptance criteria scorecard (spec §20)

| # | Criterion | Status |
|---|---|---|
| 1 | One visual identity across the UI | ✓ (single token system) |
| 2 | No default Unreal styling | ◑ buttons/panels styled; **icons are text placeholders**, engine tooltip chrome |
| 3 | Consistent styling on all panels/buttons/controls | ✓ for implemented; missing component types pending |
| 4 | All interaction states visually implemented | ◑ Normal/Hover/Pressed/Selected/Disabled ✓; **Focused/Loading/Transitions missing** |
| 5 | Looks like a modern commercial strategy game | ◑ blocked mainly by placeholder icons + no transitions |
| 6 | 3D world remains the protagonist | ✓ |
| 7 | Existing interactions still work | ◑ **[VERIFY]** in-editor (code preserves `CivPlayerController` flows) |
| 8 | No unrelated gameplay modified | ✓ (UI reads only; no sim/economy/world changes) |
| 9 | Works at target resolutions | ◑ **[VERIFY]** 1440p/2160p untested |
| 10 | Screenshots demonstrate real Unreal render | ✗ **cannot verify from this session** (no UE build/run) |

## 11. Recommended next actions (for the WS-UI owner / active session)

1. **[DECISION]** Settle the palette (warm-brown vs the spec's slate-blue). If slate-blue: apply the §1 token block in `CivUIStyle.h` (+3 new tokens), route primary-action fill to `GoldDark` and hover to `HoverSurface`.
2. Add **transitions** (§7) — subtle open/close/tab/notification tweens; this most closes the "commercial feel" gap.
3. Add a **canonical tooltip component** and a **progress bar** (construction progress) + **slider** (for the Settings UI-scale).
4. Add a **vector icon family** (WS-ART) to replace text-glyph placeholders (§14) — the single biggest visual-polish item.
5. Minor geometry: panels 8px, buttons 5px (§4). Add **keyboard-focus** outline (§7/§15).
6. Align deliverable docs to spec §19 names (add `UI_VISUAL_DESIGN_SYSTEM.md`, rename `UI_IMPLEMENTATION.md` → `..._REPORT.md`).
7. **[VERIFY]** In-editor: PIE screenshots at 1080p/1440p/2160p; confirm existing selection/order/build flows still work.
8. Keep mock values marked and **do not fake** housing/wellbeing/weather (§18) — wire them only when the sim systems exist.

## 12. Coordination note

No UE file was modified by this review (the active session owns `CivUIStyle.h`, `CivMainHUD.*`, `CivViewManager.cpp` and the `Docs/UI_*` files, which had uncommitted changes while this was written). All concrete edits above are recommendations for that session to apply, so there is a single writer per file (`GIT_WORKFLOW.md`).
