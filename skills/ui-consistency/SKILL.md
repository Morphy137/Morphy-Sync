---
name: ui-consistency
description: Enforce strict layout, sizing, alignment, and color consistency across UI components and screens. Use when building, modifying, or auditing interfaces to prevent button size shifts, uncentered text, text overflow/clipping, and cross-tab style drift.
---

# UI Structural & Visual Consistency

Your primary objective is to eliminate structural UI defects: inconsistent component dimensions across tabs/screens, uncentered labels/icons, text overflowing its container, and hardcoded style drift. Treat any unexplained variation between equivalent components as a bug, not an intentional variant.

## 1. Mandatory Pre-Execution Protocol

Before writing, editing, or auditing any UI code, execute these steps explicitly:

1. **Locate the Source of Truth:** Identify existing shared components (e.g., `<Button>`, `<Card>`, `<Badge>`), theme files, or CSS/design tokens in the project.
2. **Cross-Tab/Cross-Screen Inventory:** When working on a screen or tab, inspect at least 2 adjacent tabs or screens. Extract the exact properties used for equivalent controls:
   - Height / Min-Height and Padding (`px`, `py`)
   - Font size, line-height, and font-weight
   - Background, text, and border color tokens
   - Border radius and gap values
3. **Never Re-invent Controls:** If a button, input, or card exists in another tab, reuse the exact shared component or copy its exact token composition. Local style overrides on shared components are strictly forbidden unless explicitly requested by the user.

## 2. Hard Layout & Sizing Rules (Zero Tolerance)

Check every interactive control, card, and container against these concrete code constraints:

### A. Button & Control Consistency
- **Uniform Heights:** Equivalent buttons and inputs across all tabs MUST share the exact same height strategy (e.g., matching `min-height` + vertical padding + `line-height`).
- **Sibling Button Parity:** Buttons placed side-by-side in a footer, modal, or toolbar must share identical height, vertical padding, font-size, and border-radius.
- **Width Stability:** Prevent buttons from shrinking to awkward sizes or jumping width between states (e.g., "Save" vs. "Saving..."). Apply a sensible `min-width` or consistent horizontal padding token, and keep the container layout stable during loading states.

### B. Alignment & Centering inside Components
- **Explicit Flex/Grid Centering:** Every button, badge, chip, nav item, or input adornment containing text and/or icons MUST explicitly declare vertical and horizontal centering (e.g., `display: inline-flex; align-items: center; justify-content: center;` or `inline-flex items-center justify-center`).
- **Line-Height & Optical Alignment:** Ensure `line-height` inside compact controls (buttons, badges, inputs) does not push text off-center vertically (`leading-none` or `line-height: 1` / `1.2` for single-line controls when paired with flex centering).
- **Icon-Text Pairing:** Icons next to text must share a centered flex row with a fixed token `gap` and `flex-shrink: 0` on the icon so long text never crushes the icon.

### C. Text Containment & Overflow Prevention
- **No Fixed Heights on Dynamic Text Containers:** Never use a rigid `height` without `min-height` on elements whose text might wrap, scale, or translate, unless it is a strictly single-line control with explicit overflow handling.
- **Explicit Overflow Strategy:** Every text element inside a constrained parent (button, card header, tab, table cell) must have a defined behavior:
  - Single-line controls: `white-space: nowrap; overflow: hidden; text-overflow: ellipsis;` (or responsive wrapping if truncation hides critical action meaning).
  - Multi-line containers: `overflow-wrap: break-word;` and flexible height (`height: auto` / `min-height`).
- **Parent-Child Scale Check:** A child text element's `font-size` + `line-height` + vertical `padding` must never exceed the parent container's height.

### D. Color & State Consistency Across Tabs
- **Ban Hardcoded Colors:** Do not introduce raw hex, RGB, or arbitrary color classes if a semantic token or shared class exists.
- **Cross-Tab Parity:** A primary, secondary, or destructive button in Tab A must use the exact same color token, hover state, active state, disabled opacity, and focus ring as in Tab B.
- **Card & Row Groups:** Sibling cards in a grid or list must use equal-height alignment (`align-items: stretch` / `h-full`) with internal flex column spacing (`justify-content: space-between`) so footers and action buttons align horizontally across the row.

### E. Same-Screen Sibling Symmetry (Parent & Content Constraints)
Identical classes on two components DO NOT guarantee identical rendered sizes. You must inspect the parent container and content behavior for equivalent elements on the same screen:

- **Button Groups & Toolbars:** When two or more equivalent buttons appear in the same row, modal footer, or card:
  - They must share identical rendered height (`align-items: center` or `stretch` on parent, identical vertical padding and `line-height`).
  - In split actions (e.g., "Cancel" / "Confirm" in modals or mobile rows), enforce equal widths using `flex: 1 1 0%` or `grid-template-columns: repeat(N, minmax(0, 1fr))` so text length does not create lopsided buttons.
  - Prevent shrinking: every button or fixed control inside a flex row must have `flex-shrink: 0` (unless using equal `flex: 1` sizing) so adjacent long text never crushes it.
- **Repeated Cards, Tiles & List Rows:** When rendering a list or grid of equivalent items on the same screen:
  - Use CSS Grid (`grid-auto-rows: 1fr`) or Flex stretch so all cards in a row share the exact same rendered height regardless of text length.
  - Inside each card, use `display: flex; flex-direction: column; justify-content: space-between` (or `margin-top: auto` on the footer) so internal elements (titles, badges, bottom buttons) align on the exact same horizontal axis across the screen.
- **Input & Filter Bars:** Inputs, selects, and action buttons sitting on the same horizontal bar must share an explicit height token (e.g., all `h-10` / `40px` `box-sizing: border-box`). Never mix intrinsic padding-based heights on one control with fixed heights on its neighbor.

## 3. Mandatory Verification & Audit Output

When reviewing an interface or finishing an implementation, do not summarize with vague assurances. Output a **Consistency Check Table** comparing the components touched or audited:

| Component / Element | Location (Tab/Screen) | Height & Padding | Alignment Rules | Color / Token | Overflow Handling | Status / Fix |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |

### DOM / Computed Box Verification (When Browser/Preview Tool is Available)
Do not trust source classes alone. Run a script or inspect the DOM to compare `getBoundingClientRect()` on equivalent elements on the same screen and across tabs:
1. Measure `rect.height` of all buttons/inputs in the current view -> Flag any difference > 1px between equivalent controls.
2. Measure `rect.height` and bottom action `rect.top` across sibling cards in the same row -> Flag any vertical misalignment.
3. Verify `element.scrollWidth <= element.clientWidth` and `element.scrollHeight <= element.clientHeight` on all buttons, badges, and headers -> Any true overflow is an immediate fail.

### Audit Checklist (Must verify every item):
- [ ] **Cross-Tab Diff:** Are equivalent buttons/inputs across all tabs using identical size, padding, and color tokens?
- [ ] **Centering Check:** Does every button, badge, and input have explicit vertical and horizontal centering (`items-center justify-center`)?
- [ ] **Overflow Stress Test:** What happens if the label text is 2x longer? Does it clip, overflow the border, or crush adjacent icons?
- [ ] **Sibling Alignment:** In card grids or button groups, do heights and internal action rows align horizontally?

If inspecting via browser/screenshots, verify the computed bounding boxes (`getBoundingClientRect` or computed styles) of equivalent elements across tabs to confirm pixel-identical heights and colors.