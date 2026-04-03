---
name: write-specs
description: "Produit la spécification fonctionnelle complète depuis le cadrage — brief EPIC, flux fonctionnels, règles métier et glossaire. Use when user says 'specs fonctionnelles', 'brief fonctionnel', 'flux fonctionnel', 'règles métier', 'glossaire', or 'write-specs'."
argument-hint: "[epic-slug]"
agent: business-analyst
---

## Rôle
Produire les 4 livrables de spécification fonctionnelle

## Règle fondamentale — Neutralité visuelle
Les flux fonctionnels, règles métier et le brief **ne doivent jamais** mentionner :
- Des composants UI (bouton, modal, liste, carousel...)
- Des gestes (swipe, tap, pinch...)
- Des détails d'implémentation technique (API, endpoint, table DB...)

Ces documents décrivent **l'intention utilisateur** et **la règle métier** — pas l'interface.

## Règle fondamentale — Gestion des erreurs structurée
Chaque flux alternatif et cas d'erreur dans les flux fonctionnels doit définir :
1. **Condition de déclenchement** — quand l'erreur se produit
2. **Impact métier** — ce que ça signifie pour l'utilisateur
3. **Comportement attendu** — ce que le système doit faire

 en séquence depuis le cadrage validé.
Gate de sortie commune : Gate 1.


## Prérequis
- [ ] Gate 0 validée — `cadrage.md` + personas Approved
- [ ] `cadrage.md` disponible dans `specs/[epic-slug]/`
- [ ] `PERSONA-###` en statut Approved
- [ ] `cadrage.md` disponible — `FLUX-###` déduits des objectifs
- [ ] `PERSONA-###` × 3 disponibles

## Étape 1 — Brief Fonctionnel (EPIC)
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
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Flux Fonctionnels

> **Distinction flux fonctionnel vs user flow**
> Flux fonctionnel = logique métier (BA) | User flow = séquence d'écrans (UX)

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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 3 — Règles Métier
### Étape 1 — Identifier les règles
Depuis les réponses au Cadrage initial et le Brief Fonctionnel, extraire :
- Toute contrainte qui s'applique indépendamment du flux ("toujours", "jamais", "seulement si")
- Toute règle de validation (format, plage de valeurs, unicité)
- Toute contrainte légale ou réglementaire
- Toute décision métier qui impacte le comportement du système

### Étape 2 — Qualifier chaque règle
Chaque règle doit avoir :
- Une **source** : qui l'a définie et pourquoi
- Une **priorité** : Critique (bloquant MVP) / Haute / Moyenne
- Un **impact** : où elle s'applique dans les flux et les composants

### Étape 3 — Rédiger la matrice

```markdown
# Règles Métier — [EPIC-###] [Nom de l'épic]

## Métadonnées
- Epic : [EPIC-###]
- Business Analyst : [nom]
- Statut : [Draft / In Review / Approved]
- Dernière mise à jour : [YYYY-MM-DD]

## Matrice des règles

| ID | Règle | Priorité | Source | Impact | Flux concernés |
|----|-------|----------|--------|--------|---------------|
| RB-001 | [Énoncé précis et non ambigu] | Critique | [Qui / quoi] | [Composant / écran] | [FLUX-###] |
| RB-002 | [Énoncé précis et non ambigu] | Haute | [Qui / quoi] | [Composant / écran] | [FLUX-###] |

## Détail des règles critiques

### [RB-001] — [Titre court]
**Énoncé** : [Formulation complète et non ambiguë]
**Origine** : [Contrainte légale / Décision métier / Contrainte technique]
**Source** : [Qui l'a définie — client, PO, légal]
**Exemples** :
- ✅ Cas conforme : [exemple]
- ❌ Cas non conforme : [exemple]
**Critère d'acceptance associé** : [CA-## de la US-###]
**Impact sur les tests** : [Ce que le QA doit vérifier]

### [RB-002] — [Titre court]
[Même structure]

## Règles dépréciées
| ID | Règle | Remplacée par | Date |
|----|-------|--------------|------|
| [RB-###] | [Ancienne règle] | [RB-###] | [YYYY-MM-DD] |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Une règle métier est **immuable dans le contexte du projet** — si elle change, créer une nouvelle version
- L'énoncé doit être **testable** : éviter "le système doit être rapide", préférer "le chargement doit être < 2s"
- Chaque règle critique a obligatoirement un exemple conforme ET non conforme
- Les règles dépréciées ne sont jamais supprimées — elles sont archivées avec leur remplaçante

## Chemin de fichier
```
specs/[epic-slug]/regles-metier.md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Chaque règle a-t-elle une source identifiée et une priorité définie ? (oui / non + règles à compléter)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-regles-metier "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/regles-metier.md

Prochaines étapes :
→ /write-user-story $ARGUMENTS
→ /review-dod RB-###

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 4 — Glossaire
### Étape 1 — Identifier les termes ambigus
Parcourir les documents produits et relever :
- Tout terme métier spécifique au domaine client
- Tout terme technique utilisé dans un sens particulier sur ce projet
- Tout synonyme potentiellement confusant (ex: "utilisateur" vs "client" vs "member")
- Tout acronyme ou abréviation

### Étape 2 — Rédiger le glossaire

```markdown
# Glossaire — [Nom du projet]

## Métadonnées
- Epic(s) couverts : [EPIC-###, EPIC-###]
- Business Analyst : [nom]
- Dernière mise à jour : [YYYY-MM-DD]

## Termes

### [Terme]
- **Définition** : [Définition précise dans le contexte de ce projet]
- **Synonymes acceptés** : [Autres mots utilisés pour désigner la même chose]
- **À ne pas confondre avec** : [Terme proche mais différent]
- **Utilisé dans** : [FLUX-###, US-###, RB-###]

### [Terme 2]
[Même structure]

## Acronymes et abréviations

| Abréviation | Forme complète | Contexte d'usage |
|-------------|---------------|-----------------|
| [ABC] | [Forme complète] | [Où et quand on l'utilise] |

## Termes exclus (à ne pas utiliser)
Ces termes créent de la confusion — utiliser les termes du glossaire à la place.

| Terme exclu | Utiliser à la place | Raison |
|-------------|--------------------|----|
| [Terme] | [Terme préféré] | [Pourquoi ce terme est confus] |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Le glossaire est **vivant** — mis à jour à chaque nouveau terme introduit dans les specs
- Un terme sans définition dans le glossaire ne doit pas apparaître dans les specs
- Si deux agents utilisent des termes différents pour la même chose → arbitrage BA → glossaire mis à jour
- Les termes exclus sont aussi importants que les termes définis

## Chemin de fichier
```
specs/[epic-slug]/glossaire.md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Tous les termes ambigus du Brief Fonctionnel sont-ils couverts ? (oui / non + termes manquants)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-glossaire "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/glossaire.md

Prochaines étapes :
→ /write-brief-fonctionnel $ARGUMENTS

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-specs "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/EPIC.md
📁 specs/$ARGUMENTS/FLUX-###-[slug].md × N
📁 specs/$ARGUMENTS/regles-metier.md
📁 specs/$ARGUMENTS/glossaire.md

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ Gate 1 — Product Designer valide brief + flux + règles + glossaire
→ /write-user-story $ARGUMENTS
```
