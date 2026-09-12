# Landing page

Page statique (un seul fichier, aucune dépendance, aucun build) prête pour GitHub Pages.

## Publier

Le dépôt n'a pas encore de remote GitHub. Une fois le dépôt créé sur ton compte :

```bash
git remote add origin git@github.com:<ton-compte>/<ton-repo>.git
git push -u origin main
```

Puis sur GitHub : **Settings → Pages → Source: Deploy from a branch → Branch: `main` / dossier `/docs`**.
La page sera servie sur `https://<ton-compte>.github.io/<ton-repo>/`.

⚠️ Publier le dépôt le rend **public** si tu choisis un repo public : tout le code, l'historique
des commits et `spec/product-spec-original.txt` deviennent lisibles par n'importe qui. Si tu veux
seulement publier la page, crée un dépôt séparé ne contenant que le dossier `docs/`.

## Connecter le formulaire

Le formulaire d'inscription n'envoie rien pour l'instant (il affiche une alerte). GitHub Pages
sert uniquement des fichiers statiques : il ne peut pas recevoir de POST. Options, sans serveur :

| Service | Gratuit | Note |
|---|---|---|
| Formspree | 50 envois/mois | le plus rapide à brancher |
| Tally / Typeform | oui | formulaire hébergé, on remplace le `<form>` par un lien |
| Buttondown, Mailchimp | oui (petits volumes) | si tu veux gérer la liste comme une newsletter |

Pour Formspree par exemple, il suffit de remplacer dans `index.html` :

```html
<form action="#" method="post" onsubmit="...">
```

par :

```html
<form action="https://formspree.io/f/TON_ID" method="POST">
```

et de supprimer l'attribut `onsubmit`.

## RGPD — à faire avant de collecter des e-mails

Collecter une adresse e-mail, c'est déjà un traitement de données personnelles :

- une mention indiquant qui est responsable du traitement et pourquoi l'e-mail est collecté ;
- un lien de désinscription réel ;
- une page politique de confidentialité (à écrire — elle n'existe pas encore).

Le pied de page contient déjà les attributions **obligatoires** pour Ciqual (Licence Ouverte)
et Open Food Facts (ODbL) : ne les retire pas.
