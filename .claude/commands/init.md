# /init — Configurer votre profil VIA

Configure le système pour un nouvel utilisateur en créant `profile.md` via un dialogue interactif.

## Workflow

### Étape 1 — Bienvenue

Affiche :
```
Bienvenue dans VIA — Veille Intellectuelle Assistée !

Ce système vous aide à explorer des concepts en profondeur, construire une bibliothèque de connaissances interconnectées, et produire des contenus éditoriaux à partir de vos recherches.

Avant de commencer, je vais vous poser quelques questions pour personnaliser votre expérience.
```

Vérifie si `profile.md` existe déjà. Si oui, demande : "Un profil existe déjà. Voulez-vous le recréer depuis zéro ou le garder ?" Si l'utilisateur veut le garder, arrête-toi.

### Étape 2 — Identité

Demande :
- **Nom** : "Comment vous appelez-vous ?"
- **Rôle** : "Quel est votre rôle ou fonction ?"
- **Organisation** : "Dans quelle organisation travaillez-vous ? (optionnel)"

### Étape 3 — Contexte professionnel

C'est la question la plus importante. Demande :

"Décrivez en 2-3 phrases ce sur quoi vous travaillez et ce qui guide votre curiosité intellectuelle. C'est ce contexte qui servira de filtre pour toutes vos recherches."

Si la réponse est vague ou trop courte (moins de 20 mots), relance :
"Pouvez-vous être plus précis ? Par exemple : sur quels types de projets travaillez-vous ? Quels sujets vous préoccupent ? Qu'est-ce qui vous pousse à faire de la veille ?"

### Étape 4 — Canaux de publication (optionnel)

Demande : "Publiez-vous du contenu ? Si oui, sur quels canaux ?"

Propose les options :
- Newsletter (nom, ton)
- Blog (nom, ton)
- LinkedIn (ton, exemples)
- Outil design pour carrousels (Canva, Figma, etc.)
- Aucun pour le moment

Pour chaque canal sélectionné, pose 1-2 questions de suivi :
- Newsletter : "Quel est le nom de votre newsletter ? Quel ton utilisez-vous ?"
- Blog : "Quel est le nom de votre blog ? Quel ton ?"
- LinkedIn : "Comment décririez-vous votre ton sur LinkedIn ?"
- Design : "Quel outil utilisez-vous ?"

Si l'utilisateur ne publie pas, c'est ok. Il pourra configurer les canaux plus tard dans `profile.md`.

### Étape 5 — Mention des templates

Affiche brièvement :
"Les templates de fiches et mémos sont dans `templates/`. Vous pouvez les personnaliser plus tard si vous le souhaitez."

### Étape 6 — Création de profile.md

À partir de toutes les réponses, crée `profile.md` en suivant la structure de `profile.example.md`.

- Remplis toutes les sections avec les informations fournies
- Les sections non renseignées restent avec des placeholders clairs
- Le fichier doit être lisible et modifiable manuellement

### Étape 7 — Vérification de la structure

Vérifie que tous les dossiers nécessaires existent :
- `research/`
- `research/memos/`
- `research/sources/`
- `exports/`
- `inputs/`

Crée ceux qui manquent.

Vérifie que `research/index.json` existe et est un JSON valide. Sinon, le crée avec la structure vide :
```json
{"version": "1.0", "derniere_maj": "", "concepts": []}
```

### Étape 8 — Résumé et invitation

Affiche un résumé de ce qui a été configuré, puis :

```
Votre profil est prêt ! Vous pouvez maintenant :
→ /explore "concept" — pour explorer un sujet
→ /index — pour voir l'état de votre bibliothèque

Pour modifier votre profil plus tard, éditez directement `profile.md`.
```
