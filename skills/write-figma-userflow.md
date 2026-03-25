---
name: write-figma-userflow
description: Génère les user flows visuels dans Figma depuis les FLUX-### — permet au UI Designer de comprendre visuellement les parcours attendus. Exécuté par le ux-designer via `use_figma`.
argument-hint: [feature-slug]/[flux-slug]
disable-model-invocation: false
context: fork
agent: ux-designer
---

## Rôle
Traduire un flux fonctionnel textuel (`FLUX-###`) en user flow visuel dans Figma — diagramme de navigation avec écrans miniatures, états et transitions — sur la page `🗺️ User Flows` du fichier Projet.

## Agents consommateurs
- UX Designer (pilote — produit le flow visuel)
- UI Designer (consommateur — comprend le parcours avant de créer les écrans)
- Product Owner (consommateur — valide la conformité fonctionnelle)

## Prérequis
- [ ] Flux fonctionnel `FLUX-###` disponible dans `specs/[epic-slug]/` en statut `Approved`
- [ ] User flow textuel disponible dans `design/[feature-slug]/ux/`
- [ ] Fichier Figma Projet créé via `/setup-figma-project`
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Flux fonctionnel ou fichier Figma manquant — lancer d'abord `/write-flux-fonctionnel` et `/setup-figma-project`.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire le diagramme de flow en format texte ASCII à transmettre au designer.

## Processus de génération

### Étape 1 — Lire le flux source
Depuis le `FLUX-###` et le user flow textuel, extraire :
- Le point d'entrée
- Chaque écran / état du flux principal
- Les flux alternatifs (ALT-##)
- Les cas d'erreur (ERR-##)
- Les transitions entre écrans

### Étape 2 — Structurer le flow visuel

Organisation sur la page `🗺️ User Flows` :

```
Section : FLUX-### — [Nom du flux]
    ├── Zone "Flux principal" (gauche → droite)
    ├── Zone "Flux alternatifs" (en dessous)
    └── Zone "Cas d'erreur" (en dessous)
```

### Étape 3 — Créer les éléments via `use_figma`

**Nœuds d'écran** (rectangles 200×360pt arrondis) :
```
[S-XX]
[Nom de l'écran]
[État : Default / Loading / Error]
```
Couleur selon l'état :
- Default : fond blanc, bordure grise
- Loading : fond gris clair
- Error : fond rouge très clair
- Point d'entrée : fond bleu clair
- Point de sortie : fond vert clair

**Nœuds de décision** (losanges) :
```
[Condition ALT-##]
```

**Connecteurs** (flèches) :
- Flux principal : flèche pleine bleue
- Flux alternatif : flèche pointillée orange
- Cas d'erreur : flèche pointillée rouge
- Label sur chaque connecteur : type de transition (`push`, `modal`, `pop`, etc.)

**Légende** (en haut à droite de la section) :
```
■ Bleu = Point d'entrée
■ Vert = Point de sortie
■ Blanc = Écran nominal
■ Rouge = État d'erreur
→ Bleu = Flux principal
→ Orange = Flux alternatif
→ Rouge = Cas d'erreur
```

### Étape 4 — Ajouter les liens de prototype

Via `use_figma`, créer des liens de prototype entre les nœuds pour permettre la navigation dans le flow :
- Tap sur un nœud → nœud suivant
- Transition correspondant au type défini dans le user flow

### Étape 5 — Référencer dans la documentation

Dans la description de la section Figma :
```
Flux : FLUX-### | Feature : FEAT-###
US couvertes : US-###, US-###
Spec textuelle : design/[feature-slug]/ux/user-flow-FLUX-###.md
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les écrans du flux principal sont-ils représentés dans le diagramme — aucun saut logique ? (oui / non + écrans manquants)
2. Les flux alternatifs et cas d'erreur sont-ils visuellement distincts du flux principal — couleurs et styles de flèches corrects ? (oui / non)
3. Les liens de prototype permettent-ils de naviguer dans le flow de bout en bout sans blocage ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-figma-userflow "$ARGUMENTS" terminé
🗺️ Figma Projet → page 🗺️ User Flows mise à jour

Prochaines étapes :
→ /setup-figma-frames $ARGUMENTS (UI Designer crée les frames vides)
→ Partager le lien du flow avec le UI Designer avant qu'il démarre les écrans
```
