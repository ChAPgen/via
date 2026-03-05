# VIA — Veille Intellectuelle Assistée

Un système d'exploration intellectuelle augmentée par [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Explorez des concepts en profondeur, construisez une bibliothèque interconnectée, et produisez des contenus éditoriaux — le tout guidé par vos centres d'intérêt.

---

## Prérequis

- **Claude Code** : installé et fonctionnel ([documentation](https://docs.anthropic.com/en/docs/claude-code))
- **pandoc + XeLaTeX** (optionnel) : pour la commande `/export` (voir `.claude/skills/export-pdf/references/install.md`)

---

## Démarrage rapide

```bash
git clone https://github.com/ChAPgen/via.git
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
| `/compare "concept-a" "concept-b"` | Analyse comparative de deux concepts |
| `/connect` | Cartographier les liens entre tous vos concepts |
| `/publish "concept" "canal"` | Produire un contenu éditorial |
| `/index` | Afficher l'état de votre bibliothèque |
| `/export "concept"` | Générer un PDF via pandoc |

**Canaux disponibles pour `/publish`** : `newsletter` | `blog` | `linkedin` | `linkedin-carousel`

---

## Comment ça marche

### 5 agents spécialisés

Chaque commande orchestre des agents qui ont chacun un rôle précis :

- **Cadreur** — Produit un briefing expert du domaine avant la recherche (auteurs clés, débats, pièges)
- **Chercheur** — Recherche web avec budget adaptatif, guidée par le briefing du Cadreur
- **Synthétiseur** — Transforme la recherche brute en fiche structurée
- **Juge** — Vérifie la qualité, l'exactitude, l'honnêteté et la couverture de chaque production
- **Connecteur** — Relie les concepts entre eux, détecte des patterns

### Workflow type

```
/explore :  Cadreur → Chercheur → Synthétiseur → Juge → Fiche
/deepen :   Cadreur → Chercheur (ciblé) → Synthétiseur → Connecteur → Juge → Fiche mise à jour
/compare :  Chargement des deux fiches → Analyse croisée → Mémo
/publish :  Fiche + profil + template canal → Contenu éditorial → Juge
```

### Honnêteté intellectuelle

Le principe fondateur du système : **"Mieux vaut un trou qu'un mensonge."**

| Niveau | Situation | Comportement |
|--------|-----------|--------------|
| 1 — Certain | Source vérifiable | Présenté normalement |
| 2 — Incertain | Source fragile ou contradictions | Marqué `[⚠️ À VÉRIFIER]` |
| 3 — Flou | Ambiguïté, plusieurs interprétations | Question posée à l'utilisateur |
| 4 — Absent | Rien de fiable trouvé | Dit explicitement |

Lors de la publication (`/publish`), aucune information incertaine n'est transformée en affirmation.

### Le cycle recommandé

1. **Explorez** un concept qui vous intéresse
2. **Lisez la fiche** — si des zones d'ombre vous intriguent, lancez `/deepen`
3. **Explorez** des concepts adjacents suggérés
4. **Comparez** deux concepts pour faire émerger des insights croisés
5. **Connectez** régulièrement (tous les 3-4 concepts)
6. **Publiez** quand vous avez une fiche solide (confiance ≥ 3)

---

## Structure du projet

```
.
├── profile.md                  ← votre profil (créé par /init, gitignored)
├── profile.example.md          ← modèle de profil
├── CLAUDE.md                   ← instructions système
├── CHANGELOG.md                ← historique des versions
├── .claude/
│   ├── agents/                 ← instructions de chaque agent
│   │   ├── cadreur.md
│   │   ├── chercheur.md
│   │   ├── synthetiseur.md
│   │   ├── juge.md
│   │   └── connecteur.md
│   ├── commands/               ← les commandes
│   │   ├── init.md
│   │   ├── explore.md
│   │   ├── deepen.md
│   │   ├── compare.md
│   │   ├── connect.md
│   │   ├── publish.md
│   │   ├── index.md
│   │   └── export.md
│   └── skills/
│       └── export-pdf/         ← skill d'export PDF (template, logo, refs)
├── templates/
│   ├── fiche.md                ← modèle de fiche concept
│   ├── memo.md                 ← modèle de mémo
│   ├── meta.json               ← modèle de métadonnées
│   └── canaux/                 ← templates par canal de publication
│       ├── newsletter.md
│       ├── blog.md
│       ├── linkedin.md
│       └── linkedin-carousel.md
├── inputs/                     ← fichiers fournis par l'utilisateur
├── exports/                    ← PDFs générés par /export
└── research/                   ← tout le contenu produit
    ├── index.json              ← catalogue machine
    ├── index.md                ← catalogue lisible
    ├── carte.mermaid           ← graphe des connexions
    ├── memos/                  ← notes de réflexion et analyses comparatives
    ├── sources/                ← PDFs et documents de référence
    └── YYYY-MM/                ← fiches par mois
        ├── DATE_concept.md
        └── DATE_concept_meta.json
```

---

## Personnalisation

### Profil

Votre profil (`profile.md`) contient votre contexte professionnel et vos canaux de publication. Il est lu par tous les agents pour personnaliser les recherches et les contenus. Modifiez-le à tout moment.

### Canaux de publication

Chaque canal a son template dans `templates/canaux/`. Pour ajouter un canal personnalisé, créez un nouveau fichier `.md` dans ce dossier en suivant la structure des templates existants (format, ton, structure, à ne pas faire).

### Templates

Les modèles de fiches et mémos sont dans `templates/`. Adaptez-les à vos besoins.

### Agents

Les instructions de chaque agent sont dans `.claude/agents/`. Vous pouvez les ajuster pour modifier le comportement du système (par exemple, changer les critères d'arrêt du Chercheur ou les axes de vérification du Juge).

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
- Ajouter de nouveaux canaux de publication (créer un template dans `templates/canaux/`)
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
