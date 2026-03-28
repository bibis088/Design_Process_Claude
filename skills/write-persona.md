---
name: write-persona
description: "Crée un persona PERSONA-### archétypal exploitable par le BA, PO et UX Designer. Exécuté par le business-analyst."
argument-hint: "[epic-slug]/[persona-slug]"
disable-model-invocation: false
context: fork
agent: business-analyst
---

## Rôle
Créer un persona `PERSONA-###` complet, archétypal et exploitable par le BA, le PO et le UX Designer.

## Agents consommateurs
- Business Analyst (pilote — produit lors du Cadrage initial)
- Product Owner (contributeur — enrichit)
- UX Designer (contributeur — enrichit)

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] La question 1 du Cadrage initial a reçu une réponse ("Qui utilise cette fonctionnalité ?")
- [ ] Le contexte d'usage est connu (mobile, fréquence, environnement)
- [ ] Au minimum 2 personas sont prévus : un utilisateur régulier et un nouvel utilisateur

## Processus de génération

### Étape 1 — Identifier l'archétype
Ne pas inventer un persona de toutes pièces. Le construire depuis les réponses au Cadrage initial :
1. Identifier le rôle principal de l'utilisateur dans son contexte
2. Déduire son niveau de maturité tech depuis le contexte d'usage
3. Formuler ses frustrations depuis les pain points exprimés
4. Construire sa citation depuis son besoin principal

### Étape 2 — Vérifier l'unicité
Avant de créer un nouveau persona, vérifier que :
- Il ne couvre pas les mêmes besoins et comportements qu'un persona existant
- Si deux personas sont trop similaires → les fusionner en un seul

### Étape 3 — Rédiger le persona
Suivre strictement ce format :

```markdown
# [PERSONA-###] Nom du persona

## Métadonnées
- Epic(s) associé(s) : [EPIC-###, EPIC-###]
- Créé par : Business Analyst
- Statut : [Draft / In Review / Approved / Archived]
- Date de création : [YYYY-MM-DD]
- Dernière mise à jour : [YYYY-MM-DD]

## Identité
- Prénom fictif : [Prénom neutre et mémorable]
- Rôle : [ex: Manager terrain, Utilisateur grand public, Administrateur]
- Âge approximatif : [ex: 35 ans]

## Contexte d'usage
| Dimension | Détail |
|-----------|--------|
| Quand utilise-t-il le produit ? | [ex: En déplacement, le matin avant réunion] |
| Device principal | [iOS / Android / Les deux — préciser le modèle type si pertinent] |
| Fréquence d'usage | [Quotidien / Hebdomadaire / Occasionnel] |
| Environnement | [ex: En mouvement, connexion instable, multitâche, bruit ambiant] |
| Contexte émotionnel | [ex: Sous pression, détendu, pressé] |

## Maturité technologique
- Niveau : [Débutant / Intermédiaire / Avancé]
- Description : [1-2 phrases décrivant son aisance avec les apps mobiles et les outils numériques]

## Objectifs
1. [Objectif principal — ce qu'il veut accomplir avec ce produit]
2. [Objectif secondaire]
3. [Objectif tertiaire si pertinent]

## Frustrations actuelles
- [Frustration principale — pain point que le produit doit résoudre en priorité]
- [Frustration secondaire]
- [Frustration liée au contexte mobile si applicable]

## Besoins principaux
| Besoin | Priorité | Type |
|--------|----------|------|
| [Besoin fonctionnel 1] | Haute | [Fonctionnel / Émotionnel / Social] |
| [Besoin fonctionnel 2] | Moyenne | [Fonctionnel / Émotionnel / Social] |

## Citation représentative
> "[Une phrase à la première personne qui résume son état d'esprit ou son besoin principal.
> Doit sonner authentique — pas comme un brief marketing.]"

## Ce persona n'est PAS
- [Archétype exclu pour éviter la confusion avec un autre persona]
- [ex: "Pas un administrateur système — il n'a pas accès aux paramètres avancés"]

## User stories associées
| US | Titre | Rôle dans la story |
|----|-------|--------------------|
| [US-###] | [Titre] | [Acteur principal / Acteur secondaire] |
```

### Étape 4 — Valider le cycle de vie

| Statut | Responsable | Condition |
|--------|-------------|-----------|
| `Draft` | Business Analyst | Créé lors du Cadrage initial |
| `In Review` | PO + UX Designer | Soumis après le Brief Fonctionnel |
| `Approved` | PO | Validé, utilisable dans les stories et specs UX |
| `Archived` | BA | Obsolète, remplacé ou fusionné |


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/specs/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Un persona est un **archétype**, jamais une personne réelle
- La citation doit être **à la première personne** et sonner naturelle
- La section "Ce persona n'est PAS" est obligatoire si plusieurs personas coexistent
- Minimum 2 personas par projet — un utilisateur régulier et un nouvel utilisateur
- Un persona `Approved` peut être partagé entre plusieurs épics du même projet
- Si deux personas ont les mêmes besoins et comportements → fusionner obligatoirement

## Chemin de fichier
```
specs/[epic-slug]/personas/PERSONA-###-[slug].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Ce persona est-il suffisamment distinct des personas existants pour justifier sa création ? (oui / fusionner avec PERSONA-###)
2. La citation représentative sonne-t-elle naturelle — comme une vraie phrase d'utilisateur ? (oui / à reformuler)
3. La section 'Ce persona n'est PAS' évite-t-elle toute confusion avec les autres personas du projet ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-persona "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/personas/PERSONA-###-[slug].md

Prochaines étapes :
→ /write-brief-fonctionnel $ARGUMENTS
→ /review-dod PERSONA-###
```
