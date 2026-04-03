---
name: audit-project
description: "Audit complet d'un projet existant — specs, flows, Figma (structure, tokens, composants, score /10) et plan de migration. Use when user says 'audit projet', 'audit figma', 'projet existant', 'migration', 'analyse l existant', or 'audit-project'."
argument-hint: "[project-slug]"
agent: business-analyst
---

## Rôle
Auditer un projet existant sous deux angles : specs/process et fichiers Figma.


## Prérequis
- [ ] MCP Figma connecté
- [ ] URLs Figma existants disponibles
- [ ] `US-###` ou specs existantes accessibles
- [ ] `specs/[epic-slug]/figma-urls.md` disponible si projet partiellement configuré
- [ ] URLs Figma du projet disponibles
- [ ] Accès MCP Figma connecté
- [ ] Specs existantes accessibles (dossier `specs/` ou brief fourni)

## Partie A — Audit global projet
### Étape 1 — Validation humaine de démarrage

Avant tout, obtenir la confirmation du **Product Designer** :

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

### Étape 3 — Audit Figma (via audit-figma-existing)

Pour une revue Figma complète et structurée, invoquer le skill dédié :

```
/audit-figma-existing [figma-url]
→ 8 dimensions auditées : structure, nomenclature, tokens, composants,
  auto layout, accessibilité, dark mode, Code Connect
→ Score /100 + plan de correction P1/P2/P3
→ Produit : specs/[project-slug]/audit/figma-audit.md
```

Intégrer le score Figma dans le rapport global de migration.

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
4. Le Product Designer a-t-il validé le plan de migration avant de démarrer les actions P1 ? (oui / attendre confirmation)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ audit-existing-project "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/audit-migration.md

Prochaines étapes :
→ Valider le plan P1 avec le Product Designer
→ Démarrer les actions P1 selon le plan
→ /setup-figma-project $ARGUMENTS (si Figma à restructurer)
→ /write-cadrage $ARGUMENTS (si specs à recréer)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Partie B — Audit Figma (score /10)
### Étape 0 — Initialisation

```
whoami → vérifier seat type et permissions
get_metadata [URL] → vue d'ensemble : pages, nombre de frames, layers
```

Produire l'inventaire initial :
```markdown
## Inventaire initial
- Pages : [N] — [liste des noms]
- Frames : [N total]
- Composants locaux : [N]
- Styles de texte : [N]
- Variables / styles de couleur : [N]
- Seat type : [Full / Dev]
```

---

### Étape 1 — Audit structure des fichiers et pages

**Référence :** `rules/figma.md` — Section "Structure des fichiers Figma"

```
get_metadata [URL] → liste des pages
```

```markdown
## 1. Structure des fichiers et pages

### Pages existantes
| Page trouvée | Page attendue (rules/figma.md) | Conforme | Écart |
|-------------|-------------------------------|---------|-------|
| [Nom page] | `🎨 foundations` | ✅/❌ | [description] |
| [Nom page] | `🧩 components` | ✅/❌ | [description] |
| [Nom page] | `📐 patterns` | ✅/❌ | [description] |
| [Nom page] | `📖 documentation` | ✅/❌ | [description] |
| [Nom page] | `Userflow` | ✅/❌ | [description] |
| [Nom page] | `Work In Progress UI` | ✅/❌ | [description] |
| [Nom page] | `Ready for dev` | ✅/❌ | [description] |
| [Nom page] | `Store screen` | ✅/❌ | [description] |

### Score structure : [N/8] pages conformes
### Observations
- [Observation 1]
```

---

### Étape 2 — Audit nomenclature frames et layers

**Référence :** `rules/figma.md` — Section "Nomenclature des frames" et "Nomenclature des layers"

```
get_metadata [URL page Screens] → liste des frames et layers
```

Vérifier la convention `US_###_[nom_us]_[plateforme]_[état]` :

```markdown
## 2. Nomenclature frames et layers

### Frames — échantillon analysé (20 frames)
| Frame trouvée | Conforme à la convention | Problème |
|--------------|------------------------|---------|
| [Nom] | ✅/❌ | [ex: plateforme manquante] |

### Layers — problèmes détectés
| Problème | Fréquence | Exemple | Impact |
|---------|-----------|---------|--------|
| Noms auto Figma (`Rectangle 1`, `Group 4`) | [N occurrences] | [Frame] | Haut |
| Calques non nommés | [N] | [Frame] | Haut |
| Majuscules aléatoires | [N] | [Frame] | Moyen |
| Noms trop génériques | [N] | [Frame] | Moyen |

### Score nomenclature : [N]% de frames conformes
```

---

### Étape 3 — Audit tokens et sémantique

**Référence :** `rules/figma.md` — Section "Tokens", `design-system/tokens/`

```
get_variable_defs [URL sélection représentative]
→ extraire toutes les variables et styles définis
```

```markdown
## 3. Tokens et sémantique

### Variables de couleur
| Constat | Nombre | Exemples | Gravité |
|---------|--------|---------|---------|
| Tokens sémantiques définis | [N] | `color/content/primary` | — |
| Primitives exposées directement | [N] | `blue/500` utilisé dans composants | 🔴 Critique |
| Valeurs hex en dur (pas de variable) | [N] | `#007AFF` hardcodé | 🔴 Critique |
| Tokens sans mode dark | [N] | [liste] | 🟡 Majeur |
| Tokens sans usage documenté | [N] | [liste] | 🟢 Mineur |

### Typographie
| Constat | Nombre | Gravité |
|---------|--------|---------|
| Styles de texte définis | [N] | — |
| Tailles en dur (pas de style) | [N] | 🔴 Critique |
| Styles iOS et Android séparés | ✅/❌ | 🟡 Majeur si absent |
| Styles nommés selon convention `iOS/type/body` | ✅/❌ | 🟢 Mineur |

### Spacing
| Constat | Nombre | Gravité |
|---------|--------|---------|
| Variables de spacing définies | [N] | — |
| Valeurs en dur dans auto layout | [N occurrences] | 🔴 Critique |
| Base unit 8pt/dp respectée | ✅/❌ | 🟡 Majeur |

### Radius et shadows
| Constat | Nombre | Gravité |
|---------|--------|---------|
| Variables de radius | [N] | — |
| Radius en dur | [N] | 🟡 Majeur |
| Styles d'ombres définis | [N] | — |

### Score tokens : [N]% de tokens sémantiques vs valeurs en dur
```

---

### Étape 4 — Audit composants et bibliothèque

**Référence :** `rules/figma.md` — Section "Nomenclature des composants"

```
get_metadata [URL page Components]
search_design_system "[terme générique]" → inventaire bibliothèque
```

```markdown
## 4. Composants et bibliothèque

### Inventaire composants
| Catégorie | Nombre | Publiés en biblio | Avec variants |
|-----------|--------|------------------|--------------|
| Boutons | [N] | ✅/❌ | ✅/❌ |
| Inputs | [N] | ✅/❌ | ✅/❌ |
| Cards | [N] | ✅/❌ | ✅/❌ |
| Navigation | [N] | ✅/❌ | ✅/❌ |
| Feedback (toasts, alerts) | [N] | ✅/❌ | ✅/❌ |

### Qualité des composants — échantillon (10 composants)
| Composant | Auto layout | Tokens vs dur | Variants | Nomenclature | États couverts |
|-----------|-------------|--------------|---------|--------------|---------------|
| [Nom] | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |

### Composants manquants (vs standards du secteur)
- [ ] [Composant manquant 1]
- [ ] [Composant manquant 2]

### Score composants : [N/10] critères conformes sur l'échantillon
```

---

### Étape 5 — Audit auto layout et grilles

```
get_metadata [URL frames principales] → vérifier la présence d'auto layout
get_screenshot [URL frames] → vérifier visuellement les grilles
```

```markdown
## 5. Auto Layout et grilles

### Auto layout
| Constat | Nombre | Impact |
|---------|--------|--------|
| Composants avec auto layout | [N/N] | — |
| Composants avec positionnement fixe | [N] | 🔴 Critique — fragile |
| Frames principales avec auto layout | [N/N] | — |

### Grilles
| Constat | Valeur trouvée | Valeur attendue (rules/figma.md) | Conforme |
|---------|---------------|--------------------------------|---------|
| Colonnes iOS | [N] | 4 | ✅/❌ |
| Gouttière iOS | [N]pt | 16pt | ✅/❌ |
| Marge iOS | [N]pt | 16pt | ✅/❌ |
| Colonnes Android | [N] | 4 | ✅/❌ |
| Safe areas définies | ✅/❌ | Obligatoire | ✅/❌ |

### Score auto layout / grilles : [N]% conforme
```

---

### Étape 6 — Audit accessibilité

**Référence :** `rules/accessibility.md`

```
get_metadata [URL écrans principaux] → vérifier labels et annotations
get_screenshot [URL] → vérifier visuellement les contrastes
```

```markdown
## 6. Accessibilité

### Annotations RAAM dans Figma
| Critère RAAM | Présent | Qualité | Priorité |
|-------------|---------|---------|---------|
| Labels accessibles sur éléments interactifs | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |
| Ordre de focus numéroté | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |
| Zones tactiles min. 44pt/48dp | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |
| Overlay zones tactiles | ✅/❌ | [Bonne/Partielle/Absente] | A — Important |
| Ratios de contraste indiqués | ✅/❌ | [Bonne/Partielle/Absente] | AA — Important |
| Alt text images | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |

### Score accessibilité : [N/6] critères présents
```

---

### Étape 7 — Audit dark mode

```
get_variable_defs [URL] → vérifier présence de modes light/dark
```

```markdown
## 7. Dark mode

| Constat | Statut | Note |
|---------|--------|------|
| Variables de couleur avec mode Dark | ✅/❌ | [N] variables sans mode dark |
| Composants testés en dark mode | ✅/❌ | [N] composants non vérifiés |
| Images et illustrations adaptées | ✅/❌ | — |
| Styles de texte adaptés | ✅/❌ | — |

### Score dark mode : [N/4] critères conformes
```

---

### Étape 8 — Audit Code Connect

```
get_code_connect_map → mapping existant Figma → code
```

```markdown
## 8. Code Connect

| Constat | Nombre | Note |
|---------|--------|------|
| Composants avec mapping Code Connect | [N/N] | — |
| Composants sans mapping | [N] | Handoff dégradé |
| Frameworks connectés | [SwiftUI / Compose / Autre] | — |

### Score Code Connect : [N]% composants connectés
```

---

### Étape 9 — Rapport global et plan de correction

```markdown
# Rapport Audit Figma — [project-slug] — [YYYY-MM-DD]

## Score global

| Dimension | Score | Niveau |
|-----------|-------|--------|
| 1. Structure pages | [N/8] | 🔴/🟡/🟢 |
| 2. Nomenclature | [N]% | 🔴/🟡/🟢 |
| 3. Tokens / sémantique | [N]% | 🔴/🟡/🟢 |
| 4. Composants | [N/10] | 🔴/🟡/🟢 |
| 5. Auto layout / grilles | [N]% | 🔴/🟡/🟢 |
| 6. Accessibilité | [N/6] | 🔴/🟡/🟢 |
| 7. Dark mode | [N/4] | 🔴/🟡/🟢 |
| 8. Code Connect | [N]% | 🔴/🟡/🟢 |

**Score global : [N/100]**

| Score | Niveau de maturité |
|-------|-------------------|
| 80-100 | 🟢 Bon — ajustements mineurs |
| 50-79 | 🟡 Moyen — refactorisation partielle |
| 0-49 | 🔴 Faible — refonte nécessaire |

## Ce qu'on peut réutiliser tel quel
- [Élément réutilisable 1]
- [Élément réutilisable 2]

## Plan de correction

### 🔴 P1 — Bloquant (avant tout travail sur ce fichier)
| Action | Skill à utiliser | Effort estimé | Impact |
|--------|-----------------|--------------|--------|
| Migrer [N] valeurs hex en dur vers tokens sémantiques | `/setup-figma-tokens` | [J/H] | Critique |
| Renommer [N] frames selon convention | `/figma-use-wrapper` | [J/H] | Critique |
| Ajouter mode dark sur [N] variables | `/setup-figma-tokens` | [J/H] | Critique |

### 🟡 P2 — Important (sprint suivant)
| Action | Skill à utiliser | Effort estimé |
|--------|-----------------|--------------|
| Activer auto layout sur [N] composants | `/figma-use-wrapper` | [J/H] |
| Ajouter annotations RAAM sur écrans principaux | `/write-accessibility-annotations` | [J/H] |
| Publier [N] composants dans la bibliothèque | `/figma-use-wrapper` | [J/H] |

### 🟢 P3 — Nice-to-have (backlog)
| Action | Skill à utiliser | Effort estimé |
|--------|-----------------|--------------|
| Configurer Code Connect | `/figma-code-connect` | [J/H] |
| Créer page Documentation | `/setup-figma-project` | [J/H] |
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les 8 dimensions ont-elles toutes été auditées — aucune sautée faute d'accès ? (oui / non + dimensions manquantes)
2. Le score global reflète-t-il honnêtement l'état du fichier — pas d'optimisme excessif ? (oui / non)
3. Les actions P1 bloquantes ont-elles chacune un skill de correction identifié et un effort estimé ? (oui / non + actions à compléter)
4. Le Product Designer a-t-il été consulté sur les éléments à conserver vs à refaire ? (oui / attendre confirmation)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ audit-figma-existing "$ARGUMENTS" terminé
📁 specs/[project-slug]/audit/figma-audit.md

Score global : [N/100] — Niveau : [Bon / Moyen / Faible]

⚠️ Validation Humaine requise
"Le plan de correction P1 est-il validé avant de démarrer les corrections ?"

Prochaines étapes selon le score :
→ Score > 80 : /write-cadrage (le fichier peut être utilisé avec ajustements mineurs)
→ Score 50-79 : Traiter les P1, puis /write-cadrage
→ Score < 50 : /setup-figma-project (restructuration complète recommandée)
→ Dans tous les cas : /audit-existing-project pour les specs et le code

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ audit-project "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/audit/project-audit.md
📁 specs/$ARGUMENTS/audit/figma-audit.md (score /10)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ Corriger les points P1 (bloquants)
→ /setup-figma-init $ARGUMENTS (mode C — humain + review)
```
