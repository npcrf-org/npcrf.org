# npcrf.org

Source code of [npcrf.org](https://npcrf.org/), the blog of a non-profit,
non-governmental organization devoted to honest, factual education. All public
content is written in English.

The site is fully static: built with [Hugo](https://gohugo.io/) (extended,
pinned in the deploy workflow), styled by a bespoke theme with system fonts
only (no third-party theme, no CSS frameworks, no webfonts), and published to
GitHub Pages by the official GitHub Actions workflow.

## Licensing

This repository is covered by two licenses:

- **Code — MIT License** ([`LICENSE`](LICENSE)). Everything that makes the
  site work: templates, stylesheet, scripts, configuration and workflow.
  Copyright (c) 2026 npcrf.org.
- **Content — CC BY 4.0** ([`LICENSE-CONTENT`](LICENSE-CONTENT)). All
  editorial content, i.e. everything under [`/content`](content/).

The [License page](https://npcrf.org/license/) explains both in plain language,
including the attribution formula for reusing content.

## Repository layout

```
archetypes/   front matter templates for new content
assets/css/   the stylesheet (bundled, minified and fingerprinted by Hugo)
content/      editorial content (CC BY 4.0): posts/ plus standalone pages
layouts/      the bespoke theme: templates and partials
static/       files copied verbatim: CNAME, robots.txt, favicon.svg
.github/      deploy workflow and FUNDING.yml
```

## Writing

```sh
hugo new content posts/my-article-slug.md
```

The `posts` archetype pre-fills the front matter contract: `title`, `date`,
`lastmod`, `slug` (stable, never changed after publication), `tags`, `draft`,
`description` and `sources`. Dates are ISO 8601.

- **`sources` is mandatory** for articles: one entry per source with `url` and
  `accessed` date. It renders as the Sources section at the end of the
  article; a published article without sources fails the editorial bar and
  logs a build warning.
- **`corrections`** records substantive post-publication changes (date +
  note); it renders as a visible Corrections note on the article. Update
  `lastmod` whenever the article changes.

## Local development

Hugo is the only requirement (`brew install hugo`, or drop the pinned binary
into `bin/`, which is gitignored):

```sh
hugo server -D          # live preview with drafts at http://localhost:1313/
hugo --gc --minify      # production build into public/
```

Search is indexed by [Pagefind](https://pagefind.app/) after the build (in CI
this runs automatically):

```sh
npx -y pagefind@1.5.2 --site public   # or: bin/pagefind --site public
```

## Deployment

Every push to `main` triggers `.github/workflows/deploy.yml`: Hugo build
(version pinned), Pagefind indexing, upload, deploy to GitHub Pages. The
custom domain comes from `static/CNAME` (`npcrf.org`).

One-time repository setup:

1. **Pages**: Settings → Pages → Source: *GitHub Actions*.
2. **Custom domain**: DNS apex records for `npcrf.org` → GitHub Pages
   (A: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
   `185.199.111.153`; AAAA: `2606:50c0:8000::153` … `2606:50c0:8003::153`),
   then enable **Enforce HTTPS**.
3. **Discussions**: enable in Settings, keep the *Announcements* category.
4. **giscus**: install the [giscus app](https://github.com/apps/giscus) on the
   repository, then copy `repo`, `repoId`, `category`, `categoryId` from
   [giscus.app](https://giscus.app/) into `[params.giscus]` in `hugo.toml`.
5. **GoatCounter**: create the site code at
   [goatcounter.com](https://www.goatcounter.com/) (free non-commercial plan)
   and set `params.goatcounter` in `hugo.toml`.
6. **Funding**: fill the handles in `.github/FUNDING.yml` and the TODO
   placeholders (handles + IBAN) in `content/support.md`.
7. **Footer source link**: set `params.repository` in `hugo.toml`.

## Performance budget

The home page targets under 50 KB of HTML + CSS combined, with no webfonts and
no external libraries. JavaScript is limited to four components: the theme
switcher (inline), Pagefind (search page only), giscus (articles only, lazy)
and GoatCounter (one cookie-free script).
