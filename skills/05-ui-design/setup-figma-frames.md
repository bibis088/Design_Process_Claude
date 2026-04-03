---
name: setup-figma-frames
description: "Crée automatiquement toutes les frames vides dans Figma pour chaque US du scope — nomenclature snake_case stricte, lien direct US↔frame, grille et auto layout configurés. Exécuté par le ui-designer via use_figma. Use when user says 'crée les frames', 'setup frames', 'initialise les écrans figma', or 'prépare les frames'."
argument-hint: "[feature-slug]"
agent: ui-designer
---

## Rôle
Créer automatiquement toutes les frames Figma du scope en lisant les US directement — nomenclature snake_case stricte qui garantit le lien entre chaque frame et son US source. Zéro frame nommée manuellement, zéro nom auto Figma.

## Prérequis
- [ ] OS de base défini dans `specs/[epic-slug]/cadrage.md` — section "OS de conception de référence"
- [ ] Toutes les `US-###.md` disponibles dans `specs/[epic-slug]/stories/`
- [ ] Navigation map `SCREENS-MAP.md` disponible dans `design/[feature-slug]/`
- [ ] `setup-figma-grid` exécuté — frames de base iOS/Android disponibles
- [ ] MCP Figma connecté (`figma-read-design` exécuté)

## Gestion des erreurs
> ❌ US manquantes — lancer d'abord `/write-user-story` pour toutes les US du scope.
> ⚠️ MCP Figma indisponible — produire la liste complète de frames avec nomenclature exacte pour création manuelle.

---

## Processus

### Étape 1 — Lire l'OS de base et les US du scope

Depuis `specs/[epic-slug]/cadrage.md`, extraire :
```
OS de base     : [iOS / Android]
OS secondaire  : [Android / iOS] — déclinaison après Gate 9 via /os-decline
```

> Les frames créées dans ce skill sont uniquement pour l'**OS de base**.
> L'OS secondaire sera décliné après Gate 9 via `/os-decline`.

Pour chaque `US-###.md` dans `specs/[epic-slug]/stories/` :

Extraire :
- `ID` → ex: `US_042`
- `Titre` → ex: `Connexion par email`
- `Plateformes` → iOS / Android / les deux (depuis le cadrage)
- `États requis` → depuis les critères d'acceptance :
  - `default` → toujours présent
  - `loading` → si CA mentionne chargement, appel réseau, attente
  - `empty` → si CA mentionne liste vide, état initial sans données
  - `error` → si CA mentionne erreur, validation, message d'échec
  - `success` → si CA mentionne confirmation, validation réussie
  - `disabled` → si CA mentionne condition d'activation

Construire la liste complète des frames à créer :

```markdown
## Plan de frames — [feature-slug]

| US | Titre | iOS | Android | États |
|----|-------|-----|---------|-------|
| US_042 | login | ✅ | ✅ | default, loading, error |
| US_043 | forgot_password | ✅ | ✅ | default, success |
| US_044 | dashboard | ✅ | ✅ | default, loading, empty |
```

### Étape 2 — Convention de nomenclature stricte

**Format obligatoire :**
```
US_###_[titre_us_slug]_[plateforme]_[état]
```

Règles de transformation du titre en slug :
- Minuscules uniquement
- Espaces → `_`
- Accents supprimés (`é`→`e`, `à`→`a`...)
- Caractères spéciaux supprimés
- Maximum 30 caractères pour le slug

**Exemples concrets :**

| US | Titre | Frame générée |
|----|-------|--------------|
| US-042 | Connexion par email | `US_042_connexion_email_ios_default` ← OS de base = iOS |
| US-042 | Connexion par email | `US_042_connexion_email_ios_loading` |
| US-042 | Connexion par email | `US_042_connexion_email_ios_error` |
| US-042 | Connexion par email | `US_042_connexion_email_android_default` |
| US-043 | Mot de passe oublié | `US_043_mot_de_passe_oublie_ios_default` |
| US-044 | Dashboard | `US_044_dashboard_ios_default` |
| US-044 | Dashboard | `US_044_dashboard_ios_empty` |

**Description de chaque frame (champ Figma) :**
```
US_042 — Connexion par email | État : default | Spec : design/[feature]/ux/screens/S-XX-connexion.md
```

Cette description est le lien direct entre la frame Figma et son US + sa spec UX.

### Étape 3 — Créer les frames via `use_figma`

Pour chaque frame du plan, via `use_figma` :

```
Action : Dupliquer la frame de base
Source : [frame_ios_393 ou frame_android_412] depuis le DS
Destination : page "Work In Progress UI"
Nom : [nomenclature exacte]
Description : [US_### — Titre | État | Spec]

Configuration :
- Grille : active (iOS 4 col 16pt / Android 4 col 16dp)
- Auto Layout : vertical, fill container
- Safe Area : top et bottom respectées
```

### Étape 4 — Organiser dans la page `Work In Progress UI`

Créer une **section par US** nommée en snake_case avec l'ID en majuscules :
```
US_042_connexion_email
  ├── US_042_connexion_email_ios_default
  ├── US_042_connexion_email_ios_loading
  ├── US_042_connexion_email_ios_error
  ├── US_042_connexion_email_android_default
  └── US_042_connexion_email_android_loading

US_043_mot_de_passe_oublie
  ├── US_043_mot_de_passe_oublie_ios_default
  └── US_043_mot_de_passe_oublie_ios_success
```

Organisation visuelle dans chaque section :
- iOS frames à gauche
- Android frames à droite
- États dans l'ordre : `default → loading → empty → error → success → disabled`

### Étape 5 — Structure de layers par frame

Dans chaque frame `_default`, créer la structure vide :

```
US_###_[nom]_ios_default
├── status_bar          ← composant DS (iOS System)
├── navigation_bar      ← composant DS (si applicable selon la nav map)
├── safe_area_top       ← layer vide de hauteur = inset top
├── content             ← conteneur principal (vide — à remplir)
└── tab_bar             ← composant DS (si applicable selon la nav map)
```

```
US_###_[nom]_android_default
├── status_bar          ← composant DS (Android System)
├── top_app_bar         ← composant DS (si applicable)
├── content             ← conteneur principal (vide)
└── navigation_bar      ← composant DS bottom (si applicable)
```

### Étape 6 — Produire le rapport de frames créées

```markdown
## Rapport frames — [feature-slug] — [YYYY-MM-DD]

| US | Frames iOS | Frames Android | Total | Section Figma |
|----|-----------|---------------|-------|--------------|
| US_042 | default, loading, error | default, loading | 5 | US_042_connexion_email |
| US_043 | default, success | default | 3 | US_043_mot_de_passe_oublie |
| US_044 | default, loading, empty | default, empty | 5 | US_044_dashboard |

**Total : [N] frames créées**
**Page Figma :** Work In Progress UI
**URL :** [URL Figma page]

### Correspondance US ↔ Frames
| Frame Figma | US source | Spec UX |
|------------|----------|---------|
| US_042_connexion_email_ios_default | US-042 | S-01-connexion.md |
| US_042_connexion_email_ios_loading | US-042 | S-01-connexion.md |
| US_042_connexion_email_ios_error | US-042 | S-01-connexion.md |
```

## Phase de validation

1. Chaque US du scope a-t-elle au moins une frame `_default` — iOS ET Android si les deux plateformes sont ciblées ? (oui / non + US manquantes)
2. La nomenclature est-elle strictement snake_case sur 100% des frames — aucun espace, aucun nom auto Figma ? (oui / non + frames à corriger)
3. La description de chaque frame contient-elle l'ID de l'US source — lien traçable ? (oui / non)
4. Les états sont-ils cohérents avec les CA des US — pas d'état inventé, pas d'état manquant ? (oui / non + incohérences)
5. Les zones tactiles sont-elles ≥ 44×44pt iOS / 48×48dp Android sur les éléments interactifs visibles ? (oui / non — RAAM 13.1)

> Si tout validé → le skill se termine.
> Si non → reprendre depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ setup-figma-frames "$ARGUMENTS" terminé
📁 Figma → Work In Progress UI — [N] frames créées pour [N] US
📋 Rapport : design/[feature-slug]/frames-report.md

Correspondance US ↔ Frames : 100% traçable via nomenclature + description

Prochaines étapes :
→ [Si écrans principaux] Direction artistique — Product Designer (étape manuelle)
→ [Si écrans secondaires] /fetch-content-for-frames $ARGUMENTS
→ /create-figma-component $ARGUMENTS (composants manquants)
→ /review-figma-scope $ARGUMENTS (validation PO)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
