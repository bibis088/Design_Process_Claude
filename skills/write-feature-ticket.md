---
name: write-feature-ticket
description: Génère un ticket feature FEAT-### complet avec objectifs mesurables, périmètre et KPIs. Exécuté par le product-owner.
argument-hint: [epic-slug]/[feature-slug]
disable-model-invocation: false
context: fork
agent: product-owner
---

## Rôle
Générer un ticket feature `FEAT-###` complet avec objectifs mesurables, périmètre, flux et KPIs.

## Agents consommateurs
- Product Owner (pilote)

## Prérequis
- [ ] Brief Fonctionnel (`EPIC-###`) disponible et `Approved`
- [ ] Flux fonctionnels (`FLUX-###`) correspondants disponibles
- [ ] User stories (`US-###`) identifiées ou en cours de création
- [ ] Personas concernés définis (`PERSONA-###`)

## Processus de génération

### Étape 1 — Délimiter la feature
Une feature regroupe un ensemble cohérent de user stories qui livrent une valeur complète. Elle ne doit pas être trop large (> 1 sprint) ni trop étroite (< 2 stories).

### Étape 2 — Rédiger le ticket

```markdown
# [FEAT-###] Titre de la feature

## Métadonnées
- Epic : [EPIC-###]
- Product Owner : [nom]
- Priorité MoSCoW : [Must / Should / Could / Won't]
- Statut : [Draft / In Review / Approved / In Development / Done]
- Date de création : [YYYY-MM-DD]

## Résumé
[2-3 phrases décrivant la feature, sa valeur produit et le problème résolu.
Orienté utilisateur — pas technique.]

## Objectifs
| # | Objectif | Métrique | Cible | Délai |
|---|----------|---------|-------|-------|
| 1 | [Objectif mesurable] | [KPI] | [Valeur] | [Sprint / Date] |
| 2 | [Objectif mesurable] | [KPI] | [Valeur] | [Sprint / Date] |

## Personas concernés
- [PERSONA-###] — [Nom] — [Pourquoi cette feature le concerne]

## Périmètre fonctionnel

### Inclus dans cette feature
- [Fonctionnalité 1] — [US-###]
- [Fonctionnalité 2] — [US-###]

### Hors périmètre (explicitement exclu)
- [Ce qui ne sera PAS fait — et pourquoi]

## User Stories liées
| ID | Titre | Priorité | Estimation | Statut |
|----|-------|----------|------------|--------|
| [US-###] | [Titre] | P1 | [XS/S/M/L/XL] | [Draft] |
| [US-###] | [Titre] | P2 | [XS/S/M/L/XL] | [Draft] |

## Flux principal
1. [Acteur] [action]
2. [Système] [réponse]
3. [Acteur] [action suivante]

## Flux alternatifs et erreurs
- **Si** [condition] **alors** [comportement attendu]
- **En cas d'erreur réseau** : [comportement attendu]

## Exigences non-fonctionnelles
| Type | Exigence | Valeur cible |
|------|---------|-------------|
| Performance | [ex: Chargement de la liste] | [ex: < 2s] |
| Sécurité | [ex: Stockage du token] | [ex: Keychain / EncryptedSharedPreferences] |
| Accessibilité | [ex: Labels VoiceOver] | [ex: Sur tous les éléments interactifs] |

## KPIs de succès
| Métrique | Valeur actuelle | Cible | Délai de mesure |
|---------|----------------|-------|----------------|
| [Métrique] | [Baseline] | [Cible] | [J+30 / J+90] |

## Dépendances
- Bloqué par : [FEAT-### ou US-###] — [raison] (si applicable)
- Bloque : [FEAT-### ou US-###] — [raison] (si applicable)

## Risques identifiés
| Risque | Probabilité | Impact | Mitigation |
|--------|------------|--------|-----------|
| [Risque] | [Haute/Moyenne/Basse] | [Haute/Moyenne/Basse] | [Action préventive] |
```


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/specs/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Un `FEAT-###` sans KPI mesurable ne peut pas passer en `Approved`
- Le hors périmètre est **obligatoire** — il évite le scope creep en sprint
- Chaque user story doit référencer ce `FEAT-###` dans ses métadonnées
- Les risques identifiés doivent avoir une mitigation — pas de risque sans réponse

## Chemin de fichier
```
specs/[epic-slug]/features/FEAT-###-[slug].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les objectifs sont-ils mesurables — chaque objectif a-t-il un KPI et une valeur cible ? (oui / non + objectifs à compléter)
2. Le hors périmètre est-il explicitement documenté pour éviter les discussions futures ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-feature-ticket "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/features/FEAT-###-[slug].md

Prochaines étapes :
→ /write-user-story $ARGUMENTS
→ /review-dod FEAT-###
```
