---
name: os-decline
description: "Décline les écrans validés de l'OS de base vers l'OS secondaire — duplique les frames, applique les composants système et conventions de navigation spécifiques. Agents déclinent la structure, Product Designer ajuste les composants. Exécuté après Gate 9. Use when user says 'décline pour android', 'décline pour ios', 'adaptation OS secondaire', 'décliner les écrans', or 'os-decline'."
argument-hint: "[epic-slug]/[feature-slug]"
agent: ui-designer
---

## Rôle
Décliner les écrans validés de l'OS de base vers l'OS secondaire — en appliquant automatiquement les conventions de composants, navigation et grilles propres à chaque plateforme, puis en guidant le Product Designer pour les ajustements manuels.

## Prérequis
- [ ] Gate 9 validée — scope complet approuvé sur l'OS de base
- [ ] Toutes les frames de l'OS de base en page `Validated UI` ou `Ready for dev`
- [ ] OS de base et OS secondaire définis dans `specs/[epic-slug]/cadrage.md`
- [ ] MCP Figma connecté
- [ ] `US-###` du scope disponibles — frames OS de base identifiées
- [ ] Annotations RAAM complètes sur l'OS de base (`write-accessibility-annotations`)
- [ ] `specs/[epic-slug]/figma-urls.md` disponible

## Gestion des erreurs
> ❌ Frames OS de base non validées — la déclinaison ne démarre pas avant Gate 9.
> ⚠️ MCP Figma indisponible — produire la liste de frames à dupliquer et les règles d'adaptation pour exécution manuelle.

---

## Étape 1 — Lire le contexte depuis le cadrage

Depuis `specs/[epic-slug]/cadrage.md` :
```
OS de base      : [iOS / Android]
OS secondaire   : [Android / iOS]
Plateformes     : les deux
```

Depuis Figma via MCP :
```
get_metadata [URL page Validated UI]
→ Liste de toutes les frames validées de l'OS de base

get_variable_defs [URL DS]
→ Tokens existants pour les deux plateformes
```

---

## Étape 2 — Inventaire des frames à décliner

Pour chaque frame `_[os_base]_[état]` dans `Validated UI`, créer l'entrée correspondante :

```markdown
## Plan de déclinaison — [feature-slug]

| Frame OS de base | Frame OS secondaire à créer | Adaptations requises |
|-----------------|----------------------------|---------------------|
| US_042_login_ios_default | US_042_login_android_default | Navigation bar → Top App Bar, bouton → Material button |
| US_042_login_ios_error | US_042_login_android_error | Message erreur → Snackbar (optionnel) |
| US_044_dashboard_ios_default | US_044_dashboard_android_default | Tab bar → Bottom Navigation Bar |
| US_044_dashboard_ios_empty | US_044_dashboard_android_empty | — |

Total : [N] frames à créer
```

---

## Étape 3 — Partie automatisée (agents via MCP)

Pour chaque frame de l'OS de base, via `use_figma` :

### 3a — Dupliquer et renommer
```
Action : Dupliquer la frame [US_###_nom_ios_état]
Nouveau nom : [US_###_nom_android_état]
Destination : section US_###_[nom] dans Work In Progress UI
Description : "[US_###] — [Titre] | OS secondaire : Android | Décliné depuis iOS"
```

### 3b — Appliquer la grille OS secondaire
```
Retirer la grille iOS (4 col, 16pt, 393pt)
Appliquer la grille Android (4 col, 16dp, 412dp)
Ajuster la largeur de la frame : 412dp
Ajuster les safe areas selon Android (status bar 24dp, nav gestures 16dp)
```

### 3c — Remplacer les composants système automatisables

**Si OS de base = iOS → déclinaison Android :**

| Composant iOS | Composant Android | Automatisable |
|--------------|------------------|--------------|
| Status Bar iOS | Status Bar Android | ✅ Auto |
| Navigation Bar (push) | Top App Bar | ✅ Auto |
| Tab Bar | Bottom Navigation Bar | ✅ Auto |
| Bottom Sheet (iOS) | Bottom Sheet (Material) | ✅ Auto |
| iOS Alert | Material Dialog | ✅ Auto |
| SF Symbols icons | Material Icons | ⚠️ Mapping selon disponibilité |
| iOS Switch | Material Switch | ✅ Auto |
| iOS Segmented Control | Material Tab Row | ✅ Auto |
| iOS Slider | Material Slider | ✅ Auto |

**Si OS de base = Android → déclinaison iOS :**

| Composant Android | Composant iOS | Automatisable |
|-----------------|--------------|--------------|
| Status Bar Android | Status Bar iOS | ✅ Auto |
| Top App Bar | Navigation Bar | ✅ Auto |
| Bottom Navigation Bar | Tab Bar | ✅ Auto |
| Material Dialog | iOS Alert | ✅ Auto |
| FAB | — (pas d'équivalent direct) | ✋ Manuel |
| Material Icons | SF Symbols | ⚠️ Mapping selon disponibilité |

### 3d — Appliquer les tokens typographiques OS secondaire

```
Remplacer tous les Text Styles ios/* par android/* (ou inverse)
Ex : ios/type/body → android/type/body_large
Ex : ios/type/headline → android/type/headline_medium
```

### 3e — Rapport de la partie automatisée

```markdown
## Déclinaison automatisée — [feature-slug] — [YYYY-MM-DD]

| Frame créée | Grille | Composants système | Text Styles | Statut |
|------------|--------|-------------------|------------|--------|
| US_042_login_android_default | ✅ 412dp | ✅ Top App Bar | ✅ Android | Auto OK |
| US_044_dashboard_android_default | ✅ 412dp | ✅ Bottom Nav | ✅ Android | Auto OK |

Composants à ajuster manuellement : [N]
→ Voir section suivante
```

---

## Étape 4 — Partie manuelle (Product Designer)

Présenter la liste des ajustements que les agents ne peuvent pas faire seuls :

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AJUSTEMENTS MANUELS — Product Designer
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Les frames Android ont été créées automatiquement.
Les ajustements suivants nécessitent votre intervention :

[Pour chaque ajustement manuel :]

## Ajustement [N] — [Titre court]
Frames concernées : [US_###_nom_android_default, ...]
Problème : [Description — ex: FAB Android sans équivalent iOS direct]
Action : [Ce que le Product Designer doit faire]
Résultat attendu : [Description visuelle du résultat]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Prévenez-moi quand les ajustements sont terminés
pour lancer la vérification de conformité.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Étape 5 — Vérification de conformité OS secondaire

Après confirmation du Product Designer, vérifier via MCP :

```
get_screenshot [frames android créées]
→ Vérifier visuellement les adaptations

check-guidelines-compliance [feature-slug]/android
→ Conformité Material 3 / HIG selon l'OS secondaire
```

Checklist de vérification OS secondaire :

**Si déclinaison iOS → Android :**
- [ ] Grille 412dp active sur toutes les frames
- [ ] Status Bar Android (24dp) — pas la iOS
- [ ] Top App Bar ou Bottom Navigation selon la nav map
- [ ] Text Styles `android/*` appliqués — aucun `ios/*` restant
- [ ] Zones tactiles ≥ 48×48dp (Material) — pas 44×44pt
- [ ] Back gesture Android géré (bouton système — pas swipe bord gauche)
- [ ] Material ripple effect annoté sur les éléments interactifs
- [ ] Dynamic Color compatible (Material You)

**Si déclinaison Android → iOS :**
- [ ] Grille 393pt active sur toutes les frames
- [ ] Status Bar iOS — Dynamic Island si iPhone 14+ ciblé
- [ ] Navigation bar push ou Tab Bar selon la nav map
- [ ] Text Styles `ios/*` appliqués — aucun `android/*` restant
- [ ] Zones tactiles ≥ 44×44pt (HIG)
- [ ] Swipe back depuis bord gauche — pas de bouton Back physique
- [ ] Safe Areas respectées (notch, Dynamic Island, home indicator)
- [ ] SF Symbols utilisés — pas Material Icons

---

## Étape 6 — Déplacer vers Validated UI

Une fois la vérification passée, déplacer les frames OS secondaire vers `Validated UI` :

```
Action : Déplacer les frames [android/*] ou [ios/*] validées
De : Work In Progress UI → section US_###
Vers : Validated UI → section US_###
```

---

## Phase de validation

1. Toutes les frames de l'OS de base ont-elles leur équivalent OS secondaire créé — aucune manquante ? (oui / non + frames manquantes)
2. La grille OS secondaire est-elle active sur 100% des frames — bonne largeur et marges ? (oui / non + frames incorrectes)
3. Aucun Text Style de l'OS de base ne subsiste dans les frames OS secondaire ? (oui / non + frames à corriger)
4. Les composants système spécifiques sont-ils corrects — Navigation, Status Bar, Tab/Bottom Nav ? (oui / non)
5. Les ajustements manuels du Product Designer ont-ils été appliqués ? (oui / non)

> Si tout validé → frames OS secondaire prêtes pour les annotations RAAM et le handoff.

## Résumé de fin d'exécution

```
✅ os-decline "$ARGUMENTS" terminé
OS de base    : [iOS / Android] — [N] frames source
OS secondaire : [Android / iOS] — [N] frames créées

Automatisé : grilles, composants système, text styles
Manuel PD   : [N] ajustements appliqués

Prochaines étapes :
→ /write-accessibility-annotations $ARGUMENTS (OS secondaire)
→ /write-figma-handoff $ARGUMENTS (handoff complet iOS + Android)
→ /review-figma-scope $ARGUMENTS (scope complet deux OS)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
