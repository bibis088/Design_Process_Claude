---
name: write-flux-fonctionnel
description: "Produit un flux fonctionnel FLUX-### avec préconditions, postconditions, flux alternatifs et cas d'erreur. Exécuté par le business-analyst."
argument-hint: "[epic-slug]/[flux-slug]"
agent: business-analyst
---

## Rôle
Produire un flux fonctionnel `FLUX-###` complet avec préconditions, postconditions, flux alternatifs et cas d'erreur.

## Agents consommateurs
Business Analyst  · Product Owner 

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] Les 6 questions du Cadrage initial ont reçu une réponse
- [ ] Les acteurs du flux sont identifiés
- [ ] Les règles métier applicables (`RB-###`) sont connues
- [ ] Le périmètre V1 est défini (Brief Fonctionnel)

## Processus de génération

### Étape 1 — Identifier le flux à documenter
1. Nommer le flux par son action principale ("Connexion utilisateur", "Ajout au panier")
2. Identifier le ou les acteurs impliqués
3. Définir l'état initial (précondition) et l'état final attendu (postcondition)
4. Lister les règles métier qui s'appliquent (`RB-###`)

### Étape 2 — Cartographier le flux heureux
Documenter uniquement le chemin nominal dans un premier temps — pas d'erreurs, pas d'alternatives.

### Étape 3 — Identifier les flux alternatifs et erreurs
- **ALT** : chemin alternatif valide (ex: connexion biométrique vs email)
- **ERR** : comportement attendu en cas d'échec (ex: token invalide, réseau indisponible)

### Étape 4 — Rédiger le flux
Suivre strictement ce format :

```markdown
# [FLUX-###] Nom du flux

## Métadonnées
- Epic : [EPIC-###]
- Feature : [FEAT-###]
- Personas concernés : [PERSONA-###, PERSONA-###]
- Règles métier : [RB-###, RB-###]
- Plateformes : [iOS / Android / iOS + Android]
- Statut : [Draft / In Review / Approved]

## Préconditions
- [État requis 1 — ex: L'utilisateur n'est pas connecté]
- [État requis 2 — ex: L'app est en foreground]

## Flux principal

| Étape | Acteur | Action | Système | Résultat | Plateforme |
|-------|--------|--------|---------|----------|-----------|
| 1 | [Acteur] | [Action déclenchée] | [Réponse système] | [État résultant] | [iOS / Android / Les deux] |
| 2 | [Acteur] | [Action déclenchée] | [Réponse système] | [État résultant] | [iOS / Android / Les deux] |

## Postconditions
- [État final attendu 1 — ex: L'utilisateur est authentifié]
- [État final attendu 2 — ex: Le token est stocké localement]

## Flux alternatifs

### ALT-01 — [Nom du flux alternatif]
**Déclencheur** : [Condition qui active ce flux alternatif]
**Depuis l'étape** : [Numéro d'étape du flux principal]

| Étape | Acteur | Action | Système | Résultat | Plateforme |
|-------|--------|--------|---------|----------|-----------|
| 1 | [Acteur] | [Action] | [Réponse] | [Résultat] | [Plateforme] |

**Retour au flux principal** : [Étape de reprise ou fin alternative]

## Cas d'erreur

### ERR-01 — [Nom de l'erreur]
**Déclencheur** : [Condition d'erreur — ex: Réseau indisponible]
**Depuis l'étape** : [Numéro d'étape du flux principal]
**Comportement attendu** : [Ce que le système doit faire]
**Récupération** : [Comment l'utilisateur peut reprendre — ex: Retry, retour à l'étape 1]

### ERR-02 — [Nom de l'erreur]
**Déclencheur** : [Condition d'erreur]
**Depuis l'étape** : [Numéro d'étape]
**Comportement attendu** : [Comportement système]
**Récupération** : [Action de récupération]

## Règles métier appliquées
| ID | Règle | Étape(s) concernée(s) |
|----|-------|----------------------|
| [RB-###] | [Intitulé] | [1, 3] |
| [RB-###] | [Intitulé] | [2] |

## User Stories générées depuis ce flux
- [US-###] — [Titre de la story]
- [US-###] — [Titre de la story]
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Chaque étape a un et un seul acteur responsable
- Le système est toujours un acteur distinct de l'utilisateur
- Une étape = une action atomique (pas de "l'utilisateur remplit et soumet le formulaire")
- Les préconditions et postconditions sont vérifiables
- Chaque `ERR-##` a obligatoirement une action de récupération
- Les divergences iOS / Android sont notées dans la colonne Plateforme

## Chemin de fichier
```
specs/[epic-slug]/FLUX-###-[slug].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Le flux principal couvre-t-il toutes les étapes du besoin exprimé sans saut logique ? (oui / non + étapes manquantes)
2. Chaque cas d'erreur ERR-## a-t-il une action de récupération définie pour l'utilisateur ? (oui / non)
3. Les divergences iOS / Android sont-elles toutes notées dans la colonne Plateforme ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-flux-fonctionnel "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/FLUX-###-[slug].md

Prochaines étapes :
→ /write-user-story $ARGUMENTS
→ /write-user-flow $ARGUMENTS (après validation PO)
```
