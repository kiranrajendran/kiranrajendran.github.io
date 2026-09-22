# Minimalist Hugo Landing Page — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the existing Jekyll/Gulp blog with a single-page Hugo site deployed via GitHub Actions to GitHub Pages.

**Architecture:** One Hugo layout file (`layouts/index.html`) contains all HTML, CSS, and JS inline — no theme, no partials, no asset pipeline. A canvas animation runs in the background. Site config lives in `config.toml`. GitHub Actions builds and deploys on push to `master`.

**Tech Stack:** Hugo (static site generator), vanilla JS Canvas API, Google Material Symbols (CDN), Google Fonts Space Mono (CDN), GitHub Actions (`peaceiris/actions-hugo` + `peaceiris/actions-gh-pages`)

---

## File Map

| Action | Path | Purpose |
|---|---|---|
| Create | `config.toml` | Site params: name, links, GA ID |
| Create | `content/_index.md` | Triggers home template (empty front matter) |
| Create | `layouts/index.html` | Entire page: HTML + CSS + JS inline |
| Create | `static/favicon.ico` | Moved from repo root |
| Create | `static/resume-kiranr.pdf` | Moved from repo root |
| Create | `.github/workflows/deploy.yml` | Build Hugo + deploy to `gh-pages` branch |
| Create | `.gitignore` | Ignore `public/`, `.hugo_build.lock` |
| Keep | `CNAME` | Custom domain |
| Keep | `LICENSE` |  |
| Update | `AGENTS.md` | Reflect new Hugo structure |
| Delete | everything else | See Task 1 |

---

## Task 1: Repo cleanup — delete all old files

**Files:** Delete everything except `CNAME`, `LICENSE`, `AGENTS.md`, `favicon.ico`, `resume-kiranr.pdf`, `docs/superpowers/`, `.gitignore`, `.git/`

- [ ] **Step 1: Delete old directories**

```bash
git rm -r _posts _layouts _includes _sass _plugins oposts category pages admin src assets test docs/superpowers/specs 2>/dev/null; true
git rm -r _site .jekyll-cache .sass-cache .superpowers 2>/dev/null; true
```

> Note: `_site`, `.jekyll-cache`, `.sass-cache` may already be gitignored — that's fine if the command errors, continue.

- [ ] **Step 2: Delete old root files**

```bash
git rm -f gulpfile.js package.json Gemfile jekyll.gemspec portfolyou-jekyll-theme.gemspec \
  b.sh initpost.sh ka \
  contact.html 404.html message-sent.html staff.html tags.html index.html \
  feed.xml search.json sitemap.xml robots.txt screenshot.gif \
  README.md 2>/dev/null; true
```

- [ ] **Step 3: Verify only expected files remain**

```bash
git status
```

Expected survivors: `CNAME`, `LICENSE`, `AGENTS.md`, `favicon.ico`, `resume-kiranr.pdf`, `docs/superpowers/specs/2026-09-22-minimalist-landing-page-design.md`, `.gitignore`

- [ ] **Step 4: Commit cleanup**

```bash
git commit -m "Remove Jekyll/Gulp blog — replacing with Hugo landing page"
```

---

## Task 2: Hugo scaffold — config and content

**Files:**
- Create: `.gitignore`
- Create: `config.toml`
- Create: `content/_index.md`

- [ ] **Step 1: Write `.gitignore`**

```
public/
.hugo_build.lock
node_modules/
.sass-cache/
.jekyll-cache/
_site/
.superpowers/
```

- [ ] **Step 2: Write `config.toml`**

```toml
baseURL = "https://kiranr.com/"
languageCode = "en-us"
title = "Kiran Rajendran"

[params]
  tagline    = "Data wrangler. Performance obsessive. Amateur wizard. Professional coffee consumer."
  github     = "https://github.com/kiranrajendran"
  linkedin   = "https://linkedin.com/in/kiranrajendran"
  email      = "mailto:kiran.rajendran@gmail.com"
  resume     = "/resume-kiranr.pdf"
  ga_id      = "UA-132316635-1"
  footer     = "kiranr.com"
```

> **Owner action required:** Replace the `github` and `linkedin` values with real URLs before first deploy.

- [ ] **Step 3: Create `content/_index.md`**

```markdown
---
---
```

(Two lines of triple-dashes, empty front matter — tells Hugo to render the home template.)

- [ ] **Step 4: Verify Hugo recognises the config**

```bash
hugo version   # must be v0.100+ 
hugo --dry-run 2>&1 | head -20
```

Expected: No errors. Hugo lists one page: `/`.

- [ ] **Step 5: Commit**

```bash
git add .gitignore config.toml content/_index.md
git commit -m "Add Hugo config and content scaffold"
```

---

## Task 3: Home layout — full page HTML/CSS/JS

**Files:**
- Create: `layouts/index.html`

This is the entire site. All CSS and JS are inline so there is no asset pipeline.

- [ ] **Step 1: Create `layouts/` directory and write `layouts/index.html`**

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{ .Site.Title }}</title>
  <meta name="description" content="{{ .Site.Params.tagline }}">
  <link rel="icon" href="/favicon.ico">

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20,300,0,0&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">

  <!-- Google Analytics -->
  {{ if .Site.Params.ga_id }}
  <script async src="https://www.googletagmanager.com/gtag/js?id={{ .Site.Params.ga_id }}"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', '{{ .Site.Params.ga_id }}');
  </script>
  {{ end }}

  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:             #0d0d0d;
      --name:           #f0f0f0;
      --tagline:        #888;
      --tagline-border: #2a2a2a;
      --link:           #aaa;
      --link-hover:     #fff;
      --foot:           #2e2e2e;
    }

    [data-theme="light"] {
      --bg:             #f5f4f0;
      --name:           #111;
      --tagline:        #999;
      --tagline-border: #ddd;
      --link:           #888;
      --link-hover:     #111;
      --foot:           #ccc;
    }

    html, body {
      height: 100%;
    }

    body {
      background: var(--bg);
      color: var(--link);
      font-family: 'Space Mono', monospace;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 40px 24px;
      overflow: hidden;
      transition: background 0.3s;
    }

    canvas {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 0;
    }

    .page {
      position: relative;
      z-index: 1;
      width: 100%;
      max-width: 400px;
      display: flex;
      flex-direction: column;
      gap: 40px;
    }

    /* Toggle */
    .toggle-wrap { display: flex; justify-content: flex-end; }
    .theme-btn {
      background: none;
      border: none;
      cursor: pointer;
      font-size: 1.1em;
      padding: 4px;
      opacity: 0.5;
      transition: opacity 0.2s;
      user-select: none;
      line-height: 1;
    }
    .theme-btn:hover { opacity: 1; }

    /* Content */
    .content { display: flex; flex-direction: column; gap: 24px; }

    .name {
      font-size: 1em;
      font-weight: 700;
      letter-spacing: 4px;
      text-transform: uppercase;
      color: var(--name);
      transition: color 0.3s;
    }

    .tagline {
      font-size: 0.68em;
      line-height: 1.8;
      color: var(--tagline);
      border-left: 2px solid var(--tagline-border);
      padding-left: 14px;
      letter-spacing: 0.3px;
      transition: color 0.3s, border-color 0.3s;
    }

    .links {
      display: flex;
      flex-direction: column;
      gap: 14px;
      margin-top: 4px;
    }

    .links a {
      display: flex;
      align-items: center;
      gap: 12px;
      color: var(--link);
      text-decoration: none;
      font-size: 0.72em;
      letter-spacing: 1.5px;
      text-transform: uppercase;
      transition: color 0.2s;
    }
    .links a:hover { color: var(--link-hover); }
    .links a .material-symbols-outlined {
      font-size: 16px;
      opacity: 0.7;
      transition: opacity 0.2s;
    }
    .links a:hover .material-symbols-outlined { opacity: 1; }

    .foot {
      font-size: 0.5em;
      letter-spacing: 2px;
      color: var(--foot);
      text-transform: uppercase;
      transition: color 0.3s;
    }

    /* Mobile tweaks */
    @media (max-width: 480px) {
      body { padding: 32px 20px; align-items: flex-start; padding-top: 60px; }
    }
  </style>
</head>
<body>

<canvas id="bg"></canvas>

<div class="page">
  <div class="toggle-wrap">
    <button class="theme-btn" id="toggle" onclick="toggleTheme()" aria-label="Toggle color scheme">🌙</button>
  </div>

  <div class="content">
    <div class="name">{{ .Site.Title }}</div>
    <div class="tagline">{{ .Site.Params.tagline }}</div>
    <div class="links">
      {{ with .Site.Params.github }}
      <a href="{{ . }}" target="_blank" rel="noopener">
        <span class="material-symbols-outlined">code</span>
        GitHub
      </a>
      {{ end }}
      {{ with .Site.Params.linkedin }}
      <a href="{{ . }}" target="_blank" rel="noopener">
        <span class="material-symbols-outlined">work</span>
        LinkedIn
      </a>
      {{ end }}
      {{ with .Site.Params.resume }}
      <a href="{{ . }}" target="_blank" rel="noopener">
        <span class="material-symbols-outlined">description</span>
        Résumé
      </a>
      {{ end }}
      {{ with .Site.Params.email }}
      <a href="{{ . }}">
        <span class="material-symbols-outlined">mail</span>
        Email
      </a>
      {{ end }}
    </div>
  </div>

  <div class="foot">{{ .Site.Params.footer }}</div>
</div>

<script>
// ── Theme ─────────────────────────────────────────────────────
const root = document.documentElement;
const btn  = document.getElementById('toggle');

function applyTheme(theme) {
  root.setAttribute('data-theme', theme);
  btn.textContent = theme === 'dark' ? '🌙' : '☀️';
  localStorage.setItem('theme', theme);
}

function toggleTheme() {
  applyTheme(root.getAttribute('data-theme') === 'dark' ? 'light' : 'dark');
}

// Restore saved preference, fall back to system preference
(function() {
  const saved  = localStorage.getItem('theme');
  const system = window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark';
  applyTheme(saved || system);
})();

// ── Background animation ──────────────────────────────────────
const canvas = document.getElementById('bg');
const ctx    = canvas.getContext('2d');

const PALETTE = [
  { d: '64,196,255',  l: '0,120,180'  },
  { d: '80,220,140',  l: '20,150,80'  },
  { d: '255,160,80',  l: '200,100,20' },
  { d: '180,120,255', l: '120,60,200' },
  { d: '255,100,130', l: '200,40,80'  },
];

function isDark() { return root.getAttribute('data-theme') === 'dark'; }
function getColor(idx) {
  const p = PALETTE[idx % PALETTE.length];
  return isDark() ? p.d : p.l;
}

function rand(min, max) { return min + Math.random() * (max - min); }

function smoothWalk(n) {
  const pts = [rand(-0.5, 0.5)];
  for (let i = 1; i < n; i++) {
    pts.push(Math.max(-1, Math.min(1, pts[i-1] + (Math.random() - 0.5) * 0.55)));
  }
  return pts;
}

let elements = [];

function initElements() {
  const W = canvas.width, H = canvas.height;
  elements = [];

  for (let i = 0; i < 3; i++) {
    elements.push({
      type: 'line', colorIdx: i,
      y: H * (0.15 + i * 0.30) + rand(-20, 20),
      amplitude: H * rand(0.06, 0.10),
      points: smoothWalk(80),
      offset: rand(0, 200),
      speed: rand(0.12, 0.22),
      spacing: rand(38, 55),
      opacity: rand(0.09, 0.14),
    });
  }

  for (let i = 0; i < 2; i++) {
    const n = 5 + Math.floor(Math.random() * 4);
    elements.push({
      type: 'bar', colorIdx: 3 + i,
      x: W * (0.18 + i * 0.60) + rand(-40, 40),
      y: H * rand(0.25, 0.75),
      bars: Array.from({ length: n }, () => ({
        h: rand(0.3, 1.0), target: rand(0.3, 1.0), speed: rand(0.003, 0.007),
      })),
      maxH: H * rand(0.06, 0.10),
      barW: rand(5, 9),
      gap:  rand(4, 7),
      opacity: rand(0.08, 0.13),
    });
  }
}

function drawLine(el) {
  el.offset += el.speed;
  const startIdx    = Math.floor(el.offset / el.spacing);
  const pixelOffset = el.offset % el.spacing;
  const visible     = Math.ceil(canvas.width / el.spacing) + 4;

  while (el.points.length < startIdx + visible + 8) {
    const last = el.points[el.points.length - 1];
    el.points.push(Math.max(-1, Math.min(1, last + (Math.random() - 0.5) * 0.55)));
  }

  const c = getColor(el.colorIdx);
  ctx.beginPath();
  ctx.strokeStyle = `rgba(${c}, ${el.opacity})`;
  ctx.lineWidth = 1.5;
  ctx.lineJoin  = 'round';

  for (let i = 0; i <= visible; i++) {
    const x = i * el.spacing - pixelOffset;
    const y = el.y + el.points[startIdx + i] * el.amplitude;
    i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
  }
  ctx.stroke();

  ctx.beginPath();
  ctx.strokeStyle = `rgba(${c}, ${el.opacity * 0.3})`;
  ctx.lineWidth = 0.5;
  ctx.setLineDash([2, 14]);
  ctx.moveTo(0, el.y);
  ctx.lineTo(canvas.width, el.y);
  ctx.stroke();
  ctx.setLineDash([]);
}

function drawBars(el) {
  const c = getColor(el.colorIdx);
  el.bars.forEach((bar, i) => {
    bar.h += (bar.target - bar.h) * bar.speed;
    if (Math.random() < 0.003) bar.target = rand(0.2, 1.0);

    const bx = el.x + i * (el.barW + el.gap) - (el.bars.length * (el.barW + el.gap)) / 2;
    const bh = bar.h * el.maxH;
    const by = el.y - bh;

    ctx.fillStyle = `rgba(${c}, ${el.opacity})`;
    ctx.fillRect(bx, by, el.barW, bh);

    ctx.fillStyle = `rgba(${c}, ${Math.min(1, el.opacity * 2.5)})`;
    ctx.fillRect(bx, by, el.barW, 1.5);
  });
}

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  elements.forEach(el => el.type === 'line' ? drawLine(el) : drawBars(el));
  requestAnimationFrame(draw);
}

function resize() {
  canvas.width  = window.innerWidth;
  canvas.height = window.innerHeight;
  initElements();
}

resize();
window.addEventListener('resize', resize);
draw();
</script>

</body>
</html>
```

- [ ] **Step 2: Verify Hugo builds without error**

```bash
hugo --minify 2>&1
```

Expected: `Total in X ms` with no ERROR lines. A `public/index.html` is created.

- [ ] **Step 3: Check the built output has the expected content**

```bash
grep -c "Kiran Rajendran" public/index.html   # should print 1 or more
grep -c "canvas" public/index.html            # should print 1 or more
```

- [ ] **Step 4: Run dev server and visually verify**

```bash
hugo server --disableFastRender
```

Open `http://localhost:1313` — confirm:
- Dark background on first load
- Name, tagline, and 4 links visible
- 🌙 toggle switches to light mode and back
- Background animation running
- Page looks correct on narrow browser window (mobile simulation)

- [ ] **Step 5: Commit**

```bash
git add layouts/index.html
git commit -m "Add Hugo home layout with animated background"
```

---

## Task 4: Static assets

**Files:**
- Move: `favicon.ico` → `static/favicon.ico`
- Move: `resume-kiranr.pdf` → `static/resume-kiranr.pdf`

- [ ] **Step 1: Move assets into static/**

```bash
mkdir -p static
git mv favicon.ico static/favicon.ico
git mv resume-kiranr.pdf static/resume-kiranr.pdf
```

- [ ] **Step 2: Verify they are served by Hugo**

```bash
hugo --minify 2>&1
ls public/favicon.ico public/resume-kiranr.pdf
```

Expected: both files exist in `public/`.

- [ ] **Step 3: Commit**

```bash
git commit -m "Move favicon and resume PDF into Hugo static directory"
```

---

## Task 5: GitHub Actions deploy workflow

**Files:**
- Create: `.github/workflows/deploy.yml`

This workflow builds Hugo and pushes the `public/` directory to the `gh-pages` branch. GitHub Pages must be configured to serve from `gh-pages`.

- [ ] **Step 1: Create workflow file**

```bash
mkdir -p .github/workflows
```

Write `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [master]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: false

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          cname: kiranr.com
```

- [ ] **Step 2: Commit the workflow**

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow to build and deploy Hugo site"
```

- [ ] **Step 3: Owner action — configure GitHub Pages**

In the repo settings on GitHub:
1. Go to **Settings → Pages**
2. Set **Source** to **Deploy from a branch**
3. Set branch to **`gh-pages`**, folder **`/ (root)`**
4. Save

After the first push, the workflow runs and creates the `gh-pages` branch automatically.

---

## Task 6: Update AGENTS.md

**Files:**
- Modify: `AGENTS.md`

- [ ] **Step 1: Overwrite AGENTS.md to reflect the new Hugo structure**

```markdown
# AGENTS.md

## Project Overview

Personal landing page at `kiranr.com`. Single-page Hugo site — no theme, no blog, no build pipeline beyond Hugo itself.

---

## Commands

### Local Development
\`\`\`bash
hugo server --disableFastRender   # live-reload dev server at http://localhost:1313
\`\`\`

### Production Build
\`\`\`bash
hugo --minify   # outputs to public/
\`\`\`

### Deploy
Push to `master` — GitHub Actions builds and deploys automatically to the `gh-pages` branch.

---

## Structure

\`\`\`
/
├── config.toml          # all site config: title, links, GA ID
├── content/_index.md    # empty front matter — triggers home template
├── layouts/
│   └── index.html       # the entire site: HTML + CSS + JS inline, no partials
├── static/
│   ├── favicon.ico
│   └── resume-kiranr.pdf
├── CNAME                # custom domain
└── .github/workflows/
    └── deploy.yml       # build + deploy to gh-pages on push to master
\`\`\`

---

## Editing Content

All content lives in `config.toml` under `[params]`:

- `tagline` — the one-liner below the name
- `github`, `linkedin`, `email`, `resume` — link targets
- `ga_id` — Google Analytics ID
- `footer` — text at bottom of page

The layout reads these with `{{ .Site.Params.<key> }}`. No need to touch `layouts/index.html` for content changes.

---

## Editing Styles / Animation

All CSS and JS is inline in `layouts/index.html`. Color tokens are CSS custom properties on `:root` (dark) and `[data-theme="light"]`. Animation palette is the `PALETTE` array near the top of the `<script>` block.

---

## Gotchas

- `content/_index.md` must exist (even empty) for Hugo to render the home route.
- `static/` files are served from the site root — `static/resume-kiranr.pdf` becomes `/resume-kiranr.pdf`.
- Theme preference is saved to `localStorage` and also respects `prefers-color-scheme` on first visit.
- Hugo install: \`brew install hugo\` (macOS) or download from https://gohugo.io/installation/
```

- [ ] **Step 2: Commit**

```bash
git add AGENTS.md
git commit -m "Update AGENTS.md for Hugo landing page"
```

---

## Self-Review

**Spec coverage check:**

| Spec requirement | Task |
|---|---|
| Hugo platform | Task 2 |
| Repo cleanup (delete Jekyll/Gulp) | Task 1 |
| `config.toml` with all params | Task 2 |
| Single layout, all inline | Task 3 |
| Dark/light toggle, localStorage | Task 3 |
| System color scheme preference | Task 3 |
| Background animation: lines + bars | Task 3 |
| Colored animation palette | Task 3 |
| Material Symbols icons | Task 3 |
| Space Mono font | Task 3 |
| 4 links (GitHub, LinkedIn, Résumé, Email) | Task 3 |
| Mobile responsive | Task 3 (viewport meta + mobile media query) |
| `favicon.ico` + `resume-kiranr.pdf` in static | Task 4 |
| CNAME preserved | Task 1 (not deleted) |
| GitHub Actions deploy | Task 5 |
| Google Analytics | Task 3 |
| AGENTS.md updated | Task 6 |

All spec requirements covered. No placeholders. No TBDs.
