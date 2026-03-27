---
name: create-figma-component
description: Crée un composant Figma complet avec variants, auto layout, tokens et propriétés — basé sur la stack iOS/Android. Exécuté par le ui-designer via `use_figma`.
argument-hint: [feature-slug]/[component-slug]
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Créer un composant Figma réutilisable depuis une spec UX validée — avec états, variants, auto layout, tokens design system et conformité HIG/Material 3.

## Agents consommateurs
- UI Designer (pilote — crée le composant)
- Design System Manager (valideur — valide avant ajout à la bibliothèque DS)

## Prérequis
- [ ] Spec composant UX disponible dans `design/[feature-slug]/ux/`
- [ ] Tokens Figma configurés via `/setup-figma-tokens`
- [ ] Frames de base disponibles via `/setup-figma-grid`
- [ ] `rules/figma.md` consulté pour la nomenclature et les conventions
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Tokens Figma ou spec UX manquants — configurer les tokens et produire la spec UX avant de créer le composant.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire les spécifications détaillées du composant à créer manuellement.

## Processus de génération

### Étape 1 — Lire la spec UX du composant
Depuis `design/[feature-slug]/ux/component-[slug].md`, extraire :
- Les états : default, hover, pressed, disabled, loading, error
- Les variantes : primary, secondary, destructive
- Les dimensions et contraintes
- Les annotations d'accessibilité

### Étape 2 — Créer le composant de base via `use_figma`

Sur la page `🔧 components` du fichier Projet (ou `🧩 components` du DS si composant global) :

**Structure de layers obligatoire :**
```
[Catégorie] / [Nom] / [Variante] / [État]
    ├── Background (rectangle avec token couleur)
    ├── Content
    │   ├── Icon (si applicable)
    │   ├── Label (texte avec style typographique DS)
    │   └── [autres éléments]
    └── Focus Ring (invisible par défaut — visible sur focus)
```

**Auto Layout obligatoire :**
- Direction : selon le composant (horizontal pour bouton, vertical pour card)
- Padding : valeurs depuis tokens `spacing/*`
- Gap : valeur depuis tokens `spacing/*`
- Hug contents sur les deux axes (sauf si fill container requis)

**Tokens à appliquer :**
- Couleur fond : `color/interactive/[variante]`
- Couleur texte : `color/content/on[Variante]`
- Border radius : `radius/[taille]`
- Ombre : `shadow/[niveau]` si applicable

### Étape 3 — Créer les variants

Via `use_figma`, créer un component set avec les propriétés :

| Propriété | Valeurs |
|-----------|---------|
| `variant` | primary, secondary, destructive |
| `state` | default, hover, pressed, disabled, loading, error |
| `platform` | ios, android (si divergence visuelle) |
| `size` | sm, md, lg (si applicable) |
| `has_icon` | true, false (si applicable) |

Pour chaque combinaison applicable, configurer :
- Les tokens de couleur selon l'état et la variante
- L'opacité pour l'état Disabled (0.4)
- L'indicateur de loading (spinner) pour l'état Loading
- La bordure d'erreur pour l'état Error

### Étape 4 — Vérifier la conformité plateforme

**iOS (HIG) :**
- Zone tactile ≥ 44x44pt — agrandir si nécessaire avec padding invisible
- Texte : style typographique iOS (`iOS/type/[nom]`)
- Coins arrondis : `radius/lg` (12pt) pour les boutons principaux
- Feedback : state change visible (couleur/opacité)

**Android (Material 3) :**
- Zone tactile ≥ 48x48dp
- Texte : style typographique Android (`Android/type/[nom]`)
- Ripple effect : annoter le layer pour l'implémentation dev
- Élévation : `shadow/[niveau]` pour les composants élevés

### Étape 5 — Configurer les propriétés de contenu

Via `use_figma`, exposer les propriétés éditables :
- `Label` : propriété texte (modifiable sans entrer dans le composant)
- `Icon` : swap set d'icônes (si applicable)
- `Show Icon` : boolean (si applicable)

### Étape 6 — Publier dans la bibliothèque

Si le composant est global (réutilisable dans tout le projet) :
1. Déplacer vers la page `🧩 components` du Design System
2. Soumettre au Design System Manager pour validation
3. Publier dans la bibliothèque Figma uniquement après validation DSM

Si le composant est local à la feature :
1. Garder dans la page `🔧 components` du fichier Projet
2. Ne pas publier dans la bibliothèque DS sans validation DSM

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les états et variantes définis dans la spec UX sont-ils créés dans le component set ? (oui / non + états manquants)
2. L'auto layout est-il configuré avec des valeurs issues des tokens `spacing/*` — aucune valeur en dur ? (oui / non + valeurs à corriger)
3. La zone tactile est-elle ≥ 44x44pt (iOS) / 48x48dp (Android) sur tous les variants ? (oui / non + variants non conformes)

### ✅ Critère de sortie accessibilité (RAAM niveau A — obligatoire)

Ces critères bloquent le passage au composant ou écran suivant :

4. Chaque élément interactif du composant a-t-il un `accessibilityLabel` défini dans les annotations du layer Figma ? (oui / non + éléments manquants)
5. Le composant transmet-il l'information par autre chose que la couleur seule — icône, texte ou forme accompagne toujours la couleur ? (oui / non — RAAM 2.1)
6. Le ratio de contraste du composant est-il ≥ 3:1 pour les éléments interactifs (RAAM 2.4) ? (oui / non — indiquer le ratio mesuré)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.
> Les critères 4, 5, 6 sont des critères RAAM niveau A — ils bloquent si non conformes.

## Résumé de fin d'exécution

```
✅ create-figma-component "$ARGUMENTS" terminé
🧩 Figma → composant créé, guidelines et accessibilité validés

Prochaines étapes :
→ /check-guidelines-compliance $ARGUMENTS
→ Soumettre au Design System Manager si composant global
→ /create-figma-component $ARGUMENTS (composant suivant)
```
