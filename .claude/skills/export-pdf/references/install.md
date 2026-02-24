# Installation des prérequis — export-pdf

## macOS

### 1. Pandoc

```bash
brew install pandoc
```

### 2. XeLaTeX via BasicTeX

```bash
brew install --cask basictex
```

Après installation, **redémarrer le terminal**, puis :

```bash
# Mettre à jour le gestionnaire de packages
sudo tlmgr update --self

# Installer les packages LaTeX nécessaires
sudo tlmgr install \
  collection-fontsrecommended \
  collection-xetex \
  unicode-math \
  fancyhdr \
  titlesec \
  booktabs \
  caption \
  geometry \
  hyperref \
  xcolor \
  listings \
  mdframed \
  zref \
  needspace \
  fancyvrb \
  lastpage \
  parskip \
  enumitem \
  framed \
  csquotes \
  footmisc
```

### 3. Vérification

```bash
xelatex --version
pandoc --version
fc-list | grep "Avenir Next"
```

Les trois commandes doivent renvoyer un résultat. Si Avenir Next n'apparaît pas, la police n'est pas installée sur le système (elle est native macOS, présente par défaut).

## Mise à jour

Si Pandoc signale un package LaTeX manquant :

```bash
sudo tlmgr install <nom-du-package>
```

Pour tout mettre à jour :

```bash
sudo tlmgr update --all
```
