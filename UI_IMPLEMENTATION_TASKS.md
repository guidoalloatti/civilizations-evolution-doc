# UI_IMPLEMENTATION_TASKS — Civilization Evolutions

> **Task list for the active UI session** (owner of the Unreal UI files). Derived from the user's decisions (2026-11-04, [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) DR-14) and the gap audit in [UI_DESIGN_REVIEW.md](UI_DESIGN_REVIEW.md). These tasks are to be implemented **by the session that owns the Unreal UI files**; they are **not** to be edited from the documentation worktree (one writer per file — [GIT_WORKFLOW.md](GIT_WORKFLOW.md)). No competing implementation.
>
> Version 1.0 · 2026-11-04 · Status: ready for the UI session.

## Ground rules (from DR-14)

- **Preserve** the current UI architecture: C++ UMG, centralized tokens (`CivUIStyle.h`), current `FButtonStyle` buttons, sim/presentation separation, and **all existing functionality**. Do **not** rewrite.
- Do the palette migration **through the centralized style system** (`CivUIStyle.h`) so no widget hardcodes colors.
- Keep mock values marked (`*`) and **do not fabricate** housing/wellbeing/weather (spec §18) — wire only when the source systems exist.
- **Not "complete"** until real Unreal screenshots are checked at **≥1080p and 1440p** (UI-06).

## Owned files (the active UI session edits these; the docs worktree does not)

`Source/CivilizationSimulator/UI/CivUIStyle.h` · `UI/CivMainHUD.{h,cpp}` · `Core/CivHUD.{cpp,h}` (Canvas overlays) · `View/CivViewManager.cpp` (as needed) · `Config/DefaultGame.ini`/`DefaultEngine.ini` (DPI only, with care) · `Content/UI/**` (new icon/font assets) · `Docs/UI_*` (their UI docs). New art assets also touch `Content/` (coordinate with the art workstream).

---

## Tasks (in the user's priority order)

### UI-01 — Finalize & apply the official slate-blue palette  · Priority P0
- **Scope:** replace the warm-brown token values in `CivUIStyle.h` with the official slate-blue sRGB tokens in [UI_DESIGN_REVIEW.md](UI_DESIGN_REVIEW.md) §1. Add the 3–4 new tokens: `HoverSurface` (#30414D), `GoldDark` (#9B7C49, primary-action fill), `TextDisabled` (#56616A), optional `BorderSoft` (#35434D).
- **Then:** route button **hover** to `HoverSurface` and the **primary-action** button fill to `GoldDark` (spec §6); point disabled text at `TextDisabled`.
- **Files:** `CivUIStyle.h` (values), `CivMainHUD.cpp`/`CivHUD.cpp` (only where a color is passed explicitly — prefer the token).
- **Dependencies:** none (keystone; do first).
- **Acceptance:** every surface/border/text color resolves from a `CivUI::` token; no hardcoded warm value remains (grep for stray `FColor(`); the HUD reads as dark slate-blue with gold accents; sim/UI separation and all existing interactions unchanged.

### UI-02 — Subtle hover / selection / panel transitions  · Priority P1
- **Scope:** add the transitions missing today (spec §7, 120–200 ms, ease-out). Hover/selected currently swap brushes instantly. Add subtle tweens on: panel open/close (construction panel, notification center, management window), tab change, hover elevation, and notification entry.
- **Approach (no architecture change):** UMG C++ — drive a short interpolation in `NativeTick` (render opacity / translation / brush lerp) or add `UWidgetAnimation`s; keep critical actions instant.
- **Files:** `CivMainHUD.cpp` (panel toggles, `NativeTick`).
- **Dependencies:** UI-01 (so tweens use final colors).
- **Acceptance:** opening/closing panels and hovering feel smooth (≈120–200 ms), no animation blocks input, no per-frame cost when idle (respect the 0.2 s refresh throttle for data, separate from transition timers).

### UI-03 — Replace text placeholders with a vector icon family  · Priority P1 · (needs art)
- **Scope:** replace the text-glyph placeholders (`OVW`, `BLD`, `RES`, `MAP`, `OVL`, `CAM`, `II`, `>`, resource/nav labels) with a **consistent vector icon family** (spec §14): consistent stroke/scale, neutral default color, gold for selected/important, semantic color only when meaningful. No emojis, no unrelated stock icons.
- **Files:** `Content/UI/Icons/**` (new `.uasset` / SVG-imported textures), `CivMainHUD.cpp` (swap glyph text for `UImage` with the icon brush), `CivUIStyle.h` (optional icon-brush helper).
- **Dependencies:** UI-01 (tint tokens). **Art dependency:** sourcing the icon set (CC0/Fab per `Docs/ASSETS.md`) is a WS-ART item — coordinate so only one session adds the `.uasset`s (binary, unmergeable — [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) §binaries).
- **Acceptance:** no text-glyph placeholder remains in the shipped HUD; resource icons (one per resource) and building-category icons present; icons legible at small size; selected state uses gold. This is the biggest blocker for acceptance #2/#5.

### UI-04 — Complete the shared component library  · Priority P1
- **Scope:** add the missing canonical components as reusable `Make*` helpers (one implementation each, spec §8): **dropdown**, **input field**, **slider** (needed for Settings/UI-scale), **progress bar** (construction progress), and a **shared styled tooltip** (dark `panel/scrim`, 6px radius, ~0.4 s delay) replacing the engine-default tooltip. Also a canonical **section header** and **separator**.
- **Files:** `CivMainHUD.cpp` (helpers), `CivUIStyle.h` (brush/style helpers if shared).
- **Dependencies:** UI-01.
- **Acceptance:** each component has a single canonical styled implementation reused across panels; the tooltip component is used by icon-only controls (accessibility, spec §15); a progress bar is ready for construction work display.

### UI-05 — Refine geometry, spacing, typography, keyboard focus  · Priority P2
- **Scope:** panels → **8 px** radius, buttons → **5 px** (spec §4); confirm padding 12–16 / section 16–20 / component 8–12. Typography: add a dedicated **important-value** size (18–22 px) for headline numbers; align section size to 16. Add a **keyboard-focus** outline (gold 1.5 px) on focusable controls (spec §7/§15). **Fonts:** keep engine Roboto unless the user approves importing Rajdhani/Inter (a new `Content/UI/Fonts/` dependency — do not add without approval).
- **Files:** `CivUIStyle.h` (radii/font helpers), `CivMainHUD.cpp`.
- **Dependencies:** UI-01.
- **Acceptance:** geometry matches the spec values; headline numbers use the larger size; focusable controls show a visible focus ring; no new font dependency added without approval.

### UI-06 — Verify the rendered UI in Unreal  · Priority P0 (gate) · [VERIFY]
- **Scope:** build the UE project (editor closed), Play-in-Editor, and capture **screenshots at 1080p and 1440p** (ideally 2160p too). Confirm existing flows still work: selection/box-select, right-click orders, build mode + placement feedback, pause/speed, info panel, notifications.
- **Files:** none (verification); update `Docs/UI_IMPLEMENTATION_REPORT.md` with results + screenshots.
- **Dependencies:** UI-01..UI-05 (verify after each meaningful change; final gate after all).
- **Acceptance (the completion gate, DR-14):** screenshots attached at ≥1080p and 1440p; layout holds (no broken anchoring/DPI); all listed interactions still function. **Until this passes, the visual system is not "complete."**

---

## Dependency summary

```
UI-01 (palette) ──► UI-02 (transitions)
                ├──► UI-03 (icons)      [+ WS-ART sources the asset set]
                ├──► UI-04 (components)
                └──► UI-05 (geometry/type/focus)
UI-01..UI-05 ──► UI-06 (in-engine verification gate)
```

## Documentation alignment (active UI session's own `Docs/UI_*`)

- Add **`UI_VISUAL_DESIGN_SYSTEM.md`** (currently missing) or fold it into `UI_STYLE_GUIDE.md`; update the style guide's palette/§2 to the slate-blue tokens.
- Rename **`UI_IMPLEMENTATION.md` → `UI_IMPLEMENTATION_REPORT.md`** to match the spec's deliverable names (§19); record screenshots + resolution results there.
- Keep `UI_KNOWN_ISSUES.md` current (remove items as closed).

## Global acceptance (spec §20) — not complete until all hold

One visual identity (slate-blue) · no default UE styling (incl. real icons, not placeholders) · consistent styling on all controls · all interaction states incl. transitions + focus · reads as a modern commercial strategy UI · 3D world stays protagonist · existing interactions intact · no unrelated gameplay modified · works at 1080p/1440p(/2160p) · **real Unreal screenshots** demonstrate it.
