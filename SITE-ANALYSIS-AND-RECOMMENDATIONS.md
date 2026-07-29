# Landing Page: Analysis & Recommendations

_Prepared July 2026. Based on the current `index.html`, the 2026 resume, and
Edward's stated strategic goals (AI implementation positioning, authority
building via LinkedIn/Substack, case studies and testimonials)._

## The core problem

The site still sells the 2023 version of Edward. The About section leads with
"a hybrid background in software development, executive support, and process
automation," and the hero has no tagline at all. Meanwhile the 2026 resume and
current client work tell a sharper story: **an AI automation / n8n developer
moving toward AI implementation** (production n8n workflows, RAG pipelines with
pgvector, LLM + Supabase + API integrations).

Everything below moves the site from "generalist developer/assistant" toward
"AI automation specialist," which is also directly aligned with the strategic
goal of positioning as an AI implementation expert.

---

## Section-by-section analysis

### 1. Hero — highest impact
**Now:** "Hi, I AM EDWARD NJOGU." The subtitle (`<h4 class="title">`) is
commented out, so there is no statement of what he does.
**Recommend:** Add a clear tagline, e.g. _"AI Automation & n8n Developer"_ or
_"I build AI-powered automation systems."_ Also update the page `<title>` (now
"Edward Njogu Portfolio Page") to something SEO-friendly like
_"Edward Njogu — AI Automation & n8n Developer."_

### 2. About
**Now:** Leads with executive support and generic software development.
**Recommend:** Rewrite to lead with AI. Pull from the resume: designing
production n8n workflows, RAG pipelines with pgvector, integrating LLMs with
Supabase, Slack, and APIs. Keep the ops/automation background as supporting
context, not the headline.

### 3. Skills — most dated section
**Now:** Python, JavaScript, HTML, CSS, MySQL, Django, Flask, ExpressJS,
MongoDB, DevOps — each shown with a **percentage progress bar**.
**Problems:** (a) The entire current stack is missing. (b) Percentage bars read
as amateurish and actively undersell ("60% good at CSS").
**Recommend:** Replace the progress bars with a clean tag/pill layout, grouped
by theme, and add the current stack:
- **AI & Automation:** n8n, LLM integration, RAG, vector databases (pgvector),
  prompt engineering, workflow automation, Zapier
- **Development:** Python, JavaScript, Django, Flask, Express, APIs
- **Data & Tools:** Supabase, PostgreSQL, MongoDB, MySQL, Docker/DevOps

### 4. Projects
**Now:** Servlist, ServiceMtaani, Portfolio Website, Other — older student-era
projects.
**Recommend:** Add at least one or two AI automation **case studies** (can be
anonymized), e.g. "20+ production n8n workflows for a growth team" or "RAG
content pipeline." Case studies are explicitly strategic objective #1. Keep the
older projects but move them lower or into "Other."

### 5. Services
**Now:** "Software Development / Process Automation / Executive Support."
**Recommend:** Reframe around what Edward wants to sell:
_"AI Automation & n8n Development," "RAG & AI Implementation," "Workflow & Ops
Automation."_ De-emphasize executive support as a headline service.

### 6. Articles / authority
**Now:** Featured article is "Top Three AI Technologies ... 2025," which now
reads as dated.
**Recommend:** Move the n8n self-hosting guide to the front (on-brand). Add a
"Newsletter" or "Writing" call-to-action tied to the LinkedIn/Substack authority
strategy (free learning resources = strategic objective #1).

### 7. Cleanup — small changes, big "is this maintained?" signal
- Footer reads "© 2025" → update to 2026 or make it dynamic.
- Contact section has a "TBD" placeholder card → remove or fill.
- The floating chat widget always replies "still under development." For an AI
  specialist, a broken AI chatbot works against you: either wire it to a real
  assistant or remove it.
- `sitemap.xml` `lastmod` is stale (2023-12-15) and omits newer articles.
- Commented-out contact `<form>` still in the markup.
- `.DS_Store` files committed across folders.

---

## Suggested priority order

1. **Hero tagline + page `<title>`** (positioning, SEO) — highest impact,
   lowest effort.
2. **About rewrite** — lead with AI.
3. **Skills overhaul** — drop progress bars, add current stack.
4. **Services reframe.**
5. **Projects: add AI case studies.**
6. **Articles reorder + newsletter CTA.**
7. **Cleanup items** (copyright, TBD card, chat widget, sitemap).

---

## Edward's additional recommendations

Concrete cleanup/design items provided by Edward (mostly front-end polish):

1. **Logo** — the current navbar logo (`static/profile_app/njogued_blue.png`)
   isn't working. Remove it or swap for a clean placeholder (e.g. a simple "EN"
   monogram or wordmark) until a real logo exists.
2. **Remove the graduation photo** — drop the `static/grad.jpg` image in the
   About section.
3. **Expand the tech stack** — add the tools actually in use now: n8n, Claude,
   Supabase, GitHub, plus the rest of the current stack (see Skills section
   above). Don't limit to the old languages/frameworks list.
4. **Remove the skill percentage bars** — replace the progress-bar UI with a
   plain tag/label layout (reinforces the Skills recommendation above).
5. **Lighten projects & articles** — the cover images are too heavy (and slow /
   don't reliably load). Drop the images from the projects and articles lists;
   render them as simple text cards instead.
6. **Visual cleanup** — remove the black card borders throughout, remove the
   floating chat bot button in the bottom-right, and fix the broken/slow-loading
   images (root cause of #5).

## Design system note

Item 7 referenced a `DESIGN.md` to govern front-end revisions. The `DESIGN.md`
currently in the repo belongs to a **different project** ("Sosensus," a
Next.js/React/shadcn app) and does not apply to this static Bootstrap site.
Before front-end work begins we need either the correct portfolio `DESIGN.md`
or a new one authored from scratch (palette, typography, card/border style,
spacing) consistent with the changes above.
