---
name: setup-figma-library
description: "Connecte la bibliothèque Design System au fichier Projet Figma — active les tokens, composants et styles partagés. Exécuté par le design-system-manager après setup-figma-tokens et setup-figma-grid."
argument-hint: "[epic-slug]"
agent: design-system-manager
---

## Rôle
Connecter le fichier Design System au fichier Projet pour que tous les composants, variables et styles du DS soient accessibles dans le fichier de travail.

## Prérequis
- [ ] Fichier Design System créé (`setup-figma-project`)
- [ ] Tokens et modes configurés (`setup-figma-tokens`)
- [ ] Grilles configurées (`setup-figma-grid`)
- [ ] MCP Figma connecté

## Processus

### Étape 1 — Vérifier les fichiers via MCP

```
get_metadata [URL fichier Design System]
→ Vérifier : variables publiées, styles publiés, composants publiés

get_metadata [URL fichier Projet]
→ Vérifier : bibliothèques connectées actuellement
```

### Étape 2 — Publier la bibliothèque DS

Via `use_figma` sur le fichier Design System :

```
Action : Publier la bibliothèque
Éléments à publier :
  ✅ Variables (tokens couleurs, spacing, radius, motion)
  ✅ Text Styles (ios/* et android/*)
  ✅ Effect Styles (shadows)
  ✅ Composants (tous les composants de 🧩 components)
```

> ⚠️ La publication nécessite un Figma Team ou Organization plan.
> En plan Starter : partager le fichier DS en lien et copier les composants manuellement.

### Étape 3 — Connecter la bibliothèque dans le fichier Projet

Via `use_figma` sur le fichier Projet :

```
Action : Activer la bibliothèque
Source : [NomProjet] — Design System
Résultat attendu :
  → Variables DS disponibles dans le panneau Variables
  → Text Styles DS disponibles dans les options de texte
  → Composants DS disponibles dans Assets
```

### Étape 4 — Vérifier la connexion

```
get_variable_defs [URL fichier Projet]
→ Les variables du DS doivent apparaître comme "external"
→ Vérifier : color/interactive/primary, spacing/md, radius/lg

search_design_system [URL fichier Projet] "button"
→ Les composants du DS doivent être trouvables
```

### Étape 5 — Documenter les URLs dans le rapport

```markdown
## Rapport connexion bibliothèque — [EPIC-###] — [YYYY-MM-DD]

### Fichiers connectés
- Design System : [URL Figma DS]
- Fichier Projet : [URL Figma Projet]

### Éléments publiés et connectés
| Élément | Publié | Accessible dans le Projet |
|---------|--------|--------------------------|
| Variables couleurs | ✅ | ✅ |
| Variables spacing | ✅ | ✅ |
| Text Styles iOS | ✅ | ✅ |
| Text Styles Android | ✅ | ✅ |
| Effect Styles | ✅ | ✅ |
| Composants | ✅ | ✅ |

### Plan Figma
- Plan actuel : [Starter / Professional / Organization]
- Publication bibliothèque : [Active / Manuel — copie des composants]
```

## Phase de validation — niveau standard

1. Les variables du DS sont-elles accessibles dans le fichier Projet — apparaissent-elles comme "external" dans le panneau Variables ? (oui / non)
2. Les Text Styles iOS et Android sont-ils disponibles dans le fichier Projet ? (oui / non)
3. Les composants publiés sont-ils trouvables dans Assets du fichier Projet ? (oui / non)

> Si tout validé, le skill se termine. Si non, reprendre depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ setup-figma-library "$ARGUMENTS" terminé
🔗 DS → Projet connectés
  Variables : ✅ | Text Styles : ✅ | Composants : ✅

Prochaines étapes :
→ /setup-figma-permissions $ARGUMENTS
→ /write-figma-userflow $ARGUMENTS
```
