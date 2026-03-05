# /publish — Produire un contenu éditorial à partir d'une fiche

Transforme une fiche de recherche en contenu publiable sur un canal donné.

## Modèle recommandé

**Opus** — Cette commande exige une écriture créative de qualité, adaptée au ton et au canal. Avant de lancer : `/model opus`.

## Paramètres

1. **Concept** (obligatoire) : le concept à publier (doit exister dans l'index)
2. **Canal** (obligatoire) : nom d'un template dans `templates/canaux/` (sans l'extension `.md`)

Canaux disponibles : `newsletter` | `blog` | `linkedin` | `linkedin-carousel`

Exemple : `/publish "productivité" "linkedin-carousel"`

## Étape 0 — Vérification du profil

- Lis `profile.md`. S'il n'existe pas, affiche : "Profil non trouvé. Lancez `/init` pour configurer votre profil avant de publier." et arrête-toi.
- Charge les informations de canaux depuis `profile.md` (section "Canaux de publication").

## Règle absolue avant publication

- Vérifie le niveau de confiance de la fiche. Si < 3, préviens l'utilisateur et recommande un `/deepen` avant.
- **JAMAIS** transformer un élément marqué `[⚠️]` en affirmation. Soit on l'omet, soit on le présente comme question ouverte.
- Toute attribution (auteur, concept, date) doit être vérifiée une dernière fois.

## Workflow

### Étape 1 — Chargement
- Cherche le concept dans `research/index.json`
- Charge la fiche complète et le meta
- Vérifie le niveau de confiance

### Étape 2 — Chargement du template de canal
- Lis le template du canal dans `templates/canaux/{canal}.md`
- Si le fichier n'existe pas, affiche les canaux disponibles et arrête-toi

### Étape 3 — Sélection du contenu
- Identifie les éléments les plus pertinents selon les instructions du template de canal (section "Sélection du contenu")
- Écarte tout élément marqué `[⚠️]` (ou le transforme en question ouverte)

### Étape 4 — Rédaction
- Rédige en respectant les contraintes du template de canal (format, ton, structure, "à ne pas faire")
- Complète avec le ton et le style définis dans `profile.md`

### Étape 5 — Jugement (Agent Juge)
- Lis `.claude/agents/juge.md`, section "Pour la phase /publish"
- Vérifie : aucune incertitude en affirmation, attributions correctes, ton et longueur respectés
- Pour le carrousel : vérifier que chaque slide respecte la limite de mots et qu'il y a un arc narratif clair

### Étape 6 — Livraison
- Sauvegarde dans `research/memos/YYYY-MM-DD_publish_concept_canal.md`
- Affiche le contenu à l'utilisateur
- Ajoute une entrée dans le Journal de la fiche source
