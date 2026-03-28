---
name: figma-code-connect
description: "Connecte les composants Figma à leurs implémentations SwiftUI et Jetpack Compose via Code Connect. Permet aux développeurs de naviguer de Figma Dev Mode vers le code source. Exécuté par le ui-designer."
argument-hint: "[feature-slug]"
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Établir et maintenir le mapping bidirectionnel entre les composants Figma et leurs implémentations code (SwiftUI pour iOS, Jetpack Compose pour Android) — via la fonctionnalité Code Connect du MCP Figma.

## Pourquoi c'est critique

Sans Code Connect, les développeurs en Dev Mode voient les composants Figma mais ne savent pas quel fichier code correspond. Avec Code Connect :
- Dev Mode affiche le code réel du composant sous le design
- Les agents peuvent générer du code qui réutilise les composants existants
- `get_design_context` retourne les implémentations réelles plutôt que du code généré

## Agents consommateurs
- UI Designer (pilote — établit les connexions)
- Dev iOS / Android (consommateur — navigue de Figma vers le code)

## Prérequis
- [ ] Composants Figma créés et publiés dans la bibliothèque DS
- [ ] Composants SwiftUI et Compose implémentés dans le codebase
- [ ] `get_code_connect_map` disponible via `use_figma`
- [ ] MCP Figma connecté avec Full Seat

## Gestion des erreurs

Si les composants Figma ne sont pas publiés :
> ❌ Composants non publiés — publier d'abord dans la bibliothèque Design System.

Si les implémentations code sont introuvables :
> ❌ Implémentations manquantes — s'assurer que `/write-component-ui` a été exécuté.

## Processus

### Étape 1 — Lire les mappings existants
```
Appeler : get_code_connect_map
→ Retourne : mapping actuel node ID Figma → composant code
→ Identifier : quels composants sont déjà connectés
```

### Étape 2 — Obtenir les suggestions automatiques
```
Appeler : get_code_connect_suggestions
→ Figma analyse les noms et structures des composants
→ Retourne : suggestions de mappings Figma → code
→ Vérifier chaque suggestion avant de confirmer
```

### Étape 3 — Présenter les mappings pour validation

```markdown
## Mappings Code Connect suggérés — [feature-slug]

| Composant Figma | Node ID | Implémentation iOS | Implémentation Android | Confiance |
|----------------|---------|-------------------|----------------------|-----------|
| button/primary/default | [ID] | `PrimaryButton.swift` | `PrimaryButton.kt` | Haute |
| input/text/default | [ID] | `TextInput.swift` | `TextInput.kt` | Haute |
| Card / Product | [ID] | `ProductCard.swift` | `ProductCard.kt` | Moyenne |

Confirmes-tu ces mappings ?
```

### Étape 4 — Confirmer et créer les mappings
```
Appeler : send_code_connect_mappings
→ Confirme les mappings validés
→ Code Connect est établi dans Figma

Ou pour chaque mapping individuel :
Appeler : add_code_connect_map
→ node_id : [ID du composant Figma]
→ code_connect_src : [chemin vers le fichier code]
→ code_connect_name : [nom du composant dans le code]
```

### Étape 5 — Vérifier le résultat dans Dev Mode

Via `get_code_connect_map` :
```
→ Vérifier que tous les composants de la feature ont un mapping
→ Documenter les composants sans mapping (à connecter manuellement)
```

### Étape 6 — Produire le rapport de connexion

```markdown
# Rapport Code Connect — [feature-slug] — [YYYY-MM-DD]

## Composants connectés
| Composant Figma | iOS | Android | Statut |
|----------------|-----|---------|--------|
| Button / Primary | `PrimaryButton.swift` | `PrimaryButton.kt` | ✅ |
| Input / Text | `TextInput.swift` | `TextInput.kt` | ✅ |

## Composants non connectés
| Composant Figma | Raison | Action |
|----------------|--------|--------|
| [Nom] | Implémentation absente | Créer `/write-component-ui` |

## Impact sur get_design_context
Avec Code Connect actif, `get_design_context` retournera les composants réels :
- iOS : `clientFrameworks: "SwiftUI"` → retourne les vrais `PrimaryButton`, etc.
- Android : `clientFrameworks: "Jetpack Compose"` → retourne les vrais composants
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les composants publiés dans la bibliothèque DS ont-ils un mapping Code Connect iOS ET Android ? (oui / non + composants non connectés)
2. Les suggestions automatiques de Figma ont-elles été vérifiées manuellement avant confirmation — aucun mapping incorrect ? (oui / non)
3. `get_design_context` retourne-t-il maintenant les composants réels plutôt que du code générique ? (oui / non — tester sur un composant connecté)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ figma-code-connect "$ARGUMENTS" terminé
📋 Rapport : design/$ARGUMENTS/code-connect-report.md

Prochaines étapes :
→ /write-figma-handoff $ARGUMENTS (Code Connect actif avant handoff)
→ Les Dev peuvent naviguer de Figma Dev Mode → code source
→ get_design_context retourne maintenant les vrais composants
```
