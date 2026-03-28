# Conventions pour les Personas

## ✅ NOUVEAU [Y] — Rôle des personas dans le projet

Les personas sont des **outils de cadrage partagés** entre le BA, le PO et le UX Designer. Ils ne sont pas figés — ils sont définis au démarrage de chaque projet ou feature et mis à jour si le contexte évolue.

### Qui les produit
Le **Business Analyst** produit les personas lors du Cadrage initial (question 1 : "Qui utilise cette fonctionnalité ?"). Le **Product Owner** et le **UX Designer** les consomment et peuvent les enrichir.

### Où ils sont stockés
```
specs/
└── [epic-slug]/
    └── personas/
        └── PERSONA-###-[slug].md
```

### Convention de nommage
- `PERSONA-###` — numérotation globale au projet
- Slug descriptif : `PERSONA-001-regular-user`, `PERSONA-002-new-user`, `PERSONA-003-edge-user`
- Minimum 3 personas par projet :
  - `regular-user` — utilisateur régulier, cœur de cible
  - `new-user` — nouvel utilisateur, découverte du produit
  - `edge-user` — utilisateur aux limites : expert, senior, handicap, connexion dégradée, usage extrême

---

## Template persona

```markdown
# [PERSONA-###] Nom du persona

## Identité
- Prénom fictif : [ex: Sophie]
- Rôle : [ex: Manager terrain, Utilisateur grand public, Administrateur]
- Âge approximatif : [ex: 35 ans]

## Contexte d'usage
- Quand utilise-t-il le produit ? [ex: En déplacement, au bureau, le soir]
- Sur quel device principal ? [iOS / Android / les deux]
- Fréquence d'usage : [ex: Quotidien, hebdomadaire, occasionnel]
- Environnement : [ex: En mouvement, connexion instable, multitâche]

## Maturité technologique
- Niveau : [Débutant / Intermédiaire / Avancé]
- Description : [ex: À l'aise avec les apps mobiles grand public, peu familier avec les outils métier]

## Objectifs
1. [Ce qu'il veut accomplir avec ce produit]
2. [Son objectif secondaire]

## Frustrations actuelles
- [Ce qui le bloque ou l'énerve dans sa situation actuelle]
- [Pain point principal que le produit doit résoudre]

## Besoins principaux
- [Besoin fonctionnel 1]
- [Besoin fonctionnel 2]

## Citation représentative
> "[Une phrase qui résume son état d'esprit ou son besoin principal]"

## User stories associées
- [US-###] — [titre de la story]
- [US-###] — [titre de la story]
```

---

## Règles d'usage

- Un persona n'est jamais une personne réelle — c'est un archétype
- Chaque user story doit référencer au moins un persona
- Si deux personas ont les mêmes besoins et comportements, les fusionner en un seul
- Les personas sont revus à chaque nouvelle feature majeure
- Un persona `PERSONA-###` peut être partagé entre plusieurs épics du même projet

---

## Cycle de vie

| Statut | Responsable | Condition |
|--------|-------------|-----------|
| `Draft` | Business Analyst | Créé lors du Cadrage initial |
| `In Review` | PO + UX Designer | Soumis à validation après le Brief Fonctionnel |
| `Approved` | PO | Validé, utilisable dans les stories et specs UX |
| `Archived` | BA | Persona obsolète, remplacé ou fusionné |
