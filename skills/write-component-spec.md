---
name: write-component-spec
description: Spécifie un composant UX — comportements, états et variantes sans valeurs visuelles. Exécuté par le ux-designer.
argument-hint: [feature-slug]/[component-slug]
disable-model-invocation: false
context: fork
agent: ux-designer
---

## Rôle
Spécifier un composant UX — comportements, états, variantes et interactions — sans valeurs visuelles.

## Agents consommateurs
- UX Designer (pilote)

## Distinction avec write-component-ui
| Ce skill | write-component-ui |
|----------|--------------------|
| Décrit le comportement | Implémente le code |
| Produit par UX Designer | Produit par UI Designer |
| Sans tokens ni valeurs visuelles | Avec tokens et code SwiftUI/Compose |
| Input pour UI Designer | Output final |

## Prérequis
- [ ] User stories (`US-###`) utilisant ce composant identifiées
- [ ] Guidelines accessibilité consultées dans `./design-system/guidelines/`
- [ ] Design System Manager consulté pour vérifier si le composant existe déjà

## Processus de génération

### Étape 1 — Vérifier l'existence du composant
Consulter `./design-system/components/` avant toute création. Si un composant existant couvre le besoin → documenter pourquoi et refermer.

### Étape 2 — Rédiger la spec

```markdown
# Composant — [Nom]

## Métadonnées
- Feature : [FEAT-###]
- User Stories : [US-###, US-###]
- UX Designer : [nom]
- Statut : [Draft / In Review / Approved]

## Usage
[Dans quels contextes ce composant est utilisé.
Soyez précis — "partout" n'est pas un usage valide.]

## Anatomie
| Partie | Description | Obligatoire |
|--------|-------------|------------|
| [Partie 1] | [Description fonctionnelle] | Oui / Non |
| [Partie 2] | [Description fonctionnelle] | Oui / Non |

## États
| État | Description comportementale | Déclencheur | Interaction disponible |
|------|-----------------------------|------------|----------------------|
| Default | [Description de l'état au repos] | Initial | [tap / swipe / ...] |
| Pressed | [Retour visuel immédiat — < 100ms] | Tap | — |
| Disabled | [Non interactif, indication claire à l'utilisateur] | Condition métier | Aucune |
| Loading | [Indication de traitement en cours — non bloquant] | Action async | — |
| Error | [Indication d'erreur avec message explicatif] | Erreur système | Retry / Dismiss |

## Variantes
| Variante | Usage | Différences comportementales |
|----------|-------|------------------------------|
| Primary | [Action principale, priorité haute] | [Comportement spécifique] |
| Secondary | [Action secondaire ou complémentaire] | [Comportement spécifique] |
| Destructive | [Actions irréversibles — suppression, déconnexion] | Confirmation requise avant exécution |

## Interactions détaillées
| Geste / Action | Résultat | Plateforme |
|---------------|---------|-----------|
| Tap | [Comportement] | iOS + Android |
| Long press | [Comportement] | iOS + Android |
| Swipe | [Comportement] | [Plateforme spécifique si applicable] |

## Intention de style (pour UI Designer)
- Rôle visuel : [ex: Élément mis en avant / Action destructive / État neutre]
- Importance dans la hiérarchie : [Primaire / Secondaire / Tertiaire]
- Contexte d'apparition : [Inline / Flottant / Plein écran]

## Accessibilité
- [ ] Label VoiceOver / TalkBack : [Formulation exacte]
- [ ] Hint (si action non évidente) : [Formulation exacte]
- [ ] Zone tactile minimum : 44x44pt (iOS) / 48x48dp (Android)
- [ ] Comportement en texte agrandi : [Description]
- [ ] État désactivé communiqué autrement que par la couleur

## Comportements spécifiques plateforme
| Comportement | iOS | Android |
|-------------|-----|---------|
| [Aspect natif] | [Comportement iOS] | [Comportement Android] |
```


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Aucune valeur visuelle dans ce document (pas de couleurs, tailles, tokens)
- L'état `Destructive` requiert toujours une confirmation avant exécution
- L'état `Error` doit toujours proposer une action de récupération
- La section "Intention de style" remplace les tokens — elle guide sans contraindre

## Chemin de fichier
```
design/[feature-slug]/ux/component-[nom].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les états sont-ils couverts — default, pressed, disabled, loading, error ? (oui / manque : ...)
2. La section 'Intention de style' décrit-elle le rôle visuel sans valeurs absolues — aucune couleur ou taille en dur ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-component-spec "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/component-[slug].md

Prochaines étapes :
→ /write-component-ui $ARGUMENTS
→ /write-component-ds $ARGUMENTS (si ajout DS)
```
