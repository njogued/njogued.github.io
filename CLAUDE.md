# CLAUDE.md

Guidance for Claude (and humans) working in this repository.

## Overview

Edward Njogu's personal portfolio, served as a static site via **GitHub Pages**
at **`https://edwardnjogu.com`**. Hand-written multi-page HTML: no build step, no
framework, no package manager, no CI. Files are edited directly and pushed to
`main`, which GitHub Pages publishes.

Pushing to `main` is a live deploy. There is no staging environment.

## Domain

- Custom domain `edwardnjogu.com`, set by the root `CNAME` file.
- DNS: four `A` records on the apex → GitHub Pages, plus `www` CNAME →
  `njogued.github.io`. Registrar is Spaceship.
- **Enforce HTTPS is on.** `http://`, `www.`, and the old `njogued.github.io`
  all 301 to `https://edwardnjogu.com/`.
- The earlier custom domain `njogued.me` is dead and must not be reintroduced —
  it was hardcoded in subpage nav links for years after lapsing.

## Tech stack

- **HTML5**, hand-authored, one file per page.
- **CSS3**, two custom stylesheets, no preprocessor.
- **Geist + Geist Mono** from Google Fonts — one of two external dependencies.
- **Google Analytics 4** (`gtag.js`, property `G-7JGV5WLQPR`) — the other. The
  snippet sits just before `</head>` on every page, `articles/base.html`
  included, so new articles inherit it. Added August 2026.
- **Vanilla JS**, one three-line inline script per page (dynamic footer year),
  plus the currency-converter demo on `articles/apis.html`.

Bootstrap, htmx, Poppins, Icons8, and jsDelivr were **removed** in the July 2026
redesign. Do not reintroduce them. Social and brand icons are inline SVG.

## Repository structure

```
/
├── index.html                  # Landing page: hero, 01 about, 02 stack,
│                               #   03 work, 04 services, 05 writing, 06 contact
├── CNAME                       # edwardnjogu.com
├── robots.txt, sitemap.xml     # SEO — sitemap uses real publication dates
├── resume.pdf                  # Public resume (linked from contact)
├── favicon.ico                 # Root fallback for crawlers that ignore <link>
├── feed.xml                    # RSS 2.0 for the six articles
├── llms.txt                    # Plain-text site summary for AI crawlers
├── DESIGN.md                   # Visual system — read before any UI work
├── SITE-ANALYSIS-AND-RECOMMENDATIONS.md   # Positioning gap analysis
│
├── n8n-pro-tips/
│   └── index.html              # Lead magnet: 21 n8n failure modes, free.
│                               #   sitemap priority 0.90 — the most-shared URL
│                               #   after the landing page. Linked twice from
│                               #   index.html. Easy to miss in sitewide passes:
│                               #   it is neither an article nor a project.
├── assets/
│   ├── n8n-pro-tips.pdf        # The lead magnet, PDF form
│   └── n8n-pro-tips-skill.zip  # The same, as a Claude Skill
│
├── static/
│   ├── site.css                # THE design system (~980 lines): tokens, nav,
│   │                           #   bands, cards, footer, doc pages
│   ├── ed-site.svg             # Logo source (ED monogram, #4d0da2)
│   ├── og-card.jpg             # 1200x630 link-preview card — THE live og:image
│   ├── og-image.png            # The same card as PNG. Superseded, kept only so
│   │                           #   scrapers holding the old URL do not 404.
│   │                           #   Safe to delete once caches age out (Sep 2026).
│   ├── og-image.src.html       # Source for both — re-render with headless
│   │                           #   Chrome, then convert; never hand-edit a binary
│   ├── favicons/               # favicon.svg + PNG set + site.webmanifest
│   └── styles.css              # LEGACY, orphaned — safe to delete
│
├── articles/
│   ├── index.html              # Writing hub at /articles/ — Blog schema, lists
│   │                           #   all six. Was a 404 until Aug 2026.
│   ├── base.html               # Template for new articles
│   ├── n8n-self-hosting-docker.html    # Lead article
│   ├── apis.html               # Has an interactive Coinbase demo + inline JS
│   ├── ai-2024.html, chrome-extensions.html,
│   │   dev-lessons.html, pandas-intro.html
│   └── static/styles.css       # Long-form typography, loads ON TOP of site.css
│
└── projects/
    ├── index.html              # Detail page at /projects/, anchored: #sosensus,
    │                           #   #gmail-triage, #light-crm, #servlist,
    │                           #   #servicemtaani, #portfolio, #other
    └── all.html                # Redirect stub → /projects/ (the old URL)
```

`profile/` was deleted (both `xp.html` and the stale resume copy).

## How pages relate

- `index.html` is the entry point; nav and footer link to in-page anchors
  (`#about`, `#stack`, `#work`, `#services`, `#writing`, `#contact`).
- Work cards deep-link to `projects/#<anchor>`. **Every project card
  follows the same path: card → detail page → external link.** External URLs
  live in the detail page's metadata table, not on the landing card.
- Writing cards link to `articles/*.html`; LinkedIn posts link out directly.
- Each article ends with a "more writing" list of the other five. Adding an
  article means updating that list in every existing article.

## Stylesheets

| Page | Loads |
|---|---|
| `index.html` | `static/site.css` |
| `projects/index.html` | `/static/site.css` |
| `articles/*.html` | `/static/site.css` **then** `/articles/static/styles.css` |

`static/site.css` owns the `:root` tokens. Never hardcode a colour — reference
the variable.

## Conventions

- **Paths:** `index.html` uses relative (`static/…`); all subpages use
  root-absolute (`/static/…`). Root-absolute is the house style — it is
  depth-independent. Do not use `../`.
- **Design tokens:** defined once on `:root` in `static/site.css`.
- **Article prose markup** uses legacy class names (`.bodyText`, `.subTitle`,
  `.codeblock`, `.listItems`) styled in `articles/static/styles.css`. This was
  deliberate — restyling six files beat rewriting their prose. Keep using them.
- **No shadows.** Structure comes from hairline borders and whitespace.
- **New article:** copy `articles/base.html`; replace title, meta description,
  canonical, eyebrow topic, date, and reading time; write the body; then add a
  card to the Writing section of `index.html`, a `<url>` to `sitemap.xml`, and a
  row to the "more writing" list in every other article, a card **and** a
  `blogPost` entry to `articles/index.html`, an `<item>` to `feed.xml`, and a
  line to `llms.txt`. In the template, replace **`SLUG`
  everywhere** (canonical, `og:url`, and both JSON-LD blocks) and
  **`YYYY-MM-DD` everywhere** (`article:published_time`, the `<time datetime>`,
  and `datePublished`). The OG image and card type are inherited; leave them
  alone.

## Page metadata

Every page carries the same head contract. When adding a page, copy it whole:

- `og:type`, `og:url` (**must equal the canonical** — LinkedIn caches on
  `og:url`, not canonical), `og:site_name`, `og:title`, `og:description`
- `og:image` → `/static/og-card.jpg` with `secure_url`, `type`, width, height,
  and alt, plus `twitter:card="summary_large_image"` and `twitter:image`.
  One shared card sitewide, deliberately.
- **Keep the og block single-line and image-early.** LinkedIn reads the page
  with a Range request; multi-line tags push `og:image` past the window.
- A feed autodiscovery `<link>` to `/feed.xml`
- JSON-LD: `Person` on the landing page, `BlogPosting` + `BreadcrumbList` on
  articles, `Blog` + `BreadcrumbList` on `/articles/`, `CollectionPage` +
  `BreadcrumbList` on `/projects/`. Every `author`/`publisher` carries
  `"@id": "https://edwardnjogu.com/#person"` so the pages describe one entity.
- A visible `.crumbs` breadcrumb matching the BreadcrumbList — Google prefers
  the schema to have an on-page counterpart.

Article dates live in three places that must agree: the visible
`<time datetime>`, `article:published_time`, and JSON-LD `datePublished` —
plus `<lastmod>` in `sitemap.xml` and `<pubDate>` in `feed.xml`.

**`/n8n-pro-tips/` is not under `articles/`.** Any sitewide head change has to
touch it explicitly or it silently falls behind — this has already happened
once.

## Design system

`DESIGN.md` is the spec. **One deliberate departure from it:** the accent is the
logo violet `#4d0da2`, not the `#1d4ed8` blue the spec inherited from its
Sosensus reference. A blue CTA beside a violet mark read as an accident. The
near-black is `#100b1c` (violet-cast) for the same reason. Do not "correct"
these back to blue.

Signature elements: the hero run-log panel (a mock n8n execution that stamps in
on load), ghost section numerals `01`–`06`, `// section.x` mono eyebrows, and
the dot grid — footer only.

Orange `--signal` is reserved for live/status indicators (the run-log dot,
`● BUILDING` chips). It is never a CTA.

## Local development

**Serve the folder — do not open files directly.** Subpages use root-absolute
paths, so `file://` resolves `/static/site.css` against your disk root and the
page renders unstyled.

```bash
python3 -m http.server 8000   # then http://localhost:8000
```

## Content rules

- **Do not invent metrics.** Client work is under NDA; the hero carries
  verifiable facts (stack, timezone, availability) rather than a fabricated
  stat row.
- **No phone number.** Removed deliberately — a phone number is a persistent
  identifier tied to 2FA and SIM-swap risk, unlike a rotatable email. This
  includes `wa.me` links, which publish the number just as plainly.
- Email is the only contact channel, plus GitHub and LinkedIn.

## Known issues / open work

- **No case studies.** The work section says "client automation work is under
  NDA — case studies in progress." This is the biggest remaining content gap.
- **No FAQ section.** `FAQPage` is the one schema type here that is both
  rich-result eligible and the shape answer engines quote most directly. It
  needs real answers to real client questions, so it is waiting on content,
  not markup.
- `ai-2024.html` is a 2024 predictions piece that reads dated on a site selling
  AI expertise.
- The hero promises a reply "within 1 working day" — a commitment, not a
  placeholder.
- **Orphaned assets**, tracked but unreferenced by any live page — delete
  together, since some only reference each other:
  - `static/styles.css` (the legacy stylesheet; the *only* thing still pointing
    at `static/welcome.jpg`, so both go together)
  - `static/grad.jpg`, `static/chatbot1.png`,
    `static/profile_app/njogued_blue.png`, `static/ed-site-logo.png`
    (the PNG logo — `ed-site.svg` is the live source and must stay)
  - all five `projects/static/*` screenshots, unused since project cards became
    text-only
  - all seven `articles/static/*` images (~1.1MB), left over from the removed
    cover banners
- No SPF/DMARC records on the domain, so `edwardnjogu.com` can currently be
  spoofed. Worth adding whether or not email is set up.
- `profile/Edward_Njogu_Resume.pdf` is deleted but remains in git history
  (commit `ee9ad8f`).

## Positioning context

Focus is **AI automation / n8n development** (production workflows, RAG
pipelines with pgvector, LLM + Supabase integrations), heading toward an **AI
implementation** specialization. The site was rewritten around this in July 2026.
Copy should stay first-person and concrete; avoid agency-speak. See
`SITE-ANALYSIS-AND-RECOMMENDATIONS.md` for the original gap analysis — its
section-by-section recommendations are now implemented.
