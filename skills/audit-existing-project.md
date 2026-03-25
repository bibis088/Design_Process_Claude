---
name: audit-existing-project
description: Audite un projet existant — specs, Figma, codebase — avant d'intégrer ce process. Identifie les écarts, les manques et produit un plan de migration. Exécuté par le business-analyst en coordination avec l'humain.
argument-hint: [project-slug]
disable-model-invocation: false
context: fork
agent: business-analyst
---

## Rôle
Évaluer l'état d'un projet existant avant d'y intégrer ce process — identifier ce qui existe, ce qui manque, ce qui est en conflit, et produire un plan de migration progressif.

## Agents consommateurs
- Business Analyst (pilote — audite les specs)
- UX Designer (contributeur — audite le Figma)
- Design System Manager (contributeur — audite les tokens et composants)

## Prérequis
- [ ] Accès aux specs existantes (fichiers, Notion, Confluence, etc.)
- [ ] Accès au fichier Figma existant (URL)
- [ ] Validation humaine pour démarrer l'audit

## Gestion des erreurs

Si les sources sont inaccessibles :
> ❌ Sources inaccessibles — demander au Humain de partager les accès avant de continuer.

## Processus d'audit

### Étape 1 — Validation humaine de démarrage

Avant tout, obtenir la confirmation du **Humain** :

```
"Avant de démarrer l'audit, confirme :
1. Quels sont les fichiers/outils sources à auditer ?
   (Figma URL, dossier specs, Notion, Confluence, codebase...)
2. Quel est l'objectif de l'intégration ?
   (Aligner sur le process complet / Intégrer partiellement / Auditer avant refonte)
3. Quelle est la contrainte de temps ? (sprint en cours, deadline projet)"
```

### Étape 2 — Audit des specs fonctionnelles

Vérifier l'existence et la qualité de :

```markdown
## Audit Specs — [project-slug]

| Document | Existe | Format | Qualité | Aligné sur nos conventions |
|----------|--------|--------|---------|--------------------------|
| Cadrage / Brief | ✅/❌ | [format] | [Bonne/Moyenne/Faible] | ✅/⚠️/❌ |
| Personas | ✅/❌ | [format] | [Bonne/Moyenne/Faible] | ✅/⚠️/❌ |
| User Stories | ✅/❌ | [format] | [Bonne/Moyenne/Faible] | ✅/⚠️/❌ |
| Critères d'acceptance | ✅/❌ | [format] | [Bonne/Moyenne/Faible] | ✅/⚠️/❌ |
| Règles métier | ✅/❌ | [format] | [Bonne/Moyenne/Faible] | ✅/⚠️/❌ |
| Glossaire | ✅/❌ | [format] | [Bonne/Moyenne/Faible] | ✅/⚠️/❌ |
```

### Étape 3 — Audit Figma (via figma-read-design)

```
/figma-read-design [figma-url]
→ Structure des pages, nomenclature des frames et layers
→ Tokens disponibles vs valeurs en dur
→ Composants bibliothèque vs composants locaux
→ Annotations d'accessibilité présentes ou absentes
```

Évaluer contre `rules/figma.md` :

```markdown
## Audit Figma — [project-slug]

| Critère | État | Écart | Effort de migration |
|---------|------|-------|-------------------|
| Structure des pages | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
| Nomenclature frames | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
| Nomenclature layers | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
| Tokens vs valeurs en dur | ✅/⚠️/❌ | [N] valeurs en dur | [Faible/Moyen/Élevé] |
| Auto Layout | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
| Bibliothèque DS | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
| Annotations RAAM | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
| Code Connect | ✅/⚠️/❌ | [description] | [Faible/Moyen/Élevé] |
```

### Étape 4 — Audit design system

```markdown
## Audit Design System — [project-slug]

| Élément | État | Note |
|---------|------|------|
| Tokens couleurs sémantiques | ✅/⚠️/❌ | [description] |
| Dark mode | ✅/⚠️/❌ | [description] |
| Tokens typographie | ✅/⚠️/❌ | [description] |
| Tokens spacing | ✅/⚠️/❌ | [description] |
| Grilles iOS/Android | ✅/⚠️/❌ | [description] |
| Composants bibliothèque | ✅/⚠️/❌ | [N] composants |
| Versioning DS | ✅/⚠️/❌ | [description] |
| Guidelines RAAM | ✅/⚠️/❌ | [description] |
```

### Étape 5 — Produire le plan de migration

```markdown
## Plan de migration — [project-slug] — [YYYY-MM-DD]

## Résumé exécutif
[3 phrases : état actuel, principal écart, effort global estimé]

## Niveau de maturité actuel
[Faible / Moyen / Bon] — [justification courte]

## Priorités de migration

### P1 — Bloquant (à faire avant de continuer)
| Action | Skill à utiliser | Effort | Responsable |
|--------|-----------------|--------|-------------|
| [Action] | [skill] | [J/H] | [Agent] |

### P2 — Important (à faire dans le sprint suivant)
| Action | Skill à utiliser | Effort | Responsable |
|--------|-----------------|--------|-------------|
| [Action] | [skill] | [J/H] | [Agent] |

### P3 — Nice-to-have (backlog)
| Action | Skill à utiliser | Effort | Responsable |
|--------|-----------------|--------|-------------|
| [Action] | [skill] | [J/H] | [Agent] |

## Ce qu'on peut réutiliser tel quel
- [Élément réutilisable 1]
- [Élément réutilisable 2]

## Ce qu'on doit recréer
- [Élément à recréer avec justification]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. L'audit couvre-t-il les 4 dimensions — specs, Figma, design system, accessibilité ? (oui / non + dimensions manquantes)
2. Chaque écart est-il associé à un effort estimé et un skill de migration ? (oui / non + écarts non traités)
3. Le plan de migration distingue-t-il clairement les P1 bloquants des P2/P3 ? (oui / non)
4. Le Humain a-t-il validé le plan de migration avant de démarrer les actions P1 ? (oui / attendre confirmation)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ audit-existing-project "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/audit-migration.md

Prochaines étapes :
→ Valider le plan P1 avec le Humain
→ Démarrer les actions P1 selon le plan
→ /setup-figma-project $ARGUMENTS (si Figma à restructurer)
→ /write-cadrage $ARGUMENTS (si specs à recréer)
```
