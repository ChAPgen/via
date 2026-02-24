# Variables YAML — Référence

Les variables sont injectées dans le front-matter YAML en tête du fichier Markdown.

## Variables

### Obligatoires

| Variable | Type | Description | Exemple |
|----------|------|-------------|---------|
| `title` | string | Titre affiché sur la page de garde et dans les métadonnées PDF | `"Méta-cognition"` |
| `date` | string | Date ISO affichée dans le footer et la page de garde | `"2026-02-09"` |

### Optionnelles

| Variable | Type | Défaut | Description | Exemple |
|----------|------|--------|-------------|---------|
| `subtitle` | string | _(absent)_ | Sous-titre sur la page de garde | `"Compétences cognitives à l'ère de l'IA"` |
| `subject` | string | = `title` | Texte affiché dans le header de chaque page | `"Méta-Cognition"` |
| `author` | string | `"Future Path"` | Auteur sur la page de garde + métadonnées PDF | `"Future Path"` |
| `logo` | string | _(absent)_ | Chemin vers le fichier logo (PNG ou PDF) | `"./assets/logo-futurepath.png"` |
| `toc` | boolean | `false` | Génère une table des matières si `true` | `true` |
| `toc-depth` | integer | `2` | Profondeur de la table des matières (1 = H2 seul, 2 = H2+H3, etc.) | `2` |
| `confidential` | boolean | `false` | Affiche "CONFIDENTIEL" en rouge sur la page de garde | `true` |

## Exemple complet

```yaml
---
title: "Méta-cognition"
subtitle: "Compétences cognitives à l'ère de l'IA générative"
subject: "Méta-Cognition"
date: "2026-02-09"
author: "Future Path"
logo: "./assets/logo-futurepath.png"
toc: true
toc-depth: 2
---
```

## Exemple minimal

```yaml
---
title: "Titre du document"
date: "2026-02-16"
---
```

## Notes

- Les chemins (`logo`) sont **relatifs au répertoire d'exécution** de la commande `pandoc`, pas au fichier `.md`.
- Si `subject` est absent, le `title` est utilisé dans le header.
- Si `author` est absent, "Future Path" est utilisé par défaut (défini dans le template).
- Le `subtitle` n'est affiché que s'il est explicitement présent — pas de valeur par défaut.
