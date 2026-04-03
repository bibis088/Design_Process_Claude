---
name: write-ux-content
description: "Produit les specs de contenu UX — parcours d'onboarding et aide contextuelle (empty states, messages d'erreur, tooltips). Use when user says 'onboarding', 'aide contextuelle', 'empty states', 'messages erreur', 'tooltips', or 'ux content'."
argument-hint: "[feature-slug]"
agent: ux-designer
---

## Rôle
Documenter le contenu UX fonctionnel — onboarding et aide contextuelle.


## Prérequis
- [ ] `US-###` disponibles — contexte d'usage et états vides/erreurs identifiés
- [ ] Specs d'écrans disponibles (`write-screen`) ou en cours
- [ ] `PERSONA-###` disponibles — aide contextuelle adaptée au profil utilisateur
- [ ] `write-user-flow` exécuté pour le flux concerné

## Partie A — Spec Onboarding
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Partie B — Aide contextuelle
### Étape 1 — Inventorier tous les états contextuels

Pour chaque écran, identifier :
- État `Empty` — quand il n'y a pas de contenu
- État `Error` — erreurs réseau, validation, permissions
- État `Loading` — messages de chargement si texte nécessaire
- Tooltips et messages d'aide inline

### Étape 2 — Rédiger les textes

**Règles de rédaction :**
- Les messages d'erreur expliquent **quoi faire**, pas juste ce qui s'est passé
- Les états vides proposent toujours une **action** (CTA)
- Le ton est cohérent avec la direction de marque
- Les textes sont en français (langue du projet)
- Pas de jargon technique visible pour l'utilisateur

```markdown
# Aide Contextuelle — [NomFeature] — [YYYY-MM-DD]

## [S-XX] [Nom de l'écran]

### État Empty
- **Titre** : [ex: "Aucune tâche pour le moment"]
- **Sous-titre** : [ex: "Commencez par créer votre première tâche"]
- **CTA** : [ex: "Créer une tâche"]
- **Accessibilité** : [Label VoiceOver pour l'illustration si applicable]

### Messages d'erreur

| Code erreur | Message affiché | Action proposée | Accessibilité |
|-------------|----------------|----------------|--------------|
| Réseau indisponible | "Impossible de se connecter. Vérifiez votre connexion." | Bouton "Réessayer" | "Erreur réseau. Bouton Réessayer disponible." |
| Champ vide | "Ce champ est obligatoire." | Focus retourné sur le champ | "Erreur : [Nom du champ] est obligatoire." |
| Erreur serveur | "Une erreur est survenue. Nous travaillons à la résoudre." | Bouton "Contacter le support" | "Erreur serveur. Bouton Contacter le support disponible." |
| Permission refusée | "Autorisez l'accès à [ressource] pour continuer." | Bouton "Modifier les paramètres" | "Permission requise. Bouton Modifier les paramètres disponible." |

### Tooltips / aide inline
| Élément | Texte d'aide | Déclencheur |
|---------|-------------|------------|
| [Champ complexe] | [Explication courte] | Tap sur icône ? |
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Chaque message d'erreur propose-t-il une action concrète — pas juste un constat ? (oui / non + messages à retravailler)
2. Chaque état vide a-t-il un CTA visible qui guide l'utilisateur vers la prochaine action ? (oui / non + états manquants)
3. Les textes sont-ils cohérents avec le ton défini lors de la direction artistique ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-contextual-help "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/contextual-help.md

Prochaines étapes :
→ Intégrer les textes dans les frames Figma correspondantes
→ /write-onboarding-spec $ARGUMENTS (si onboarding prévu)
→ /write-accessibility-annotations $ARGUMENTS (vérifier les labels)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-ux-content "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/onboarding-spec.md
📁 design/$ARGUMENTS/ux/contextual-help.md

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
