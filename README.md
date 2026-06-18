# wcag22-audit — a Claude Code skill

A skill for web accessibility audit and remediation against **WCAG 2.2 (level A/AA, with AAA recommendations)**, with deep coverage for **Angular** (and applicable to React/Next.js and any web app).

It checks against WCAG 2.2, a superset of the WCAG 2.1 AA required by law under the European Accessibility Act (in Italy, Legislative Decree 82/2022) via the EN 301 549 standard — so using it clears more than the legal minimum.

## What's inside

```
wcag22-audit/
├── SKILL.md                          # main workflow (inventory → checklist → tooling → report)
└── references/
    ├── wcag22-checklist.md           # full per-Success-Criterion checklist (incl. the 9 new ones in 2.2)
    ├── angular-patterns.md           # fix patterns (CDK a11y, focus routing, ControlValueAccessor, Material)
    └── tooling.md                    # @angular-eslint a11y, @axe-core/playwright, pa11y-ci / Lighthouse CI
```

## Installation

Get the files first — either clone the repo, or download the ZIP from GitHub (**Code → Download ZIP**).

```bash
git clone https://github.com/Yurcek/wcag22-audit.git
```

### macOS / Linux

```bash
# Project level (recommended): the repo's .claude/skills folder
mkdir -p .claude/skills
cp -r wcag22-audit .claude/skills/

# Or user level, available across all projects
mkdir -p ~/.claude/skills
cp -r wcag22-audit ~/.claude/skills/
```

### Windows (PowerShell)

If you downloaded the ZIP, extract it with `Expand-Archive` (NOT by double-clicking the `.lnk`, which can create a shortcut instead of extracting):

```powershell
Expand-Archive -Path .\wcag22-audit.zip -DestinationPath .\wcag22-audit-extracted -Force

# Project level
New-Item -ItemType Directory -Force -Path .\.claude\skills | Out-Null
Copy-Item -Recurse -Force .\wcag22-audit-extracted\wcag22-audit .\.claude\skills\

# Or user level
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills" | Out-Null
Copy-Item -Recurse -Force .\wcag22-audit-extracted\wcag22-audit "$env:USERPROFILE\.claude\skills\"
```

Make sure the final path is `.claude/skills/wcag22-audit/SKILL.md` (the skill must live in its own subfolder, not loose inside `skills/`).

## Usage

In Claude Code, just ask in natural language, for example:

- "Run a WCAG 2.2 accessibility audit of this Angular app"
- "Check the accessibility of this dialog component"
- "Fix the contrast and keyboard navigation of this form"

The skill activates automatically on accessibility topics (a11y, WCAG, screen readers, contrast, ARIA, axe-core, pa11y…). For a single issue it uses *quick triage mode*; for a whole app it follows the full workflow, with a persistent inventory in `a11y-audit/INVENTORY.md` and a final report in `a11y-audit/REPORT.md`.

## License

MIT — see [LICENSE](LICENSE).
