# Automated Tooling Setup (Phase 2)

Automated tools catch ~30–40% of WCAG issues. They are a regression net, not the audit. Set them up AFTER manual fixes so the baseline is clean.

## 1. @angular-eslint template accessibility rules

In the template section of `eslint.config.js` / `.eslintrc.json` (rules live in `@angular-eslint/eslint-plugin-template`):

```json
{
  "@angular-eslint/template/alt-text": "error",
  "@angular-eslint/template/elements-content": "error",
  "@angular-eslint/template/label-has-associated-control": "error",
  "@angular-eslint/template/valid-aria": "error",
  "@angular-eslint/template/role-has-required-aria": "error",
  "@angular-eslint/template/interactive-supports-focus": "error",
  "@angular-eslint/template/click-events-have-key-events": "error",
  "@angular-eslint/template/mouse-events-have-key-events": "error",
  "@angular-eslint/template/no-autofocus": "warn",
  "@angular-eslint/template/no-distracting-elements": "error",
  "@angular-eslint/template/table-scope": "error",
  "@angular-eslint/template/button-has-type": "error"
}
```

Note: in older @angular-eslint versions these rules carry an `accessibility-` prefix (e.g. `accessibility-alt-text`). Check the installed version before configuring.

## 2. axe-core in Playwright E2E

```bash
npm i -D @axe-core/playwright
```

```ts
import AxeBuilder from '@axe-core/playwright';

test('home has no serious a11y violations', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa', 'wcag22aa'])
    .analyze();
  const serious = results.violations.filter(v => ['serious', 'critical'].includes(v.impact ?? ''));
  expect(serious).toEqual([]);
});
```

Scan every significant route AND state: open dialogs, expanded menus, forms in error state, empty states. A helper that runs AxeBuilder after each interaction step keeps tests DRY. Use `.exclude()` sparingly and only with a code comment justifying it.

## 3. pa11y-ci (route-level sweep)

```bash
npm i -D pa11y-ci
```

`.pa11yci.json`:

```json
{
  "defaults": { "standard": "WCAG2AA", "timeout": 30000, "runners": ["axe", "htmlcs"] },
  "urls": ["http://localhost:4200/", "http://localhost:4200/login", "..."]
}
```

GitLab CI job (after a build + serve step or against a preview deployment):

```yaml
a11y:
  stage: test
  script:
    - npm ci
    - npm run build
    - npx serve -s dist/<app> -l 4200 &
    - npx wait-on http://localhost:4200
    - npx pa11y-ci
```

## 4. Lighthouse CI (optional, scores trend)

`@lhci/cli` with `assert.assertions["categories:accessibility"] = ["error", {"minScore": 0.95}]`. Useful for trend visibility; axe/pa11y remain the gate.

## Reporting tool results

Map every tool finding back to its SC number in the report. axe rule IDs map to SCs in their docs metadata (`results.violations[].tags`). Deduplicate against manual findings.
