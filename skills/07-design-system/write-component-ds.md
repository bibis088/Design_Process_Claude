---
name: write-component-ds
description: "Documente un composant du Design System — spec comportementale, définition DS et guideline d'usage. Use when user says 'composant design system', 'doc DS', 'guideline composant', 'component spec', or 'write-component-ds'."
argument-hint: "[component-slug]"
agent: design-system-manager
---

## Rôle
Documenter complètement un composant DS

> Note : les specs techniques (Anatomy, API, Color, Structure, Screen Reader) sont générées par Documentation UX/UI
> directement dans Figma dans le cadre de `write-component` (Étape 4).
> Ce skill produit la documentation d'usage (quand utiliser, cas interdits, exemples).
 — spec, définition et guideline en une passe.


## Prérequis
- [ ] Composant créé dans Figma DS (`write-component`)
- [ ] `US-###` sources identifiées — cas d'usage réels du composant
- [ ] `design/[feature-slug]/ux/` specs disponibles — cas d'usage réels documentés
- [ ] Tokens DS disponibles — `design-system/design-tokens.md` renseigné + `setup-figma-tokens` exécuté
- [ ] Composant créé dans Figma (`write-component` exécuté)

## Étape 1 — Spec comportementale du composant
### Étape 1 — Vérifier l'existence du composant
Consulter `./design-system/components/` avant toute création. Si un composant existant couvre le besoin → documenter pourquoi et refermer.

### Étape 2 — Rédiger la spec

```markdown
# Composant — [Nom]

## Métadonnées
- Feature : [FEAT-###]
- User Stories : [US-###, US-###]
- UX Designer : [nom]
- Statut : [Draft / In Review / Approved]

## Usage
[Dans quels contextes ce composant est utilisé.
Soyez précis — "partout" n'est pas un usage valide.]

## Anatomie
| Partie | Description | Obligatoire |
|--------|-------------|------------|
| [Partie 1] | [Description fonctionnelle] | Oui / Non |
| [Partie 2] | [Description fonctionnelle] | Oui / Non |

## États
| État | Description comportementale | Déclencheur | Interaction disponible |
|------|-----------------------------|------------|----------------------|
| Default | [Description de l'état au repos] | Initial | [tap / swipe / ...] |
| Pressed | [Retour visuel immédiat — < 100ms] | Tap | — |
| Disabled | [Non interactif, indication claire à l'utilisateur] | Condition métier | Aucune |
| Loading | [Indication de traitement en cours — non bloquant] | Action async | — |
| Error | [Indication d'erreur avec message explicatif] | Erreur système | Retry / Dismiss |

## Variantes
| Variante | Usage | Différences comportementales |
|----------|-------|------------------------------|
| Primary | [Action principale, priorité haute] | [Comportement spécifique] |
| Secondary | [Action secondaire ou complémentaire] | [Comportement spécifique] |
| Destructive | [Actions irréversibles — suppression, déconnexion] | Confirmation requise avant exécution |

## Interactions détaillées
| Geste / Action | Résultat | Plateforme |
|---------------|---------|-----------|
| Tap | [Comportement] | iOS + Android |
| Long press | [Comportement] | iOS + Android |
| Swipe | [Comportement] | [Plateforme spécifique si applicable] |

## Intention de style (pour UI Designer)
- Rôle visuel : [ex: Élément mis en avant / Action destructive / État neutre]
- Importance dans la hiérarchie : [Primaire / Secondaire / Tertiaire]
- Contexte d'apparition : [Inline / Flottant / Plein écran]

## Accessibilité
- [ ] Label VoiceOver / TalkBack : [Formulation exacte]
- [ ] Hint (si action non évidente) : [Formulation exacte]
- [ ] Zone tactile minimum : 44x44pt (iOS) / 48x48dp (Android)
- [ ] Comportement en texte agrandi : [Description]
- [ ] État désactivé communiqué autrement que par la couleur

## Comportements spécifiques plateforme
| Comportement | iOS | Android |
|-------------|-----|---------|
| [Aspect natif] | [Comportement iOS] | [Comportement Android] |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Aucune valeur visuelle dans ce document (pas de couleurs, tailles, tokens)
- L'état `Destructive` requiert toujours une confirmation avant exécution
- L'état `Error` doit toujours proposer une action de récupération
- La section "Intention de style" remplace les tokens — elle guide sans contraindre

## Chemin de fichier
```
design/[feature-slug]/ux/component-[nom].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les états sont-ils couverts — default, pressed, disabled, loading, error ? (oui / manque : ...)
2. La section 'Intention de style' décrit-elle le rôle visuel sans valeurs absolues — aucune couleur ou taille en dur ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-component-spec "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/component-[slug].md

Prochaines étapes :
→ /write-component-ui $ARGUMENTS
→ /write-component-ds $ARGUMENTS (si ajout DS)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Définition DS (variants, tokens, états)
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
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 3 — Guideline d'usage
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
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-component-ds "$ARGUMENTS" terminé
📁 design-system/components/[component-slug].md
📁 design-system/guidelines/[component-slug].md

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
