---
name: setup-figma-prototype
description: "Configure les connexions de prototype natif Figma entre les frames — flows de navigation, transitions, interactions — pour permettre une présentation interactive sans quitter Figma. Exécuté par le ui-designer après les écrans principaux."
argument-hint: "[feature-slug]/[flow-slug]"
agent: ui-designer
---

## Rôle
Configurer les connexions de prototype dans Figma natif — liens entre frames, transitions, flows de démarrage — pour permettre une présentation client interactive directement depuis Figma, en complément ou en alternative au prototype React.

## Différence avec le prototype React

| | Prototype Figma natif | Prototype React (`write-prototype-react`) |
|-|-----------------------|------------------------------------------|
| Format | Lien Figma | Fichier HTML standalone |
| Fidélité | Haute — design exact | Haute — contenu réel |
| Interactivité | Navigation + transitions | Navigation + états + micro-interactions |
| Accès | Lien Figma (compte requis ou lien public) | URL directe — aucun compte |
| Usage recommandé | Présentation interne, revue rapide | Tests utilisateur, partage client |

Les deux sont complémentaires. Ce skill configure le prototype Figma natif.

## Prérequis
- [ ] Frames des écrans principaux créées (`setup-figma-frames`)
- [ ] Frames nommées selon convention snake_case (`US_###_[nom]_[plateforme]_[état]`)
- [ ] User flow du flux à prototyper disponible (`write-figma-userflow`)
- [ ] MCP Figma connecté

## Processus

### Étape 1 — Lire le user flow de référence

Depuis `design/[feature-slug]/ux/user-flow-FLUX-###.md` et la page `🗺️ user_flows` Figma, extraire :
- Séquence des écrans dans le flux
- Points de bifurcation (erreurs, états alternatifs)
- Point d'entrée et de sortie du flux

```
get_metadata [URL page user_flows]
→ Identifier les frames du flux à connecter et leur ordre
```

### Étape 2 — Configurer le flow de démarrage

Via `use_figma` :

```
Action : Définir le Starting Point du prototype
Frame de départ : [US_###_[nom]_ios_default] — premier écran du flux
Nom du flow : [FLUX_###_[nom_flux]]
Device preview : iPhone 16 Pro (iOS) ou Pixel 9 (Android) selon la plateforme
Background : #000000
```

### Étape 3 — Créer les connexions entre frames

Pour chaque transition du flux, via `use_figma` :

```
Format : Source → Déclencheur → Destination → Transition

Connexions à créer pour [FLUX_###] :

1. US_###_login_ios_default
   → Tap bouton "Se connecter"
   → US_###_dashboard_ios_default
   → Transition : Smart Animate | Durée : 300ms | Easing : Ease Out

2. US_###_login_ios_default
   → Tap lien "Mot de passe oublié"
   → US_###_reset_ios_default
   → Transition : Push (droite → gauche) | 250ms | Ease In Out

3. US_###_login_ios_default
   → Tap hors champ (après erreur)
   → US_###_login_ios_error
   → Transition : Smart Animate | 200ms | Ease Out

[Continuer pour chaque interaction du flux]
```

### Types de transitions selon la convention plateforme

| Action | iOS | Android | Transition Figma |
|--------|-----|---------|-----------------|
| Navigation push | Push left → right | Slide up | Push / Slide In |
| Retour | Push right → left | Slide down | Push back |
| Modal | Slide from bottom | Slide from bottom | Slide In (bas) |
| Bottom sheet | Slide from bottom | Slide from bottom | Slide In (bas) |
| Alert | Fondu | Scale + fondu | Dissolve |
| Tab switch | Crossfade | Crossfade | Dissolve |

### Étape 4 — Configurer les overlays et modales

Pour les bottom sheets et modales via `use_figma` :

```
Type : Overlay
Position : Bottom center (bottom sheet) / Center (modale)
Background : Semi-transparent #00000080
Fermeture : Tap outside → fermer l'overlay
Animation : Move In (bas) | 300ms
```

### Étape 5 — Configurer le thumbnail du fichier

Via `use_figma`, sur la page `📱 screens` :

```
Action : Définir le thumbnail du fichier
Frame à utiliser : [Écran principal le plus représentatif du projet]
Format : Ratio 4:3 recommandé par Figma
```

> ⚠️ Le thumbnail améliore la lisibilité dans le browser Figma et le partage client.

### Étape 6 — Documenter le lien prototype

```markdown
## Prototype Figma natif — [feature-slug] — [FLUX_###]

- URL prototype : [URL Figma en mode présentation]
- Device : [iPhone 16 Pro / Pixel 9]
- Flow : [FLUX_###] — [Nom du flux]
- Écran de départ : [US_###_nom_ios_default]
- Transitions : Smart Animate + Push selon plateforme
- Partage : [Anyone with link — Can view]
- Date : [YYYY-MM-DD]

### Flows configurés
| Flow | Écrans couverts | États inclus | Statut |
|------|----------------|-------------|--------|
| [FLUX_###] | [N écrans] | Default + Error | ✅ |
```

## Phase de validation — niveau standard

1. Le flow de démarrage est-il défini sur la bonne frame d'entrée ? (oui / non)
2. Toutes les interactions principales du flux sont-elles connectées — navigation avant et retour ? (oui / non + interactions manquantes)
3. Les transitions respectent-elles les conventions plateforme — Push pour iOS, Slide pour Android ? (oui / non)
4. Le thumbnail du fichier est-il configuré sur un écran représentatif ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine.

## Résumé de fin d'exécution

```
✅ setup-figma-prototype "$ARGUMENTS" terminé
🔗 Lien prototype Figma : [URL]
🖼️ Thumbnail : configuré

Prochaines étapes :
→ /setup-figma-permissions $ARGUMENTS (si pas encore fait)
→ /write-prototype-react $ARGUMENTS (prototype HTML pour tests utilisateur)
→ Partager le lien prototype en interne pour revue
```
