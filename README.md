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

`git push` sur `main` suffit : Coolify (prod-cookie, application
`ces0ch8jj9lvqotlifvp1blf`, build pack `static`) récupère le dépôt et sert la racine
telle quelle, sans build. Compter une à deux minutes.

Vérifier le dernier déploiement (`finished` + le commit poussé) :

```bash
ssh prod-cookie "docker exec coolify-db psql -U coolify -d coolify -At -F'|' -c \"select status, left(commit,7) from application_deployment_queues where application_id = (select id from applications where uuid='ces0ch8jj9lvqotlifvp1blf')::text order by created_at desc limit 1;\""
```

Plus de release GitHub ni de `.tgz` : l'ancien flux via monkey (dev-cookie) est mort.

## Crédits

- Sprites du singe : [Pixel Art Monkey](https://tiki-ted.itch.io/pixel-art-monkey) par tiki-ted
- Explosion de cœur : [Reactorcore](https://reactorcore.itch.io/explosion-spriteanim-sheet-minipack)
- Police : [Press Start 2P](https://fonts.google.com/specimen/Press+Start+2P) par CodeMan38, sous [SIL OFL 1.1](fonts/OFL.txt)

## Licence

Le code et les textes du site sont sous [licence MIT](LICENSE).

**Les médias ne le sont pas.** Les sprites appartiennent à leurs auteurs (voir Crédits),
et les captures d'écran (`.gif`, `.webm`, `.mp4`, `assets/og.png`) montrent en plus des
fonds d'écran Microsoft et Apple — elles sont là pour documenter l'application, pas pour
être réutilisées. Même distinction que dans le repo de l'app.
