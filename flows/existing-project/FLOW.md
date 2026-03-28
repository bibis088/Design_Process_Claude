# Flow — Projet Existant

Ce document décrit la séquence pour intégrer le process AI Assisted Design dans un projet existant — sans repartir de zéro.

---

## Pré-requis avant de démarrer
- [ ] Accès au fichier Figma existant (URL)
- [ ] Accès aux specs existantes (dossier, Notion, Confluence, etc.)
- [ ] MCP Figma connecté
- [ ] Validation du Product Designer pour démarrer l'audit

---

## Phase A — Initialisation + Audit Global
**Agent :** `design-system-manager` | **Automatisation :** Semi-auto
**Gate de sortie :** Validation Product Designer du plan de migration

```
/setup-figma-init [epic-slug]
→ Le Product Designer choisit le mode C (Humain + review) pour un projet existant
→ Fournit toutes les URLs Figma existants

/audit-figma-existing [figma-url]    ← déclenché automatiquement en mode C
/audit-existing-project [project-slug]
```

**Livrables :**
- `specs/[project-slug]/audit/figma-audit.md` — score /100 sur 8 dimensions
- `specs/[project-slug]/audit/audit-migration.md` — plan P1/P2/P3

**⚠️ Product Designer :** Valider le plan de migration avant de démarrer les corrections

**Décision selon le score Figma :**
| Score | Action recommandée |
|-------|-------------------|
| > 80 | Continuer avec ajustements mineurs P1 |
| 50-79 | Traiter les P1 (1-2 sprints) puis reprendre le process |
| < 50 | Restructuration complète recommandée — voir `/setup-figma-project` |

---

## Phase B — Corrections P1 (bloquantes)
**Agent :** selon les actions du plan | **Automatisation :** selon les actions
**Gate de sortie :** Validation Product Designer

Exécuter les actions P1 identifiées dans `audit-migration.md`.

Actions fréquentes :
```
/setup-figma-tokens [project-slug]    ← si tokens manquants ou en dur
/figma-use-wrapper [action]           ← renommage frames/layers
/setup-figma-project [project-slug]   ← si structure pages à revoir
/write-accessibility-annotations [feature-slug]/[screen-id]  ← RAAM
```

---

## Phase C — Récupération des specs existantes
**Agent :** `business-analyst` | **Automatisation :** Semi-auto
**Gate de sortie :** Gate 0 — Product Designer valide

Si les specs existent mais ne suivent pas les conventions :

```
/write-cadrage [epic-slug]              ← reformaliser depuis l'existant
/write-persona [epic-slug]/[slug]       ← formaliser les personas existants
/write-brief-fonctionnel [epic-slug]    ← formaliser le brief existant
```

Si les specs sont absentes :
→ Reprendre depuis la **Phase 0 du Flow nouveau projet**

---

## Phase D — Research Complémentaire (si nécessaire)
**Agent :** `ux-researcher` | **Automatisation :** Auto + Semi-auto
**Optionnel** — uniquement si les insights utilisateurs manquent

```
/write-competitive-analysis [project-slug]   ← si pas de veille concurrentielle
/write-sector-watch [project-slug]           ← si réglementations à vérifier
/write-research-plan [epic-slug]             ← si pas d'interviews utilisateurs
```

---

## Phase E — Mise à niveau UX (si nécessaire)
**Agent :** `ux-designer` | **Automatisation :** Auto
**Optionnel** — uniquement si les specs UX manquent ou sont insuffisantes

```
/write-navigation-map [feature-slug]
/write-screen-spec [feature-slug]/[screen-id]   ← × N manquants
/write-accessibility-spec [feature-slug]
```

---

## Phase F — Reprendre le process standard
Une fois l'audit et les corrections P1 terminés, reprendre depuis la phase correspondante du **Flow nouveau projet** :

| Situation | Reprendre depuis |
|-----------|-----------------|
| Figma restructuré, specs ok | Phase 5 — Composants UI |
| Figma ok, specs à compléter | Phase 2 — User Stories |
| Tout à reconstruire | Phase -1 — Research Stratégique |
| Nouvelle feature sur projet aligné | Phase 2 — User Stories |

---

## Commande semi-auto complète (exemple)

```
"On a un projet existant [project-slug] avec un Figma à [URL].
Lance l'audit complet du fichier Figma et des specs existantes.
Arrête-toi après l'audit et présente-moi le plan de migration
pour que je valide les priorités P1."
```

---

## Différences clés avec le Flow nouveau projet

| Aspect | Nouveau projet | Projet existant |
|--------|---------------|-----------------|
| Point de départ | Research stratégique | Audit de l'existant |
| Personas | Créés from scratch | Formalisés depuis l'existant |
| Figma | Structure créée | Restructurée selon audit |
| Research | Complet | Complémentaire uniquement |
| Durée phase initiale | 2-3 jours | 1-5 jours selon l'état |
