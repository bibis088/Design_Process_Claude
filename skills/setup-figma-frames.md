---
name: setup-figma-frames
description: Crée les frames vides dans Figma nommées selon les US et états, avec grille et auto layout. Demande le contenu réel à intégrer avant de créer. Exécuté par le ui-designer via `use_figma`.
argument-hint: [feature-slug]
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Créer toutes les frames vides du projet dans Figma avec la nomenclature correcte, les grilles actives et l'auto layout configuré — prêtes à accueillir les composants et le contenu.

## Agents consommateurs
- UI Designer (pilote)

## Prérequis
- [ ] Navigation map `SCREENS-MAP.md` disponible dans `design/[feature-slug]/`
- [ ] User flows visuels Figma créés via `/write-figma-userflow`
- [ ] Frames de base disponibles dans le fichier Design System via `/setup-figma-grid`
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Navigation map ou user flows manquants — lancer d'abord `/write-navigation-map` et `/write-figma-userflow`.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire la liste de frames à créer manuellement avec leur nomenclature exacte.

## Processus de génération

### Étape 1 — Collecter le contenu réel

Avant de créer les frames, demander systématiquement :

```
Avant de créer les frames, j'ai besoin de savoir quel contenu intégrer.
Pour chaque type de contenu dans cette feature, indique :

1. Textes — as-tu des textes réels (titres, labels, corps) ou dois-je utiliser
   des placeholders ? Si tu as une URL de référence, partage-la.

2. Images — as-tu des images ou assets visuels disponibles ?
   URL d'une page de référence, dossier d'assets, ou placeholder ?

3. Données dynamiques — les listes, profils, prix sont-ils disponibles
   quelque part (URL, fichier JSON, API) ou dois-je créer des données fictives ?
```

Selon la réponse :
- URL fournie → déclencher `/fetch-content-for-frames $ARGUMENTS`
- Assets disponibles → les référencer dans les frames
- Rien de disponible → utiliser du contenu placeholder structuré (pas de Lorem ipsum)

### Étape 2 — Lire la navigation map
Depuis `design/[feature-slug]/SCREENS-MAP.md`, extraire :
- La liste de tous les écrans (`S-01`, `S-02`...)
- Les US couvertes par chaque écran
- Les états requis par écran (depuis les specs d'écran)
- Les plateformes concernées

### Étape 3 — Créer les frames via `use_figma`

Pour chaque écran, créer les frames selon la convention :
```
US_###_[nom_us]_ios_default
[US-###] [NomUS] / iOS / Loading
[US-###] [NomUS] / iOS / Empty
[US-###] [NomUS] / iOS / Error
[US-###] [NomUS] / Android / Default
[US-###] [NomUS] / Android / Loading
[US-###] [NomUS] / Android / Empty
[US-###] [NomUS] / Android / Error
```

Pour chaque frame :
- Dupliquer la frame de base correspondante (iOS 393 ou Android 412)
- Appliquer la grille active
- Activer l'auto layout vertical
- Ajouter en description : `Spec : S-XX-[nom] | US-###`

### Étape 4 — Organiser dans la page Screens

Dans la page `📱 screens`, organiser les frames :
- Une section par `US-###`
- iOS frames à gauche, Android frames à droite
- États dans l'ordre : Default → Loading → Empty → Error

### Étape 5 — Créer les layers de base

Dans chaque frame Default, créer la structure de layers vide :
```
US_###_[nom_us]_ios_default
    ├── Status Bar (composant DS)
    ├── Navigation Bar (composant DS — si applicable)
    ├── Content
    │   └── (vide — à remplir)
    └── Tab Bar (composant DS — si applicable)
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Toutes les frames de la navigation map sont-elles créées — iOS et Android, tous les états ? (oui / non + frames manquantes)
2. La nomenclature est-elle exacte sur toutes les frames — `US_###_[nom_us]_[plateforme]_[état]` ? (oui / non + frames mal nommées)
3. Le contenu à intégrer est-il identifié — URL, assets ou placeholder structuré ? (oui / non + contenu à préciser)

### ✅ Critère de sortie accessibilité (RAAM niveau A — obligatoire sur les écrans créés)

Ces critères s'appliquent à chaque lot d'écrans créés — principaux ou secondaires :

4. L'ordre de focus est-il logique sur chaque frame créée — de haut en bas, gauche à droite ? (oui / non + frames à corriger — RAAM 7.1)
5. Les zones tactiles sont-elles ≥ 44x44pt (iOS) / 48x48dp (Android) sur tous les éléments interactifs visibles ? (oui / non + éléments non conformes — RAAM 13.1)
6. Les états Empty et Error affichent-ils un message texte — pas uniquement une icône ou une couleur pour communiquer l'état ? (oui / non — RAAM 2.1)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.
> Les critères 4, 5, 6 sont des critères RAAM niveau A — ils bloquent si non conformes.

## Résumé de fin d'exécution

```
✅ setup-figma-frames "$ARGUMENTS" terminé
📱 Figma Projet → page 📱 screens — [N] frames créées
✅ Revue accessibilité RAAM niveau A : [Pass / À corriger]

Prochaines étapes :
→ [Si écrans principaux] Direction artistique humaine (Product Designer) — étape manuelle
→ [Si écrans secondaires] /fetch-content-for-frames $ARGUMENTS (si URL disponible)
→ /figma-code-connect $ARGUMENTS (après tous les écrans)
→ /review-figma-scope $ARGUMENTS (validation PO)
```
