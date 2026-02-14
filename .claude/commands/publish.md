# /publish — Produire un contenu éditorial à partir d'une fiche

Transforme une fiche de recherche en contenu publiable sur un canal donné.

## Paramètres

1. **Concept** (obligatoire) : le concept à publier (doit exister dans l'index)
2. **Canal** (obligatoire) : `newsletter` | `blog` | `linkedin` | `linkedin-carousel`

Exemple : `/publish "productivité" "linkedin-carousel"`

## Étape 0 — Vérification du profil

- Lis `profile.md`. S'il n'existe pas, affiche : "Profil non trouvé. Lancez `/init` pour configurer votre profil avant de publier." et arrête-toi.
- Charge les informations de canaux depuis `profile.md` (section "Canaux de publication").

## Règle absolue avant publication

- Vérifie le niveau de confiance de la fiche. Si < 3, préviens l'utilisateur et recommande un `/deepen` avant.
- **JAMAIS** transformer un élément marqué `[⚠️]` en affirmation. Soit on l'omet, soit on le présente comme question ouverte.
- Toute attribution (auteur, concept, date) doit être vérifiée une dernière fois.

---

## Canal : Newsletter

### Format
- **Longueur** : 800-1200 mots
- **Objet du mail** : court, intrigant, jamais clickbait

### Ton
- Défini dans `profile.md` → section Newsletter.
- Par défaut : personnel, en "je". L'utilisateur partage une découverte ou une réflexion. Pédagogique mais pas professoral. Curieux, pas sachant. Chaleureux, accessible.

### Structure type
1. **Accroche** : une anecdote, une question, un paradoxe
2. **"Pourquoi ça m'a intéressé"** : le contexte personnel
3. **Le cœur** : 2-3 idées forces, vulgarisées, avec les auteurs clés
4. **"Ce que ça change"** : implications concrètes, lien avec le contexte de l'utilisateur
5. **Ouverture** : une question, un lien vers le blog, ou un appel à réaction

### À ne pas faire
- Pas de jargon académique non expliqué
- Pas de liste de bullet points (rédiger en prose)
- Pas de ton corporate ou distant
- Ne pas essayer de tout dire — un angle fort vaut mieux qu'un survol
- Consulter les "À éviter" dans `profile.md` si définis

---

## Canal : Blog

### Format
- **Longueur** : 1500-2500 mots
- **Titre** : clair, informatif, peut contenir une question ou une affirmation

### Ton
- Défini dans `profile.md` → section Blog.
- Par défaut : expert accessible. Montrer la maîtrise sans être académique. Plus structuré que la newsletter, mais toujours engageant. On s'adresse à des professionnels qui veulent comprendre en profondeur.

### Structure type
1. **Introduction** : poser le problème. Pourquoi ce sujet maintenant ?
2. **Contexte** : d'où vient le concept, pourquoi il est pertinent
3. **Développement** : idées forces avec sources, tensions et débats
4. **Frameworks & méthodes** : si la fiche en contient, les rendre applicables
5. **Implications pour l'entreprise** : ce que ça change concrètement
6. **Conclusion** : synthèse + ouverture

### À ne pas faire
- Pas de copier-coller de la fiche (c'est un article, pas un rapport)
- Ne pas noyer le lecteur sous les sources — 3-4 max
- Pas de conclusions définitives sur des sujets encore débattus

---

## Canal : LinkedIn

### Format
- **Longueur** : 150-250 mots maximum
- **Pas de titre** — le post commence directement par l'accroche

### Ton
- Défini dans `profile.md` → section LinkedIn.
- Par défaut : direct, affirmé. Un point de vue, pas un résumé. Une seule idée développée. Engageant : finir par une question ou une ouverture.

### Structure type
1. **Accroche** (1-2 lignes) : phrase percutante, contre-intuitive, ou question forte
2. **Développement** (3-5 lignes) : l'idée avec un argument ou un exemple concret
3. **Ouverture** (1-2 lignes) : question au réseau ou affirmation à débattre

### À ne pas faire
- Pas de hashtags à rallonge (max 3 ciblés)
- Pas de ton "coach motivationnel"
- Pas d'émojis en début de chaque ligne
- Pas de "je suis ravi de vous annoncer" ou de fausse humilité
- Ne pas citer plus d'une source

---

## Canal : LinkedIn Carrousel

### Format
- **Post d'accompagnement** : 80-150 mots maximum (le carrousel fait le travail, le post accroche)
- **Carrousel** : 6-10 slides, jamais plus. Viser 8 idéalement.
- **Texte par slide** : 20-40 mots max. Une idée = une slide.

### Structure du carrousel
1. **Slide 1 — Couverture** : titre accrocheur + sous-titre d'une ligne. C'est la slide qui décide si on swipe ou pas.
2. **Slide 2 — Le problème ou la question** : poser la tension
3. **Slides 3 à 7 — Le développement** : une idée par slide, avec un chiffre ou une citation courte si possible. Arc narratif clair : chaque slide donne envie de voir la suivante.
4. **Slide 8 (ou avant-dernière) — Le retournement ou la synthèse** : l'insight principal
5. **Slide finale — Call to action** : question ouverte, invitation à commenter, ou renvoi vers le blog/newsletter

### Ton
- Visuel et percutant. Penser "affiche", pas "paragraphe".
- Chaque slide doit être compréhensible isolément mais gagner en force dans la séquence.
- Pas de phrases longues — privilégier les formulations courtes et frappantes.

### Output attendu
Le livrable est un fichier markdown structuré comme suit :

```markdown
## Post LinkedIn
[texte du post d'accompagnement]

## Carrousel

### Slide 1 — Couverture
**Titre** : [titre accrocheur]
**Sous-titre** : [une ligne]

### Slide 2 — [titre court]
[contenu de la slide — 20-40 mots max]

### Slide 3 — [titre court]
[contenu]

[... etc ...]

### Slide N — Call to action
[question ou invitation]
```

L'utilisateur se charge ensuite de la mise en forme dans son outil de design (voir `profile.md`).

### À ne pas faire
- Pas de slides "remplissage" — chaque slide doit apporter quelque chose
- Pas de mur de texte (si une slide dépasse 40 mots, la couper en deux)
- Pas de slide qui répète la précédente en d'autres mots
- Pas de chiffres non sourcés (si un chiffre vient de la fiche et est marqué ⚠️, ne pas l'utiliser)

---

## Workflow

### Étape 1 — Chargement
- Cherche le concept dans `research/index.json`
- Charge la fiche complète et le meta
- Vérifie le niveau de confiance

### Étape 2 — Sélection du contenu
- Identifie les éléments les plus pertinents pour le canal
- Écarte tout élément marqué `[⚠️]` (ou le transforme en question ouverte)
- LinkedIn : UNE seule idée forte
- LinkedIn carrousel : un arc narratif en 6-10 points, tiré des idées forces et tensions de la fiche
- Newsletter : 2-3 idées + un angle personnel
- Blog : l'ensemble, structuré pour le lecteur professionnel

### Étape 3 — Rédaction
- Rédige en respectant les contraintes du canal
- Utilise le ton et le style définis dans `profile.md`

### Étape 4 — Jugement (Agent Juge)
- Lis `agents/juge.md`, section "Pour la phase /publish"
- Vérifie : aucune incertitude en affirmation, attributions correctes, ton et longueur respectés
- Pour le carrousel : vérifier que chaque slide respecte la limite de mots et qu'il y a un arc narratif clair

### Étape 5 — Livraison
- Sauvegarde dans `research/memos/YYYY-MM-DD_publish_concept_canal.md`
- Affiche le contenu à l'utilisateur
- Ajoute une entrée dans le Journal de la fiche source
