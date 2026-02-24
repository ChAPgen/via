---
name: export-pdf
description: Convertit un fichier Markdown (.md) en PDF professionnel avec le branding fourni dans 'assets/'. Utiliser cette skill quand l'utilisateur veut générer un PDF à partir d'un '.md' — que ce soit une étude, un article, une note de recherche, ou tout autre contenu issu de 'VIA'. La skill gère le nettoyage du Markdown (suppression des sections ⚠️, du journal, des connexions internes), l'injection des métadonnées YAML, et la compilation via Pandoc + XeLaTeX avec le template Future Path.
---

# Export PDF

Convertit un fichier Markdown en PDF professionnel brandé via Pandoc + XeLaTeX.

## Prérequis système

La machine de l'utilisateur doit avoir installé :
- **Pandoc** (`brew install pandoc`)
- **XeLaTeX** via BasicTeX (`brew install --cask basictex`)
- Les packages LaTeX listés dans `references/install.md`
- La police **Avenir Next** (native macOS)

## Fichiers de la skill

```
.claude/skills/export-pdf/
├── SKILL.md                          ← Ce fichier
├── assets/
│   ├── my-org-template.tex       ← Template LaTeX (éditable par l'utilisateur)
│   └── my-org-logo.png           ← Logo Future Path
└── references/
    ├── install.md                    ← Guide d'installation des prérequis
    ├── yaml-variables.md             ← Référence des variables YAML
    └── customization.md              ← Guide de personnalisation du template
```

## Workflow

### Étape 1 — Analyser le fichier source

Lire le `.md` source et en extraire :
1. **Le titre** : premier `# Heading` du fichier
2. **La date** : dans les métadonnées (ex: `Date : 2026-02-09`) ou dans le nom de fichier
3. **Le sujet** : identique au titre sauf indication contraire

### Étape 2 — Nettoyer le Markdown

Produire un `.md` intermédiaire nettoyé :

**Sections à supprimer entièrement** (titre + contenu) :
- `## Journal` — historique des modifications
- `## Connexions` — liens internes au Second Brain (`→ [[...]]`)
- `## Concepts connexes à explorer` — notes de recherche
- `## Et pour moi ?` — notes personnelles / business

**Contenus à supprimer** :
- Lignes ou bullets contenant `⚠️` (zones d'ombre non résolues)
- Texte entre crochets contenant ⚠️ : `[⚠️ ...]`
- La ligne de métadonnées blockquote (ex: `> Date : ... | Statut : ... | Confiance : ...`)

**À conserver tel quel** :
- Tout le reste du contenu (hypothèse, définitions, contexte, idées forces, frameworks, tensions, auteurs/sources)
- Les liens hypertextes `[texte](url)` 
- Le formatage Markdown (gras, italique, listes, etc.)

### Étape 3 — Injecter le front-matter YAML

Ajouter en tête du `.md` nettoyé :

```yaml
---
title: "<titre extrait>"
subtitle: "<sous-titre si pertinent, sinon omettre>"
subject: "<sujet pour le header>"
date: "<date ISO>"
author: "Future Path"
logo: "<chemin vers le logo>"
toc: true
toc-depth: 2
---
```

**Règles pour le sous-titre** :
- Si le fichier contient une section `## En une phrase`, utiliser son contenu (tronqué si trop long) comme subtitle
- Sinon, ne pas inclure de subtitle

### Étape 4 — Compiler le PDF

Exécuter :

```bash
pandoc "<fichier-nettoyé>.md" \
  -o "<nom-sortie>.pdf" \
  --pdf-engine=xelatex \
  --template="<chemin>/my-org-template.tex" \
  --highlight-style=zenburn
```

**Conventions de nommage du fichier de sortie** :
- Format : `YYYY-MM-DD_<slug-sujet>.pdf`
- Exemple : `2026-02-09_meta-cognition.pdf`

### Étape 5 — Vérifier et livrer

1. Vérifier que le PDF a été généré sans erreur
2. Vérifier le nombre de pages (cohérence avec la longueur du .md)
3. Informer l'utilisateur du résultat

## Template LaTeX

Le template `assets/my-org-template.tex` produit :

### Page de garde
- Logo Future Path (centré)
- Titre en Avenir Next Heavy, bleu nuit (`#1B2A4A`)
- Sous-titre en Avenir Next DemiBold, gris
- Ligne décorative bleue (`#2D7DD2`)
- Auteur + date
- Mention "Future Path — Intelligence stratégique & Innovation"

### Header (pages courantes)
- Gauche : sujet en italique gris
- Droite : logo (petit format)

### Footer (pages courantes)
- Gauche : "Une étude Future Path"
- Centre : page x/y
- Droite : date ISO

### Typographie
- **Titre publication** : Avenir Next Heavy, 32pt
- **Titres de section** (H2) : Avenir Next DemiBold, Large, bleu nuit + filet bleu
- **Sous-titres** (H3) : Avenir Next DemiBold, large, bleu nuit
- **Corps** : Avenir Next Regular, 11pt
- **Liens et code** : Menlo, 88% scale
- **Blockquotes** : fond bleu clair, barre latérale bleue, italique gris

### Couleurs

| Nom | Code | Usage |
|-----|------|-------|
| `fp-primary` | `#1B2A4A` | Titres, texte principal |
| `fp-accent` | `#2D7DD2` | Liens, accents, filets |
| `fp-accent-light` | `#E8F1FB` | Fond des blockquotes |
| `fp-gray` | `#6B7280` | Texte secondaire, métadonnées |
| `fp-lightgray` | `#F3F4F6` | Fond des blocs de code |

## Gestion des erreurs

| Erreur | Cause probable | Action |
|--------|---------------|--------|
| `Font not found: Avenir Next` | BasicTeX sans accès aux polices système | Vérifier `fc-list \| grep Avenir` |
| `Missing LaTeX package` | Package non installé | `sudo tlmgr install <package>` |
| `Unicode char not set up` | Caractère spécial non supporté | Vérifier encodage UTF-8 du .md |
| PDF vide ou 1 page | Erreur de parsing du YAML | Vérifier la syntaxe du front-matter |

## Exemples d'invocation

```
/export "méta-cognition"
/export "productivité" "all"
/export "research/memos/2026-02-09_meta-cognition_exploration.md"
```
