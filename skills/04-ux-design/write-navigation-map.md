---
name: write-navigation-map
description: "Produit le SCREENS-MAP.md — vue globale de tous les écrans et connexions d'une feature. Exécuté par le ux-designer."
argument-hint: "[feature-slug]"
agent: ux-designer
---

## Rôle
Produire le `SCREENS-MAP.md` d'une feature — vue d'ensemble structurelle de tous les écrans et de leurs connexions.

## Agents consommateurs
UX Designer 

## Distinction avec les autres documents de flux

| Document | Niveau | Produit par | Ce qu'il décrit |
|----------|--------|------------|----------------|
| Flux fonctionnel (`FLUX-###`) | Logique métier | BA | Acteurs, actions, règles — sans interface |
| User flow | Séquence d'écrans | UX Designer | Parcours séquentiel écran par écran |
| Navigation map (`SCREENS-MAP.md`) | Vue globale | UX Designer | Carte de tous les écrans et leurs connexions |

La navigation map n'est **pas** un flux séquentiel. Elle montre la structure de navigation complète — tous les chemins possibles, pas un chemin en particulier.

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] Au moins un `FLUX-###` validé pour la feature
- [ ] Au moins un user flow produit depuis ce flux
- [ ] Les user stories (`US-###`) de la feature sont connues
- [ ] Les specs fonctionnelles sont disponibles dans `./specs/[NomFeature]/`
- [ ] `design/[feature-slug]/` dossier créé
- [ ] `SCREENS-MAP.md` à créer par ce skill — pas de prérequis

## Processus de génération

### Étape 1 — Inventorier tous les écrans
Depuis les user flows disponibles, lister exhaustivement :
- Tous les écrans nécessaires (y compris les états modaux et overlays)
- Les écrans partagés avec d'autres features
- Les points d'entrée externes (notifications, deep links, tab bar)
- Les points de sortie (retour au tab bar, deep link externe)

### Étape 2 — Attribuer les IDs
Convention : `S-01`, `S-02`... remis à zéro pour chaque feature.
Nommage : `S-01-login`, `S-02-dashboard`, `S-03-profile-edit`

### Étape 3 — Cartographier les connexions
Pour chaque écran, identifier :
- Depuis quels écrans on peut y accéder
- Vers quels écrans on peut naviguer
- Le type de transition utilisé pour chaque connexion

### Étape 4 — Rédiger la navigation map
Suivre strictement ce format :

```markdown
# Navigation Map — [NomFeature]

## Métadonnées
- Feature : [FEAT-###]
- UX Designer : [nom]
- Version : [x.x]
- Statut : [To Do / In Progress / In Review / Approved]
- Date de création : [YYYY-MM-DD]
- Dernière mise à jour : [YYYY-MM-DD]

## Points d'entrée
| Source | Type | Déclencheur |
|--------|------|------------|
| [Tab bar] | Navigation principale | Tap sur tab [Nom] |
| [Notification push] | Deep link | `app://[chemin]` |
| [Écran externe S-XX] | push | [Action déclenchante] |

## Inventaire des écrans

| ID | Nom | Type | Description courte |
|----|-----|------|-------------------|
| S-01 | [nom] | [Root / Child / Modal / Overlay] | [1 phrase] |
| S-02 | [nom] | [Root / Child / Modal / Overlay] | [1 phrase] |
| S-03 | [nom] | [Root / Child / Modal / Overlay] | [1 phrase] |

## Carte des connexions

| De | Vers | Transition | Déclencheur | Notes |
|----|------|-----------|------------|-------|
| S-01 | S-02 | push | Tap sur [élément] | — |
| S-02 | S-01 | pop | Bouton retour / Swipe back | iOS : swipe natif |
| S-02 | S-03 | modal | Tap sur [élément] | Bottom sheet sur iOS |
| S-03 | S-02 | dismiss | Tap fermer / Swipe down | — |

## Points de sortie
| Vers | Type | Déclencheur |
|------|------|------------|
| [Tab bar — autre feature] | Navigation principale | Tap sur tab [Nom] |
| [Deep link externe] | Externe | [Action déclenchante] |

## Écrans partagés (hors feature)
| ID | Nom | Feature propriétaire | Type d'accès |
|----|-----|---------------------|-------------|
| [S-XX] | [Nom] | [FEAT-###] | [push / modal] |

## Flux UX couverts
| Flux | Nom | Écrans principaux | US associées |
|------|-----|------------------|-------------|
| [FLUX-###] | [Nom] | S-01 → S-02 → S-03 | [US-###, US-###] |
| [FLUX-###] | [Nom] | S-01 → S-04 | [US-###] |

## Comportements spécifiques plateforme
| Comportement | iOS | Android |
|-------------|-----|---------|
| Navigation retour depuis S-02 | Swipe back natif + bouton | Bouton back système |
| Présentation S-03 | Bottom sheet | Bottom sheet Material |

## Notes
- [Contrainte de navigation spécifique]
- [Comportement conditionnel selon rôle utilisateur ou état]
```

### Étape 5 — Vérifier la complétude
Avant de soumettre en revue :
- [ ] Tous les écrans des user flows sont présents dans l'inventaire
- [ ] Aucun écran n'est orphelin (accessible depuis nulle part)
- [ ] Tous les points d'entrée et de sortie sont documentés
- [ ] Les divergences iOS / Android sont notées
- [ ] Les écrans partagés référencent leur feature propriétaire


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- La navigation map est **toujours produite avant** les specs d'écran individuelles
- Un écran sans point d'entrée documenté = écran orphelin → bloquer jusqu'à résolution
- Les transitions doivent utiliser les conventions définies dans `rules/design.md` (`push`, `pop`, `modal`, `dismiss`, `replace`, `fade`)
- La navigation map est mise à jour à chaque ajout d'écran — jamais laissée en retard sur les specs

## Chemin de fichier
```
design/[feature-slug]/SCREENS-MAP.md
```


## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Tous les écrans des user flows sont-ils présents dans l'inventaire — aucun écran orphelin ? (oui / non + écrans manquants)
2. Chaque écran a-t-il au moins un point d'entrée documenté ? (oui / non + écrans sans entrée)
3. Les transitions utilisent-elles uniquement les conventions définies dans rules/design.md — push, pop, modal, dismiss, replace, fade ? (oui / non + transitions à corriger)
4. Les écrans partagés avec d'autres features référencent-ils leur feature propriétaire ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-navigation-map "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/SCREENS-MAP.md

Prochaines étapes :
→ /write-screen-spec $ARGUMENTS/[screen-id]
→ /review-dod SCREENS-MAP

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
