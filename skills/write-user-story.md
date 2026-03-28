---
name: write-user-story
description: "Génère une user story US-### complète avec critères d'acceptance, règles métier et DoD. Exécuté par le product-owner."
argument-hint: "[epic-slug]/[story-slug]"
agent: product-owner
---

## Rôle
Générer une user story `US-###` complète, testable et prête pour le backlog.

## Agents consommateurs
Product Owner  · Business Analyst 

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] Brief Fonctionnel (`EPIC-###`) produit par le BA
- [ ] Flux fonctionnel (`FLUX-###`) correspondant
- [ ] Règles métier (`RB-###`) applicables
- [ ] Persona(s) concerné(s) (`PERSONA-###`)

## Processus de génération

### Étape 1 — Identifier le contexte
1. Lire le `FLUX-###` correspondant
2. Identifier le persona principal concerné
3. Isoler l'action spécifique que cette story couvre
4. Vérifier les `RB-###` applicables

### Étape 2 — Rédiger la story
Suivre strictement ce format :

```markdown
# [US-###] Titre concis et orienté valeur

## Métadonnées
- Epic : [EPIC-###]
- Feature : [FEAT-###]
- Persona : [PERSONA-###]
- Flux : [FLUX-###]
- Priorité MoSCoW : [Must / Should / Could / Won't]
- Estimation : [XS / S / M / L / XL]
- Statut : [Draft / In Review / Approved / In Development / Done]

## Contexte
[1-2 phrases qui expliquent POURQUOI cette story existe et sa valeur métier.
Ne pas décrire le comment — décrire le problème résolu.]

## User Story
**En tant que** [PERSONA-### — rôle],
**Je veux** [action précise et concrète],
**Afin de** [bénéfice métier mesurable].

## Critères d'acceptance

### Flux heureux (happy path)
- [ ] CA-01 : [Condition observable et testable — sujet + verbe + résultat attendu]
- [ ] CA-02 : [Condition observable et testable]

### Flux alternatifs
- [ ] CA-03 : [Comportement attendu si [condition alternative]]

### Cas d'erreur
- [ ] CA-04 : [Comportement attendu si erreur réseau]
- [ ] CA-05 : [Comportement attendu si données manquantes ou invalides]
- [ ] CA-06 : [Comportement attendu si permissions refusées]

## Règles métier applicables
- [RB-###] : [Intitulé de la règle — copié depuis le document BA]
- [RB-###] : [Intitulé de la règle]

## Maquettes / Écrans concernés
- [S-XX-nom] — [description courte de l'interaction sur cet écran]

## Dépendances
- Bloqué par : [US-### ou FEAT-###] — [raison] (si applicable)
- Bloque : [US-### ou FEAT-###] — [raison] (si applicable)

## Estimation
- Taille : [XS / S / M / L / XL]
- Points Scrum : [1 / 2 / 3-5 / 8 / 13+]
- Justification : [1 phrase expliquant la complexité estimée]

## Definition of Done

### Socle non négociable
- [ ] Critères d'acceptance validés par le PO
- [ ] Comportement défini sur iOS et Android
- [ ] Accessibilité couverte (VoiceOver / TalkBack)
- [ ] Dépendances documentées
- [ ] Story relue par l'agent aval concerné (Dev ou QA)

### Exigences additionnelles (cocher selon la story)
- [ ] Coverage tests unitaires > 80% — si story technique
- [ ] Revue sécurité — si données sensibles ou authentification
- [ ] Validation design — si nouvel écran ou modification UI
- [ ] Revue performance — si chargement de données ou liste longue
- [ ] Revue légale / RGPD — si collecte ou traitement de données personnelles
```

### Étape 3 — Vérifier la qualité INVEST

Avant de finaliser, valider chaque critère :

| Critère | Question de vérification |
|---------|-------------------------|
| **I**ndépendante | Cette story peut-elle être livrée sans dépendre d'une autre non terminée ? |
| **N**égociable | Les détails d'implémentation sont-ils ouverts à discussion ? |
| **V**aluable | Apporte-t-elle une valeur visible pour l'utilisateur ou le métier ? |
| **E**stimable | L'équipe peut-elle la chiffrer avec les infos disponibles ? |
| **S**mall | Est-elle livrable en 1 sprint maximum ? |
| **T**estable | Les critères d'acceptance sont-ils vérifiables sans ambiguïté ? |

Si un critère échoue → retravailler la story avant de la soumettre.


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité des critères d'acceptance

✅ Valide :
- "L'utilisateur voit un message d'erreur si le champ email est vide"
- "Le bouton Connexion est désactivé pendant le chargement"

❌ Invalide :
- "Le formulaire fonctionne bien" — trop vague
- "L'API retourne 200" — niveau technique, pas fonctionnel
- "L'écran s'affiche correctement" — non testable

## Chemin de fichier
```
specs/[epic-slug]/stories/US-###-[slug].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Le persona référencé correspond-il bien au contexte de cette story ? (confirme PERSONA-### ou corrige)
2. Les critères d'acceptance couvrent-ils tous les cas d'erreur identifiés dans le FLUX-### source ? (oui / non + cas manquants)
3. L'estimation est-elle cohérente avec la complexité réelle — aucune inconnue technique cachée ? (oui / ajuste)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-user-story "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/stories/US-###-[slug].md

Prochaines étapes :
→ /review-dod US-###
→ /write-feature-ticket $ARGUMENTS (si toutes les stories sont Done)
```
