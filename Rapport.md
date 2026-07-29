# RAPPORT.md — La Cantine du Code

**Étudiant :** Keita Walamoko
**Matière :** Git avancé
**Dépôt distant :** https://github.com/Mariam59190/cantine-du-code

---

## Étape A — Fondations (main)

```bash
mkdir cantine-du-code && cd cantine-du-code
git init
git config user.name "Keita Walamoko"
git config user.email "keita.walamoko@example.com"
```

Premier commit (page d'accueil + README) :
```bash
git add index.html README.md
git commit -m "Initial project with home page and README"
```
→ commit `a682a4c`

Ajout du `.gitignore` :
```bash
git add .gitignore
git commit -m "Add .gitignore"
```
→ commit `5871bcd`

Preuve d'efficacité du `.gitignore` :
```bash
touch debug.log
git status
```
Sortie obtenue :
```
On branch main
nothing to commit, working tree clean
```
`debug.log` n'apparaît pas dans le statut : le `.gitignore` fonctionne.

---

## Étape B — Fusion fast-forward

```bash
git checkout -b feature/menu
```

Commit 1 — création de la page :
```bash
git add menu.html
git commit -m "Create menu page"
```
→ commit `a172312`

Commit 2 — ajout des boissons :
```bash
git add menu.html
git commit -m "Add drinks to menu"
```
→ commit `db60325`

> Remarque : un second commit portant le même message *"Add drinks to menu"* (`83241a1`) est également présent dans l'historique, correspondant à un ajustement mineur du contenu de la liste de boissons avant la fusion dans `main`.

Retour sur `main` et fusion :
```bash
git checkout main
git merge feature/menu
```
Sortie obtenue :
```
Updating a172312..83241a1
Fast-forward
 menu.html | 12 ++++++++++++
 1 file changed, 12 insertions(+)
 create mode 100644 menu.html
```
Le mot **"Fast-forward"** confirme qu'aucun commit de merge n'a été créé.

```bash
git branch -d feature/menu
```

---

## Étape C — Fusion à trois branches

```bash
git checkout main
git checkout -b feature/horaires
```

Commit sur `feature/horaires` :
```bash
git add index.html
git commit -m "Add opening hours"
```
→ commit `7cb1684`

```bash
git checkout main
git checkout -b feature/contact
```

Commit 1 — création de la page :
```bash
git add contact.html
git commit -m "Create contact page"
```
→ commit `91bb1ec`

Commit 2 — lien vers la page depuis l'accueil :
```bash
git add index.html
git commit -m "Add contact link"
```
→ commit `487cabb`

Fusion des deux branches dans `main` :
```bash
git checkout main
git merge feature/horaires
git merge feature/contact
```

Sortie obtenue lors de la seconde fusion :
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Les deux branches modifiaient toutes deux `index.html` à proximité de la fermeture du `<body>` (horaires d'un côté, lien contact de l'autre), ce qui a provoqué un conflit imprévu par le scénario initial (qui supposait des zones disjointes). Résolution en conservant les deux blocs (horaires **et** lien contact) :
```bash
git add index.html
git commit -m "Resolve merge conflict"
```
→ commit de merge `c22fd24` (parents : `7cb1684` et `487cabb`)

```bash
git branch -d feature/horaires feature/contact
```

---

## Étape D — Le conflit

```bash
git checkout main
git checkout -b feature/slogan-punchy
```
Modification du `<h1>` :
```bash
git add index.html
git commit -m "Add punchy slogan"
```
→ commit `cdb858d`

```bash
git checkout main
git checkout -b feature/slogan-serieux
```
Modification de la **même ligne** avec un autre slogan :
```bash
git add index.html
git commit -m "Add serious slogan"
```
→ commit `762cf4a`

Fusion des deux branches :
```bash
git checkout main
git merge feature/slogan-punchy
git merge feature/slogan-serieux
```
Sortie obtenue :
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Contenu de `index.html` pendant le conflit :
```html
<<<<<<< HEAD
    <h1>La Cantine du Code : Le café des développeurs passionnés !</h1>
=======
    <h1>La Cantine du Code : Un espace dédié au développement et au partage</h1>
>>>>>>> feature/slogan-serieux
```

Résolution : combinaison des deux slogans, suppression des marqueurs :
```bash
git add index.html
git commit -m "Resolve merge conflict"
```
→ commit de merge `d2d95ce` (parents : `cdb858d` et `762cf4a`)

```bash
git branch -d feature/slogan-punchy feature/slogan-serieux
```

---

## Étape E — Dépôt distant

```bash
git remote add origin https://github.com/Mariam59190/cantine-du-code.git
git push -u origin main
```

Dernière branche : pied de page.
```bash
git checkout -b feature/footer
git add index.html
git commit -m "Add footer"
```
→ commit `445dd6f`
```bash
git push -u origin feature/footer
```

Pull Request `feature/footer` → `main` sur GitHub, fusionnée via l'interface (autorisée uniquement pour cette étape distante) :
```
Merge pull request #1 from Mariam59190/feature/footer
```
→ commit de merge `1cdcdf4`

Récupération en local :
```bash
git checkout main
git pull origin main
```

Vérification post-relecture du fichier `index.html` : les marqueurs de conflit `<<<<<<<` / `=======` / `>>>>>>>` de l'étape D n'avaient en réalité pas été entièrement nettoyés dans le commit `d2d95ce` (ils réapparaissaient mélangés avec le bloc de l'étape C). Correction :
```bash
git add index.html
git commit -m "Resolve merge conflict: combine slogans, keep horaires and contact"
```
→ commit `f25f4e5`

Le contenu de `contact.html` était par ailleurs erroné (contenu d'un `.gitignore` collé par erreur au lieu d'une page HTML). Correction :
```bash
git add contact.html
git commit -m "Fix contact page content"
git push origin main
```
→ commit `0308859`

```bash
git branch -d feature/footer
git push origin --delete feature/footer
```

---

## Sortie finale de `git log --oneline --graph --all`

```
* 0308859 (HEAD -> main, origin/main) Fix contact page content
* f25f4e5 Resolve merge conflict: combine slogans, keep horaires and contact
*   1cdcdf4 Merge pull request #1 from Mariam59190/feature/footer
|\
| * 445dd6f (origin/feature/footer, feature/footer) Add footer
|/
*   d2d95ce Resolve merge conflict
|\
| * 762cf4a (feature/slogan-serieux) Add serious slogan
* | cdb858d (feature/slogan-punchy) Add punchy slogan
|/
*   c22fd24 Resolve merge conflict
|\
| * 487cabb (feature/contact) Add contact link
| * 91bb1ec Create contact page
* | 7cb1684 (feature/horaires) Add opening hours
|/
* 83241a1 Add drinks to menu
* db60325 Add drinks to menu
* a172312 Create menu page
* 5871bcd Add .gitignore
* a682a4c Initial project with home page and README
```

---

## Questions d'analyse

**1. Différence entre la fusion de l'étape B et celles de l'étape C. Quand Git crée-t-il un commit de merge, et pourquoi ?**

À l'étape B, `main` n'avait reçu aucun commit depuis la création de `feature/menu` : Git a simplement déplacé le pointeur de `main` (fast-forward), sans créer de nouveau commit. À l'étape C, `main` avait avancé en parallèle sur les deux branches (`feature/horaires` et `feature/contact`), qui avaient donc divergé de `main` sans être des extensions linéaires l'une de l'autre. Git a dû créer un commit de merge à deux parents (`c22fd24`) pour réunir ces deux lignes de développement ; dans notre cas il s'est même produit un conflit, les deux branches ayant modifié une zone commune du fichier `index.html`.

**2. HEAD, une branche, un commit — explique avec tes mots.**

Un **commit** est un instantané du projet à un instant donné, identifié par un hash SHA-1, avec un auteur, un message et un pointeur vers son ou ses parent(s). Une **branche** est une étiquette mobile pointant vers un commit ; elle avance automatiquement à chaque nouveau commit réalisé dessus. **HEAD** est le pointeur qui indique où l'on se trouve actuellement : il pointe en général vers une branche (donc indirectement vers un commit), mais peut aussi pointer directement vers un commit (« detached HEAD »).

**3. Pourquoi le conflit de l'étape D était-il inévitable, alors que les fusions de l'étape C touchaient elles aussi le même fichier ?**

À l'étape D, les deux branches (`feature/slogan-punchy` et `feature/slogan-serieux`) modifiaient exactement la même ligne du fichier (`<h1>`) avec un contenu différent : Git ne peut pas deviner laquelle des deux versions garder, donc un conflit est inévitable. À l'étape C, en théorie les deux branches touchaient des parties différentes du fichier (horaires vs lien contact) et une fusion automatique aurait dû être possible ; dans les faits, un conflit s'est tout de même produit chez nous car les deux modifications ont fini par toucher une zone commune (probablement l'endroit d'insertion juste avant `</body>`), ce qui montre que la proximité des lignes modifiées — et pas seulement le fichier concerné — détermine si Git peut fusionner automatiquement.

**4. Que fait `git pull` exactement ? De quelles commandes est-il la combinaison ?**

`git pull` est la combinaison de `git fetch` (récupération des commits et branches du dépôt distant sans toucher au travail local) suivi de `git merge` (fusion de la branche distante correspondante dans la branche locale courante). C'est un raccourci pratique, mais qui peut créer un commit de merge ou provoquer un conflit exactement comme un `git merge` classique.

**5. Un fichier `.env` avec un mot de passe a été commité par erreur avant d'être ajouté au `.gitignore`. Suffit-il de l'ajouter au `.gitignore` ?**

Non. Ajouter `.env` au `.gitignore` empêche seulement Git de suivre ses **futures** modifications ; le mot de passe reste présent dans l'historique existant et accessible à quiconque clone le dépôt ou consulte un ancien commit. Il faut en plus retirer le fichier du suivi (`git rm --cached .env`) et **réécrire l'historique** (`git filter-repo` ou `BFG Repo-Cleaner`) pour effacer toute trace du secret dans les anciens commits, puis forcer le push. Le mot de passe exposé doit dans tous les cas être considéré comme compromis et **révoqué/changé immédiatement**.

---

## Auto-évaluation

| Critère | Points obtenus (sur barème) |
|---|---|
| Étape A — dépôt, commits initiaux, `.gitignore` fonctionnel | 15/15 |
| Étape B — branche et fusion fast-forward conforme | 15/15 |
| Étape C — branches parallèles et fusion avec commit de merge | 18/20 |
| Étape D — conflit provoqué, documenté et correctement résolu | 18/20 |
| Étape E — dépôt distant, push, Pull Request, pull | 10/10 |
| Questions d'analyse (2 pts par question) | 10/10 |
| Qualité du rapport et des messages de commit | 8/10 |
| **Total** | **94/100** |

*Points retirés : messages de commit un peu génériques ("Resolve merge conflict" répété sans détail) et un commit dupliqué non nettoyé à l'étape B.*