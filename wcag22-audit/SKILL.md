---
name: wcag22-audit
description: Audit and fix web application accessibility against WCAG 2.2 (level A/AA, with AAA recommendations), with deep Angular-specific coverage. Use this skill whenever the user mentions accessibility, accessibilità, a11y, WCAG, screen readers, keyboard navigation, color contrast, ARIA, axe-core, pa11y, or asks to audit/verify/fix accessibility of components, pages, forms, dialogs or an entire app — even if they don't say "WCAG" explicitly. Also use it when reviewing or generating Angular templates/components where accessibility correctness matters (custom controls, dialogs, forms, routing focus management).
license: MIT
---

# WCAG 2.2 Audit & Remediation (Angular-focused)

Perform systematic, exhaustive accessibility audits against **WCAG 2.2 level AA** (flag AAA opportunities as optional recommendations) and apply fixes. Built for Angular projects, but the checklist applies to any web app.

## Non-negotiable rules

1. **Total coverage, no sampling.** Analyze EVERY component, one by one. Never skip a component because it "looks similar" to one already reviewed. Never declare the audit complete until every inventory item is verified.
2. **Persistent tracking.** Before starting, create `a11y-audit/INVENTORY.md` listing every component with a status (`pending` / `in-review` / `fixed` / `compliant`). Update it after each component. If the session is interrupted, resume from this file.
3. **Every violation must cite the Success Criterion** (e.g. "1.4.3 Contrast (Minimum) — AA") with severity (critical / serious / moderate / minor), file and line, description, and proposed fix.
4. **No regressions.** Every change must preserve existing functional behavior. If a fix requires a design decision (e.g. changing a brand color for contrast), propose it and ask — don't decide unilaterally.
5. **First rule of ARIA: don't use ARIA** when native semantic HTML exists. Use `aria-*` only where indispensable.
6. **WCAG 2.2 removed 4.1.1 Parsing** — never report it as a violation.
7. **Match the user's language** for reports and communication (Italian if the user writes in Italian); keep code identifiers and tooling config in English.

## Workflow

### Phase 0 — Inventory

Scan the filesystem and build the complete inventory:

- All `*.component.ts`, `*.component.html`, `*.component.scss|css`
- Inline templates (`template:` inside `.ts` files) — not just separate `.html` files
- Directives (`*.directive.ts`) touching DOM, focus or events; pipes producing user-visible content
- `index.html`, global styles/themes, routing files (route configs, guards affecting navigation)
- Dialogs, bottom sheets, tooltips, snackbars, menus, overlays (Angular Material / CDK or custom)
- Dynamically created components (`ViewContainerRef`, `createComponent`, `*ngComponentOutlet`)

Write the inventory to `a11y-audit/INVENTORY.md` with a total count. Then proceed component by component, starting with shared/core components (layout, header, nav, reusable form controls) because their fixes propagate.

### Phase 1 — Per-component checklist

For EACH component, work through the full checklist in **`references/wcag22-checklist.md`** (read it now if you haven't in this session). Explicitly mark non-applicable criteria as N/A so coverage is demonstrable. Pay special attention to the **9 criteria new in 2.2**: 2.4.11, 2.4.12 (AAA), 2.4.13 (AAA), 2.5.7, 2.5.8, 3.2.6, 3.3.7, 3.3.8, 3.3.9 (AAA).

For Angular-specific issues and the correct fix patterns (CDK a11y, routing focus, ControlValueAccessor, Material, reduced motion), consult **`references/angular-patterns.md`** before writing fixes — prefer CDK primitives over hand-rolled solutions.

### Phase 2 — Automated verification

After fixes, set up and run automated tooling following **`references/tooling.md`**: @angular-eslint accessibility rules, `@axe-core/playwright` scans per route/state, pa11y-ci or Lighthouse CI in the pipeline. Fail the build on serious/critical violations.

Remember (and state in the report): automated tools cover only ~30–40% of criteria. The Phase 1 manual checklist remains the source of truth.

### Phase 3 — Final report

Generate `a11y-audit/REPORT.md` with this exact structure:

```
# Accessibility Audit Report — WCAG 2.2 AA
## Executive summary        (totals: components, violations by severity and by SC)
## Component findings        (table: component → violations → fixes applied)
## Decisions required        (design/content choices needing human input)
## Residual manual testing   (screen reader NVDA/VoiceOver, keyboard-only, 200% zoom)
## Optional AAA recommendations  (2.4.12, 2.4.13, 3.3.9, others encountered)
```

## Quick triage mode

If the user asks about a **single component or a specific issue** (not a full audit), skip the inventory phases: apply the relevant sections of `references/wcag22-checklist.md` and `references/angular-patterns.md` directly to that component, still citing SC numbers and severities.
