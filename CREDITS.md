# Credits

This project is a **front-end study recreation**. It exists to practise reproducing a clean,
minimal European SaaS page. It is not affiliated with, endorsed by, or connected to Lekteo.

## Design reference

- Layout, structure and visual language are inspired by the public home page of
  **https://lekteo.eu/** (French version).
- The French interface copy is kept verbatim on purpose, so the recreated page can be compared
  faithfully against the original during study. All text and the Lekteo name/wordmark belong to
  their respective owner. Do not reuse them commercially.

## Typefaces (self-hosted, unmodified)

| Font | Designer / foundry | Licence |
| --- | --- | --- |
| **Inter** (variable) | Rasmus Andersson — https://rsms.me/inter/ | SIL Open Font License 1.1 |
| **Source Serif 4** (variable) | Adobe — https://github.com/adobe-fonts/source-serif | SIL Open Font License 1.1 |
| **JetBrains Mono** (variable) | JetBrains — https://www.jetbrains.com/lp/mono/ | SIL Open Font License 1.1 |

Files live in `assets/fonts/`. The SIL Open Font License 1.1 permits bundling and redistribution
of the fonts with software; the full licence text is available at
https://openfontlicense.org/open-font-license-official-text/ and in each project's repository.

## Icons

The inline SVG sprite is drawn from **Lucide** (https://lucide.dev/), ISC Licence.
Paths are inlined in `index.html` as `<symbol>` elements — no icon font, no external requests.

## Type

Markup, CSS and JavaScript in this repository were written for this exercise.

## Fonts in this package

The three `.woff2` files are the Latin subsets published by the original site's font pipeline.
They are included so the page renders offline and identically to the reference; replace them with
the upstream releases from the links above if you prefer to redistribute from source.
