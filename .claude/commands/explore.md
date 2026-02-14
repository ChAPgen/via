# /explore — Explorer un nouveau concept

Explore un concept en profondeur et produit une fiche structurée.

## Paramètres

L'utilisateur fournit jusqu'à 3 éléments :
1. **Concept** (obligatoire) : le nom du concept à explorer
2. **Périmètre** (optionnel) : délimitation du champ d'exploration
3. **Hypothèse** (optionnel) : conviction ou intuition à tester

Exemple : `/explore "productivité" "des premiers outils humains à l'IA" "le mythe de la productivité avec l'IA"`

Si le périmètre ou l'hypothèse ne sont pas fournis, demande à l'utilisateur s'il veut en préciser avant de démarrer.

## Workflow

### Étape 0 — Vérification du profil
- Vérifie que `profile.md` existe. S'il n'existe pas, affiche : "Profil non trouvé. Lancez `/init` pour configurer votre profil avant d'explorer." et arrête-toi.
- Lis `profile.md` pour connaître le contexte professionnel de l'utilisateur.

### Étape 1 — Préparation
- Récupère la date du jour
- Crée le dossier mensuel sous `research/` s'il n'existe pas (format `YYYY-MM/`)
- Vérifie dans `research/index.json` si le concept n'a pas déjà été exploré. Si oui, propose un `/deepen` à la place.

### Étape 2 — Recherche (Agent Chercheur)
- Lis les instructions dans `agents/chercheur.md`
- Applique-les avec le concept, le périmètre et l'hypothèse fournis
- Produis le document brut de recherche

### Étape 3 — Synthèse (Agent Synthétiseur)
- Lis les instructions dans `agents/synthetiseur.md`
- Lis le template dans `templates/fiche.md` et `templates/meta.json`
- Rédige la fiche et le fichier meta à partir du document brut

### Étape 4 — Jugement (Agent Juge)
- Lis les instructions dans `agents/juge.md`
- Évalue la fiche produite
- Si verdict = À CORRIGER → retour à l'étape 3 avec les corrections
- Si verdict = VALIDE ou À APPROFONDIR → continue

### Étape 5 — Sauvegarde
- Écris la fiche dans `research/YYYY-MM/YYYY-MM-DD_concept.md`
- Écris le meta dans `research/YYYY-MM/YYYY-MM-DD_concept_meta.json`
- Mets à jour `research/index.json` (ajoute l'entrée)
- Regénère `research/index.md` depuis `research/index.json`

### Étape 6 — Rapport
- Affiche un résumé à l'utilisateur : concept exploré, niveau de confiance, zones d'ombre, suggestions de /deepen éventuelles
- Indique les concepts adjacents découverts qui pourraient faire l'objet d'une exploration future
