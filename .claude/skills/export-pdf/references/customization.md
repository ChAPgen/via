# Personnalisation du template — futurepath-template.tex

Le template est un fichier LaTeX standard, éditable avec n'importe quel éditeur de texte. Les zones personnalisables sont marquées par des commentaires `%% EDITABLE`.

## Couleurs

Chercher : `%% EDITABLE : Palette de couleurs Future Path`

```latex
\definecolor{fp-primary}{HTML}{1B2A4A}      % Bleu nuit — titres
\definecolor{fp-accent}{HTML}{2D7DD2}       % Bleu vif — accents, liens
\definecolor{fp-accent-light}{HTML}{E8F1FB}  % Bleu très clair — fonds
\definecolor{fp-gray}{HTML}{6B7280}         % Gris — texte secondaire
\definecolor{fp-lightgray}{HTML}{F3F4F6}    % Gris clair — fonds de code
\definecolor{fp-dark}{HTML}{111827}         % Quasi-noir — texte courant
\definecolor{fp-rule}{HTML}{D1D5DB}         % Gris moyen — lignes/règles
```

Les codes sont en hexadécimal (format HTML). Modifier les 6 caractères après `{HTML}` pour changer une couleur.

## Polices

Chercher : `%% EDITABLE : Polices`

### Police principale (corps de texte)

```latex
\setmainfont{Avenir Next}[
  UprightFont    = *-Regular,
  BoldFont       = *-Bold,
  ItalicFont     = *-Italic,
  BoldItalicFont = *-BoldItalic,
]
```

**Alternatives macOS** : `Helvetica Neue`, `SF Pro Text`, `Gill Sans`
**Alternatives cross-platform** : `Inter`, `Source Sans Pro`, `Noto Sans`

### Police des titres de section

```latex
\newfontfamily\sectionfont{Avenir Next}[
  UprightFont = *-DemiBold,
  BoldFont    = *-Bold,
]
```

Utilise la graisse DemiBold pour les titres de section.

### Police du titre de la publication (page de garde)

```latex
\newfontfamily\titlefont{Avenir Next}[
  UprightFont = *-Heavy,
  BoldFont    = *-Heavy,
]
```

Utilise la graisse Heavy (la plus lourde) pour le titre principal.

### Police mono (liens et code)

```latex
\setmonofont{Menlo}[Scale=0.88]
```

**Alternatives** : `SF Mono`, `Fira Code`, `JetBrains Mono`, `Inconsolata`

Le paramètre `Scale` ajuste la taille relative. 0.88 aligne visuellement la mono avec le corps de texte en Avenir Next.

## Header / Footer

Chercher : `%% EDITABLE : Contenu des headers/footers`

### Header

```latex
% Gauche : sujet en italique
\fancyhead[L]{\small\color{fp-gray}\textit{...}}

% Droite : logo
\fancyhead[R]{\includegraphics[height=0.9cm]{...}}
```

- `[L]` = gauche, `[C]` = centre, `[R]` = droite
- `height=0.9cm` : ajuster la taille du logo dans le header

### Footer

```latex
\fancyfoot[L]{Une étude Future Path}    % Gauche : mention
\fancyfoot[C]{\thepage\,/\,\pageref{LastPage}}  % Centre : pagination
\fancyfoot[R]{...date...}               % Droite : date
```

Pour changer le texte du footer gauche (ex: "Un rapport Future Path"), modifier la chaîne dans `\fancyfoot[L]`.

### Lignes de séparation

```latex
\renewcommand{\headrulewidth}{0.4pt}  % Épaisseur ligne sous le header
\renewcommand{\footrulewidth}{0.4pt}  % Épaisseur ligne au-dessus du footer
```

Mettre `0pt` pour supprimer une ligne.

## Style des titres

Chercher : `%% EDITABLE : Style des titres`

```latex
% Section (H2 en Markdown) — avec filet bleu dessous
\titleformat{\section}
  {\sectionfont\Large\bfseries\color{fp-primary}}
  {\thesection}{0.8em}{}
  [\vspace{-0.3em}{\color{fp-accent}\hrule height 1.5pt}\vspace{0.3em}]

% Sous-section (H3 en Markdown)
\titleformat{\subsection}
  {\sectionfont\large\bfseries\color{fp-primary}}
  {\thesubsection}{0.6em}{}

% Sous-sous-section (H4 en Markdown) — en couleur accent
\titleformat{\subsubsection}
  {\sectionfont\normalsize\bfseries\color{fp-accent}}
  {\thesubsubsection}{0.5em}{}
```

Pour supprimer le filet bleu sous les H2, retirer la ligne `[\vspace{-0.3em}...]`.

## Marges

```latex
\usepackage[
  top=3cm,
  bottom=3cm,
  left=2.5cm,
  right=2.5cm,
  headheight=1.5cm,
  headsep=0.8cm,
  footskip=1.5cm,
]{geometry}
```

## Page de garde

La page de garde est construite dans le bloc `\begin{document}`. Éléments modifiables :
- Taille du logo : `\includegraphics[width=5cm]{...}` — modifier `5cm`
- Taille du titre : `\fontsize{32}{38}` — premier chiffre = taille, second = interligne
- Épaisseur des lignes décoratives : `\hrule height 2pt`
- Texte de pied de page de garde : `Future Path — Intelligence stratégique \& Innovation`

## Blockquotes

Les blockquotes Markdown (`> texte`) sont rendues avec une barre latérale bleue et un fond clair :

```latex
\renewenvironment{quote}{%
  \begin{mdframed}[
    linewidth=3pt,           % Épaisseur de la barre latérale
    linecolor=fp-accent,     % Couleur de la barre
    backgroundcolor=fp-accent-light,  % Fond
    fontcolor=fp-gray,       % Couleur du texte
  ]\itshape                  % Italique
```

## Coloration syntaxique des blocs de code

Contrôlée par l'option Pandoc `--highlight-style`. Choix disponibles :
- `zenburn` — sombre, doux (recommandé)
- `tango` — coloré, lisible
- `espresso` — sombre, contrasté
- `kate` — clair, classique
- `monochrome` — sans couleur
