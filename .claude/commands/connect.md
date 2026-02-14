# /connect — Cartographier les liens entre concepts

Analyse tous les concepts explorés et met à jour la carte des connexions.

## Paramètres

Aucun paramètre obligatoire. Optionnellement, l'utilisateur peut spécifier deux concepts pour explorer leur lien spécifique.

Exemples :
- `/connect` → analyse globale
- `/connect "productivité" "paradoxe-de-solow"` → lien spécifique

## Workflow

### Étape 1 — Inventaire
- Lis `research/index.json` pour lister tous les concepts
- Si moins de 2 concepts, informe l'utilisateur qu'il n'y a pas assez de matière
- Charge tous les fichiers `_meta.json`

### Étape 2 — Analyse des liens (Agent Connecteur)
- Lis les instructions dans `agents/connecteur.md`
- Si analyse globale : évalue toutes les paires de concepts
- Si lien spécifique : analyse en détail la relation entre les deux concepts donnés

### Étape 3 — Production
- Met à jour `research/carte.mermaid`
- Met à jour les champs `connexions` dans les fichiers meta concernés
- Met à jour les sections "Connexions" des fiches markdown concernées
- Regénère `research/index.json` et `research/index.md`
- Si des patterns émergent (clusters, ponts, trous), produit un mémo dans `research/memos/`

### Étape 4 — Rapport
- Affiche la carte mermaid
- Signale les nouveaux liens découverts
- Signale les trous éventuels (concepts manquants qui relieraient des clusters)
- Suggère des explorations futures
