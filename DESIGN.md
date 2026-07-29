# DESIGN.md

Front-end design reference for the **njogued.github.io** portfolio site.
**Read this before touching any UI.** Use it for all styling decisions so the
pages read as one system.

The design language is inferred from the "Sosensus" landing page
(`Sosensus — the social intelligence layer for your team.html`, kept in the repo
as a visual reference only — it is not part of the published site), and refined
with a computed-styles / DOM pass. Sosensus is a Next.js/React/Tailwind app; this
portfolio is **static HTML + Bootstrap 5 + vanilla JS**, so the tokens below are
translated to plain CSS variables in `static/styles.css`, not shadcn/Tailwind
theme tokens.

`CLAUDE.md` covers repo structure and conventions; this file covers everything
visual.

---

## Design direction

**Technical editorial / Swiss-brutalist.** Warm, near-monochrome, square, and
shadowless. Everything is built from **type, generous whitespace, and hairline
borders** — not color, imagery, or elevation. The page reads as alternating
horizontal bands (white → warm paper → white → near-black).

The distinguishing move is a **two-voice typographic system**: a tight, heavy
geometric sans (Geist) for statements, and a monospace (Geist Mono) for
*metadata* — section labels written as code comments (`// section.problem`),
stats with mono captions, version strings in the corners.

This is a deliberate departure from the old site: retire Poppins, drop the
royal-blue theme (`#1948c9`/`#2f56c1`), remove heavy cover images, black card
borders, drop shadows, and skill progress bars.

Three rules carry most of the aesthetic:

1. **No shadows, ever.** Borders instead of elevation.
2. **Every section gets an eyebrow label and (optionally) a ghost numeral.**
3. **Mono for any number, label, status, or path; sans for any sentence meant to
   persuade.**

---

## Color tokens

Define on `:root` in `static/styles.css` and reference everywhere. Stop
hardcoding the old brand blues.

```css
:root {
  --background: #ffffff;
  --foreground: #0a0a0a;   /* headings, near-black */
  --card: #ffffff;
  --primary: #0a0a0a;
  --primary-foreground: #ffffff;
  --secondary: #f5f3ee;    /* warm paper band background */
  --muted: #f5f3ee;
  --muted-foreground: #8a8580;  /* mono captions, meta */
  --body-text: #4a4744;         /* paragraph grey (most-used) */
  --accent: #ede9e0;            /* hairlines, ghost numerals */
  --border: #ede9e0;
  --input: #d8d4cc;             /* card borders (stronger hairline) */
  --hairline-dashed: #c8c5be;   /* dashed rules on stat/terminal rows */
  --link: #1d4ed8;              /* primary blue — links + primary CTA */
  --signal: #ea580c;            /* orange accent, sparing */
  --destructive: #c2410c;
  --ring: #0a0a0a;
  --radius: 2px;
  /* dark band */
  --dark-bg: #0a0a0a;
  --dark-elevated: #1a1a1a;
  --dark-border: #2a2a2a;
  --dark-muted: #c8c5be;
  --dark-accent-soft: #93b5f0;
}
```

The palette is deliberately narrow: near-black, warm paper, one blue, one orange.
**Blue** is the primary action + link color. **Orange** is reserved for "signal"
moments — live indicators, badges, index numbers, eyebrow labels on dark
sections, left rules on highlight cards. Never use blue and orange as CTAs in the
same block. Do **not** reintroduce `#1948c9` / `#2f56c1`.

---

## Typography

**Fonts:** Geist for everything expressive, Geist Mono for all metadata —
**site-wide** (Poppins is retired). The reference app ships local woff2 files;
this static site does not have them, so load Geist + Geist Mono from a CDN
(Google Fonts serves both) and replace the Poppins `<link>` in both
`static/styles.css` and `articles/static/styles.css`. Fallbacks:
`'Geist', system-ui, -apple-system, sans-serif` and
`'Geist Mono', ui-monospace, monospace`.

The scale is compressed with aggressively negative tracking at the large end.
Headings are weight **600**, not 700 — the tight tracking does the heavy lifting.

| Role | Font | Size | Weight | Line height | Tracking |
|---|---|---|---|---|---|
| Ghost numeral | Geist | 220px | 600 | 0.85 | −0.04em, color `--accent` |
| Display XL | Geist | 96px | 600 | 0.95 | −0.035em |
| Display L | Geist | 80px | 600 | 0.96 | −0.035em |
| Hero | Geist | 64px | 600 | 1.00 | −0.03em |
| Section H2 | Geist | 56px | 600 | 1.00 | −0.03em |
| H3 | Geist | 28px | 600 | 1.5 | −0.02em |
| Card title | Geist | 19–22px | 600 | 1.25–1.5 | −0.01em |
| Lead paragraph | Geist | 18px | 400 | 1.5 | — |
| Body | Geist | 15–16px | 400 | 1.45–1.55 | color `--body-text` |
| Small body | Geist | 13–14px | 400 | 1.5–1.65 | — |
| Mono label / eyebrow | Geist Mono | 13px | 500 | — | 0.02em, normal case |
| Mono micro | Geist Mono | 11–12px | 500 | — | 0.04em, `--muted-foreground` |
| Mono stat | Geist Mono | 32px | 500 | — | numeral + colored unit suffix |
| Footer col header | Geist Mono | 11px | 500 | — | wide tracking, UPPERCASE |

**Caveat:** Geist's tight geometric forms are what make −0.03em tracking work. If
Geist ever fails to load and a system stack is used, ease heading tracking to
~−0.02em or headlines will look cramped.

---

## Shape, borders, spacing

- **Radius:** `--radius: 2px` (near-square, essentially never visible). Circles
  (`50%`) only for status dots / traffic-light dots.
- **Borders:** structural cards `1px solid var(--input)` (`#d8d4cc`); lighter
  dividers `1px solid var(--border)` (`#ede9e0`); dashed rows
  `1px dashed var(--hairline-dashed)` (`#c8c5be`). Use `1px solid var(--fg)` only
  for a deliberate high-emphasis edge. **Never** the old `border: solid 1px black`.
- **Shadows:** none. Structure comes from borders + whitespace only. Remove the
  navbar's heavy blue box-shadow.
- **Spacing:** 8px base with a large-step rhythm. Section padding `120px`
  vertical / `56px` horizontal; hero `88px / 96px`; thin band strips (logo wall)
  `32px`. Content max width ~`1328px`.
- **Grid gaps by column count:** 4-col → `24px`, 3-col → `32px`, 2-col →
  `56–64px`. Hero is an asymmetric split (~1.26fr / 1fr) with a `64px` gap.
- **Breakpoints:** essentially one — `max-width: 600px` — plus
  `prefers-reduced-motion`. Desktop-and-mobile, no tablet tier.

---

## Signature visual devices

**Ghost section numerals** — the strongest identity cue. An absolutely-positioned
`01`–`08` at 220px in `--accent` (`#1a1a1a` on dark bands), anchored to the right
edge and bleeding past the heading.

**Dot grid** — behind the hero, dark sections, and footer:

```css
background-image: radial-gradient(circle, #ede9e0 1.2px, transparent 1.2px);
background-size: 24px 24px;
/* dark variant: radial-gradient(circle, #1a1a1a 1.2px, transparent 1.2px) */
```

**Eyebrow labels** — precede every section heading. Mono, 13px, ~0.26px
tracking, prefixed with `//` in dot-notation (`// section.pricing`,
`// system.start()`). Blue or grey on light bands, orange on dark.

**Dashed hairlines** — `1px dashed #c8c5be`, separate stat rows and terminal
footers, distinguishing them from solid structural borders.

**Alternating bands** — sections alternate white → paper (`#f5f3ee`) → white →
near-black (`#0a0a0a`) for rhythm.

---

## Component rules (maps to the cleanup list)

**Nav / logo** — static, not sticky, no backdrop blur, no shadow. Mono 13px
links in `--body-text`, active item near-black. The current `njogued_blue.png`
logo is broken/off-brand — use a plain text wordmark (`njogued` or `EN`) in Geist
Mono until a real mark exists. CTA is a small solid near-black square button
(`10px 18px`).

**Buttons** — all zero-radius (2px token), no shadow, transition
`all .3s cubic-bezier(.4,0,.2,1)`:

| Variant | Style |
|---|---|
| Primary CTA | bg `#1d4ed8`, white, 16px / w500, padding `18px 32px` |
| Secondary | transparent, `#0a0a0a`, `1px solid #0a0a0a`, padding `17px 30px` |
| Small mono | bg `#0a0a0a`, white, mono 13px, padding `10px 18px` |
| On dark | bordered variant uses `1px solid #2a2a2a` |

> Correction from the first draft: the large **primary CTA is blue**, not
> near-black. Near-black is the small compact mono button.

**Cards (projects, articles, services)** — text-only, no cover images. Default:
white, `1px solid #d8d4cc`, 2px radius, `36px 32px` padding. Title Geist 600,
optional one-line description in `--body-text`, optional mono tag row.
Highlight/"problem" cards drop the border for a `#f5f3ee` fill + `3px` orange
left rule, `28px 24px` padding. Feature cards can **share hairlines** to form a
single table-like grid rather than separate objects — cheap and deliberate. This
replaces the heavy image cards and their black borders.

**Skills** — no progress bars. Render as **mono pills/tags** grouped by theme:

- _AI & Automation_ — n8n, Claude, LLM integration, RAG, pgvector, prompt
  engineering, Zapier
- _Development_ — Python, JavaScript, Django, Flask, Express, APIs
- _Data & Infra_ — Supabase, PostgreSQL, MongoDB, GitHub, Docker

Pills: `--secondary` fill or transparent with `1px solid var(--input)`, 2px
radius, Geist Mono ~12px.

**Stat blocks** — 32px mono numeral with the unit suffix (`×`, `h`, `%`, `s`)
colored blue, over a two-line mono caption at 11–12px in `--muted-foreground`.

**Terminal / code panel** (optional, for an AI/automation showcase) — `#0a0a0a`
shell with `#1a1a1a` inset, mono 12–13px, three circular traffic-light dots (the
only place `border-radius: 50%` appears), mono filename left + status right,
orange live dot, dashed rule above the footer row. Syntax: blue keys, orange
strings/accent, white values.

**Footer** — dark with the dot grid, a brand blurb column plus link columns under
mono uppercase 11px wide-tracked headers, closed by a mono meta row (copyright
left, version/uptime right). Update the year from `© 2025`.

**Removed elements** — the floating chat-bot button (`.chat-icon` + modal), the
graduation photo (`static/grad.jpg`), all cover images in project/article lists,
the "TBD" contact placeholder card, and the commented-out contact form.

---

## Images

The reference design uses essentially no imagery — it's type and layout. Lean the
same way: remove the heavy, slow/failing project/article covers, keep any
remaining images small and lazy-loaded.

---

## Open design work

- Load Geist + Geist Mono via CDN; retire Poppins across `static/styles.css` and
  `articles/static/styles.css`.
- Introduce the token block above on `:root`; replace hardcoded blues.
- Rebuild Skills from progress bars → grouped mono pills.
- Convert project/article/service cards to text-first, hairline-bordered cards.
- Add eyebrow labels + ghost numerals to sections; add the dot-grid to hero/footer.
- Keep `articles/static/styles.css` in sync with these tokens.
