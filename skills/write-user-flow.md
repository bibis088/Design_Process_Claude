---
name: write-user-flow
description: "Traduit un FLUX-### en séquence d'écrans avec états et transitions. Exécuté par le ux-designer."
argument-hint: "[feature-slug]/[flux-slug]"
agent: ux-designer
---

## Rôle
Traduire un flux fonctionnel (`FLUX-###`) en séquence d'écrans avec états, interactions et transitions.

## Agents consommateurs
UX Designer 

## Distinction avec les autres documents de flux

| Document | Niveau | Produit par |
|----------|--------|------------|
| Flux fonctionnel (`FLUX-###`) | Logique métier | BA |
| User flow | Séquence d'écrans | UX Designer |
| Navigation map (`SCREENS-MAP.md`) | Vue globale | UX Designer |

Le user flow est la **traduction interface** d'un flux fonctionnel. Il ne réinvente pas la logique métier — il la traduit en séquence d'écrans concrète.

## Prérequis
- [ ] `FLUX-###` source disponible et en statut `Approved`
- [ ] Personas concernés identifiés (`PERSONA-###`)
- [ ] Guidelines accessibilité consultées dans `./design-system/guidelines/`

## Processus de génération

### Étape 1 — Lire le flux fonctionnel source
Depuis le `FLUX-###`, extraire :
- Les étapes du flux principal
- Les flux alternatifs (`ALT-##`)
- Les cas d'erreur (`ERR-##`)
- Les règles métier appliquées (`RB-###`)

### Étape 2 — Mapper chaque étape sur un écran
Chaque étape du flux fonctionnel devient un ou plusieurs états d'écran. Une étape système (traitement en arrière-plan) devient un état loading sur l'écran courant — pas un nouvel écran.

### Étape 3 — Rédiger le user flow

```markdown
# User Flow — [Nom du flux]

## Métadonnées
- Flux fonctionnel source : [FLUX-###]
- Feature : [FEAT-###]
- Personas : [PERSONA-###, PERSONA-###]
- UX Designer : [nom]
- Statut : [Draft / In Review / Approved]

## Point d'entrée
- Depuis : [Écran précédent / Notification / Deep link `app://[chemin]`]
- Trigger : [Action qui déclenche ce flux]
- Précondition : [État requis — aligné sur le FLUX-### source]

## Flux principal

### [S-XX] — [Nom de l'écran]
**Layout** : [Description de la disposition générale]

**Composants visibles** :
- Header : [titre, bouton retour, action droite]
- Corps : [liste / formulaire / carte / etc.]
- Footer : [si présent]

**Actions possibles** :
- [Élément] → [Destination S-XX / comportement]
- [Geste] → [Résultat]

**États de cet écran** :
- Loading : [description]
- Empty : [description]
- Error : [description + action de récupération]
- Success : [état nominal]

**Transition vers** : [S-XX] via [push / modal / fade]

---

### [S-XX] — [Nom de l'écran suivant]
[Même structure]

## Point de sortie
- Postcondition : [État final — aligné sur le FLUX-### source]
- Écran d'arrivée : [S-XX ou action système]

## Flux alternatifs

### ALT-01 — [Nom — aligné sur FLUX-###]
- Déclencheur : [Condition]
- Depuis : [S-XX, action]
- Écrans impliqués : [S-XX → S-XX]
- Retour au flux principal : [S-XX ou fin alternative]

## Cas d'erreur

### ERR-01 — [Nom — aligné sur FLUX-###]
- Déclencheur : [Condition d'erreur]
- Depuis : [S-XX]
- Comportement : [Ce que l'utilisateur voit]
- Récupération : [Action disponible → destination]

## Comportements spécifiques plateforme
| Comportement | iOS | Android |
|-------------|-----|---------|
| [Navigation retour] | Swipe back natif | Bouton back système |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Chaque `ALT-##` et `ERR-##` du flux fonctionnel source doit avoir un équivalent dans le user flow
- Un état système (traitement) = état loading sur l'écran courant, pas un nouvel écran
- Les transitions utilisent les conventions de `rules/design.md`
- Le user flow est produit **avant** les specs d'écran individuelles et **avant** la navigation map

## Chemin de fichier
```
design/[feature-slug]/ux/flow-[FLUX-###]-[slug].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Chaque étape du FLUX-### source est-elle couverte par au moins un écran ou état dans le user flow ? (oui / non + étapes manquantes)
2. Les cas d'erreur ERR-## sont-ils tous traduits en comportements d'écran visibles ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-user-flow "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/user-flow-FLUX-###-[slug].md

Prochaines étapes :
→ /write-navigation-map $ARGUMENTS
→ /write-screen-spec $ARGUMENTS/[screen-id]
```
