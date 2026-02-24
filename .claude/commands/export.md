# /export — Exporter une fiche ou un mémo en PDF

Génère un PDF professionnel brandé Future Path à partir d'une fiche concept ou d'un mémo.

**Cette commande utilise la skill `export-pdf`** (`.claude/skills/export-pdf/`) pour le nettoyage, l'injection YAML et la compilation. Lis le `SKILL.md` de la skill pour les détails du workflow de conversion.

## Paramètres

1. **Cible** (obligatoire) : soit un nom de concept (exporte la fiche), soit un chemin relatif vers un mémo
2. **Format** (optionnel) : `fiche` (défaut) | `memo` | `all` (fiche + tous les mémos liés)

Exemples :
- `/export "productivité"` → exporte la fiche concept en PDF
- `/export "research/memos/2026-02-07_productivite_exploration.md"` → exporte ce mémo en PDF
- `/export "productivité" "all"` → exporte la fiche + tous les mémos liés en un seul PDF

## Prérequis

Ce workflow utilise `pandoc` + `xelatex` avec le template Future Path.

### Vérification
Avant toute conversion, vérifie que les outils sont installés :
```bash
pandoc --version
xelatex --version
```

### Si les prérequis manquent
Consulte `.claude/skills/export-pdf/references/install.md` pour les instructions d'installation complètes (pandoc, BasicTeX, packages LaTeX nécessaires).

Si l'utilisateur ne veut pas installer les prérequis, propose l'alternative : "Tu peux m'uploader le fichier .md sur claude.ai et je te génèrerai un PDF avec une mise en page soignée."

## Workflow

### Étape 1 — Identification de la cible
- Si c'est un nom de concept : cherche dans `research/index.json`, récupère le champ `chemin` qui pointe vers le fichier dans son dossier mensuel (ex : `research/2026-02/2026-02-07_productivite.md`)
- Si c'est un chemin direct : vérifie que le fichier existe (peut être dans `research/YYYY-MM/` ou `research/memos/`)
- Si format = `all` : récupère la fiche depuis son dossier mensuel ET tous les mémos dans `research/memos/` dont le nom contient le slug du concept

### Étape 2 — Préparation
- Crée le dossier `exports/` à la racine du projet s'il n'existe pas
- Détermine le nom du fichier de sortie : `YYYY-MM-DD_concept_type.pdf`
  - Exemple : `2026-02-07_productivite_fiche.pdf`
  - Pour `all` : `2026-02-07_productivite_complet.pdf`

### Étape 3 — Nettoyage et injection YAML (via la skill export-pdf)

Suivre les étapes 1 à 3 de la skill `export-pdf` (`SKILL.md`) :

1. **Analyser le fichier source** : extraire titre, date, sujet
2. **Nettoyer le markdown** :
   - Supprimer les sections : `## Journal`, `## Connexions`, `## Concepts connexes à explorer`, `## Et pour moi ?`
   - Supprimer les lignes/bullets contenant `⚠️` et les `[⚠️ ...]`
   - Supprimer la ligne de métadonnées blockquote (`> Date : ... | Statut : ...`)
   - Supprimer les lignes de template `{{...}}` et les commentaires `<!-- TODO -->`
3. **Injecter le front-matter YAML** avec titre, subtitle (si section "En une phrase" existe), subject, date, author, logo, toc

Si format = `all`, concatène les fichiers nettoyés avec un saut de page entre chaque (`\newpage`). Le front-matter YAML n'est injecté qu'une seule fois en tête du fichier concaténé.

### Étape 4 — Compilation PDF

Les chemins vers le template et le logo sont relatifs à la racine du projet :

```bash
pandoc "<fichier-nettoyé>.md" \
  -o "exports/YYYY-MM-DD_concept_type.pdf" \
  --pdf-engine=xelatex \
  --template=".claude/skills/export-pdf/assets/my-org-template.tex" \
  --highlight-style=zenburn
```

Le logo dans le front-matter YAML doit pointer vers :
```yaml
logo: ".claude/skills/export-pdf/assets/my-org-logo.png"
```

Note : avec pandoc + xelatex, les liens markdown `[texte](url)` sont cliquables dans le PDF.

Si la conversion échoue, consulter la table de gestion des erreurs dans le `SKILL.md` de la skill, puis informer l'utilisateur du problème exact.

### Étape 5 — Nettoyage et rapport
- Supprime le fichier temporaire
- Affiche : chemin du PDF généré, taille du fichier, nombre de pages si possible
- Ajoute une entrée dans le Journal de la fiche : `YYYY-MM-DD : exporté en PDF`
