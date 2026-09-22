# AGENTS.md

## Project Overview

Personal landing page at `kiranr.com`. Single-page Hugo site — no theme, no blog, no build pipeline beyond Hugo itself.

---

## Commands

```bash
hugo server --disableFastRender   # dev server at http://localhost:1313
hugo --minify                     # production build → public/
```

Deploy: run `hugo --minify`, commit the `docs/` folder, push to `master`. GitHub Pages serves from `master/docs`.

---

## Structure

```
/
├── config.toml          # all content: title, links, GA ID, tagline
├── content/_index.md    # empty front matter — required to trigger home template
├── layouts/
│   └── index.html       # the entire site: HTML + CSS + JS inline, no partials
├── static/
│   ├── favicon.ico
│   └── resume-kiranr.pdf
├── CNAME                # custom domain kiranr.com
└── .github/workflows/
    └── deploy.yml       # build + push to gh-pages on push to master
```

---

## Editing Content

All content is in `config.toml` under `[params]`:

| Key | Purpose |
|---|---|
| `tagline` | one-liner below the name |
| `github` | GitHub profile URL |
| `linkedin` | LinkedIn profile URL |
| `email` | mailto: link |
| `resume` | path to PDF (served from `static/`) |
| `ga_id` | Google Analytics measurement ID |
| `footer` | text shown at bottom of page |

The layout reads these with `{{ .Site.Params.<key> }}`. No need to touch `layouts/index.html` for content changes.

---

## Editing Styles / Animation

All CSS and JS is inline in `layouts/index.html`:

- Color tokens: CSS custom properties on `:root` (dark) and `[data-theme="light"]`
- Animation palette: `PALETTE` array near the top of the `<script>` block
- Animation elements: 3 line chart lanes + 2 bar chart groups, all ~10% opacity

---

## GitHub Pages Setup

After first push, go to **Settings → Pages** in the repo and set:
- Source: **Deploy from a branch**
- Branch: **`gh-pages`**, folder: **`/ (root)`**

The workflow creates the `gh-pages` branch automatically on first run.

---

## Gotchas

- `content/_index.md` must exist (even with just `---\ntitle: ""\n---`) for Hugo to render the home route.
- `static/` files are served from the site root — `static/resume-kiranr.pdf` → `/resume-kiranr.pdf`.
- Theme preference saves to `localStorage` and respects `prefers-color-scheme` on first visit.
- Hugo install: `brew install hugo` (macOS) or https://gohugo.io/installation/
- `disableKinds = ["taxonomy", "term"]` in config suppresses Hugo warnings about missing taxonomy templates.
