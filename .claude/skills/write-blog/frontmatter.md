# Frontmatter, slugs, tags

This site runs the **hugo-profile** theme. Frontmatter here is not the same as on Puneeth's
site, which runs Toha. Don't copy his.

---

## The block

```yaml
---
title: 'Sentence case title, no full stop'
date: 2026-07-26T09:00:00+02:00
draft: true
author: 'Nayana Keshava Murthy'
description: 'One sentence, 140 to 160 characters, that makes someone want to read it.'

tags: ['CSRD', 'ESRS', 'Sustainability Reporting', 'Double Materiality']
categories: ['ESG & Reporting']

toc:
  enable: true
---
```

That's the whole thing. Nothing else.

**`draft: true` always.** `/publish-site` flips it. You don't.

---

## Field rules

**`title`** — Sentence case. "What double materiality actually means", not "What Double
Materiality Actually Means". No full stop. Colons are fine. Under about 65 characters or it
wraps badly in the post list.

**`date`** — Full ISO timestamp with the Austrian offset:
- **`+02:00`** from late March to late October (CEST, summer)
- **`+01:00`** from late October to late March (CET, winter)

Today is in summer time. Use the real current date and a sensible hour like `09:00:00`.
Getting the offset wrong shifts the displayed date by a day at the boundaries.

**`author`** — Always exactly `'Nayana Keshava Murthy'`.

**`description`** — This is the SEO meta description *and* the preview text on the blog
list page. Write it as a hook, not a summary. 140 to 160 characters. No em dashes here
either.

**`tags`** — 3 to 6. Title Case. Use the vocabulary below; only invent a new tag when
nothing fits, and add it to this file when you do.

**`categories`** — Exactly one, from the fixed list below. Never invent a category.

**`toc.enable`** — `true` for anything over about 1,000 words, `false` for short opinion
pieces where a contents box on a 700-word post looks silly.

**`image`** — Only if the file actually exists in `static/images/`. Reference it as
`/images/name.jpg`. If there's no image, leave the field out entirely rather than pointing
at nothing.

---

## Do not include

- **`menu.sidebar`** — a Toha-theme block. It appears in
  `content/blogs/building-your-portfolio-with-hugo.md` because that post came from
  Puneeth's site. It does nothing on hugo-profile. Don't copy it.
- `weight`, `identifier`, `hero`, `summary`, `slug` — not needed. The filename is the URL.

---

## Categories (pick exactly one)

| Category | For |
|---|---|
| `ESG & Reporting` | CSRD, ESRS, GRI, BRSR, disclosure, assurance, standards |
| `Carbon & Climate` | GHG Protocol, Scope 1/2/3, carbon accounting, Net Zero, transition plans |
| `Career & Learning` | Her own path, the CA to ESG move, Edinburgh, job-hunting in the DACH region |
| `Opinion` | Arguments and positions. Greenwashing, ratings, policy rollbacks. |
| `How-To` | Step-by-step things she's actually built or run |

Five is enough. Resist adding a sixth unless a post genuinely has nowhere to go.

---

## Tag vocabulary

Reuse these. A tag used once is a dead end.

**Standards and frameworks:** `CSRD` `ESRS` `GRI` `BRSR` `ISSB` `SFDR` `CSDDD`
`EU Taxonomy` `GHG Protocol` `TCFD`

**Topics:** `Sustainability Reporting` `Carbon Accounting` `Double Materiality`
`Scope 3` `Net Zero` `ESG Ratings` `Corporate Governance` `Greenwashing` `Assurance`
`ESG Data` `Materiality Assessment` `Climate Risk`

**Career and personal:** `Career Change` `Chartered Accountancy` `Edinburgh`
`Working in Austria` `Job Hunting` `Learning` `Volunteering`

**Practical:** `Excel` `Data Quality` `Auditing` `Tools`

---

## Filenames and URLs

The filename is the URL. `content/blogs/what-double-materiality-means.md` becomes
`nayanamurthy.com/blogs/what-double-materiality-means/`.

- Lowercase, hyphens, no dates in the filename, no underscores.
- 3 to 6 words. Shorter than the title is fine and usually better.
- Descriptive, not clever. Someone should know what it is from the URL alone.
- **Never rename a published post's file.** That breaks the live URL and anything linking
  to it. If she wants a different title, change `title:` and leave the filename.

Good: `csrd-omnibus-what-changed.md` · `ca-to-esg.md` · `reading-a-sustainability-report.md`

Bad: `2026-07-26-post.md` · `my-thoughts-on-the-corporate-sustainability-reporting-directive-changes.md`

---

## Where it goes

`content/blogs/<slug>.md`. Flat, no subfolders.

Don't put anything in `content/blogs/Template/` (a hand-written starter she keeps) or touch
`content/blogs/.obsidian/` (her Obsidian vault) or `content/blogs/hugo.yaml` (a symlink to
the root config).

---

## Checking it renders

Frontmatter errors show up as a Hugo build failure, not a broken page. After saving:

```bash
hugo --gc --minify
```

If that passes, the frontmatter is valid. `/preview-site` shows drafts.
