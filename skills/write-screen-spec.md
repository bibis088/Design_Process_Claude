---
name: write-screen-spec
description: "Spécifie un écran S-XX avec hiérarchie visuelle, 4 états et accessibilité. Exécuté par le ux-designer."
argument-hint: "[feature-slug]/[screen-id]-[screen-slug]"
disable-model-invocation: false
context: fork
agent: ux-designer
---

## Rôle
Spécifier un écran détaillé `S-XX-[nom].md` avec hiérarchie visuelle, états, interactions et accessibilité.

## Agents consommateurs
- UX Designer (pilote)

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] User flow correspondant produit et validé
- [ ] Navigation map (`SCREENS-MAP.md`) à jour
- [ ] User stories (`US-###`) couvertes par cet écran
- [ ] Guidelines accessibilité consultées dans `./design-system/guidelines/accessibility.md`

## Processus de génération

### Étape 1 — Identifier l'écran
1. Attribuer l'ID selon la convention : `S-01`, `S-02`... (remis à zéro par feature)
2. Nommer l'écran de façon descriptive : `S-01-login`, `S-02-dashboard`
3. Identifier le type : Root / Child / Modal / Overlay
4. Lister les user stories que cet écran couvre

### Étape 2 — Documenter les 4 états obligatoires
Chaque écran doit avoir ses 4 états documentés avant d'être considéré complet :
- **Loading** : que voit l'utilisateur pendant le chargement ?
- **Empty** : que voit l'utilisateur si aucune donnée n'est disponible ?
- **Error** : que voit l'utilisateur en cas d'erreur ?
- **Success** : l'état nominal avec données

### Étape 3 — Rédiger la spec
Suivre strictement ce format :

```markdown
# [S-XX] Nom de l'écran — [Plateforme]

## Métadonnées
- Feature : [FEAT-###]
- User Stories couvertes : [US-###, US-###]
- Flux UX source : [FLUX-###]
- Plateforme : [iOS / Android / iOS + Android]
- Statut : [To Do / In Progress / In Review / Approved / Dev Ready]

## Références Figma
- Frame : [URL directe — ex: https://figma.com/file/xxx?node-id=yyy]
- Statut Figma : [to_do / in_progress / ready_for_dev]
- Annotations accessibilité : [To Do / In Progress / Done]

## Informations générales
- Route / deeplink : `app://[chemin]`
- Barre de navigation : [Visible / Cachée / Transparente]
- Status bar : [Light / Dark / Automatique]
- Scroll : [Oui — vertical / Non / Horizontal]
- Point d'entrée depuis : [Écran(s) ou trigger(s)]
- Points de sortie vers : [Écran(s) ou action(s)]

## État — Loading
[Description de ce que voit l'utilisateur pendant le chargement]
- Composant : [Skeleton / Spinner / Shimmer]
- Durée max avant timeout : [ex: 10s]
- Comportement si timeout : [ex: Affiche ERR-01]

## État — Empty
[Description de l'état vide]
- Illustration : [Description de l'illustration ou icône]
- Message principal : [Texte affiché]
- Message secondaire : [Texte explicatif optionnel]
- CTA : [Libellé du bouton d'action → destination]

## État — Error
[Description de l'état d'erreur]
- Type d'erreur : [Réseau / Serveur / Validation / Permission]
- Message affiché : [Texte d'erreur — doit expliquer quoi faire, pas juste ce qui s'est passé]
- Action de récupération : [Retry / Retour / Contacter le support]

## État — Success (nominal)

### Hiérarchie visuelle (de haut en bas)

1. **[Composant 1 — ex: Header]**
   - Contenu : [Description précise]
   - Comportement au scroll : [Sticky / Disparaît / Collapse]
   - Actions possibles : [tap → destination / comportement]

2. **[Composant 2 — ex: Liste]**
   - Contenu : [Description précise]
   - Nombre d'items : [Fixe / Variable — max recommandé]
   - Comportement au scroll : [Normal / Infini / Pagination]
   - Actions possibles :
     - tap sur item → [destination]
     - swipe gauche → [action destructive]

3. **[Composant 3 — ex: CTA fixe]**
   - Contenu : [Libellé]
   - Position : [Fixe en bas / Flottant]
   - Actions possibles : [tap → destination]

### Typographie (rôles sémantiques)
| Élément | Rôle sémantique | Hiérarchie | Usage |
|---------|----------------|------------|-------|
| [Titre] | Heading 1 | Priorité haute | [Contexte] |
| [Sous-titre] | Body | Priorité moyenne | [Contexte] |

## Transitions
| Vers | Type | Déclencheur |
|------|------|------------|
| [S-XX-nom] | push / modal / fade | [Action déclenchante] |
| [S-XX-nom] | pop / dismiss | [Action déclenchante] |

## Accessibilité
- [ ] Labels VoiceOver (iOS) / Content Description (Android) sur tous les éléments interactifs
- [ ] Ordre de focus logique documenté (de haut en bas, gauche à droite)
- [ ] Zones tactiles minimum : 44x44pt (iOS) / 48x48dp (Android)
- [ ] Aucune information transmise uniquement par la couleur
- [ ] Comportement en texte agrandi (Dynamic Type / Large Font) décrit
- [ ] Gestes alternatifs disponibles pour tout swipe ou geste complexe

### Détail des labels d'accessibilité
| Élément | Label VoiceOver / TalkBack | Hint (si nécessaire) |
|---------|---------------------------|---------------------|
| [Élément interactif] | [Label précis] | [Indication d'action si non évidente] |

## Comportements spécifiques plateforme
| Comportement | iOS | Android |
|-------------|-----|---------|
| [Navigation retour] | Swipe back natif | Bouton back système |
| [Autre divergence] | [Comportement iOS] | [Comportement Android] |

## User Stories couvertes
| US | Critères d'acceptance couverts |
|----|-------------------------------|
| [US-###] | [CA-01, CA-02, CA-03] |
```


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Les 4 états sont **obligatoires** — un écran sans état Empty ou Error n'est pas complet
- Les messages d'erreur doivent expliquer **quoi faire**, pas juste ce qui s'est passé
- Chaque élément interactif a un label d'accessibilité — aucune exception
- Les divergences iOS / Android sont documentées explicitement
- Un écran ne peut pas être en statut `Approved` sans que les 4 états soient documentés

## Chemin de fichier
```
design/[feature-slug]/screens/S-XX-[nom].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les 4 états sont-ils tous documentés — loading, empty, error, success ? (oui / manque : ...)
2. Chaque élément interactif a-t-il un label d'accessibilité défini ? (oui / non + éléments manquants)
3. Les comportements divergents iOS / Android sont-ils tous notés explicitement ? (oui / non)
4. L'URL du frame Figma est-elle renseignée dans la section "Références Figma" ? (oui / non — ajouter l'URL avant de continuer)
5. Le frame Figma contient-il en description la référence `S-XX` / `US-###` / `FLUX-###` ? (oui / non — à compléter dans Figma)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-screen-spec "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/screens/S-XX-[nom].md

Prochaines étapes :
→ /review-dod S-XX
→ /write-component-spec $ARGUMENTS (si nouveau composant)
```
