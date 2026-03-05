# /deepen — Approfondir un concept existant

Reprend une fiche existante et l'enrichit avec des recherches complémentaires ciblées.

## Modèle recommandé

**Opus** — Cette commande exige vérification de sources, nuance et jugement critique. Avant de lancer : `/model opus`.
Pour les sous-agents (Task tool), utiliser `model: "opus"`.

## Stratégie de recherche approfondie

Le `/deepen` va plus loin que le `/explore` en termes de rigueur :
- **Parallélisation** : lancer des sous-agents Task en parallèle pour investiguer chaque zone d'ombre simultanément
- **WebFetch systématique** : pour chaque source clé identifiée dans les résultats de recherche, lire l'article complet avec WebFetch (pas seulement les snippets)
- **Double vérification** : chaque fait marqué `[⚠️]` doit être confirmé par au moins 2 sources indépendantes avant de retirer le marqueur
- **Budget recherche adaptatif** : critères d'arrêt définis dans `.claude/agents/chercheur.md`, garde-fou à 25 recherches

## Paramètres

1. **Concept** (obligatoire) : le nom du concept à approfondir (doit exister dans l'index)

Exemple : `/deepen "productivité"`

## Workflow

### Étape 1 — Chargement
- Cherche le concept dans `research/index.json`
- Si introuvable, propose un `/explore` à la place
- Charge la fiche existante et son meta
- Identifie les zones d'ombre et les points marqués `[⚠️]`

### Étape 2 — Cadrage expert (Agent Cadreur)
- Lis les instructions dans `.claude/agents/cadreur.md`
- Produis un briefing expert en tenant compte de la fiche existante : concentre-toi sur les zones d'ombre, les `[⚠️]`, et les facettes non encore couvertes
- Ce briefing sera transmis au Chercheur et au Juge

### Étape 3 — Recherche ciblée (Agent Chercheur)
- Lis les instructions dans `.claude/agents/chercheur.md`
- Transmets-lui le briefing expert produit à l'étape 2
- Concentre la recherche sur :
  - Les zones d'ombre identifiées dans la fiche
  - Les lacunes identifiées dans le briefing expert
  - Les controverses et critiques du concept
  - Les cas d'usage concrets et études empiriques
  - Les concepts adjacents non encore explorés
- Ne refais PAS la recherche de base — complète ce qui manque

### Étape 4 — Mise à jour (Agent Synthétiseur)
- Lis les instructions dans `.claude/agents/synthetiseur.md`
- Enrichis la fiche existante (ne la réécris pas de zéro)
- Augmente le niveau de confiance si justifié
- Ajoute une entrée dans le Journal de la fiche

### Étape 5 — Connexion (Agent Connecteur)
- Lis les instructions dans `.claude/agents/connecteur.md`
- Cherche les liens avec les autres concepts de la bibliothèque
- Mets à jour les connexions dans le meta et dans la fiche

### Étape 6 — Jugement (Agent Juge)
- Lis les instructions dans `.claude/agents/juge.md`
- Transmets-lui le briefing expert produit à l'étape 2 pour vérifier la couverture (axe 5)
- Évalue les ajouts (pas toute la fiche, juste le nouveau contenu)
- Si verdict = À CORRIGER → retour à l'étape 4

### Étape 7 — Sauvegarde
- Écrase la fiche et le meta existants
- Mets à jour `research/index.json` (date_maj, confiance, connexions)
- Regénère `research/index.md`

### Étape 8 — Rapport
- Affiche ce qui a été ajouté/modifié
- Indique les zones d'ombre restantes
- Suggère d'autres concepts à explorer si pertinent
