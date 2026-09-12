# Landing page

Page statique : **un seul fichier**, aucune dépendance externe, aucun build, aucun formulaire.
`index.html` contient le HTML, le CSS et le JS. Prête pour GitHub Pages.

## Principes de conception

- **Humaine, pas « IA »** : pas de cerveau numérique, pas de particules, pas de néon, pas de
  hologramme. Palette naturelle (vert profond, crème, terre cuite), une seule police
  d'affichage (Fraunces) sur une pile système.
- **Le produit est montré, pas décrit** : la démo du hero et le tableau de bord sont des
  maquettes réelles en HTML/CSS, pas des illustrations abstraites.
- **Zéro carte générique** : sections différenciées par le rythme (colonnes, filets, reçu,
  diptyque), pas par une grille de cartes répétée.

## Règle à ne pas casser : les chiffres doivent se réconcilier

L'argument central de la page est « le calcul ne devine pas ». Tous les nombres affichés
doivent donc être cohérents entre eux. Ils dérivent d'un seul jeu de valeurs CIQUAL :

| Aliment | /100 g | kcal | P | G | L |
|---|---|---|---|---|---|
| Œuf entier cuit | | 145 | 12,6 | 0,3 | 10,3 |
| Pain complet | | 250 | 9,4 | 41,0 | 3,3 |
| Blanc de poulet | | 110 | 29,8 | 0 | 1,0 |
| Riz blanc cuit | | 145 | 2,7 | 30,9 | 0,3 |
| Brocolis | | 37,6 | 2,8 | 3,1 | 0,4 |

Ce qui donne, et ce qui est affiché partout :

- **Petit-déjeuner** (2 œufs 110 g + pain complet 100 g) → **410 kcal**, 23 P / 41 G / 15 L
- **Déjeuner** (poulet 150 g + riz 200 g + brocolis 150 g) → **511 kcal**, 54 P / 66 G / 3 L
  (détail du reçu : 165 + 290 + 56 = 511)
- **Journée** → 410 + 511 = **921 kcal**, 77 P / 107 G / 18 L sur un objectif 2 100 / 130 / 220 / 70
- **Reste après le petit-déjeuner** (démo du hero) → 2 100 − 410 = **1 690 kcal**, 130 − 23 = **107 g** de protéines

Si tu modifies un repas, refais toute la chaîne : anneau (`data-count="921"` + le `921 / 2100`
dans le script), barres (`data-fill`), lignes de repas, et les `aria-label` des deux maquettes.

## Accessibilité — vérifié

- Contraste **AA sur les 258 nœuds de texte**, en thème clair **et** sombre.
  Attention au piège : en mode sombre le vert s'éclaircit, donc le texte posé dessus doit
  s'assombrir. C'est le rôle des variables `--on-green*` ; ne remets jamais `#fff` en dur
  sur un fond vert.
- Le marqueur « bientôt » des tarifs est **textuel**, pas seulement coloré.
- `prefers-reduced-motion` : aucune animation, aucun curseur clignotant, compteurs à leur
  valeur finale.
- **Sans JavaScript, la page reste entièrement lisible.** Le masquage des animations n'est
  appliqué que si le script a pu poser la classe `js-anim` — et un filet de sécurité
  révèle tout au bout de 2,5 s si l'IntersectionObserver reste muet. Ne jamais inverser
  cette logique (`.rv { opacity: 0 }` sans garde = page blanche en cas de pépin).

Testé à 375 / 390 / 430 / 768 / 1024 / 1440 px : aucun débordement horizontal.

## Publier

```bash
git push
```

Puis sur GitHub : **Settings → Pages → Source: Deploy from a branch → Branch `main`, dossier `/docs`**.

⚠️ Un dépôt public expose tout le code et l'historique, y compris `spec/product-spec-original.txt`.
Pour ne publier que la page, utilise un dépôt séparé ne contenant que `docs/`.

## Ce qui reste à faire avant d'ouvrir vraiment les inscriptions

Les boutons « Commencer gratuitement » pointent sur `#` : **il n'y a pas encore d'application
hébergée derrière**. Avant de diffuser le lien :

1. héberger l'app et faire pointer les CTA dessus ;
2. écrire une politique de confidentialité (RGPD — l'app traite des données de santé) ;
3. brancher un moyen de paiement, sinon les formules Coach et Coach+ ne sont pas honorables.

Le pied de page contient les attributions **obligatoires** Ciqual (Licence Ouverte / Etalab)
et Open Food Facts (ODbL) : ne les retire pas.
