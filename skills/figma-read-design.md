---
name: figma-read-design
description: "Pipeline de lecture Figma — structure, tokens, composants existants — avant toute opération d'écriture. Utilise get_metadata, get_variable_defs, get_screenshot et search_design_system. Exécuté par ui-designer ou design-system-manager."
argument-hint: "[figma-url ou feature-slug]"
agent: ui-designer
---

## Rôle
Lire et analyser le contenu d'un fichier ou frame Figma avant toute opération d'écriture. Produit un rapport de l'existant — structure, tokens disponibles, composants bibliothèque — pour éviter les doublons et garantir la cohérence avec le design system.

## Agents consommateurs
UI Designer  · Design System Manager  · UX Designer 

## Quand utiliser ce skill
- Avant `/create-figma-component` — vérifier que le composant n'existe pas
- Avant `/setup-figma-tokens` — vérifier les tokens déjà disponibles
- Avant `/write-figma-handoff` — vérifier l'état des frames
- Avant toute modification d'un fichier existant

## Prérequis
- [ ] URL Figma ou feature-slug fourni
- [ ] MCP Figma connecté (lecture disponible sur tous les seat types)

## Gestion des erreurs
> ❌ URL Figma invalide ou permissions insuffisantes — vérifier l'accès au fichier.
> ⚠️ Frame trop large — utiliser `get_metadata` seul pour la structure, puis `get_design_context` sur les frames individuels.

## Processus de lecture

### Étape 1 — Identifier l'utilisateur et les permissions
```
Appeler : whoami
→ Retourne : nom, email, seat type
→ Si Dev Seat : noter que l'écriture sera limitée aux drafts
```

### Étape 2 — Lire la structure du fichier
```
Appeler : get_metadata [URL]
→ Retourne : arborescence XML des layers (IDs, noms, types, positions)
→ Analyser : pages existantes, frames présentes, composants locaux
```

### Étape 3 — Extraire les tokens disponibles
```
Appeler : get_variable_defs [URL sélection ou frame]
→ Retourne : couleurs, spacing, typographie, radius utilisés
→ Analyser : quels tokens sont définis vs quels tokens manquent
```

### Étape 4 — Rechercher dans les bibliothèques
```
Appeler : search_design_system "[terme de recherche]"
→ Retourne : composants, variables, styles correspondants
→ Analyser : quels composants existent et peuvent être réutilisés
```

### Étape 5 — Capture visuelle (si nécessaire)
```
Appeler : get_screenshot [URL frame]
→ Retourne : screenshot de la sélection
→ Utiliser : vérification visuelle avant modification
```

### Étape 6 — Lire le contexte design (si génération de code nécessaire)
```
Appeler : get_design_context [URL frame]
    → Pour iOS/SwiftUI : préciser clientFrameworks: "SwiftUI"
    → Pour Android/Compose : préciser clientFrameworks: "Jetpack Compose"
→ Retourne : code structuré depuis le design
```

### Étape 7 — Produire le rapport de lecture

```markdown
# Rapport Lecture Figma — [URL / feature-slug] — [YYYY-MM-DD]

## Identité
- Utilisateur : [nom]
- Seat type : [Full / Dev]
- Permissions écriture : [Oui / Drafts uniquement]

## Structure du fichier
| Page | Frames présentes | Layers | Notes |
|------|-----------------|--------|-------|
| [Page] | [N frames] | [N layers] | [Observations] |

## Tokens disponibles
| Catégorie | Tokens trouvés | Tokens manquants |
|-----------|---------------|-----------------|
| Couleurs | [liste] | [manquants] |
| Spacing | [liste] | [manquants] |
| Typographie | [liste] | [manquants] |
| Radius | [liste] | [manquants] |

## Composants disponibles dans les bibliothèques
| Composant | Bibliothèque | Version | Réutilisable pour |
|-----------|-------------|---------|------------------|
| [Nom] | [DS / Local] | [v] | [Contexte] |

## Composants à créer (non trouvés en bibliothèque)
| Composant | Raison | Skill à utiliser |
|-----------|--------|-----------------|
| [Nom] | [Absent du DS] | `/create-figma-component` |

## Frames existantes
| Frame | Nomenclature conforme | États couverts | Notes |
|-------|----------------------|---------------|-------|
| [Nom] | ✅ / ❌ | [états] | [Observations] |

## Recommandations
- [Action 1 à réaliser avant d'écrire]
- [Action 2]
```

## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Le rapport de lecture est-il complet — tokens, composants bibliothèque et structure identifiés ? (oui / non + éléments manquants)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ figma-read-design "$ARGUMENTS" terminé
📋 Rapport : design/$ARGUMENTS/figma-read-report.md

Prochaines étapes :
→ /figma-use-wrapper [action] (si écriture nécessaire)
→ /create-figma-component $ARGUMENTS (si composants manquants)
→ /setup-figma-tokens $ARGUMENTS (si tokens manquants)
```
