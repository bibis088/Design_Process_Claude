---
name: write-guideline
description: "Rédige une guideline du design system (accessibilité, dark mode, motion) avec exemples. Exécuté par le design-system-manager."
argument-hint: "[accessibilite|dark-mode|motion]"
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Rédiger une guideline du design system — accessibilité, dark mode ou motion — avec exemples de bon et mauvais usage.

## Agents consommateurs
- Design System Manager (pilote)
- UX Designer (contributeur — relecture comportementale)

## Types de guidelines couverts
| Type | Fichier | Contenu |
|------|---------|---------|
| Accessibilité | `guidelines/accessibility.md` | WCAG, zones tactiles, labels, contrastes |
| Dark mode | `guidelines/dark-mode.md` | Tokens sémantiques, cas limites, tests |
| Motion | `guidelines/motion.md` | Durées, courbes, reduced motion |

## Prérequis
- [ ] Besoin identifié depuis une spec composant ou une review design
- [ ] Standards externes applicables identifiés (WCAG 2.1, Apple HIG, Material Design 3)
- [ ] Design System Manager a validé le besoin de la guideline

## Processus de génération

### Étape 1 — Identifier le scope
Une guideline couvre un sujet précis — pas "toutes les règles d'accessibilité" mais "les labels VoiceOver sur les éléments interactifs". Scope trop large = guideline inutilisable.

### Étape 2 — Rédiger la guideline

```markdown
# Guideline — [Titre précis]

## Métadonnées
- Version : [x.x.x]
- Catégorie : [Accessibilité / Dark mode / Motion]
- Design System Manager : [nom]
- Relu par : [UX Designer — pour cohérence comportementale] [Dev — pour faisabilité]
- Statut : [Draft / In Review / Approved]
- Standards de référence : [WCAG 2.1 AA / Apple HIG / Material Design 3]
- Dernière mise à jour : [YYYY-MM-DD]

## Principe
[1-2 phrases résumant le principe directeur de cette guideline.
Doit être mémorisable et actionnable.]

## Règles

### Règle 1 — [Titre court]
**Obligatoire / Recommandé / Optionnel**

[Description de la règle]

**Référence standard** : [WCAG 1.4.3 / HIG — Accessibility / Material — Accessibility]

#### ✅ Bon usage
[Description ou pseudo-code montrant l'application correcte]

#### ❌ Mauvais usage
[Description ou pseudo-code montrant l'erreur courante]

#### Implémentation iOS
[Indication technique SwiftUI — sans code complet]

#### Implémentation Android
[Indication technique Compose — sans code complet]

---

### Règle 2 — [Titre court]
[Même structure]

## Checklist de vérification
Pour chaque écran ou composant, vérifier :
- [ ] [Critère vérifiable 1]
- [ ] [Critère vérifiable 2]
- [ ] [Critère vérifiable 3]

## Cas limites documentés
| Cas | Comportement attendu | Justification |
|-----|---------------------|---------------|
| [Cas particulier] | [Ce qu'il faut faire] | [Pourquoi] |

## Ressources
- [Apple HIG — Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility)
- [Material Design — Accessibility](https://m3.material.io/foundations/overview/principles)
- [WCAG 2.1](https://www.w3.org/WAI/WCAG21/quickref/)
```


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design-system/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Chaque règle a un exemple **bon usage ET mauvais usage** — pas de règle sans les deux
- Les règles sont qualifiées : Obligatoire / Recommandé / Optionnel
- La checklist de vérification doit être utilisable par n'importe quel agent sans lire toute la guideline
- Les cas limites documentés évitent les interprétations divergentes en review

## Chemin de fichier
```
design-system/guidelines/[accessibility|dark-mode|motion].md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Les exemples de bon et mauvais usage couvrent-ils les cas les plus fréquents en production ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-guideline "$ARGUMENTS" terminé
📁 design-system/guidelines/$ARGUMENTS.md

Prochaines étapes :
→ /write-changelog $ARGUMENTS
→ /review-dod $ARGUMENTS
```
