# Angular Accessibility Fix Patterns

Prefer Angular CDK a11y primitives over hand-rolled solutions. Import from `@angular/cdk/a11y`.

## Routing & focus management

On route change, screen reader users get no signal by default. Fix:

```ts
// app shell: move focus to main content (or h1) after navigation
constructor(private router: Router, private announcer: LiveAnnouncer) {
  this.router.events.pipe(filter(e => e instanceof NavigationEnd)).subscribe(() => {
    const main = document.querySelector<HTMLElement>('main');
    main?.focus(); // requires <main tabindex="-1">
    this.announcer.announce(`Navigated to ${document.title}`, 'polite');
  });
}
```

Also set per-route titles via the `title` property in route config (Angular 14+) or a `TitleStrategy` for templated titles. Skip link in app shell, first focusable element:

```html
<a class="skip-link" href="#main-content">Salta al contenuto principale</a>
...
<main id="main-content" tabindex="-1">
```

`.skip-link` is visually hidden but becomes visible on `:focus` (never `display:none` — it must be focusable).

## Dialogs / overlays

- Use CDK `cdkTrapFocus` (or MatDialog, which traps automatically).
- Set initial focus to the first meaningful control or the dialog title (`cdkFocusInitial`).
- On close, focus MUST return to the triggering element. MatDialog does this if the trigger still exists; verify for elements removed via `*ngIf`.
- `role="dialog"` + `aria-modal="true"` + `aria-labelledby` pointing at the title.
- Esc closes the dialog (MatDialog default; verify custom overlays).

## Conditional content (`*ngIf` / `@if`)

When the focused element is removed from the DOM, focus drops to `<body>` — a 2.4.3 failure. Before removing, move focus to a logical successor with `FocusMonitor` or direct `.focus()` on the container.

## Custom form controls (ControlValueAccessor)

- The wrapping label must reach the inner control: forward an `id` and use `<label for>`, or `aria-labelledby`.
- Implement `setDisabledState()` and reflect it with `disabled`/`aria-disabled`.
- Propagate validation state: `aria-invalid` bound to control errors, `aria-describedby` pointing at the visible error message element.
- Custom toggle → `role="switch"` + `aria-checked`; custom select → prefer native `<select>` or full ARIA combobox pattern (it's hard — flag the cost).

## Reactive Forms error wiring (3.3.1)

```html
<input id="email" formControlName="email"
       [attr.aria-invalid]="email.invalid && email.touched"
       [attr.aria-describedby]="email.invalid && email.touched ? 'email-err' : null">
<p id="email-err" role="alert" *ngIf="email.invalid && email.touched">
  Inserisci un indirizzo email valido (es. nome@dominio.it)
</p>
```

## Live announcements (4.1.3)

Use `LiveAnnouncer` for ephemeral state (results count after filtering, async save success) instead of manual `aria-live` regions — it handles the timing quirks. For persistent inline status, a `role="status"` container that is ALWAYS in the DOM (content swapped, not the container itself — live regions added to the DOM at announce-time often don't fire).

MatSnackBar announces automatically; set `politeness` appropriately ('assertive' only for errors).

## Lists, menus, tabs — keyboard patterns

Use CDK `ListKeyManager` / `FocusKeyManager` for arrow-key navigation (roving tabindex) in custom listboxes, menus, tab bars. Composite widgets: ONE tab stop for the widget, arrows move within (WAI-ARIA APG).

## Drag & drop (2.5.7 — NEW 2.2)

CDK DragDrop has no built-in keyboard support. Every `cdkDrag` list needs an alternative: per-item "move up / move down" buttons (visible or in a menu), or position number input. The same control satisfies both keyboard access (2.1.1) and dragging alternative (2.5.7).

## Target size (2.5.8 — NEW 2.2)

Material icon buttons default to ~40–48px (compliant), but density settings (`density: -2` and below) or custom CSS can shrink them under 24px. Check computed sizes, not source intent. Inline chips' remove buttons and table action icons are common offenders. Fix with min-width/min-height 24px or padding, keeping visual size via background sizing if design requires small visuals.

## Focus not obscured (2.4.11 — NEW 2.2)

With sticky headers: set `scroll-padding-top: <header-height>` on the scroll container (usually `html`), and `scroll-margin-top` on anchor targets. Verify Tab navigation near sticky footers and cookie banners.

## Angular Material specifics

- Icon-only buttons: REQUIRE `aria-label` (`<button mat-icon-button aria-label="Chiudi">`).
- `mat-form-field` needs `<mat-label>` (placeholder is not a label).
- Custom themes: re-verify contrast — `mat.define-palette` hues can produce sub-4.5:1 text. Compute ratios of the actual generated values.
- `matTooltip` content is announced as the accessible description; don't duplicate it in `aria-label`.

## Reduced motion

```scss
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

For Angular Animations, inject and check `matchMedia('(prefers-reduced-motion: reduce)')` and disable via `@.disabled` binding or `provideNoopAnimations()` conditionally.

## innerHTML / dynamic markup

Markup injected via `[innerHTML]` (CMS content, markdown render) must follow the same semantic rules — audit the generator/sanitizer output, not just templates.
