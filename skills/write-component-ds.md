---
name: write-component-ds
description: Documente un composant dans le design system — référence partagée UX, UI et Dev. Exécuté par le design-system-manager.
argument-hint: [component-slug]
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Documenter un composant dans le design system — référence partagée entre UX Designer, UI Designer et Devs.

## Agents consommateurs
- Design System Manager (pilote)

## Distinction avec write-component-spec et write-component-ui
| write-component-spec | write-component-ds | write-component-ui |
|---------------------|-------------------|--------------------|
| Spec UX par feature | Référence design system | Implémentation code |
| Produit par UX Designer | Produit par Design System Manager | Produit par UI Designer |
| Comportements d'un écran | Standard réutilisable global | Code natif |

## Prérequis
- [ ] Demande validée par le Design System Manager
- [ ] Spec composant UX source disponible (si applicable)
- [ ] Tokens nécessaires existants dans `./design-system/tokens/`
- [ ] Composant absent de `./design-system/components/`

## Processus de génération

### Étape 1 — Valider la généricité
Un composant du design system est **global et réutilisable**. Avant de créer :
- Est-il utilisé dans au moins 2 features différentes ?
- Est-il suffisamment générique pour ne pas être lié à un contexte spécifique ?
- Si non → garder dans la spec UX de la feature, ne pas l'élever au design system

### Étape 2 — Rédiger la documentation

```markdown
# Composant DS — [Nom]

## Métadonnées
- Version introduite : [x.x.x]
- Design System Manager : [nom]
- Statut : [Draft / In Review / Approved / Deprecated]
- Dernière mise à jour : [YYYY-MM-DD]

## Usage
[Description précise des contextes d'utilisation.
Inclure les cas où ce composant NE doit PAS être utilisé.]

## Anatomie
| Partie | Description | Obligatoire | Token associé |
|--------|-------------|------------|---------------|
| [Partie 1] | [Description] | Oui / Non | [token.nom] |
| [Partie 2] | [Description] | Oui / Non | [token.nom] |

## États
| État | Visuel | Token(s) | Interaction |
|------|--------|---------|------------|
| Default | [Description] | [color.xxx] | [tap / ...] |
| Pressed | [Description] | [color.xxx] | — |
| Disabled | [Description] | [color.interactive.disabled] | Aucune |
| Loading | [Description] | — | — |
| Error | [Description] | [color.feedback.error] | Retry |

## Variantes
| Variante | Usage | Tokens spécifiques |
|----------|-------|-------------------|
| Primary | [Usage] | [color.interactive.primary] |
| Secondary | [Usage] | [color.interactive.secondary] |
| Destructive | [Actions irréversibles] | [color.interactive.destructive] |

## Tokens utilisés
| Propriété | Token | Valeur light | Valeur dark |
|-----------|-------|-------------|------------|
| Fond | [color.background.xxx] | [hex] | [hex] |
| Texte | [color.content.xxx] | [hex] | [hex] |
| Bordure | [color.border.xxx] | [hex] | [hex] |
| Radius | [radius.md] | 8pt/dp | 8pt/dp |
| Padding | [spacing.md] | 16pt/dp | 16pt/dp |

## Implémentation iOS (SwiftUI)
[Référence vers le fichier d'implémentation]
```
Voir : design/[feature]/ui/[NomComposant].swift
```

## Implémentation Android (Compose)
[Référence vers le fichier d'implémentation]
```
Voir : design/[feature]/ui/[NomComposant].kt
```

## Accessibilité
- Label VoiceOver / TalkBack : [Standard recommandé]
- Zone tactile min. : 44x44pt (iOS) / 48x48dp (Android)
- Contraintes de contraste : [Ratio minimum selon WCAG 2.1 AA]

## Exemples d'usage

### ✅ Bon usage
[Description d'un usage correct]

### ❌ Mauvais usage
[Description d'un usage incorrect — et quel composant utiliser à la place]

## Historique des versions
| Version | Type | Changement |
|---------|------|-----------|
| [x.x.x] | MINOR | Ajout initial |
```


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design-system/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Chaque token utilisé doit exister dans `./design-system/tokens/`
- Les exemples bon/mauvais usage sont **obligatoires**
- Le composant doit référencer ses implémentations iOS et Android
- Un composant `Deprecated` reste documenté jusqu'à la prochaine version MAJOR

## Chemin de fichier
```
design-system/components/[nom].md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Le composant référence-t-il uniquement des tokens sémantiques — aucune valeur primitive en dur ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-component-ds "$ARGUMENTS" terminé
📁 design-system/components/$ARGUMENTS.md

Prochaines étapes :
→ /write-changelog $ARGUMENTS
→ /review-dod $ARGUMENTS
```
