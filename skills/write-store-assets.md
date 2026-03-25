---
name: write-store-assets
description: Prépare les assets nécessaires pour la publication sur l'App Store et le Play Store — icône, bannière Android, screenshots smartphone et tablet. Exécuté par le ui-designer via `use_figma`.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Créer et exporter tous les assets visuels requis pour soumettre l'application sur l'App Store (iOS) et le Play Store (Android) — formats, tailles et contenus conformes aux exigences des stores.

## Agents consommateurs
- UI Designer (pilote)

## Prérequis
- [ ] Handoff Figma terminé et validé (`write-figma-handoff`)
- [ ] Frames d'écrans finalisées et `Ready for dev`
- [ ] Icône app designée (1024×1024pt — sans transparence)
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Handoff non terminé — compléter `/write-figma-handoff` avant de préparer les assets store.

## Processus de génération

### Étape 1 — Créer la page Store Assets

Via `use_figma`, sur la page `📦 Store Assets` :

Créer les sections :
1. `App Icon`
2. `iOS App Store Screenshots`
3. `Android Play Store Screenshots`
4. `Android Feature Banner`

### Étape 2 — App Icon

**Règles Apple :**
- Format : 1024×1024pt, PNG, sans couche alpha, sans transparence
- Pas d'arrondi dans le fichier — Apple applique le masque automatiquement
- Pas de texte si possible

**Règles Google :**
- Format : 512×512dp, PNG
- Arrondi dans le fichier — Google affiche l'icône avec arrondi variable

Via `use_figma`, créer dans la section `App Icon` :
```
App Icon / iOS / 1024pt (frame 1024×1024)
App Icon / Android / 512dp (frame 512×512)
App Icon / Preview / iOS rounded (frame avec masque pour prévisualisation)
App Icon / Preview / Android adaptive (frame avec couche avant/arrière)
```

### Étape 3 — Screenshots iOS App Store

**Formats requis (minimum 5 screenshots par taille) :**

| Format | Taille | Device |
|--------|--------|--------|
| 6.7" | 1290×2796px | iPhone 15 Pro Max |
| 5.5" | 1242×2208px | iPhone 8 Plus |
| iPad 12.9" | 2048×2732px | iPad Pro |

Via `use_figma`, pour chaque format :
1. Créer 5 frames aux dimensions exactes
2. Nommer : `iOS Screenshots / 6.7" / 01 — [Description]`
3. Adapter les captures d'écran aux bonnes dimensions
4. Ajouter un fond de marque cohérent
5. Ajouter un titre accrocheur et une description courte si applicable

**Contenu recommandé pour les 5 screenshots :**
1. Écran principal / feature phare
2. Flux utilisateur clé
3. Fonctionnalité différenciante
4. État vide + onboarding
5. Paramètres ou personnalisation

### Étape 4 — Screenshots Android Play Store

**Formats requis :**

| Format | Taille | Type |
|--------|--------|------|
| Smartphone | 1080×1920px | Portrait |
| Tablet | 1200×1920px | Portrait |

Via `use_figma` :
1. Créer 5 frames smartphone `Android Screenshots / Phone / 01 — [Description]`
2. Créer 2 frames tablet `Android Screenshots / Tablet / 01 — [Description]`

### Étape 5 — Android Feature Banner

Format : 1024×500px (paysage)

Contenu :
- Image représentative de l'app
- Logo ou nom de l'app
- Tagline courte (optionnelle)
- Pas de texte trop petit (illisible sur mobile)
- Pas de bordure externe

Via `use_figma`, créer : `Android Feature Banner / 1024×500`

### Étape 6 — Exporter les assets

Via `use_figma`, configurer l'export pour chaque asset :

| Asset | Format | Résolution |
|-------|--------|-----------|
| App Icon iOS | PNG | 1× (1024px) |
| App Icon Android | PNG | 1× (512px) |
| Screenshots iOS | PNG | 1× |
| Screenshots Android | PNG | 1× |
| Feature Banner | PNG | 1× |

### Étape 7 — Checklist de validation store

```markdown
## Checklist App Store (iOS)
- [ ] Icône 1024×1024px sans transparence
- [ ] 5 screenshots en 6.7" (1290×2796px)
- [ ] 5 screenshots en 5.5" (1242×2208px)
- [ ] 5 screenshots iPad 12.9" (2048×2732px)
- [ ] Aucun texte de l'interface utilisateur tronqué
- [ ] Aucun badge "NEW" ou prix dans les screenshots
- [ ] Aucun contenu de tiers non autorisé

## Checklist Play Store (Android)
- [ ] Icône 512×512px
- [ ] Feature Banner 1024×500px
- [ ] 5 screenshots smartphone (1080×1920px)
- [ ] 2 screenshots tablet (1200×1920px)
- [ ] Aucun contenu trompeur
- [ ] Aucune mention de prix ou promotion dans les images
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les dimensions de tous les assets sont-elles exactes selon les exigences Apple et Google ? (oui / non + assets non conformes)
2. L'icône iOS est-elle sans transparence et sans arrondi dans le fichier ? (oui / non)
3. Les 5 screenshots iOS couvrent-ils les 5 thèmes recommandés — feature phare, flux clé, différenciant, onboarding, personnalisation ? (oui / non + thèmes manquants)
4. Le Feature Banner Android est-il en 1024×500px sans bordure externe ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-store-assets "$ARGUMENTS" terminé
📦 Figma Projet → page 📦 Store Assets complète

Prochaines étapes :
→ Exporter les assets depuis Figma (Export panel)
→ Soumettre sur App Store Connect (iOS)
→ Soumettre sur Google Play Console (Android)
→ /write-qa-report $ARGUMENTS (tests finaux avant soumission)
```
