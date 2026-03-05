# /explore — Explorer un nouveau concept

Explore un concept en profondeur et produit une fiche structurée.

## Modèle recommandé

**Opus** — Cette commande exige recherche approfondie, synthèse créative et jugement critique. Avant de lancer : `/model opus`.
Pour les sous-agents (Task tool), utiliser `model: "opus"` pour les étapes Chercheur et Synthétiseur.

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

### Étape 2 — Cadrage expert (Agent Cadreur)
- Lis les instructions dans `.claude/agents/cadreur.md`
- Produis un briefing expert du domaine à partir du concept, du périmètre et de l'hypothèse
- Ce briefing sera transmis au Chercheur (comme checklist de couverture) et au Juge (comme grille de vérification)

### Étape 3 — Recherche (Agent Chercheur)
- Lis les instructions dans `.claude/agents/chercheur.md`
- Transmets-lui le briefing expert produit à l'étape 2
- Applique les instructions avec le concept, le périmètre et l'hypothèse fournis
- Produis le document brut de recherche (incluant le bilan de couverture vs. le briefing)

### Étape 4 — Synthèse (Agent Synthétiseur)
- Lis les instructions dans `.claude/agents/synthetiseur.md`
- Lis le template dans `templates/fiche.md` et `templates/meta.json`
- Rédige la fiche et le fichier meta à partir du document brut

### Étape 5 — Jugement (Agent Juge)
- Lis les instructions dans `.claude/agents/juge.md`
- Transmets-lui le briefing expert produit à l'étape 2 pour vérifier la couverture (axe 5)
- Évalue la fiche produite
- Si verdict = À CORRIGER → retour à l'étape 4 avec les corrections
- Si verdict = VALIDE ou À APPROFONDIR → continue

### Étape 6 — Sauvegarde
- Écris la fiche dans `research/YYYY-MM/YYYY-MM-DD_concept.md`
- Écris le meta dans `research/YYYY-MM/YYYY-MM-DD_concept_meta.json`
- Mets à jour `research/index.json` (ajoute l'entrée)
- Regénère `research/index.md` depuis `research/index.json`

### Étape 7 — Rapport
- Affiche un résumé à l'utilisateur : concept exploré, niveau de confiance, zones d'ombre, suggestions de /deepen éventuelles
- Indique les concepts adjacents découverts qui pourraient faire l'objet d'une exploration future
