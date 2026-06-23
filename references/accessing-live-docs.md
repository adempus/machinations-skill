# Accessing the live Machinations.io docs (verified workflow)

The Machinations.io documentation is a **Nuxt client-side SPA**. A plain HTTP fetch
(`curl`, `web_extract`, etc.) returns the **same ~50KB app shell for every doc URL** —
navigation skeleton only, **zero article content**. This is the trap that defeats
naive doc-scraping. Verified June 2026: 9 different `/docs/<slug>` URLs returned
byte-identical HTML with no extractable body text.

## How to actually read a page

1. **Discover real routes** — the nav links use SPA router onClick (empty `href`), so
   you can't scrape URLs from the index. Instead pull the docs sitemap:
   ```
   curl -s https://machinations.io/sitemap.xml          # index -> lists sub-sitemaps
   curl -s https://machinations.io/static/docs-sitemap.xml \
     | grep -oE '<loc>[^<]+</loc>' | sed 's/<\/?loc>//g'  # real /docs/<slug> URLs
   ```
2. **Render the JS** — navigate the browser to the slug, then extract the rendered DOM
   (the accessibility snapshot is fine but often truncates; `browser_console` with
   `document.querySelector('main h1').parentElement.innerText` gives clean full text).
3. Slugs are **not** always the obvious form. Some carry suffixes: `triggers-gates-2`,
   `delays-queues-2`, `labels` (not `labels-types-and-intervals`), `activation-modes`.
   Always resolve via the sitemap, don't guess.

## Highest-value source-of-truth pages (slug -> what it confirms)

| Slug | Authoritative for |
|---|---|
| `framework-basics` | The 6-node / 2-connection taxonomy + single-letter codes (S/P/D/V/X/G, C, Alt+C) |
| `labels` | Intervals (pipe), ranges (`..`), the full State-Connection formula table, `=` overwrite, combining conditions |
| `registers` | math.js in passive registers; Initial/Step Value, Limits, `fx` display, interactive != formula |
| `randomness` | Randomness custom variable: Interval/Array, Gaussian/Uniform, On Play/Each Step |
| `math-expression` | math.js variables, cross-referencing other custom vars, 1D/2D arrays |
| `math-js-functions` | full math.js function list (not yet harvested — pull if a formula needs a specific fn) |

## Source-precedence rule

When the 2012 book and the live tool disagree on a *label character*, the **live tool
wins** (book uses `/` for intervals and `3-6` for ranges; tool uses pipe and `..`).
Concepts are identical across both; only a few tokens differ. See the "Functional
syntax" section of SKILL.md for the full mapping — all of it now verified against the
live docs, not self-reported.
