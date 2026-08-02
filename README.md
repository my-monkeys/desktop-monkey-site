# desktop-monkey-site

Site vitrine de [Desktop Monkey](https://github.com/my-monkeys/desktop-monkey) —
en ligne sur **https://desktop-monkey.my-monkey.fr**.

Statique : `index.html` + `styles.css` + `app.js`, aucune dépendance, aucun build.

Les démos sont servies en `.webm`/`.mp4` (les `.gif` d'origine restent là : le README
de l'app les référence). Pour en régénérer une après une nouvelle capture :

```bash
ffmpeg -i X.gif -vf "scale=iw*2:ih*2:flags=neighbor" -c:v libvpx-vp9 -pix_fmt yuv420p -crf 32 -b:v 0 -an X.webm
ffmpeg -i X.gif -vf "scale=iw*2:ih*2:flags=neighbor" -c:v libx264 -profile:v high -pix_fmt yuv420p -tune animation -crf 24 -movflags +faststart -an X.mp4
```

Le `×2 nearest` garde le pixel art net sur écran retina et garantit des dimensions
paires (obligatoire pour `yuv420p`).

⚠️ **Cache** : rien ne purge Cloudflare après un déploiement, et `styles.css`/`app.js`
sont mis en cache. Si tu les modifies, **incrémente le `?v=N`** dans `index.html` et
`404.html`, sinon les visiteurs gardent l'ancienne version jusqu'à 24 h.

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
