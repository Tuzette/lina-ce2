# Notes techniques — Le cahier de Lina (CE2)

Site statique (GitHub Pages), copie du moteur du site de Luka (luka-6eme) avec quelques ajouts.
À lire avant toute évolution, au lieu d'explorer le code.

## Arborescence
- `index.html` : accueil, choix de la matière (Maths, Dictée).
- `commun/moteur.js` (mécanique), `commun/style.css` (thème violet, mode sombre ; couleur `--maths`).
- `maths/index.html`, `dictee/index.html` : contenu seul (couleurs `--maths`, `--dictee`). `outils/test.py` : test automatique.

## ⚠ Ne jamais changer
- `CLE = "linaCE2.v1"` (localStorage). Doit rester différente de celle de Luka (`lukaEnglish.v1`) :
  les deux sites sont sur le même domaine tuzette.github.io et partagent le stockage.
- Les `id` des thèmes existants.

## Ids déjà pris
maths : m21 … m28 (Fiche 2 - 1 à 8), m2b (bilan fiche 2), m10c m10m m10v m10r m10s m10p m10k (Leçon 10) · dictee : d9 (liste 9 · La lettre e).
Convention : `m<n° fiche><n° exo>`, `m<n° fiche>b` pour le bilan.

## Ajouts au moteur (par rapport à luka-6eme)
- `nb` : nombre de questions d'une série (défaut 10).
- `defi:false` : masque « Le grand défi ».
- `regle:"<html>"` : bouton « 📖 La méthode » (écran avec les exemples de la fiche).
- Saisie de nombre : clavier numérique sur mobile. Saisie libre : `verif(v)` (ex. « 4 cm 9 mm ») et `aide` (texte d'exemple).
- **Calcul chrono** : partie `{en:"chrono"}` + `gen.chrono` + `chrono:{duree, nb, ceintures:[[max, nom, couleur]…]}` ;
  pas de correction pendant le chrono, corrections et ceinture à la fin.
- `window.__q` = question en cours (utilisé par outils/test.py).
- Type **dictée** (`dictee:true`) : voix française (`direFr`), boutons Écouter / Plus lentement,
  correction lettre à lettre, « Refaire mes erreurs » en fin de série. Voir le modèle ci-dessous.

## Fiches de calcul mental (maths/index.html)
Une fabrique renvoie des calculs `{cons, q, r, rappel, astuce, pieges}`.
`exo(fabrique)` donne deux niveaux : `facile` (3 réponses au choix : la bonne + 2 pièges) et `dur` (saisie).
Leçon 10 : `regleSvg(d0, d1, A, B)` dessine une règle (cm) et un segment (mm) ; `niveauxMm` pour les réponses en cm et mm.
Fabriques disponibles : `table(t)`, `tables(...)`, `ajout([k…])`, `retrait([k…])`, `combien()`, `melangeExos([...])` pour un bilan.
Libellés : chapitre `"Calcul mental · Fiche N"`, thèmes `"Fiche N - n° · Titre de la fiche"`.

## Dictées (dictee/index.html)
`mots([["mot à écrire", "indice facultatif"], …])` ; thème `{id:"d<n° liste>", nom:"<n°> · <titre>", dictee:true, muet:true, maitrise:2, mots, parties:[{id, nom, e, mots:[…]}, {id, nom:"Tout mélangé"}]}`.
Orthographe stricte (accents comptés), majuscule exigée seulement si le mot en a une, « oe » accepté pour « œ »,
oubli de l'article signalé. Indice affiché seulement pour les mots ambigus à l'oral (haut, plus, chez…) et les verbes.

## Avant livraison
`python3 outils/test.py` doit afficher « Tout est OK ✔ » et « Erreurs JS : aucune » (QCM et saisies testés).
