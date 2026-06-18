# WCAG 2.2 Per-Component Checklist (A + AA, with AAA notes)

Apply every item to each component. Mark items N/A explicitly when they don't apply. Criteria marked **NEW 2.2** were added in WCAG 2.2. 4.1.1 Parsing was REMOVED in 2.2 — do not report it.

## Table of contents

1. Perceivable (1.1–1.4)
2. Operable (2.1–2.5)
3. Understandable (3.1–3.3)
4. Robust (4.1)
5. AAA recommendations worth flagging

---

## 1. Perceivable

- **1.1.1 Non-text Content (A):** every `<img>` has meaningful `alt`, or `alt=""` if decorative. Icons (Material Icons, SVG, icon fonts): `aria-hidden="true"` if decorative, accessible label if interactive. Informative `<svg>`: `role="img"` + `<title>` or `aria-label`.
- **1.2.x Time-based media (A/AA):** prerecorded audio/video needs captions (1.2.2) and audio description (1.2.5 AA); audio-only needs transcript (1.2.1); live video needs captions (1.2.4 AA). N/A if no media.
- **1.3.1 Info and Relationships (A):** correct, hierarchical heading structure (one `<h1>` per view, no skipped levels); landmarks `<nav>/<main>/<header>/<footer>/<aside>`; real lists (`<ul>/<ol>`); data tables with `<th scope>` and `<caption>`; form fields with `<label for>` or `aria-labelledby` — NEVER placeholder-only; field groups with `<fieldset>/<legend>` or `role="group"` + label.
- **1.3.2 Meaningful Sequence (A):** DOM order matches visual order. Watch for `order`, `flex-direction: row-reverse`, grid placement reordering visually.
- **1.3.3 Sensory Characteristics (A):** no instructions relying only on shape/position/color ("click the green button on the right").
- **1.3.4 Orientation (AA):** no orientation lock; layout works in portrait and landscape.
- **1.3.5 Identify Input Purpose (AA):** user-data fields carry proper `autocomplete` (`name`, `email`, `tel`, `street-address`, `current-password`, …).
- **1.4.1 Use of Color (A):** states (error, selection, links in body text) not conveyed by color alone — add icon, text, or underline.
- **1.4.2 Audio Control (A):** autoplaying audio > 3s has pause/stop control.
- **1.4.3 Contrast Minimum (AA):** normal text ≥ 4.5:1; large text (≥ 24px, or ≥ 18.66px bold) ≥ 3:1. Compute real ratios from SCSS/theme values; include hover/focus states, readable-disabled text, and text over images.
- **1.4.4 Resize Text (AA):** 200% zoom without loss of content/functionality; `rem`/`em` for fonts; no fixed heights clipping text.
- **1.4.5 Images of Text (AA):** no text baked into images when real text would do (logos exempt).
- **1.4.10 Reflow (AA):** no horizontal scroll at 320px width (except data tables/maps); verify breakpoints.
- **1.4.11 Non-text Contrast (AA):** input borders, functional icons, state indicators, and focus rings ≥ 3:1 against adjacent background.
- **1.4.12 Text Spacing (AA):** layout survives line-height 1.5, letter-spacing 0.12em, word-spacing 0.16em, paragraph spacing 2em; avoid `overflow: hidden` on fixed-height text containers.
- **1.4.13 Content on Hover or Focus (AA):** tooltips/popovers are dismissible with Esc, hoverable (pointer can move into them), and persistent while relevant.

## 2. Operable

- **2.1.1 Keyboard (A) / 2.1.2 No Keyboard Trap (A):** every function reachable and operable by keyboard. Custom clickables (`(click)` on `div`/`span`) → convert to `<button>`/`<a>`, or add `tabindex="0"`, proper role, and `keydown` handling for Enter/Space. No unintended focus traps.
- **2.1.4 Character Key Shortcuts (A):** single-key shortcuts must be disableable/remappable, or active only when the component has focus.
- **2.2.1 Timing Adjustable (A) / 2.2.2 Pause, Stop, Hide (A):** timeouts extendable; carousels/auto-updating content offer pause controls.
- **2.3.1 Three Flashes (A):** nothing flashes more than 3 times per second.
- **2.4.1 Bypass Blocks (A):** "Skip to content" link as first focusable element in app shell, targeting `<main tabindex="-1">`.
- **2.4.2 Page Titled (A):** every route updates the document title (Angular `Title` service or route `title` config) descriptively.
- **2.4.3 Focus Order (A):** logical tab order; no `tabindex` > 0; dialogs/overlays trap focus (CDK `cdkTrapFocus`) and return it to the trigger on close.
- **2.4.4 Link Purpose in Context (A):** no "click here"; self-explanatory link text or contextual `aria-label`.
- **2.4.5 Multiple Ways (AA):** more than one way to reach pages (nav + search or sitemap), where applicable.
- **2.4.6 Headings and Labels (AA):** descriptive headings and labels.
- **2.4.7 Focus Visible (AA):** focus indicator always visible; NEVER `outline: none` without an equivalent replacement; focus style contrast ≥ 3:1 (ties into 1.4.11).
- **2.4.11 Focus Not Obscured Minimum (AA) — NEW 2.2:** the focused element must not be entirely hidden by sticky headers, fixed footers, cookie banners, or FABs. Fix with `scroll-padding-top` / `scroll-margin` and overlay management.
- **2.5.1 Pointer Gestures (A):** multipoint/path gestures (pinch, swipe) have single-pointer alternatives.
- **2.5.2 Pointer Cancellation (A):** actions fire on the up-event, not down.
- **2.5.3 Label in Name (A):** the accessible name contains the visible label text (watch `aria-label`s diverging from visible text — breaks voice control).
- **2.5.4 Motion Actuation (A):** device-motion features have UI alternatives.
- **2.5.7 Dragging Movements (AA) — NEW 2.2:** every drag-and-drop feature (list reordering, sliders, kanban, CDK DragDrop) has a single-pointer, non-dragging alternative (up/down buttons, numeric input, context menu).
- **2.5.8 Target Size Minimum (AA) — NEW 2.2:** interactive targets ≥ 24×24 CSS px, or spaced so a 24px circle on each target doesn't overlap neighbors. Exceptions: inline-in-text links, unmodified native controls, equivalent alternative available. Check icon buttons, pagination, chip close buttons, table row actions.

## 3. Understandable

- **3.1.1 Language of Page (A):** `<html lang="...">` correct in `index.html`; with i18n, update `lang` on language switch.
- **3.1.2 Language of Parts (AA):** parts in a different language get their own `lang`.
- **3.2.1 / 3.2.2 On Focus / On Input (A):** no automatic context change on focus or input (no auto-submit/navigation on select change without warning).
- **3.2.3 / 3.2.4 Consistent Navigation / Identification (AA):** navigation and identical components consistent across pages.
- **3.2.6 Consistent Help (A) — NEW 2.2:** help mechanisms (contact info, chat, FAQ, support links) appear in the same relative order on every page where they occur.
- **3.3.1 Error Identification (A):** form errors identified in text and tied to the field: `aria-describedby` pointing at the error message, `aria-invalid="true"` on the control. With Reactive Forms, wire `ValidationErrors` messages this way.
- **3.3.2 Labels or Instructions (A):** labels/instructions present; required fields indicated in text, not only `*` or color.
- **3.3.3 Error Suggestion (AA):** correction suggestions when known (date format, password requirements).
- **3.3.4 Error Prevention — Legal/Financial/Data (AA):** confirm, review, or undo for legal/financial/destructive operations.
- **3.3.7 Redundant Entry (A) — NEW 2.2:** multi-step flows never ask for the same info twice: auto-populate or make previously entered data selectable (e.g. "billing address = shipping").
- **3.3.8 Accessible Authentication Minimum (AA) — NEW 2.2:** login must not require cognitive tests (transcribing codes, puzzles, memorization) without alternatives: allow paste in password/OTP fields (never block `paste`), support password managers (`autocomplete="current-password"`, `username`, `one-time-code`), no cognitive CAPTCHA without alternative.

## 4. Robust

- **4.1.2 Name, Role, Value (A):** every custom control exposes name, role, state via ARIA. `ControlValueAccessor` components propagate `disabled`, label and state; custom toggles → `role="switch"` + `aria-checked`; tabs → WAI-ARIA Tabs pattern (`role="tablist/tab/tabpanel"`, `aria-selected`, arrow-key navigation); accordions, comboboxes, menus follow the corresponding WAI-ARIA APG patterns.
- **4.1.3 Status Messages (AA):** status messages (toasts, snackbars, search result counts, "saved successfully", filter counts) announced without moving focus: `role="status"`/`aria-live="polite"` for info, `role="alert"` for errors, or CDK `LiveAnnouncer`.
- **4.1.1 Parsing: REMOVED in WCAG 2.2.** Do not report.

## 5. AAA recommendations worth flagging (optional)

- **2.4.12 Focus Not Obscured Enhanced (AAA) — NEW 2.2:** focused element not hidden at all (not even partially).
- **2.4.13 Focus Appearance (AAA) — NEW 2.2:** focus indicator area ≥ 2px perimeter, contrast ≥ 3:1 between focused/unfocused states.
- **3.3.9 Accessible Authentication Enhanced (AAA) — NEW 2.2:** no cognitive test at all, not even object recognition.
- **1.4.6 Contrast Enhanced (AAA):** 7:1 / 4.5:1 — worth proposing for body text where cheap.
- **2.3.3 Animation from Interactions (AAA):** honor `prefers-reduced-motion` (treat as de-facto required; see angular-patterns.md).
