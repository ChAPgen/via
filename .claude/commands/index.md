# /index — Afficher et regénérer l'index

Regénère le fichier `research/index.md` à partir de `research/index.json` et l'affiche.

## Paramètres

Aucun.

## Workflow

### Étape 1 — Lecture
- Charge `research/index.json`
- Si le fichier n'existe pas ou est vide, affiche "Aucune recherche pour le moment. Lance `/explore` pour commencer."

### Étape 2 — Génération du markdown
- Regroupe les concepts par mois
- Pour chaque concept, affiche : nom (lien vers la fiche), confiance (●), tags, connexions
- Ajoute une section "À approfondir" (confiance < 3)
- Ajoute une section "Zones d'ombre ouvertes" (concepts avec zones_ombre > 0)
- Ajoute des statistiques : nombre total de concepts, confiance moyenne, concepts les plus connectés

### Étape 3 — Sauvegarde et affichage
- Écris `research/index.md`
- Affiche le contenu à l'utilisateur
