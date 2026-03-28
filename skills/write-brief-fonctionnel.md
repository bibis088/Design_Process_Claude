---
name: write-brief-fonctionnel
description: "Génère le Brief Fonctionnel EPIC-### complet depuis les réponses au Cadrage initial. Exécuté par le business-analyst."
argument-hint: "[epic-slug]"
disable-model-invocation: false
context: fork
agent: business-analyst
---

## Rôle
Générer un Brief Fonctionnel complet à partir des réponses au Cadrage initial.

## Agents consommateurs
- Business Analyst (pilote)

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] Les 6 questions du Cadrage initial ont toutes reçu une réponse
- [ ] Les personas sont identifiés ou en cours de création (`PERSONA-###`)
- [ ] Le contexte mobile est connu (plateformes cibles, versions min.)

## Processus de génération

### Étape 1 — Valider les inputs
Relire les réponses au Cadrage initial et vérifier :
- Le problème est clairement identifié (pas une solution déguisée)
- Les acteurs sont nommés et distincts
- Les objectifs sont mesurables (pas "améliorer l'expérience")
- Le périmètre V1 est réaliste et découpable en stories

### Étape 2 — Rédiger le brief
Suivre strictement ce format :

```markdown
# Brief Fonctionnel — [EPIC-###] [Nom du projet / feature]

## Métadonnées
- Epic : [EPIC-###]
- Business Analyst : [nom]
- Date de création : [YYYY-MM-DD]
- Statut : [Draft / In Review / Approved]
- Dernière mise à jour : [YYYY-MM-DD]

## Contexte & Problème
[2-3 phrases max. Quel problème on résout ? Pour qui ? Pourquoi maintenant ?
Ne pas décrire la solution — décrire le problème.]

## Objectifs produit
| # | Objectif | Métrique de succès | Cible |
|---|----------|--------------------|-------|
| 1 | [Objectif mesurable] | [KPI] | [Valeur cible] |
| 2 | [Objectif mesurable] | [KPI] | [Valeur cible] |

## Acteurs
| Acteur | Description | Besoins principaux | Persona associé |
|--------|-------------|-------------------|----------------|
| [Acteur 1] | [Qui est-il] | [Ce qu'il veut accomplir] | [PERSONA-###] |
| [Acteur 2] | [Qui est-il] | [Ce qu'il veut accomplir] | [PERSONA-###] |

## Contexte mobile
| Plateforme | Version min. supportée | Contraintes spécifiques |
|------------|----------------------|------------------------|
| iOS | [ex: iOS 16+] | [ex: Face ID requis, widgets supportés] |
| Android | [ex: Android 10+] | [ex: Biométrie via BiometricPrompt] |

## Périmètre

### V1 — MVP
- [Fonctionnalité incluse et justification courte]
- [Fonctionnalité incluse et justification courte]

### V2 — Future
- [Fonctionnalité future — pourquoi reportée]
- [Fonctionnalité future — pourquoi reportée]

### Hors périmètre
- [Ce qui ne sera jamais fait ici — et pourquoi]
- [Ce qui ne sera jamais fait ici — et pourquoi]

## Dépendances inter-features

### Cette feature dépend de
| EPIC | Feature | Livrable requis | Statut |
|------|---------|----------------|--------|
| — | — | — | *(laisser "—" si aucune dépendance)* |

### Cette feature est requise par
| EPIC | Feature | Livrable fourni | Statut |
|------|---------|----------------|--------|
| — | — | — | *(laisser "—" si aucune dépendance)* |

## Flux principaux identifiés
- [FLUX-###] — [Nom du flux] — [à produire]
- [FLUX-###] — [Nom du flux] — [à produire]

## Règles métier identifiées
| ID | Règle | Priorité | Source |
|----|-------|----------|--------|
| [RB-###] | [Règle] | [Critique / Haute / Moyenne] | [Qui l'a définie] |

## Glossaire (termes clés)
| Terme | Définition | Synonymes |
|-------|-----------|-----------|
| [Terme] | [Définition précise dans ce contexte] | [Autres mots utilisés] |

## Questions ouvertes
| # | Question | Responsable | Deadline |
|---|----------|-------------|---------|
| 1 | [Question sans réponse à ce stade] | [Qui doit répondre] | [Date] |

## Livrables attendus
- [ ] [FLUX-###] — [Nom du flux]
- [ ] [US-###] — [Titre de la story]
- [ ] [PERSONA-###] — [Nom du persona]
```

### Étape 3 — Vérifier la complétude

Avant de soumettre en revue, valider la DoD BA :
- [ ] Les 6 questions du Cadrage initial ont toutes reçu une réponse
- [ ] Le Brief a été relu et validé par le Product Owner
- [ ] Tous les flux identifiés ont des préconditions et postconditions
- [ ] Chaque règle métier a un ID, une priorité et une source
- [ ] Le glossaire couvre tous les termes ambigus
- [ ] La section Contexte mobile est complétée


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/specs/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Les objectifs doivent être **SMART** : Spécifiques, Mesurables, Atteignables, Réalistes, Temporels
- Le hors périmètre est aussi important que le périmètre — documenter explicitement ce qu'on ne fait pas
- Les questions ouvertes ne bloquent pas la création du brief mais doivent être résolues avant `Approved`
- Un brief trop long est un signal que le périmètre n'est pas assez cadré — viser 1-2 pages max

## Chemin de fichier
```
specs/[epic-slug]/EPIC.md
```


## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Le problème décrit est-il bien un problème — pas une solution déguisée ? (oui / reformuler)
2. Chaque objectif est-il SMART — Spécifique, Mesurable, Atteignable, Réaliste, Temporel ? (oui / non + objectifs à retravailler)
3. Le hors périmètre est-il aussi détaillé que le périmètre V1 — rien d'ambigu laissé sans réponse ? (oui / non)
4. Les questions ouvertes bloquantes ont-elles toutes un responsable et une deadline ? (oui / non + questions à compléter)
5. La DoD BA est-elle complète — les 6 cases sont-elles toutes cochées ? (oui / non + items manquants)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-brief-fonctionnel "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/EPIC.md

Prochaines étapes :
→ /write-flux-fonctionnel $ARGUMENTS
→ /write-persona $ARGUMENTS
```
