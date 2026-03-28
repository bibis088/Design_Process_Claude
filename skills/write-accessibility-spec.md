---
name: write-accessibility-spec
description: "Produit la spec d'accessibilité RAAM d'une feature — critères applicables, annotations par écran, checklist de conformité niveau A et AA. Exécuté par le ux-designer en coordination avec le qa-engineer."
argument-hint: "[feature-slug]"
disable-model-invocation: false
context: fork
agent: ux-designer
---

## Rôle
Produire la spécification d'accessibilité RAAM complète d'une feature — critères applicables, annotations écran par écran, checklist de conformité niveau A (obligatoire) et AA (cible).

## Agents consommateurs
- UX Designer (pilote — produit la spec)
- QA Engineer (consommateur — utilise comme référence de vérification)
- UI Designer (consommateur — implémente les annotations)

## Référentiel
RAAM — Référentiel d'Accessibilité des Applications Mobiles
Consulter `rules/accessibility.md` pour le détail complet des critères.

**Niveaux :**
- Niveau A : obligatoire — bloque le passage en `Done`
- Niveau AA : cible — requis pour conformité complète

## Prérequis
- [ ] Navigation map (`SCREENS-MAP.md`) disponible dans `./design/$ARGUMENTS/`
- [ ] Specs d'écran (`S-XX-[nom].md`) disponibles dans `./design/$ARGUMENTS/ux/screens/`
- [ ] User stories (`US-###`) disponibles dans `./specs/[epic-slug]/stories/`
- [ ] `rules/accessibility.md` consulté avant de démarrer

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Specs d'écran ou navigation map manquantes — produire d'abord `/write-navigation-map $ARGUMENTS` et `/write-screen-spec $ARGUMENTS/[screen-id]`.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Processus de génération

### Étape 1 — Identifier les populations concernées
Depuis l'épic (`EPIC.md`), identifier les utilisateurs potentiellement handicapés :
- Déficience visuelle (malvoyance, daltonisme, cécité)
- Déficience motrice (navigation switch, contrôle vocal, tremblement)
- Déficience auditive (si contenu audio/vidéo)
- Déficience cognitive (si flux complexe ou charge cognitive élevée)

### Étape 2 — Sélectionner les critères RAAM applicables
Pour chaque écran de la feature, identifier les critères RAAM qui s'appliquent selon le contenu :

| Contenu présent | Critères RAAM applicables |
|----------------|--------------------------|
| Images informatives | RAAM 1.1, 1.2, 1.3 |
| Couleurs porteuses d'info | RAAM 2.1 |
| Textes | RAAM 2.2, 2.3, 10.2 |
| Boutons / liens | RAAM 5.1, 6.1, 13.1, 13.2 |
| Formulaires | RAAM 6.2, 6.3, 6.4, 11.1, 11.2 |
| Navigation entre écrans | RAAM 7.1, 7.2, 7.3, 9.1 |
| Gestes (swipe, pinch) | RAAM 12.1, 12.2, 12.3 |
| Médias (vidéo, audio) | RAAM 3.1, 3.2 |
| Tableaux | RAAM 4.1, 4.2 |

### Étape 3 — Produire la spec d'accessibilité

```markdown
# Spec Accessibilité — [FEAT-###] [NomFeature]

## Métadonnées
- Feature : [FEAT-###]
- Référentiel : RAAM (Référentiel d'Accessibilité des Applications Mobiles)
- Niveau minimum : A (obligatoire)
- Niveau cible : AA
- UX Designer : [nom]
- Statut : [Draft / In Review / Approved]
- Date : [YYYY-MM-DD]

## Populations concernées
- [ ] Déficience visuelle — malvoyance, daltonisme, cécité
- [ ] Déficience motrice — navigation switch, contrôle vocal
- [ ] Déficience auditive — [si applicable]
- [ ] Déficience cognitive — [si applicable]

## Critères RAAM applicables à cette feature

### Niveau A — obligatoire
| Critère | Description | Écrans concernés |
|---------|-------------|-----------------|
| RAAM 1.1 | Alt text images informatives | [S-01, S-03] |
| RAAM 2.1 | Info non transmise par couleur seule | [Tous] |
| RAAM 6.1 | Labels boutons accessibles | [Tous] |
| RAAM 7.1 | Ordre de focus logique | [Tous] |
| RAAM 13.1 | Zones tactiles min. 44x44pt / 48x48dp | [Tous] |

### Niveau AA — cible
| Critère | Description | Écrans concernés |
|---------|-------------|-----------------|
| RAAM 2.2 | Contraste texte normal ≥ 4.5:1 | [Tous] |
| RAAM 2.3 | Contraste grands textes ≥ 3:1 | [S-01, S-02] |
| RAAM 2.4 | Contraste composants interactifs ≥ 3:1 | [Tous] |
| RAAM 10.2 | Lisible à 200% taille de police | [Tous] |

## Annotations par écran

### [S-01] — [Nom de l'écran]

#### Éléments interactifs
| Élément | Label accessible | Rôle | Critère RAAM |
|---------|----------------|------|-------------|
| [Bouton Connexion] | "Se connecter à votre compte" | Button | 6.1 |
| [Champ Email] | "Adresse email, champ obligatoire" | TextField | 6.2 |
| [Image logo] | "Logo [NomApp]" | Image | 1.1 |
| [Image fond] | (décoratif — ignoré) | Decoration | 1.2 |

#### Ordre de focus
```
1 → [Header / Titre de l'écran]
2 → [Champ Email]
3 → [Champ Mot de passe]
4 → [Bouton Connexion]
5 → [Lien Mot de passe oublié]
```

#### Contrastes à vérifier
| Élément | Token couleur | Ratio cible | RAAM |
|---------|-------------|------------|------|
| Texte principal | `color.content.primary` | ≥ 4.5:1 | 2.2 |
| Bouton CTA | `color.interactive.primary` | ≥ 3:1 | 2.4 |
| Texte erreur | `color.feedback.error` | ≥ 4.5:1 | 2.2 |

#### Gestes alternatifs
| Geste | Alternative | RAAM |
|-------|-----------|------|
| Swipe gauche pour supprimer | Bouton Supprimer visible en appui long | 12.2 |

#### Annotations Figma
- Frame : [nom du frame Figma]
- Annotations ajoutées : labels, ordre de focus (1→N), zones tactiles overlay
- Plugin : [Stark / Figma Accessibility / autre]

---

### [S-02] — [Nom de l'écran suivant]
[Même structure]

---

## Checklist de conformité globale

### Niveau A — obligatoire (bloque `Done` si non coché)
- [ ] RAAM 1.1 — Alt text sur toutes les images informatives
- [ ] RAAM 2.1 — Aucune info transmise uniquement par la couleur
- [ ] RAAM 6.1 — Labels accessibles sur tous les boutons
- [ ] RAAM 6.2 — Labels sur tous les champs de formulaire
- [ ] RAAM 7.1 — Ordre de focus logique vérifié sur tous les écrans
- [ ] RAAM 7.2 — Focus visible à tout moment
- [ ] RAAM 9.1 — Titres d'écran pertinents et uniques
- [ ] RAAM 12.1 — Aucune fonctionnalité accessible uniquement par geste complexe
- [ ] RAAM 13.1 — Zones tactiles ≥ 44x44pt (iOS) / 48x48dp (Android)

### Niveau AA — cible
- [ ] RAAM 2.2 — Contraste texte normal ≥ 4.5:1
- [ ] RAAM 2.3 — Contraste grands textes ≥ 3:1
- [ ] RAAM 2.4 — Contraste composants interactifs ≥ 3:1
- [ ] RAAM 6.3 — Erreurs identifient le champ concerné
- [ ] RAAM 7.3 — Navigation cohérente entre les écrans
- [ ] RAAM 10.2 — Contenu lisible à 200% de taille de police
- [ ] RAAM 12.2 — Gestes complexes ont une alternative simple
- [ ] RAAM 13.2 — Espacement suffisant entre zones tactiles

## Annotations Figma — récapitulatif feature
| Écran | Frame Figma | Labels | Ordre focus | Zones tactiles | Contrastes |
|-------|------------|--------|------------|---------------|-----------|
| S-01 | [nom/lien] | ✅ / ❌ | ✅ / ❌ | ✅ / ❌ | ✅ / ❌ |
| S-02 | [nom/lien] | ✅ / ❌ | ✅ / ❌ | ✅ / ❌ | ✅ / ❌ |
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Tous les écrans de la feature ont-ils une section d'annotations complète — labels, ordre de focus, contrastes ? (oui / non + écrans manquants)
2. Les critères RAAM niveau A sont-ils tous documentés pour chaque écran ? (oui / non + critères manquants)
3. Les annotations Figma sont-elles en place sur tous les frames — labels, numérotation de focus, overlay zones tactiles ? (oui / non + frames non annotés)
4. Les gestes complexes ont-ils tous une alternative simple documentée ? (oui / non + gestes sans alternative)
5. La checklist de conformité est-elle complète et signée par le UX Designer ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-accessibility-spec "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/accessibility-spec.md

Prochaines étapes :
→ /write-screen-ui $ARGUMENTS/[screen-id] (implémentation UI avec annotations)
→ /review-dod $ARGUMENTS/accessibility (vérification conformité RAAM avant Done)
→ QA Engineer : utiliser cette spec comme référence pour les tests d'accessibilité
```
