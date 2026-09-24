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

## Règle à ne pas casser : aucun chiffre n'est saisi à la main

L'argument central de la page est « le calcul ne devine pas ». **Tous les chiffres
nutritionnels de cette page sont générés par le dépôt de l'application**, depuis la même
base CIQUAL et les mêmes règles de calcul que l'application elle-même :

```bash
# dans le dépôt de l'application (app-diet)
npx tsx scripts/showcase-figures.ts --write   # recalcule et reporte les chiffres ici
npm run check:landing                         # échoue si cette page dit autre chose
```

- Les exemples (phrases, aliments retenus, grammes) sont définis dans
  `src/server/data/showcase.ts` de l'application. Chaque phrase est dans son corpus de
  non-régression : si l'application cessait de la comprendre ainsi, ses tests tomberaient.
- Ici, deux endroits portent des chiffres : le bloc
  `<script type="application/json" id="showcase-figures">`, lu par le JS des démos, et
  chaque élément `data-fig="…"` du HTML. **Ne modifie ni l'un ni l'autre à la main.**
- `check:landing` refuse aussi tout chiffre en kcal écrit hors d'un `data-fig`.

**Pourquoi.** En septembre 2026, cette page annonçait 511 kcal pour « 150 g de poulet,
200 g de riz et des brocolis » ; l'application en calculait 532. Le tableau de valeurs
d'ici avait été tapé à la main : poulet à sa valeur crue, œuf et pain complet sur d'autres
lignes CIQUAL que celles de l'application, brocolis à 150 g quand l'application prend
80 g sans quantité dite. La même page affirmait aussi que NutriCoach « ne remplit jamais
un blanc », alors que l'application pré-remplit une portion standard, marquée estimée,
depuis le 14 septembre. Ces textes ont été corrigés en même temps.

## Règle à ne pas casser : ne rien vendre qui n'existe pas

Les formules, leurs prix, leurs fonctions et la durée de l'essai sont **générés depuis
`src/lib/plan.ts` de l'application** (`data-fig="plans.…"`), par le même script que les
chiffres. `check:landing` échoue si une fonction de l'application manque sur la page, ou si
la page en affiche une qui n'existe plus.

L'application n'a que deux formules : Gratuit et la formule payante (Premium est masquée
dans `plan.ts`). Chaque compte commence par un essai de la formule payante, sans carte ; le
paiement n'est pas ouvert (`BILLING_ENABLED` baissé). La page le dit, et ne doit rien dire
d'autre tant que c'est vrai : une version précédente affirmait que « tout ce qui est codé
est utilisable sans payer », faux depuis que la formule ne s'écrit plus depuis le navigateur.

## Le nom du produit

« NutriCoach » n'est écrit nulle part à la main : chaque occurrence est un
`<span data-fig="brand.name">`, le titre et les balises meta portent `data-fig` /
`data-fig-content`, et la valeur vient de `src/lib/brand.ts` de l'application. Pour
renommer : changer la constante là-bas, puis `npx tsx scripts/showcase-figures.ts --write`.
`check:landing` refuse le nom écrit en dur hors de ces marqueurs.

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
