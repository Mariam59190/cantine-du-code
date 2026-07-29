# La Cantine du Code

Site vitrine fictif d'un café associatif pour développeurs, réalisé dans le cadre d'un TP de Git avancé (branches, fusions, conflits, dépôt distant).

## Contenu du site

- `index.html` — page d'accueil (slogan, horaires d'ouverture, lien vers le contact, pied de page)
- `menu.html` — menu des boissons proposées
- `contact.html` — page de contact

## Historique Git

Ce dépôt a été construit pas à pas pour illustrer :
- un cycle de base (working directory → staging → commit),
- une fusion fast-forward,
- une fusion avec commit de merge (`--no-ff`),
- un conflit de fusion provoqué et résolu manuellement,
- un flux de travail avec dépôt distant (push, pull, Pull Request).

Le détail complet des commandes utilisées, le graphe de commits et les réponses aux questions d'analyse se trouvent dans [`RAPPORT.md`](./RAPPORT.md).

## Voir le graphe de commits

```bash
git log --oneline --graph --all
```