# /compare — Analyse comparative de deux concepts

Compare deux concepts de la bibliothèque et produit un mémo structuré mettant en lumière convergences, divergences et angles morts.

## Modèle recommandé

**Opus** — Cette commande exige une lecture croisée fine et une analyse nuancée. Avant de lancer : `/model opus`.

## Paramètres

1. **Concept A** (obligatoire) : premier concept à comparer (doit exister dans l'index)
2. **Concept B** (obligatoire) : second concept à comparer (doit exister dans l'index)

Exemple : `/compare "productivité" "meta-cognition"`

## Workflow

### Étape 1 — Chargement
- Cherche les deux concepts dans `research/index.json`
- Si l'un est introuvable, affiche les concepts disponibles et arrête-toi
- Charge les deux fiches complètes et leurs metas

### Étape 2 — Analyse comparative
Produis une analyse structurée en 4 dimensions :

**1. Convergences**
- Thèses partagées ou complémentaires
- Auteurs et sources en commun
- Dynamiques ou mécanismes similaires
- Contextes d'application qui se recoupent

**2. Divergences**
- Positions ou conclusions opposées
- Contextes différents qui changent la perspective
- Niveaux d'analyse distincts (macro vs. micro, théorique vs. empirique)

**3. Angles morts**
- Ce que le concept A éclaire et que le concept B ignore (et inversement)
- Questions que la confrontation fait émerger et qu'aucune des deux fiches ne traite
- Tensions non résolues entre les deux perspectives

**4. Implications croisées**
- Ce que la lecture croisée change pour les projets de l'utilisateur (voir `profile.md`)
- Pistes concrètes qui n'apparaissent que dans la confrontation
- Si rien de concret : le dire

### Étape 3 — Sauvegarde
- Sauvegarde le mémo dans `research/memos/YYYY-MM-DD_compare_conceptA_conceptB.md` en suivant le template `templates/memo.md`
- Mets à jour les connexions dans les metas des deux concepts si de nouveaux liens sont identifiés
- Mets à jour `research/index.json` et regénère `research/index.md`

### Étape 4 — Rapport
- Affiche le mémo à l'utilisateur
- Signale les nouveaux liens identifiés
- Suggère d'éventuels concepts intermédiaires à explorer
