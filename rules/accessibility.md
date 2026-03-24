# Conventions pour l'Accessibilité — Référentiel RAAM

## Référentiel de référence

Le projet suit le **RAAM — Référentiel d'Accessibilité des Applications Mobiles**, référentiel français spécifique aux applications iOS et Android, distinct du WCAG qui est orienté web.

| Référentiel | Périmètre | Usage dans ce projet |
|------------|-----------|---------------------|
| RAAM | Applications mobiles iOS / Android | Référentiel principal — obligatoire |
| WCAG 2.1 | Web | Référence complémentaire pour les principes généraux |
| Apple HIG Accessibility | iOS natif | Implémentation iOS |
| Material Design Accessibility | Android natif | Implémentation Android |

Ressource officielle : https://accessibilite.numerique.gouv.fr/raam/

---

## Niveaux de conformité

| Niveau | Statut | Définition |
|--------|--------|-----------|
| **A** | Obligatoire — bloque le passage en `Done` | Critères de base sans lesquels certains utilisateurs ne peuvent pas utiliser l'application |
| **AA** | Cible — requis pour conformité RAAM complète | Critères avancés qui améliorent significativement l'expérience des utilisateurs handicapés |
| **AAA** | Bonus — non obligatoire | Critères d'excellence, effort significatif |

**Règle projet : niveau A minimum obligatoire, niveau AA cible sur toutes les features.**

---

## Critères RAAM par catégorie

### 1. Éléments graphiques (RAAM 1)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 1.1 | A | Chaque image porteuse d'information a une alternative textuelle |
| 1.2 | A | Les images décoratives sont ignorées par les technologies d'assistance |
| 1.3 | AA | Les alternatives sont pertinentes et suffisamment descriptives |

### 2. Couleurs (RAAM 2)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 2.1 | A | L'information n'est pas transmise uniquement par la couleur |
| 2.2 | AA | Contraste texte normal ≥ 4.5:1 |
| 2.3 | AA | Contraste grands textes (≥ 18pt normal / ≥ 14pt gras) ≥ 3:1 |
| 2.4 | AA | Contraste composants interactifs ≥ 3:1 |

### 3. Multimédia (RAAM 3)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 3.1 | A | Les médias temporels ont des sous-titres ou une transcription |
| 3.2 | AA | Les sous-titres sont synchronisés et pertinents |

### 4. Tableaux (RAAM 4)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 4.1 | A | Les tableaux de données ont des en-têtes identifiés |
| 4.2 | AA | Les tableaux complexes ont un résumé |

### 5. Liens (RAAM 5)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 5.1 | A | Chaque lien a un intitulé explicite ou une alternative |
| 5.2 | AA | Les liens identiques ont la même destination |

### 6. Éléments interactifs (RAAM 6)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 6.1 | A | Chaque bouton a un label accessible |
| 6.2 | A | Les champs de formulaire ont un label associé |
| 6.3 | AA | Les messages d'erreur identifient le champ en erreur |
| 6.4 | AA | Les champs obligatoires sont identifiés avant soumission |

### 7. Navigation (RAAM 7)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 7.1 | A | L'ordre de focus est logique et cohérent |
| 7.2 | A | Le focus est visible à tout moment |
| 7.3 | AA | La navigation est cohérente entre les écrans |
| 7.4 | AA | Les raccourcis clavier ne créent pas de conflits |

### 8. Éléments obligatoires (RAAM 8)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 8.1 | A | La langue principale de l'application est définie |
| 8.2 | AA | Les changements de langue sont identifiés |

### 9. Structuration (RAAM 9)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 9.1 | A | Les titres d'écran sont pertinents et uniques |
| 9.2 | AA | La hiérarchie des titres est cohérente |

### 10. Présentation (RAAM 10)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 10.1 | A | L'information n'est pas transmise uniquement par la forme ou la position |
| 10.2 | AA | Le contenu reste lisible avec un texte agrandi jusqu'à 200% |
| 10.3 | AA | L'espacement du texte peut être modifié sans perte de contenu |

### 11. Formulaires (RAAM 11)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 11.1 | A | Chaque champ a un label visible |
| 11.2 | AA | Les aides à la saisie sont disponibles |
| 11.3 | AA | Le délai de saisie est signalé et ajustable |

### 12. Gestes et interactions (RAAM 12)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 12.1 | A | Les fonctionnalités ne dépendent pas d'un geste complexe uniquement |
| 12.2 | AA | Les gestes ont une alternative simple (tap) |
| 12.3 | AA | Les actions déclenchées par mouvement ont une alternative |

### 13. Zones tactiles (RAAM 13)
| Critère | Niveau | Description |
|---------|--------|-------------|
| 13.1 | A | Les zones tactiles font au minimum 44x44pt (iOS) / 48x48dp (Android) |
| 13.2 | AA | Les zones tactiles ont un espacement suffisant entre elles |

---

## Annotations d'accessibilité obligatoires

### Dans les Épics (`EPIC.md`)
Chaque épic doit inclure une section :

```markdown
## Obligations d'accessibilité

### Niveau cible
- Niveau A : obligatoire sur toutes les features de cet épic
- Niveau AA : cible sur toutes les features de cet épic

### Critères RAAM prioritaires pour cet épic
| Critère | Description | Impact |
|---------|-------------|--------|
| RAAM 6.1 | Labels boutons | Critique — tous les CTA |
| RAAM 13.1 | Zones tactiles | Critique — navigation principale |
| RAAM 2.2 | Contrastes texte | Critique — tous les écrans |

### Populations concernées
- [ ] Déficience visuelle (malvoyance, daltonisme, cécité)
- [ ] Déficience motrice (navigation au switch, contrôle vocal)
- [ ] Déficience auditive (si contenu audio/vidéo présent)
- [ ] Déficience cognitive (si flux complexe)
```

### Dans les User Stories (`US-###`)
Chaque US doit inclure :

```markdown
## Critères d'accessibilité (RAAM)

| Critère | Niveau | Élément concerné | Attendu |
|---------|--------|-----------------|---------|
| RAAM 6.1 | A | Bouton [Nom] | label="[texte accessible]" |
| RAAM 13.1 | A | Zone tactile [Nom] | min 44x44pt iOS / 48x48dp Android |
| RAAM 2.2 | AA | Texte [Nom] | contraste ≥ 4.5:1 |

## Critères d'acceptance accessibilité
- [ ] ACA-01 : Navigation VoiceOver/TalkBack complète sans blocage
- [ ] ACA-02 : Tous les éléments interactifs sont focusables dans l'ordre logique
- [ ] ACA-03 : Aucune information transmise uniquement par la couleur
- [ ] ACA-04 : Zones tactiles ≥ 44x44pt (iOS) / 48x48dp (Android)
```

### Dans les specs d'écran (`S-XX-[nom].md`)
La section accessibilité existante est enrichie avec les références RAAM :

```markdown
## Accessibilité RAAM

### Niveau A — obligatoire
| Critère RAAM | Élément | Annotation |
|-------------|---------|-----------|
| 6.1 | [Bouton principal] | accessibilityLabel="[texte]" |
| 6.2 | [Champ email] | label associé : "Adresse email" |
| 7.1 | Ordre de focus | Header → Contenu → CTA → Tab bar |
| 13.1 | [Tous les éléments interactifs] | min 44x44pt / 48x48dp |

### Niveau AA — cible
| Critère RAAM | Élément | Valeur cible |
|-------------|---------|-------------|
| 2.2 | Texte principal | Ratio ≥ 4.5:1 — valider avec le token `color.content.primary` |
| 2.4 | Bouton principal | Ratio ≥ 3:1 sur fond |
| 10.2 | Contenu texte | Lisible à 200% de taille de police |

### Annotations Figma
Chaque spec d'écran doit référencer les annotations Figma correspondantes :
- Frame Figma : [lien ou nom du frame]
- Annotations ajoutées : [liste des annotations — labels, ordre de focus, rôles]
- Plugin utilisé : [Stark / Figma Accessibility / autre]
```

---

## Annotations Figma — conventions

### Éléments à annoter obligatoirement dans Figma
- **Tous les éléments interactifs** : label accessible + rôle (bouton, lien, champ)
- **Ordre de focus** : numérotation visible sur le frame
- **Zones tactiles** : overlay montrant la zone réelle de 44x44pt / 48x48dp
- **Contrastes** : indication du ratio pour les textes et composants interactifs
- **Images informatives** : alt text à côté de l'image

### Convention de nommage des layers Figma
```
[type] — [nom] — [label accessible]
Ex : Button — Connexion — "Se connecter à votre compte"
Ex : Image — Avatar — "Photo de profil de [Nom]"
Ex : Input — Email — "Adresse email, champ obligatoire"
```

---

## Checklist de conformité RAAM — par feature

### Niveau A (obligatoire — bloque `Done`)
- [ ] RAAM 1.1 — Toutes les images informatives ont une alternative textuelle
- [ ] RAAM 2.1 — Aucune information transmise uniquement par la couleur
- [ ] RAAM 6.1 — Tous les boutons ont un label accessible
- [ ] RAAM 6.2 — Tous les champs ont un label associé
- [ ] RAAM 7.1 — Ordre de focus logique vérifié sur tous les écrans
- [ ] RAAM 7.2 — Focus visible à tout moment
- [ ] RAAM 9.1 — Titres d'écran pertinents et uniques
- [ ] RAAM 12.1 — Aucune fonctionnalité accessible uniquement par geste complexe
- [ ] RAAM 13.1 — Zones tactiles ≥ 44x44pt (iOS) / 48x48dp (Android)

### Niveau AA (cible)
- [ ] RAAM 2.2 — Contraste texte normal ≥ 4.5:1
- [ ] RAAM 2.3 — Contraste grands textes ≥ 3:1
- [ ] RAAM 2.4 — Contraste composants interactifs ≥ 3:1
- [ ] RAAM 6.3 — Messages d'erreur identifient le champ concerné
- [ ] RAAM 7.3 — Navigation cohérente entre les écrans
- [ ] RAAM 10.2 — Contenu lisible à 200% de taille de police
- [ ] RAAM 12.2 — Gestes complexes ont une alternative simple
- [ ] RAAM 13.2 — Espacement suffisant entre les zones tactiles

---

## Responsabilités par agent

| Agent | Responsabilité accessibilité |
|-------|----------------------------|
| Business Analyst | Identifier les obligations RAAM dans l'épic, lister les populations concernées |
| Product Owner | Intégrer les critères d'acceptance accessibilité dans chaque US |
| UX Designer | Spécifier les annotations (labels, ordre de focus, rôles) + annotations Figma |
| UI Designer | Implémenter les tokens accessibles, respecter les zones tactiles, vérifier les contrastes |
| Design System Manager | Garantir que tous les tokens et composants respectent les ratios de contraste RAAM AA |
| QA Engineer | Vérifier la conformité RAAM A minimum, documenter les non-conformités dans le rapport |
