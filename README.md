# desktop-monkey-site

Site vitrine de [Desktop Monkey](https://github.com/my-monkeys/desktop-monkey) —
en ligne sur **https://desktop-monkey.my-monkey.fr**.

Statique : `index.html` + `styles.css` + `app.js`, aucune dépendance, aucun build.
Les GIFs de démo à la racine servent aussi d'illustrations au README de l'app.

## Déployer

Le déploiement se déclenche sur une **release GitHub** (webhook org → monkey) :

```bash
cd ..
tar czf /tmp/app.tgz --exclude='.git' desktop-monkey-site/
gh release create v1.0.1 --repo my-monkeys/desktop-monkey-site /tmp/app.tgz \
  --title "Site v1.0.1" --notes "…"
```

Puis confirmer `status: success` sur `http://monkey.cookie/` (le `gh release create`
rend la main avant la fin du pipeline).

Les versions du site sont **indépendantes** de celles de l'app : sortir une release
ici ne touche ni les binaires ni l'auto-update de Desktop Monkey.
