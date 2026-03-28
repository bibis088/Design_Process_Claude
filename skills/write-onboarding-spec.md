---
name: write-onboarding-spec
description: "Spécifie le parcours d'onboarding in-app — walkthroughs, tooltips de découverte, écrans d'introduction — pour les nouveaux utilisateurs. Exécuté par le ux-designer."
argument-hint: "[epic-slug]"
agent: ux-designer
---

## Rôle
Produire la spec complète du parcours d'onboarding pour les nouveaux utilisateurs — depuis le premier lancement jusqu'à la première action réussie.

## Agents consommateurs
UX Designer  · UI Designer  · PO 

## Prérequis
- [ ] Persona nouvel utilisateur disponible (`PERSONA-###-new-user`)
- [ ] User stories de l'epic disponibles
- [ ] Direction artistique définie (pour cohérence du ton)

## Gestion des erreurs
> ❌ Personas manquants — lancer d'abord `/write-persona $ARGUMENTS` (génère les 3 personas).

## Processus de génération

### Étape 1 — Définir l'objectif de l'onboarding
L'onboarding réussit quand le nouvel utilisateur a accompli **une action à valeur** — pas juste parcouru des slides.

### Étape 2 — Choisir le format

| Format | Quand l'utiliser |
|--------|-----------------|
| Walkthrough (écrans d'intro) | App complexe, concept nouveau, premier lancement |
| Tooltips contextuels | App simple, découverte progressive in-context |
| Empty state actif | Onboarding implicite via l'état vide |
| Combinaison | App avec permission critique (notifications, caméra) |

### Étape 3 — Rédiger la spec

```markdown
# Spec Onboarding — [EPIC-###] [NomApp] — [YYYY-MM-DD]

## Objectif
[L'utilisateur a réussi son onboarding quand : ...]

## Persona ciblé
- [PERSONA-###] — [Nom] — [Niveau tech] — [Contexte d'usage]

## Format choisi
[Walkthrough / Tooltips / Empty state / Combinaison]
Justification : [Pourquoi ce format pour ce persona]

## Parcours d'onboarding

### Étape 1 — [Nom de l'étape]
- **Écran** : [S-XX-onboarding-step1]
- **Contenu** : [Titre + sous-titre + visuel décrit]
- **Action** : [Bouton principal + libellé]
- **Possibilité de skip** : [Oui / Non — si oui, impact sur la suite]
- **Accessibilité** : [Label VoiceOver pour le visuel]

### Étape 2 — [Nom de l'étape]
[Même structure]

### Demande de permissions (si applicable)
| Permission | Moment de la demande | Justification affichée |
|-----------|---------------------|----------------------|
| Notifications | Après 1ère action réussie | "Pour recevoir vos rappels" |
| Caméra | Quand l'utilisateur tente de l'utiliser | "Pour scanner vos documents" |

## Règle : jamais de permission au premier lancement
Les permissions sont demandées uniquement au moment où elles sont nécessaires — jamais à froid.

## Métriques de succès onboarding
- Taux de complétion : [cible %]
- Taux de skip : [acceptable jusqu'à %]
- Temps moyen : [cible en secondes]
- 1ère action réussie : [définition]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. L'objectif de l'onboarding est-il défini par une action à valeur — pas juste "avoir vu les écrans" ? (oui / reformuler)
2. Le parcours est-il adapté au persona nouvel utilisateur — ton, complexité, nombre d'étapes ? (oui / non + ajustements)
3. Aucune permission n'est demandée au premier lancement sans contexte d'usage — règle respectée ? (oui / non + étapes à déplacer)
4. Les métriques de succès sont-elles définies et mesurables ? (oui / non + KPIs manquants)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-onboarding-spec "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/onboarding-spec.md

Prochaines étapes :
→ Créer les frames Figma d'onboarding (/setup-figma-frames)
→ /write-contextual-help $ARGUMENTS
→ /review-figma-scope $ARGUMENTS (PO valide le parcours)
```
