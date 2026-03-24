# Update_Antoine — Guide du projet

Copie modifiée et enrichie de `Agent_skills_Joris`.
Chaque fichier modifié est annoté avec `✅ MODIFIÉ [X]` ou `✅ NOUVEAU [X]` — la lettre renvoie au tableau de référence en bas de ce document.

---

## 🚀 Guide de démarrage rapide

### Nouveau projet — par où commencer ?

```
Étape 1 — Cadrage (BA)
    /write-cadrage [epic-slug]
    → Pose les 6 questions au client
    → Produit : specs/[epic-slug]/cadrage.md

Étape 2 — Personas (BA)
    /write-persona [epic-slug]/[persona-slug]
    → Minimum 2 : utilisateur régulier + nouvel utilisateur
    → Produit : specs/[epic-slug]/personas/PERSONA-###-[slug].md

Étape 3 — Brief Fonctionnel (BA)
    /write-brief-fonctionnel [epic-slug]
    → Depuis les réponses au cadrage
    → Produit : specs/[epic-slug]/EPIC.md

Étape 4 — Flux fonctionnels (BA)
    /write-flux-fonctionnel [epic-slug]/[flux-slug]
    → Un flux par parcours utilisateur principal
    → Produit : specs/[epic-slug]/FLUX-###-[slug].md

Étape 5 — Règles métier (BA)
    /write-regles-metier [epic-slug]
    → Produit : specs/[epic-slug]/regles-metier.md

Étape 6 — User Stories (PO)
    /write-feature-ticket [epic-slug]/[feature-slug]
    /write-user-story [epic-slug]/[story-slug]
    → Produit : specs/[epic-slug]/features/ + stories/

Étape 7 — Navigation map (UX)
    /write-navigation-map [feature-slug]
    → Produit : design/[feature-slug]/SCREENS-MAP.md

Étape 8 — User flows (UX)
    /write-user-flow [feature-slug]/[flux-slug]
    → Produit : design/[feature-slug]/ux/user-flow-FLUX-###.md

Étape 9 — Specs écrans (UX)
    /write-screen-spec [feature-slug]/[screen-id]-[screen-slug]
    → Produit : design/[feature-slug]/ux/screens/S-XX-[nom].md

Étape 9b — Spec accessibilité RAAM (UX)
    /write-accessibility-spec [feature-slug]
    → Critères RAAM A + AA, annotations par écran, checklist conformité
    → Produit : design/[feature-slug]/ux/accessibility-spec.md
    → Annotations Figma à compléter en parallèle

Étape 10 — Design system (DSM — si besoin)
    /write-token [type]/[token-slug]
    /write-component-ds [component-slug]
    → Produit : design-system/tokens/ + components/

Étape 11 — Implémentation UI (UI Designer)
    /write-component-ui [feature-slug]/[component-slug]
    /write-screen-ui [feature-slug]/[screen-id]-[screen-slug]
    → Produit : design/[feature-slug]/ui/

Étape 12 — QA (QA Engineer)
    → Teste chaque US-### contre ses CA-##
    → Produit : specs/[epic-slug]/qa/QA-###-[slug].md
```

### Gates de validation obligatoires

Avant chaque passage de statut, lancer :
```
/review-dod [type-livrable]/[id-livrable]
```

| Gate | Qui valide | Condition |
|------|-----------|-----------|
| `Draft` → `In Review` | Agent auteur | DoD interne cochée |
| `In Review` → `Approved` | PO | Cohérence fonctionnelle validée |
| `Approved` → `Dev Ready` | Tech Lead | Faisabilité technique confirmée |
| `Dev Ready` → `Done` | PO + QA | Critères d'acceptance et rapport QA validés |

### Nouvelle feature sur projet existant

Si le projet a déjà un `EPIC.md` et des personas approuvés, démarrer directement à l'étape 6 :
```
/write-feature-ticket [epic-slug]/[feature-slug]
```

### Besoin client non formalisé (backlog)

Si le besoin arrive brut depuis le client :
```
1. Créer specs/[epic-slug]/backlog/BACKLOG-###-[slug].md
2. Faire valider par le client (statut : Validated)
3. Lancer /write-cadrage [epic-slug]
```

---

## Arborescence

```
Update_Antoine/
├── agents/
│   ├── business-analyst.md
│   ├── product-owner.md
│   ├── ux-designer.md
│   ├── design-system-manager.md
│   ├── ui-designer.md
│   └── qa-engineer.md
│
├── rules/
│   ├── specs.md
│   ├── design.md
│   ├── personas.md
│   ├── qa.md
│   └── communication.md
│
├── skills/                         ← 19 skills invocables
│
├── specs/
│   └── [epic-slug]/
│       ├── EPIC.md
│       ├── cadrage.md
│       ├── regles-metier.md
│       ├── glossaire.md
│       ├── personas/
│       ├── features/
│       ├── stories/
│       ├── qa/
│       └── backlog/
│
├── design/
│   └── [feature-slug]/
│       ├── SCREENS-MAP.md
│       ├── DESIGN-BRIEF.md
│       ├── COMPONENTS.md
│       ├── ux/
│       └── ui/
│
└── design-system/
    ├── tokens/
    ├── components/
    └── guidelines/
```

---

## Chaîne des agents

```
client (backlog)
    ↓
business-analyst ──→ specs/[epic-slug]/
    ↓ EPIC + FLUX + PERSONA + RB
product-owner ─────→ specs/[epic-slug]/features/ + stories/
    ↓ FEAT + US
ux-designer ───────→ design/[feature-slug]/ux/
    ↓ SCREENS-MAP + user-flows + S-XX + accessibility-spec
ui-designer ───────→ design/[feature-slug]/ui/
    ↓ composants + écrans
qa-engineer ───────→ specs/[epic-slug]/qa/
    ↓ QA-###
Done (PO valide)
```

```
design-system-manager
    ← consulté par UX Designer (guidelines accessibilité, comportements)
    ← consulté par UI Designer (tokens, composants disponibles)
    ← sollicité par UI/UX (demandes de nouveaux tokens ou composants)
    → valide et documente tout ajout au design-system/
    → notifie les agents consommateurs en cas de breaking change
```

```
Figma (canal transversal)
    ← UX Designer (frames, annotations RAAM, ordre de focus)
    ← UI Designer (handoff, statut Ready for dev)
    → QA Engineer (référence visuelle pour vérification)
    → Dev (implémentation)
    Règle : chaque frame référence sa spec .md | chaque spec référence son URL Figma
```

---

## Référence des changements

### Agents

| Réf | Fichier | Description |
|-----|---------|-------------|
| A | business-analyst | Contexte mobile iOS/Android dans Brief + colonne Plateforme dans flux |
| B | business-analyst | Trigger élargi au raffinage de specs existantes |
| C | business-analyst + product-owner | Conventions de nommage inter-agents |
| D | business-analyst | Cadrage initial — 6 questions posées en une fois |
| E | product-owner | Relation BA → PO formalisée |
| F | product-owner | Personas génériques |
| G | product-owner | DoD adaptable (socle + exigences additionnelles) |
| H | product-owner | Convention dossier specs/ alignée inter-agents |
| I | business-analyst | DoD BA ajoutée |
| J | product-owner | DoD PO refactorisée |
| K | ux-designer | Relation amont BA + PO formalisée |
| L | ux-designer | Périmètre UX vs UI — tokens supprimés |
| M | ux-designer | DoD UX ajoutée |
| N | ux-designer + product-owner | Structure dossiers Option B |
| O | design-system-manager | Position dans la chaîne + rôle de validation |
| P | design-system-manager | Structure dossiers confirmée (transversal) |
| Q | design-system-manager | Versioning sémantique + changelog |
| R | design-system-manager | 4 DoD (token, composant, guideline, breaking change) |
| S | ui-designer | Titre et intro recentrés UI Mobile uniquement |
| T | ui-designer | Relation Design System Manager formalisée |
| U | ui-designer | Relation UX Designer formalisée |
| V | ui-designer | 2 DoD (composant, écran) |
| W | ux-designer | Relation Design System Manager — tag de référence ajouté |
| X | ui-designer | Exemples de code SwiftUI/Compose remplacés par conventions de style |
| AB | ux-designer | Navigation map ajoutée comme livrable explicite |

### Rules

| Réf | Fichier | Description |
|-----|---------|-------------|
| Y | rules/personas | Nouveau — template persona + cycle de vie |
| Z | rules/design | Template SCREENS-MAP.md — navigation map |
| AA | rules/design | Distinction flux fonctionnel / user flow / navigation map |
| A | rules/design | Structure dossiers alignée Option B |
| B | rules/design | Statuts en anglais + agents responsables par transition |
| C | rules/design | Template structuré pour REVIEW-[date].md |
| D | rules/specs | Statuts en anglais + agents responsables |
| E | rules/specs | Numérotation EPIC-### alignée sur convention agents |
| F | rules/specs | Backlog documenté comme zone de staging client |

### Skills (19)

| Priorité | Skill | Agent pilote |
|----------|-------|-------------|
| Haute | `write-cadrage` | BA |
| Haute | `write-brief-fonctionnel` | BA |
| Haute | `write-flux-fonctionnel` | BA |
| Haute | `write-user-story` | PO |
| Haute | `write-screen-spec` | UX |
| Haute | `write-token` | DSM |
| Moyenne | `write-persona` | BA |
| Moyenne | `write-navigation-map` | UX |
| Moyenne | `write-component-ui` | UI |
| Moyenne | `write-cadrage` | BA |
| Basse | `write-user-flow` | UX |
| Basse | `write-regles-metier` | BA |
| Basse | `write-glossaire` | BA |
| Basse | `write-feature-ticket` | PO |
| Basse | `write-component-spec` | UX |
| Basse | `write-screen-ui` | UI |
| Basse | `write-component-ds` | DSM |
| Basse | `write-guideline` | DSM |
| Basse | `write-changelog` | DSM |
| Transversal | `review-dod` | Tous |
