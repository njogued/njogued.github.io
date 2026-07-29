# CLAUDE.md

Guidance for Claude (and humans) working in this repository.

## Overview

This is Edward Njogu's personal portfolio website, served as a static site via
**GitHub Pages** at `https://njogued.github.io` (previously mapped to the custom
domain `njogued.me`). It is a hand-written, multi-page static site: no build
step, no framework, no package manager. Files are edited directly and pushed to
the default branch, which GitHub Pages publishes.

## Tech stack

- **HTML5** — hand-authored, one file per page.
- **CSS3** — custom stylesheets (no preprocessor).
- **Bootstrap 5.3.2** — loaded from CDN (jsDelivr) for layout and components.
- **Vanilla JavaScript** — small inline `<script>` blocks (e.g. the chat widget
  and contact-form validation on `index.html`). No JS framework.
- **htmx 1.9.6** — loaded from CDN on every page, but currently not doing much;
  most interactivity is plain JS.
- **Google Fonts (Poppins)** — loaded from CDN.
- **Icons8 / inline SVG** — for skill icons and social icons.

There is no Node, Python, or bundler in the pipeline despite the `.gitignore`
listing `*.pyc`. Treat this as a pure static-HTML project.

## Repository structure

```
/
├── index.html                     # Landing page (hero, about, skills, projects,
│                                   #   articles, services, contact, chat widget)
├── README.md                      # Short project blurb
├── robots.txt, sitemap.xml        # SEO (sitemap lastmod is stale: 2023-12-15)
├── Edward Njogu Resume - 2026.pdf # Current resume (root copy)
│
├── static/                        # Global assets for the landing page
│   ├── styles.css                 # Main stylesheet (~324 lines)
│   ├── welcome.jpg, grad.jpg      # Hero + about images
│   ├── chatbot1.png, chatcircle.png
│   ├── favicons/                  # Full favicon set + site.webmanifest
│   └── profile_app/njogued_blue.png  # Navbar logo
│
├── articles/                      # Blog / article pages (one HTML file each)
│   ├── base.html                  # Article template / starting point
│   ├── n8n-self-hosting-docker.html
│   ├── ai-2024.html               # "AI trends ... 2025"
│   ├── apis.html, chrome-extensions.html,
│   │   dev-lessons.html, pandas-intro.html
│   └── static/                    # Article-specific CSS + images
│       └── styles.css             # Separate stylesheet for article pages
│
├── projects/
│   ├── all.html                   # Full projects listing (anchored: #servlist,
│   │                              #   #servicemtaani, #portfolio, #other)
│   └── static/                    # Project screenshots
│
├── profile/
│   ├── xp.html                    # Experience / CV-style page
│   └── Edward_Njogu_Resume.pdf    # Second (older) resume copy
│
└── .vscode/settings.json
```

## How pages relate

- `index.html` is the entry point. Its navbar and footer link to in-page anchors
  (`#about`, `#skills`, `#projects`, `#contact`, `#articles`) plus out to
  `projects/all.html` and individual `articles/*.html` pages.
- Project cards on the landing page deep-link into `projects/all.html#<anchor>`.
- Article cards link directly to individual `articles/*.html` files.
- `articles/base.html` is the reusable scaffold for new articles.

## Conventions

- **Styling:** the landing page and `profile/` use `static/styles.css`; article
  pages use `articles/static/styles.css`. Keep them in sync where shared.
- **Paths:** `index.html` uses relative asset paths (`static/...`); subpages use
  root-absolute paths (`/static/...`). Match the pattern of the page you edit.
- **CDNs pinned with SRI:** Bootstrap, htmx, and jsDelivr links include
  `integrity` hashes. If you bump a version, update the hash too.
- **New article workflow:** copy `articles/base.html`, replace `<title>`, cover
  image (`articles/static/`), and body; then add a card to the Articles section
  of `index.html` and a `<url>` entry to `sitemap.xml`.
- **Fonts/colors:** Poppins throughout; primary brand blue is roughly
  `#1948c9`/`#2f56c1` (see `static/styles.css`).

## Local development

No server or build required. Open `index.html` directly, or serve the folder
for correct absolute-path resolution:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deployment

Pushing to the default branch publishes automatically via GitHub Pages. There is
no CI, no staging environment, and no build artifacts.

## Known issues / maintenance notes

- `sitemap.xml` `lastmod` dates are stale (2023-12-15) and the file omits some
  current articles (e.g. the n8n self-hosting guide).
- Footer copyright on `index.html` reads "© 2025".
- The floating chat widget on `index.html` is a mock — it always replies
  "this feature is still under development."
- The contact `<form>` is commented out; a "TBD" placeholder card remains.
- Two resume PDFs exist (`/Edward Njogu Resume - 2026.pdf` and
  `/profile/Edward_Njogu_Resume.pdf`); the root copy is current.
- `.DS_Store` files are committed in several folders and should be gitignored.

## Positioning context

Edward's current professional focus is **AI automation / n8n development**
(building production workflows, RAG pipelines with pgvector, LLM + Supabase
integrations) heading toward an **AI implementation** specialization. The live
site still frames him more generically ("software development, executive
support, process automation"). Content changes should move the site toward the
AI-automation positioning. See `SITE-ANALYSIS-AND-RECOMMENDATIONS.md` for the
detailed gap analysis and update plan.
