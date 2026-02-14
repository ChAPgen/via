# /export — Exporter une fiche ou un mémo en PDF

Génère un PDF propre à partir d'une fiche concept ou d'un mémo.

## Paramètres

1. **Cible** (obligatoire) : soit un nom de concept (exporte la fiche), soit un chemin relatif vers un mémo
2. **Format** (optionnel) : `fiche` (défaut) | `memo` | `all` (fiche + tous les mémos liés)

Exemples :
- `/export "productivité"` → exporte la fiche concept en PDF
- `/export "research/memos/2026-02-07_productivite_exploration.md"` → exporte ce mémo en PDF
- `/export "productivité" "all"` → exporte la fiche + tous les mémos liés en un seul PDF

## Prérequis : pandoc

Ce workflow utilise `pandoc` pour la conversion markdown → PDF.

### Vérification
Avant toute conversion, vérifie que pandoc est installé :
```bash
pandoc --version
```

### Si pandoc n'est pas installé
Informe l'utilisateur et propose l'installation :
- **macOS** : `brew install pandoc` et `brew install --cask basictex` (pour le moteur LaTeX)
- **Ubuntu/Debian** : `sudo apt install pandoc texlive-latex-recommended`
- **Windows** : `winget install pandoc` ou télécharger depuis https://pandoc.org

Si l'utilisateur ne veut pas installer pandoc, propose l'alternative : "Tu peux m'uploader le fichier .md sur claude.ai et je te génèrerai un PDF avec une mise en page soignée."

## Workflow

### Étape 1 — Identification de la cible
- Si c'est un nom de concept : cherche dans `research/index.json`, récupère le champ `chemin` qui pointe vers le fichier dans son dossier mensuel (ex : `research/2026-02/2026-02-07_productivite.md`)
- Si c'est un chemin direct : vérifie que le fichier existe (peut être dans `research/YYYY-MM/` ou `research/memos/`)
- Si format = `all` : récupère la fiche depuis son dossier mensuel ET tous les mémos dans `research/memos/` dont le nom contient le slug du concept (ex : `research/memos/2026-02-09_productivite_publish-linkedin.md`)

### Étape 2 — Préparation
- Crée le dossier `exports/` à la racine du projet s'il n'existe pas
- Détermine le nom du fichier de sortie : `YYYY-MM-DD_concept_type.pdf`
  - Exemple : `2026-02-07_productivite_fiche.pdf`
  - Pour `all` : `2026-02-07_productivite_complet.pdf`

### Étape 3 — Nettoyage du markdown pour export
- Copie le fichier source dans un fichier temporaire
- Retire les marqueurs internes qui ne doivent pas apparaître dans un PDF public :
  - Les lignes de template `{{...}}`
  - Les commentaires `<!-- TODO -->`
- Conserve les marqueurs `[⚠️]` — ils sont importants pour la traçabilité
- Si format = `all`, concatène les fichiers avec un saut de page entre chaque (`\newpage`)

### Étape 4 — Conversion
```bash
pandoc temp_source.md \
  -o exports/YYYY-MM-DD_concept_type.pdf \
  --pdf-engine=xelatex \
  -V geometry:margin=2.5cm \
  -V fontsize=11pt \
  -V mainfont="Helvetica" \
  -V documentclass=article \
  -V colorlinks=true \
  -V linkcolor=blue \
  -V urlcolor=blue \
  --highlight-style=tango
```

Note : avec pandoc + xelatex, les liens markdown `[texte](url)` sont cliquables dans le PDF. Si l'utilisateur veut aussi que les URLs s'affichent en clair (utile pour l'impression), pré-traiter le markdown pour transformer `[texte](url)` en `texte (url)` avant la conversion.

Si xelatex n'est pas disponible, essayer avec le moteur par défaut :
```bash
pandoc temp_source.md -o exports/YYYY-MM-DD_concept_type.pdf -V geometry:margin=2.5cm
```

Si la conversion échoue, informer l'utilisateur du problème exact et suggérer l'alternative claude.ai.

### Étape 5 — Nettoyage et rapport
- Supprime le fichier temporaire
- Affiche : chemin du PDF généré, taille du fichier, nombre de pages si possible
- Ajoute une entrée dans le Journal de la fiche : `YYYY-MM-DD : exporté en PDF`
