# CHANGELOG — AI Assisted Design Process

Ce fichier trace l'évolution du process lui-même — agents, rules, skills, flows.
Maintenu par le **Product Designer** sur la base des rapports `process-watch`.

---

## Convention de versioning du process

```
MAJOR.MINOR.PATCH

MAJOR : Refonte d'un agent, ajout d'une phase entière, changement de chaîne
MINOR : Nouveau skill, nouvelle gate, mise à jour d'une rule
PATCH : Correction, clarification, mise à jour d'une source externe (RAAM, HIG...)
```

---

## [Unreleased]

*(Changements en cours non encore versionnés)*

---

## [1.0.0] — 2026-03-26

### Agents (7)
- `business-analyst` — Cadrage, flux, règles métier, glossaire, personas
- `product-owner` — US, FEAT, priorisation MoSCoW, livrables client
- `ux-researcher` — Research stratégique (auto) + Research utilisateur (humain guidé)
- `ux-designer` — Specs UX, flows, navigation map, accessibilité RAAM
- `ui-designer` — Composants Figma, écrans, guidelines HIG/M3, prototype React, handoff
- `design-system-manager` — Tokens, composants DS, documentation design system
- `qa-engineer` — Tests CA, RAAM, rapport QA

### Rules (7)
- `specs.md` — Conventions specs, versioning MAJOR.MINOR, dépendances inter-features
- `design.md` — Conventions design, statuts, SCREENS-MAP, liaison Figma↔docs
- `personas.md` — Template PERSONA-###, cycle de vie
- `accessibility.md` — Référentiel RAAM complet (13 catégories, niveaux A/AA)
- `figma.md` — Outils MCP (13 outils lecture/écriture), règle "lire avant écrire", Remote vs Desktop, nomenclature, grilles iOS/Android, HIG/M3
- `communication.md` — 13 gates Product Designer (Gate -1 à Gate 11), changements de scope, format validation en lot
- `qa.md` — Types de tests, rapport QA, niveaux de sévérité bugs

### Skills (53)
**Research stratégique (auto) :** `write-competitive-analysis`, `write-sector-watch`, `write-ux-benchmark`
**Research utilisateur (humain guidé) :** `write-research-plan`, `write-interview-guide`, `write-research-insights`, `write-usability-test`
**Spécification fonctionnelle :** `write-cadrage`, `write-brief-fonctionnel`, `write-flux-fonctionnel`, `write-regles-metier`, `write-glossaire`, `write-persona`
**Product Owner :** `write-feature-ticket`, `write-user-story`
**UX Design :** `write-navigation-map`, `write-user-flow`, `write-screen-spec`, `write-accessibility-spec`, `write-accessibility-annotations`, `write-contextual-help`, `write-onboarding-spec`
**UI / Figma :** `figma-use-wrapper`, `figma-read-design`, `figma-code-connect`, `setup-figma-project`, `setup-figma-tokens`, `setup-figma-grid`, `write-figma-userflow`, `setup-figma-frames`, `fetch-content-for-frames`, `create-figma-component`, `check-guidelines-compliance`, `review-figma-scope`, `write-figma-handoff`, `write-store-assets`, `write-prototype-react`
**Design System :** `write-token`, `write-component-ds`, `write-component-spec`, `write-component-ui`, `write-screen-ui`, `write-guideline`, `write-changelog`, `write-ds-documentation`
**QA :** `write-qa-report`
**Documentation client :** `write-functional-doc`, `write-client-deliverable`, `write-release-notes`
**Maintenance :** `audit-existing-project`, `audit-figma-existing`, `process-watch`
**Transversal :** `review-dod`

### Flows (2)
- `flows/new-project/FLOW.md` — Séquence complète pour un nouveau projet
- `flows/existing-project/FLOW.md` — Séquence pour intégrer le process dans un projet existant

### Documentation
- `README.md` — Guide de démarrage, guide d'exécution (3 niveaux), 13 gates, séquences semi-auto
- `CONTRIBUTING.md` — Comment ajouter un agent, une rule, un skill
- `PRODUCT_DESIGNER_ROLE.md` — Ce que le Product Designer fait vs délègue
- `CHANGELOG.md` — Ce fichier

---

## Comment mettre à jour ce fichier

Quand `process-watch` détecte une mise à jour impactante :

1. Le Product Designer décide d'appliquer la mise à jour
2. Modifier le(s) fichier(s) concerné(s)
3. Ajouter une entrée dans `[Unreleased]` avec le format :

```markdown
### PATCH — Mise à jour [Source]
- `[fichier-modifié]` : [Description du changement]
- Source : [URL de la mise à jour]
- Impact : [Ce qui change concrètement]
```

4. À chaque version stable, déplacer `[Unreleased]` vers `[X.X.X] — YYYY-MM-DD`

---

## Sources surveillées (via `process-watch`)

| Source | Fréquence de vérification | Dernière vérification |
|--------|--------------------------|----------------------|
| RAAM | Hebdomadaire | [YYYY-MM-DD] |
| Apple HIG | Hebdomadaire | [YYYY-MM-DD] |
| Material Design 3 | Hebdomadaire | [YYYY-MM-DD] |
| MCP Figma changelog | Hebdomadaire | [YYYY-MM-DD] |
| Claude / Anthropic | Hebdomadaire | [YYYY-MM-DD] |
