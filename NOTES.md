# Notes techniques — Le cahier de Lina (CE2)

Site statique (GitHub Pages), copie du moteur du site de Luka (luka-6eme) avec quelques ajouts.
À lire avant toute évolution, au lieu d'explorer le code.

## Arborescence
- `index.html` : accueil, choix de la matière (Maths seulement pour l'instant).
- `commun/moteur.js` (mécanique), `commun/style.css` (thème violet, mode sombre ; couleur `--maths`).
- `maths/index.html` : contenu seul. `outils/test.py` : test automatique.

## ⚠ Ne jamais changer
- `CLE = "linaCE2.v1"` (localStorage). Doit rester différente de celle de Luka (`lukaEnglish.v1`) :
  les deux sites sont sur le même domaine tuzette.github.io et partagent le stockage.
- Les `id` des thèmes existants.

## Ids déjà pris
maths : m21 … m28 (Fiche 2 - 1 à 8), m2b (bilan fiche 2).
Convention : `m<n° fiche><n° exo>`, `m<n° fiche>b` pour le bilan.

## Ajouts au moteur (par rapport à luka-6eme)
- `nb` : nombre de questions d'une série (défaut 10).
- `defi:false` : masque « Le grand défi ».
- `regle:"<html>"` : bouton « 📖 La méthode » (écran avec les exemples de la fiche).
- Saisie de nombre : clavier numérique sur mobile.

## Fiches de calcul mental (maths/index.html)
Une fabrique renvoie des calculs `{cons, q, r, rappel, astuce, pieges}`.
`exo(fabrique)` donne deux niveaux : `facile` (3 réponses au choix : la bonne + 2 pièges) et `dur` (saisie).
Fabriques disponibles : `table(t)`, `tables(...)`, `ajout([k…])`, `retrait([k…])`, `combien()`, `melangeExos([...])` pour un bilan.
Libellés : chapitre `"Calcul mental · Fiche N"`, thèmes `"Fiche N - n° · Titre de la fiche"`.

## Avant livraison
`python3 outils/test.py` doit afficher « Tout est OK ✔ » et « Erreurs JS : aucune » (QCM et saisies testés).
