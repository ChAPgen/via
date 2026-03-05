# Changelog

Toutes les modifications notables du projet VIA sont documentées ici.

Format inspiré de [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/).

---

## [0.3.0] — 2026-03-05

Refonte majeure issue d'un audit qualité du système (9 concepts explorés, 40 claims vérifiées, 10 modifications structurelles).

### Ajouté
- **Agent Cadreur** — Produit un briefing expert du domaine *avant* la recherche. 6 sections : auteurs de référence, débats structurants, termes techniques, sources, pièges courants, facettes à ne pas ignorer. Transmis au Chercheur (comme checklist) et au Juge (comme grille de couverture).
- **Commande `/compare`** — Analyse comparative de deux concepts. Produit un mémo structuré : convergences, divergences, angles morts, implications croisées.
- **Templates de canaux** (`templates/canaux/`) — Les specs de chaque canal de publication (newsletter, blog, linkedin, linkedin-carousel) sont extraites de `/publish` dans des fichiers dédiés. Ajouter un canal = ajouter un fichier.

### Modifié
- **Agents déplacés** : `agents/` → `.claude/agents/` (convention Claude Code pour les fichiers d'instructions).
- **Agent Chercheur** : le budget de recherche passe d'une limite fixe (8-12 recherches) à des **critères d'arrêt adaptatifs** (couverture des auteurs clés, sources par idée force, débats documentés) avec garde-fou (15 pour `/explore`, 25 pour `/deepen`).
- **Agent Juge** : ajout d'un **5ème axe de vérification "Couverture"** basé sur le briefing du Cadreur (auteurs couverts, débats traités, facettes absentes).
- **Agent Synthétiseur + template fiche** : la section "Hypothèse de départ" + "En une phrase" est remplacée par **"Entrée en matière"** — 3 paragraphes (contexte, enjeu, hypothèse) qui donnent envie de lire la suite.
- **Commandes `/explore` et `/deepen`** : intégration de l'étape Cadreur dans le workflow.
- **Commande `/publish`** : les specs de canaux sont externalisées vers `templates/canaux/`. La commande charge dynamiquement le template du canal demandé.
- **Skill export-pdf** : améliorations du template LaTeX et du workflow de nettoyage.

### Migration depuis 0.2.0

1. **Déplacer vos agents** (si vous les aviez personnalisés) :
   ```bash
   mkdir -p .claude/agents
   cp agents/*.md .claude/agents/
   rm -rf agents/
   ```
   Si vous ne les aviez pas modifiés, le simple `git pull` suffit.

2. **Vos données sont intactes** : les dossiers `research/`, `exports/`, `inputs/` et `profile.md` ne sont pas touchés par cette mise à jour.

3. **Fiches existantes** : l'ancien format "Hypothèse de départ" / "En une phrase" reste lisible. Les nouvelles fiches utiliseront "Entrée en matière". Si vous voulez convertir une ancienne fiche, lancez `/deepen "concept"`.

---

## [0.2.0] — 2026-02-24

### Ajouté
- **Skill export-pdf** (`.claude/skills/export-pdf/`) — Génère des PDFs professionnels via pandoc + XeLaTeX avec template et logo personnalisables.
- **Commande `/export`** — Interface utilisateur pour l'export PDF (fiche, mémo, ou tout un concept).

---

## [0.1.0] — 2026-02-14

### Ajouté
- Version initiale du système VIA.
- 4 agents : Chercheur, Synthétiseur, Juge, Connecteur.
- 6 commandes : `/init`, `/explore`, `/deepen`, `/connect`, `/publish`, `/index`.
- Templates : fiche, mémo, meta.
- Système d'honnêteté intellectuelle à 4 niveaux.
- Profil utilisateur (`/init`) avec contexte et canaux de publication.
