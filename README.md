# Lekteo — front-end recreation

A pixel-accurate, **static** recreation of the French home page of **https://lekteo.eu/**,
built for educational and study purposes.

> **This is a study project, not the real service.** It is not affiliated with Lekteo, and it is
> deliberately front-end only: no backend, no uploads, no file parsing, no validation, no export,
> no APIs. Every button, drop area and link is a visual placeholder.

---

## 1. Quick start

There is **no build step and there are no dependencies**. The files in this folder are the
finished, production-ready site.

```bash
# any static file server works, e.g. Python
python3 -m http.server 4173
# then open http://localhost:4173/
```

Optional Node preview (zero dependencies, the file lives in `tools/`):

```bash
npm start          # == node tools/serve.mjs  → http://localhost:4173/
```

You can also just double-click `index.html`: the page is fully self-contained apart from its
relative `css/`, `js/` and `assets/` files, which browsers load fine from `file://`.

**Deployment requires nothing more than copying this folder to any static host.**

---

## 2. Project structure

```
lekteo-clone/
├── index.html                  # the page (semantic markup)
├── css/
│   └── styles.css              # all styles: design tokens, layout, components, responsive rules
├── js/
│   └── main.js                 # appearance controls + placeholder interactions (~2.6 KB)
├── assets/
│   ├── favicon.svg             # site icon (SVG)
│   └── fonts/
│       ├── inter-latin-wght-normal.woff2              # body / UI typeface (variable)
│       ├── source-serif-4-latin-wght-normal.woff2     # headings (variable)
│       └── jetbrains-mono-latin-wght-normal.woff2     # code + FAQ +/- glyphs (variable)
├── tools/
│   └── serve.mjs               # optional local preview server (Node built-ins only)
├── standalone.html             # optional single-file build: all CSS/JS/fonts inlined
├── package.json                # metadata + preview script (no dependencies to install)
├── .nojekyll                   # tells GitHub Pages to serve the files as-is
├── README.md
└── CREDITS.md
```

Notes:

- **Icons** are an inline SVG sprite at the top of `index.html` (Lucide paths), so there are no
  icon font files or extra image requests.
- **Fonts** are self-hosted `woff2` variable fonts referenced from `css/styles.css` with
  relative paths (`../assets/fonts/…`), so the site works from a sub-folder or a project page.
- **No absolute paths anywhere** — every reference is relative, which is what makes GitHub Pages
  project sites (`https://user.github.io/repo-name/`) work unchanged.

---

## 3. Deploy to GitHub Pages

### Option A — publish from a branch (no tooling needed)

1. Create a new repository on GitHub, e.g. `lekteo-clone`.
2. Upload the **contents** of this folder to the repository root (`index.html`, `css/`, `js/`,
   `assets/`, …). Keep the folder names exactly as they are — GitHub Pages is case-sensitive.
   - With git:
     ```bash
     cd lekteo-clone
     git init
     git add .
     git commit -m "Lekteo front-end recreation"
     git branch -M main
     git remote add origin https://github.com/<your-username>/<repo>.git
     git push -u origin main
     ```
3. In the repository: **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Branch: **main**, folder: **/ (root)** → **Save**.
6. Wait for the green check (usually under a minute). Your site is published at
   `https://<your-username>.github.io/<repo>/`.

The included `.nojekyll` file stops GitHub's Jekyll pipeline from processing the folder, so the
files are served byte-for-byte.

### Option B — publish from a `/docs` folder

If you prefer to keep the site out of the repository root: move `index.html`, `css/`, `js/` and
`assets/` into a `docs/` folder, then choose **Branch: main**, **Folder: /docs** in the Pages
settings. All internal paths are relative, so nothing else needs to change.

### Option C — GitHub Actions (automatic deploys)

Add `.github/workflows/deploy.yml`:

```yaml
name: Deploy static site to Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: .            # the folder that contains index.html
      - id: deployment
        uses: actions/deploy-pages@v4
```

Then set **Settings → Pages → Source** to **GitHub Actions**.

### Other static hosts

The same folder deploys unchanged to Netlify, Cloudflare Pages, Vercel (as a static site),
GitLab Pages, S3 + CloudFront, or any web server. No environment variables, no server settings,
no redirects or rewrites are required.

### Custom domain (optional)

Create a `CNAME` file containing only your domain (e.g. `lekteo-clone.example.com`), add a CNAME
DNS record pointing at `<your-username>.github.io`, and enable HTTPS in the Pages settings.

---

## 4. "Production build"

**Nothing to build.** The delivered files *are* the production output: one HTML document, one
stylesheet, one small script, one SVG icon and three `woff2` files, totalling ~150 KB.

If you want to squeeze it further, any generic minifier works on the existing files, e.g.:

```bash
npx lightningcss --minify --bundle css/styles.css -o css/styles.min.css
npx terser js/main.js -c -m -o js/main.min.js
```

…then point the `<link>` and `<script>` tags in `index.html` at the `.min` files. This is entirely
optional and not required for the site to run.

---

## 5. Dependencies

| Kind | Needed? |
| --- | --- |
| Runtime dependencies | **None** — plain HTML, CSS and vanilla JS |
| Build tools / bundler | **None** — no React, Vite, webpack, Sass or PostCSS |
| Package install (`npm install`) | **Not required** — `package.json` has no dependencies |
| Node.js | Only for the optional `npm start` preview script (Node 18+), never for hosting |

---

## 6. What is reproduced

- Language suggestion strip, header with wordmark and navigation, desktop link row with the
  `Comprendre` disclosure on small screens, language pills (FR/EN/DE/NL) and appearance pills
  (light / dark / system).
- Hero with the faint grid backdrop, headline, lead paragraph and the dashed drop area
  (including its touch-device variant with the "Choisir un fichier" button).
- Three benefit cards, three sample-invoice cards (third one highlighted), batch banner.
- Editorial band: five long-form sections plus the FAQ accordion.
- Footer with three link columns.

Layout, typography, colour, spacing, radii and shadows mirror the reference's design tokens. The
render was diffed against the live site and matches pixel-for-pixel at 21 viewport widths from
320 px to 1920 px, in light and dark mode, with both mouse and touch input, including the
open-FAQ, open-menu, forced-theme and dismissed-strip states.

### The only JavaScript (and what it does *not* do)

`js/main.js` is ~2.6 KB and is limited to presentation state:

- light / dark / system theme switching — `localStorage` + `data-theme` on `<html>`;
  a tiny inline snippet in `<head>` re-applies the stored choice before first paint (no flash);
- dismissing the language suggestion strip;
- preventing placeholder links from navigating away.

It never reads files, never touches an `<input type="file">` value, and makes **no network
requests**. The browser console is clean.

---

## 7. Customising

| What | Where |
| --- | --- |
| Colours, type scale, radii, shadows, easing | `css/styles.css` → `:root` custom properties (light + dark palettes) |
| Page copy | `index.html` |
| Fonts | drop a new `woff2` in `assets/fonts/` and update the three `@font-face` blocks |
| Icons | the `<symbol>` sprite near the top of `<body>` in `index.html` |
| Content width | `--content-width` (default `76rem`) |

---

## 8. Credits and licence

See **CREDITS.md**. In short: the layout is inspired by Lekteo's public site and the French copy
is retained verbatim so the study comparison stays faithful; fonts are Inter, Source Serif 4 and
JetBrains Mono (all SIL Open Font License 1.1); icons follow the Lucide set (ISC).

Please keep this project for personal study. Do not present it as the real service, and do not
reuse Lekteo's name, logo or copy commercially.
