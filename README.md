# qaun.homes

Portfolio for Qaun Huyen (Quản Huyền / Vũ Lê Hoàng), interdisciplinary artist, Hanoi.

Static site. `index.html` at root, all images in a single flat `images/` folder.
No build step — Vercel serves it as-is.

```
index.html          the live site
images/             109 images, flat, slug-prefixed filenames
content/            works.json, readings.json, profile.md
reference/demo.html approved prototype (identical to index.html)
HANDOFF.md          brief for the Astro rebuild
```

Image paths are `images/<slug>-NN.jpg` — no subfolders. Keep it that way.
Never commit zip archives; upload extracted contents only.
