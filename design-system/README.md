# Design System

Géré exclusivement par le `design-system-manager`.
Transversal à toutes les features — ne pas modifier sans passer par le process DS.

## Fichiers

| Fichier | Rôle |
|---------|------|
| `design-tokens.md` | Tokens visuels — couleurs, typo, spacing, composants (format Stitch/IA) |
| `tokens/` | Exports par plateforme (iOS, Android, Web) |
| `components/` | Documentation des composants |
| `guidelines/` | Règles d'usage |

## Règle fondamentale

> Toute valeur visuelle (couleur, taille, radius…) est définie dans `design-tokens.md`.
> Les autres fichiers référencent ces tokens — ils ne contiennent jamais de valeurs brutes.

## Process d'ajout

1. Vérifier que le token n'existe pas → `write-token`
2. Ajouter dans `design-tokens.md`
3. Publier dans Figma via `setup-figma-tokens`
4. Documenter le composant si applicable → `write-component-ds`
5. Logger dans `write-changelog`
