# /deepen — Approfondir un concept existant

Reprend une fiche existante et l'enrichit avec des recherches complémentaires ciblées.

## Paramètres

1. **Concept** (obligatoire) : le nom du concept à approfondir (doit exister dans l'index)

Exemple : `/deepen "productivité"`

## Workflow

### Étape 1 — Chargement
- Cherche le concept dans `research/index.json`
- Si introuvable, propose un `/explore` à la place
- Charge la fiche existante et son meta
- Identifie les zones d'ombre et les points marqués `[⚠️]`

### Étape 2 — Recherche ciblée (Agent Chercheur)
- Lis les instructions dans `agents/chercheur.md`
- Concentre la recherche sur :
  - Les zones d'ombre identifiées dans la fiche
  - Les controverses et critiques du concept
  - Les cas d'usage concrets et études empiriques
  - Les concepts adjacents non encore explorés
- Ne refais PAS la recherche de base — complète ce qui manque

### Étape 3 — Mise à jour (Agent Synthétiseur)
- Lis les instructions dans `agents/synthetiseur.md`
- Enrichis la fiche existante (ne la réécris pas de zéro)
- Augmente le niveau de confiance si justifié
- Ajoute une entrée dans le Journal de la fiche

### Étape 4 — Connexion (Agent Connecteur)
- Lis les instructions dans `agents/connecteur.md`
- Cherche les liens avec les autres concepts de la bibliothèque
- Mets à jour les connexions dans le meta et dans la fiche

### Étape 5 — Jugement (Agent Juge)
- Lis les instructions dans `agents/juge.md`
- Évalue les ajouts (pas toute la fiche, juste le nouveau contenu)
- Si verdict = À CORRIGER → retour à l'étape 3

### Étape 6 — Sauvegarde
- Écrase la fiche et le meta existants
- Mets à jour `research/index.json` (date_maj, confiance, connexions)
- Regénère `research/index.md`

### Étape 7 — Rapport
- Affiche ce qui a été ajouté/modifié
- Indique les zones d'ombre restantes
- Suggère d'autres concepts à explorer si pertinent
