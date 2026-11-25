# Notes techniques — Le cahier de Lina (CE2)

Site statique (GitHub Pages), copie du moteur du site de Luka (luka-6eme) avec quelques ajouts.
À lire avant toute évolution, au lieu d'explorer le code.

## Arborescence
- `index.html` : accueil, choix de la matière (Maths, Dictée, Histoire).
- `commun/moteur.js` (mécanique), `commun/style.css` (thème violet, mode sombre ; couleur `--maths`).
- `maths/index.html`, `dictee/index.html` : contenu seul (couleurs `--maths`, `--dictee`). `outils/test.py` : test automatique.

## ⚠ Ne jamais changer
- `CLE = "linaCE2.v1"` (localStorage). Doit rester différente de celle de Luka (`lukaEnglish.v1`) :
  les deux sites sont sur le même domaine tuzette.github.io et partagent le stockage.
- Les `id` des thèmes existants.

## Ids déjà pris
maths : m21 … m28 (Fiche 2 - 1 à 8), m2b (bilan fiche 2), m10c m10m m10v m10r m10s m10p m10k (Leçon 10), mmon1 … mmon5 (Utiliser la monnaie), mfr1 mfr2 (Les fractions), mfr1 mfr2 (Les fractions) · dictee : d9 (liste 9 · La lettre e) · histoire : thème `hist`, textes h2, h3o, h3f, h4 (leçons 2 à 4).
Convention : `m<n° fiche><n° exo>`, `m<n° fiche>b` pour le bilan.

## Ajouts au moteur (par rapport à luka-6eme)
- `nb` : nombre de questions d'une série (défaut 10).
- `defi:false` : masque « Le grand défi ».
- `regle:"<html>"` : bouton « 📖 La méthode » (écran avec les exemples de la fiche).
- Saisie de nombre : clavier numérique sur mobile. Saisie libre : `verif(v)` (ex. « 4 cm 9 mm ») et `aide` (texte d'exemple).
- **Calcul chrono** : partie `{en:"chrono"}` + `gen.chrono` + `chrono:{duree, nb, ceintures:[[max, nom, couleur]…]}` ;
  pas de correction pendant le chrono, corrections et ceinture à la fin.
- `window.__q` = question en cours (utilisé par outils/test.py).
- Type **colorier** (`kind:"colorier"`) : figures SVG dont les parts `.part[data-i][data-f]` se colorient au toucher ;
  `verif([[f,i]…])` → juste/faux, `sol` = data-i à colorier (pour le test). Compteur « Parts coloriées » + Valider.
- QCM : `affiche(v)` = HTML du bouton (ex. fraction empilée), `data-v` reste le texte. `regleNom` = libellé du bouton leçon.
- Type **dictée** (`dictee:true`) : voix française (`direFr`), boutons Écouter / Plus lentement,
  correction lettre à lettre, « Refaire mes erreurs » en fin de série. Voir le modèle ci-dessous.
- Type **colorier** (gen `kind:"colorier"`) : `html` contient des figures avec `.part[data-i][data-f]` cliquables
  (data-i unique, data-f = n° de figure) ; `verif([[f,i]…])` → vrai/faux ; `sol` = data-i à colorier (pour outils/test.py).
  Compteur « Parts coloriées » et bouton Valider fournis par le moteur.
- QCM : `affiche(v)` facultatif = HTML d'un choix (ex. fraction écrite en hauteur). `regleNom` : libellé du bouton de leçon.

## Fiches de calcul mental (maths/index.html)
Une fabrique renvoie des calculs `{cons, q, r, rappel, astuce, pieges}`.
`exo(fabrique)` donne deux niveaux : `facile` (3 réponses au choix : la bonne + 2 pièges) et `dur` (saisie).
Leçon 10 : `regleSvg(d0, d1, A, B)` dessine une règle (cm) et un segment (mm) ; `niveauxMm` pour les réponses en cm et mm.
Fabriques disponibles : `table(t)`, `tables(...)`, `ajout([k…])`, `retrait([k…])`, `combien()`, `melangeExos([...])` pour un bilan.
Libellés : chapitre `"Calcul mental · Fiche N"`, thèmes `"Fiche N - n° · Titre de la fiche"`.

Monnaie (chapitre `CHM = "Utiliser la monnaie"`) : sommes en centimes ; `euro(c)` écrit « 24 € 36 » / « 94 c » ;
`porteMonnaie([valeurs])` dessine billets et pièces ; `niveauxEuro(fab)` gère les réponses en euros (`forme:"e"`, saisie
vérifiée par `verifEuro`, « 3 € 5 » refusé, il faut « 3 € 05 »), en centimes (`"c"`) ou en euros ronds (`"n"`) ;
`decompose(c)` = paiement avec le moins de pièces et billets.

## Fractions (maths/index.html, chapitre `CHF = "Les fractions"`)
`frac(n,d)` écrit une fraction empilée ; `bande(d,o)` / `disque(d,o)` dessinent d parts égales (`o.plein`, `o.clic`, `o.f`, `o.lab`) ;
`lit(n,d)` = « trois quarts ». Parties : facile (QCM), colorier, dur (saisie, accents facultatifs). nb:8.

## Histoire (histoire/index.html)
Un seul thème `hist` de type **leçon à trous** (`trous:true`) ; une leçon = un texte `{id, titre:"Leçon N · …", e, texte}`.
`[mot]` = trou, `[mot|variante]` = variantes acceptées en difficile. Ajouter une leçon = ajouter un texte (nouvel id `h<n°>`).

## Dictées (dictee/index.html)
`mots([["mot à écrire", "indice facultatif"], …])` ; thème `{id:"d<n° liste>", nom:"<n°> · <titre>", dictee:true, muet:true, maitrise:2, mots, parties:[{id, nom, e, mots:[…]}, {id, nom:"Tout mélangé"}]}`.
Orthographe stricte (accents comptés), majuscule exigée seulement si le mot en a une, « oe » accepté pour « œ »,
oubli de l'article signalé. Indice affiché seulement pour les mots ambigus à l'oral (haut, plus, chez…) et les verbes.

## Avant livraison
`python3 outils/test.py` doit afficher « Tout est OK ✔ » et « Erreurs JS : aucune » (QCM et saisies testés).

## Fractions (maths/index.html, chapitre `CHF = "Les fractions"`)
`frac(n, d)` écrit une fraction en hauteur ; `bande(d, o)` / `disque(d, o)` dessinent d parts égales
(`o.plein` parts coloriées, `o.clic` + `o.f` pour colorier, `o.lab` pour écrire 1/d) ; `lit(n, d)` → « trois quarts ».
Thèmes à 3 parties : `facile` (QCM), `colorier`, `dur` (saisie, accents facultatifs).
