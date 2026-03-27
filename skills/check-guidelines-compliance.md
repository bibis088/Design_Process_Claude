---
name: check-guidelines-compliance
description: Vérifie la conformité des composants et écrans Figma selon HIG (iOS) et Material 3 (Android). Intégré dans l'agent UI Designer. Exécuté via `use_figma`.
argument-hint: [feature-slug]/[component-ou-screen-slug]
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Auditer un composant ou un écran Figma pour vérifier sa conformité aux guidelines Apple HIG et Material Design 3 — et produire un rapport de non-conformités avec actions correctives.

## Agents consommateurs
- UI Designer (pilote — audite et corrige)
- Design System Manager (contributeur — valide la conformité avant publication bibliothèque)
- QA Engineer (consommateur — référence pour les tests de conformité)

## Prérequis
- [ ] Composant ou écran Figma créé et accessible
- [ ] `rules/figma.md` consulté pour les valeurs de référence HIG/Material 3
- [ ] MCP Figma connecté pour lire les propriétés des layers

## Gestion des erreurs

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire la checklist de conformité à vérifier manuellement.

## Processus de vérification

### Étape 1 — Lire les propriétés via `use_figma`
Extraire depuis le composant ou l'écran :
- Dimensions de tous les éléments interactifs
- Valeurs de padding et spacing
- Styles typographiques appliqués
- Couleurs et tokens utilisés
- Border radius
- Structure de l'auto layout

### Étape 2 — Audit iOS (HIG)

```markdown
## Audit HIG — iOS

| Critère | Valeur attendue | Valeur trouvée | Statut | Action |
|---------|----------------|----------------|--------|--------|
| Zones tactiles | ≥ 44x44pt | [valeur] | ✅/❌ | [correction] |
| Navigation push | Bottom Sheet ou Push | [type utilisé] | ✅/❌ | |
| Tab bar items | 3-5 max | [nombre] | ✅/❌ | |
| Safe area insets | Respectés | [oui/non] | ✅/❌ | |
| Typographie | Styles iOS/* | [styles trouvés] | ✅/❌ | |
| Border radius boutons | radius/lg (12pt) | [valeur] | ✅/❌ | |
| Dynamic Type | Supporté | [oui/non] | ✅/❌ | |
| SF Symbols | Utilisés si applicable | [oui/non/N/A] | ✅/N/A | |
| Swipe back | Gestionnaire configuré | [oui/non] | ✅/❌ | |
```

### Étape 3 — Audit Android (Material 3)

```markdown
## Audit Material 3 — Android

| Critère | Valeur attendue | Valeur trouvée | Statut | Action |
|---------|----------------|----------------|--------|--------|
| Zones tactiles | ≥ 48x48dp | [valeur] | ✅/❌ | [correction] |
| FAB présent | Si action principale | [oui/non/N/A] | ✅/N/A | |
| Bottom Navigation | 3-5 items | [nombre] | ✅/❌ | |
| Ripple effect | Annoté | [oui/non] | ✅/❌ | |
| Dynamic Color | Material You | [oui/non] | ✅/❌ | |
| Edge-to-edge | Window insets respectés | [oui/non] | ✅/❌ | |
| Typographie | Styles Android/* | [styles trouvés] | ✅/❌ | |
| Élévation | Tokens shadow/* | [oui/non] | ✅/❌ | |
| Material Icons | Utilisés si applicable | [oui/non/N/A] | ✅/N/A | |
```

### Étape 4 — Audit tokens et nomenclature

```markdown
## Audit tokens et nomenclature

| Critère | Attendu | Trouvé | Statut |
|---------|---------|--------|--------|
| Couleurs | Tokens sémantiques uniquement | [oui/non] | ✅/❌ |
| Typographie | Styles DS uniquement | [oui/non] | ✅/❌ |
| Spacing | Tokens spacing/* | [oui/non] | ✅/❌ |
| Radius | Tokens radius/* | [oui/non] | ✅/❌ |
| Nomenclature layers | Convention rules/figma.md | [oui/non] | ✅/❌ |
| Auto layout | Activé sur tous les composants | [oui/non] | ✅/❌ |
| Variants nommés | Convention State/Variant/Platform | [oui/non] | ✅/❌ |
```

### Étape 5 — Rapport de conformité

```markdown
# Rapport Conformité Guidelines — [NomComposant/Écran] — [YYYY-MM-DD]

## Résultat global
- iOS HIG : [✅ Conforme / ⚠️ Réserves / ❌ Non conforme]
- Material 3 : [✅ Conforme / ⚠️ Réserves / ❌ Non conforme]
- Tokens DS : [✅ Conforme / ⚠️ Réserves / ❌ Non conforme]

## Non-conformités bloquantes
| # | Plateforme | Critère | Correction requise |
|---|-----------|---------|-------------------|
| 1 | iOS | Zone tactile bouton X : 36pt < 44pt | Agrandir padding à 4pt minimum |

## Non-conformités mineures
| # | Plateforme | Critère | Correction suggérée |
|---|-----------|---------|-------------------|
| 1 | Android | Ripple non annoté | Ajouter annotation layer |

## Anti-patterns détectés
| # | Anti-pattern | Élément | Correction |
|---|-------------|---------|-----------|
| 1 | [Anti-pattern de la liste `rules/figma.md`] | [Layer/Frame] | [Correction] |

## Verdict
- [ ] ✅ Conforme — composant/écran validé pour la suite
- [ ] ❌ Non conforme — corrections bloquantes à apporter avant de continuer
```

### Étape 5 — Vérification anti-patterns (checklist systématique)

Vérifier la liste des anti-patterns de `rules/figma.md` — section "Anti-patterns UI mobiles" :

**Critiques — bloquants :**
- [ ] Aucun emoji utilisé comme icône de navigation
- [ ] Information jamais transmise par couleur seule
- [ ] Focus rings visibles sur tous les éléments interactifs
- [ ] Boutons désactivés pendant les opérations async
- [ ] Gestes système non bloqués (swipe back iOS, back Android)
- [ ] Toutes les zones tactiles ≥ 44pt iOS / 48dp Android

**Hauts — importants :**
- [ ] Messages d'erreur au niveau du champ — pas uniquement en haut
- [ ] Labels visibles — placeholder ne remplace pas le label
- [ ] `prefers-reduced-motion` respecté sur toutes les animations
- [ ] Aucune valeur hex en dur dans les composants — tokens uniquement
- [ ] Swipe actions ont un affordance visuel
- [ ] Skeleton screens pour les opérations > 1s

**Moyens — à corriger avant handoff :**
- [ ] Un seul jeu d'icônes dans le projet
- [ ] Dark mode : variantes dédiées — pas d'inversion
- [ ] Listes 50+ items : virtualisation prévue (annotation Dev)

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Toutes les non-conformités bloquantes ont-elles été corrigées dans Figma ? (oui / non + éléments restants)
2. Les zones tactiles sont-elles conformes sur iOS (≥ 44pt) ET Android (≥ 48dp) sur tous les éléments interactifs ? (oui / non)
3. Aucune valeur de couleur, typographie ou spacing en dur dans les layers — uniquement des tokens DS ? (oui / non + layers à corriger)
4. Tous les anti-patterns critiques de la checklist sont-ils absents ? (oui / non + anti-patterns détectés)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ check-guidelines-compliance "$ARGUMENTS" terminé
📋 Rapport : design/$ARGUMENTS/guidelines-compliance-report.md

Prochaines étapes :
→ /review-figma-scope $ARGUMENTS (PO valide la conformité fonctionnelle)
→ /write-figma-handoff $ARGUMENTS (après validation PO)
```
