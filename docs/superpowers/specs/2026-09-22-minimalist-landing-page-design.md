# Design: Minimalist Hugo Landing Page

**Date:** 2026-09-22
**Status:** Approved

---

## Overview

Replace the existing Jekyll/Gulp blog with a single-page Hugo site hosted on GitHub Pages. The page is a minimalist personal landing — name, tagline, and four icon links — with a subtle animated data visualization background. No blog, no posts, no build pipeline complexity.

---

## Platform

- **Hugo** — static site generator, single binary, no Ruby/Node dependency for the core site
- **GitHub Pages** from `master` branch (existing CNAME stays, custom domain `kiranr.com`)
- Local dev: `hugo server` — instant live reload, nothing to install beyond the Hugo binary

---

## Content

| Element | Value |
|---|---|
| Name | Kiran Rajendran |
| Tagline | "Data wrangler. Performance obsessive. Amateur wizard. Professional coffee consumer." |
| Links | GitHub, LinkedIn, Résumé (PDF download), Email |
| Resume file | `static/resume-kiranr.pdf` (moved from repo root) |
| Google Analytics | UA-132316635-1 |
| Domain | kiranr.com (CNAME preserved) |

Link targets (placeholders — owner fills real URLs in `config.toml`):
- GitHub: `https://github.com/kiranrajendran`
- LinkedIn: `https://linkedin.com/in/kiranrajendran`
- Résumé: `/resume-kiranr.pdf`
- Email: `mailto:kiran.rajendran@gmail.com`

---

## Layout (Layout B — stacked)

```
[                          🌙 ]
KIRAN RAJENDRAN
| Data wrangler. Performance obsessive.
| Amateur wizard. Professional coffee consumer.

⬡ GitHub
⬡ LinkedIn
⬡ Résumé
⬡ Email

kiranr.com
```

- Full-viewport centered column, max-width ~400px, vertically centered
- Name: Space Mono, bold, uppercase, wide letter-spacing
- Tagline: Space Mono, regular, left-border accent, muted color
- Links: Space Mono, uppercase, icon + label, stacked list
- Footer: domain name, very faint
- Toggle: top-right corner, 🌙 / ☀️ emoji button

---

## Icons

Google Material Symbols Outlined (loaded via Google Fonts CDN):
- GitHub → `code`
- LinkedIn → `work`
- Résumé → `description`
- Email → `mail`

---

## Color Tokens

| Token | Dark | Light |
|---|---|---|
| Background | `#0d0d0d` | `#f5f4f0` |
| Name | `#f0f0f0` | `#111111` |
| Tagline | `#888` | `#999` |
| Tagline border | `#2a2a2a` | `#ddd` |
| Links | `#aaa` | `#888` |
| Links hover | `#fff` | `#111` |
| Footer | `#2e2e2e` | `#ccc` |

Default mode: **dark**. Preference saved to `localStorage`.

---

## Background Animation

Canvas element, `position: fixed`, full viewport, `z-index: 0`, `pointer-events: none`.

**3 line chart lanes** — slowly scroll left to right:
- Each lane is a smooth random walk tracing a line chart
- Dashed baseline per lane
- Speed varies per lane (~0.12–0.22px/frame)

**2 bar chart groups** — fixed position, bars breathe up/down:
- 5–8 bars per group, each easing toward a random target height
- Brighter top cap per bar

**Color palette** (muted, ~10% opacity):

| Element | Dark RGB | Light RGB |
|---|---|---|
| Line 1 | `64,196,255` (cyan-blue) | `0,120,180` |
| Line 2 | `80,220,140` (green) | `20,150,80` |
| Line 3 | `255,160,80` (amber) | `200,100,20` |
| Bars 1 | `180,120,255` (purple) | `120,60,200` |
| Bars 2 | `255,100,130` (rose) | `200,40,80` |

Animation re-initializes on window resize.

---

## Hugo Structure

```
/
├── config.toml          # site config: title, params (links, GA)
├── content/
│   └── _index.md        # empty front matter, triggers home template
├── layouts/
│   └── index.html       # single home template (all HTML/CSS/JS inline)
├── static/
│   ├── resume-kiranr.pdf
│   └── favicon.ico
├── CNAME
└── AGENTS.md
```

No theme dependency. Single self-contained layout file with all CSS and JS inlined — no separate asset pipeline needed.

---

## Repo Cleanup

Everything below is **deleted**:

- `_posts/`, `_layouts/`, `_includes/`, `_sass/`, `_plugins/`, `_site/`, `.jekyll-cache/`, `.sass-cache/`
- `oposts/`, `category/`, `pages/`, `admin/`, `src/`, `assets/`
- `gulpfile.js`, `package.json`
- `Gemfile`, `jekyll.gemspec`, `portfolyou-jekyll-theme.gemspec`
- `b.sh`, `initpost.sh`, `ka`
- `contact.html`, `404.html`, `message-sent.html`, `staff.html`, `tags.html`, `index.html`, `feed.xml`, `search.json`, `sitemap.xml`, `robots.txt`
- `screenshot.gif`, `resume-kiranr.pdf` (moved to `static/`)
- `test/`, `docs/` (spec files are ephemeral)

**Preserved:**
- `CNAME`
- `favicon.ico` → moved to `static/favicon.ico`
- `resume-kiranr.pdf` → moved to `static/resume-kiranr.pdf`
- `LICENSE`
- `AGENTS.md` (will be updated post-build)

---

## Mobile

Single-column layout is inherently responsive. Viewport meta tag set. Font sizes use `em` so they scale. Max-width constraint centers content on wide screens. No media queries needed beyond a small padding adjustment for very narrow viewports.

---

## Non-Goals

- No blog, no posts, no RSS
- No contact form
- No CMS
- No JavaScript framework
- No npm / Node build step
- No separate CSS files
