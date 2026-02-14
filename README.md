# VIA — Veille Intellectuelle Assistée

Un système d'exploration intellectuelle augmentée par [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Explorez des concepts en profondeur, construisez une bibliothèque interconnectée, et produisez des contenus éditoriaux — le tout guidé par vos centres d'intérêt.

---

## Prérequis

- **Claude Code** : installé et fonctionnel ([documentation](https://docs.anthropic.com/en/docs/claude-code))
- **pandoc** (optionnel) : pour la commande `/export` (`brew install pandoc` sur macOS)

---

## Démarrage rapide

```bash
git clone <url-du-repo> via
cd via
claude
```

Puis dans Claude Code :

```
/init
```

Le système vous pose quelques questions pour créer votre profil (`profile.md`), puis vous êtes prêt :

```
/explore "intelligence artificielle" "impact sur les métiers" "l'IA ne remplace pas, elle transforme"
```

---

## Commandes

| Commande | Description |
|----------|-------------|
| `/init` | Configurer votre profil (identité, contexte, canaux de publication) |
| `/explore "concept" "périmètre" "hypothèse"` | Explorer un nouveau concept en profondeur |
| `/deepen "concept"` | Approfondir un concept déjà exploré |
| `/connect` | Cartographier les liens entre tous vos concepts |
| `/publish "concept" "canal"` | Produire un contenu éditorial (newsletter, blog, linkedin, linkedin-carousel) |
| `/index` | Afficher l'état de votre bibliothèque |
| `/export "concept"` | Générer un PDF via pandoc |

---

## Comment ça marche

### 4 agents spécialisés

Chaque commande orchestre des agents qui ont chacun un rôle précis :

- **Chercheur** — Recherche web, collecte de sources, exploration du concept
- **Synthétiseur** — Transforme la recherche brute en fiche structurée
- **Juge** — Vérifie la qualité, l'exactitude et l'honnêteté de chaque production
- **Connecteur** — Relie les concepts entre eux, détecte des patterns

### Honnêteté intellectuelle

Le principe fondateur du système : **"Mieux vaut un trou qu'un mensonge."**

| Niveau | Situation | Comportement |
|--------|-----------|--------------|
| 1 — Certain | Source vérifiable | Présenté normalement |
| 2 — Incertain | Source fragile ou contradictions | Marqué `[⚠️ À VÉRIFIER]` |
| 3 — Flou | Ambiguïté, plusieurs interprétations | Question posée à l'utilisateur |
| 4 — Absent | Rien de fiable trouvé | Dit explicitement |

Lors de la publication (`/publish`), aucune information incertaine n'est transformée en affirmation. Les éléments douteux sont omis ou présentés comme questions ouvertes.

### Le cycle recommandé

1. **Explorez** un concept qui vous intéresse
2. **Lisez la fiche** — si des zones d'ombre vous intriguent, lancez `/deepen`
3. **Explorez** des concepts adjacents suggérés
4. **Connectez** régulièrement (tous les 3-4 concepts)
5. **Publiez** quand vous avez une fiche solide (confiance ≥ 3)

---

## Structure du projet

```
.
├── profile.md              ← votre profil (créé par /init, gitignored)
├── profile.example.md      ← modèle de profil
├── CLAUDE.md               ← instructions système
├── README.md               ← ce fichier
├── LICENSE                  ← MIT
├── .gitignore
├── .claude/
│   └── commands/           ← les commandes
│       ├── init.md
│       ├── explore.md
│       ├── deepen.md
│       ├── connect.md
│       ├── publish.md
│       ├── index.md
│       └── export.md
├── agents/                 ← instructions de chaque agent
│   ├── chercheur.md
│   ├── synthetiseur.md
│   ├── juge.md
│   └── connecteur.md
├── templates/              ← modèles de documents
│   ├── fiche.md
│   ├── meta.json
│   └── memo.md
├── inputs/                 ← fichiers fournis par l'utilisateur
├── exports/                ← PDFs générés par /export
└── research/               ← tout le contenu produit
    ├── index.json          ← catalogue machine
    ├── index.md            ← catalogue lisible
    ├── carte.mermaid       ← graphe des connexions
    ├── memos/              ← notes de réflexion
    ├── sources/            ← PDFs et documents de référence
    └── YYYY-MM/            ← fiches par mois
        ├── DATE_concept.md
        └── DATE_concept_meta.json
```

---

## Personnalisation

### Profil

Votre profil (`profile.md`) contient votre contexte professionnel et vos canaux de publication. Il est lu par tous les agents pour personnaliser les recherches et les contenus. Modifiez-le à tout moment.

### Canaux de publication

Les canaux (newsletter, blog, LinkedIn, carrousel) sont configurés dans votre profil et utilisés par `/publish`. Vous pouvez aussi ajuster les contraintes détaillées dans `.claude/commands/publish.md`.

### Templates

Les modèles de fiches et mémos sont dans `templates/`. Adaptez-les à vos besoins.

---

## Permissions Claude Code

VIA utilise les outils suivants de Claude Code :
- **Lecture/écriture de fichiers** dans le dossier du projet
- **Recherche web** pour l'exploration de concepts
- **Bash** pour pandoc (export PDF uniquement)

Aucun accès réseau sortant autre que la recherche web. Aucune commande destructrice.

---

## Contribuer

Les contributions sont les bienvenues. Quelques pistes :

- Améliorer les agents (prompts plus précis, meilleure vérification)
- Ajouter de nouveaux canaux de publication
- Proposer de nouveaux templates de fiches
- Améliorer la détection de patterns par le Connecteur

---

## Limites connues

- **Paywalls** : Claude Code ne peut pas accéder aux articles académiques payants. Vous pouvez fournir des PDFs dans `inputs/`.
- **Livres complets** : le système ne peut pas lire un livre entier. Il trouvera des résumés, critiques et extraits.
- **Profondeur** : ce n'est pas un outil de recherche académique. C'est une veille intellectuelle augmentée.
- **Vérification** : l'Agent Juge fait de son mieux mais n'est pas infaillible. Pour les publications importantes, relisez la fiche source.

---

## Licence

[MIT](LICENSE)
