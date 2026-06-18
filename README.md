# wcag22-audit — Skill per Claude Code

Skill per audit e remediation dell'accessibilità web contro **WCAG 2.2 (livello A/AA, con raccomandazioni AAA)**, con copertura approfondita per **Angular** (e applicabile a React/Next.js e qualsiasi web app).

Verifica contro WCAG 2.2, che è un superset delle WCAG 2.1 AA richieste per legge dall'European Accessibility Act (D.Lgs. 82/2022) tramite la norma EN 301 549: usandola, superi il minimo normativo.

## Cosa contiene

```
wcag22-audit/
├── SKILL.md                          # workflow principale (inventory → checklist → tooling → report)
└── references/
    ├── wcag22-checklist.md           # checklist completa per Success Criterion (incl. i 9 nuovi della 2.2)
    ├── angular-patterns.md           # pattern di fix (CDK a11y, focus routing, ControlValueAccessor, Material)
    └── tooling.md                    # @angular-eslint a11y, @axe-core/playwright, pa11y-ci / Lighthouse CI
```

## Installazione

### macOS / Linux

```bash
# A livello di progetto (consigliato): cartella .claude/skills del repo
mkdir -p .claude/skills
cp -r wcag22-audit .claude/skills/

# Oppure a livello utente, valido per tutti i progetti
mkdir -p ~/.claude/skills
cp -r wcag22-audit ~/.claude/skills/
```

### Windows (PowerShell)

Se hai scaricato lo zip, estrailo con `Expand-Archive` (NON con il doppio click sul `.lnk`, che può creare uno shortcut invece di estrarre):

```powershell
Expand-Archive -Path .\wcag22-audit.zip -DestinationPath .\wcag22-audit-extracted -Force

# A livello di progetto
New-Item -ItemType Directory -Force -Path .\.claude\skills | Out-Null
Copy-Item -Recurse -Force .\wcag22-audit-extracted\wcag22-audit .\.claude\skills\

# Oppure a livello utente
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills" | Out-Null
Copy-Item -Recurse -Force .\wcag22-audit-extracted\wcag22-audit "$env:USERPROFILE\.claude\skills\"
```

Verifica che il percorso finale sia `.claude/skills/wcag22-audit/SKILL.md` (la skill deve stare in una sua sottocartella, non sciolta dentro `skills/`).

## Uso

In Claude Code, basta chiedere in linguaggio naturale, ad esempio:

- "Fai un audit di accessibilità WCAG 2.2 di questa app Angular"
- "Verifica l'accessibilità di questo componente dialog"
- "Sistema il contrasto e la navigazione da tastiera di questo form"

La skill si attiva automaticamente sui temi di accessibilità (a11y, WCAG, screen reader, contrasto, ARIA, axe-core, pa11y…). Per una singola issue usa la *quick triage mode*; per l'app intera segue il workflow completo con inventario persistente in `a11y-audit/INVENTORY.md` e report finale in `a11y-audit/REPORT.md`.
