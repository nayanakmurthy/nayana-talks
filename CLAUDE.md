# CLAUDE.md — nayanamurthy.com

Nayana Keshava Murthy's portfolio and blog. Hugo + hugo-profile theme, deployed to
Cloudflare Workers at **https://nayanamurthy.com**.

---

## Working with Nayana (read this first)

**Nayana is not a developer.** She does not use VS Code, a terminal, or any code editor.
She works entirely by talking to Claude. This changes how you behave:

- **Never tell her to open a file, run a command, or edit YAML.** Do it yourself.
- **Explain in plain English before you change anything.** "I'll add ADEME to your
  certifications list, between CSRD Fundamentals and Double Materiality" — not a diff.
- **Never `git push` without an explicit yes.** Pushing to `main` is publishing. See
  [Deployment](#deployment).
- **Never leave the site broken.** Run `hugo --gc --minify` after any edit to
  `hugo.yaml`, `layouts/`, or `static/style.css`. One bad indent takes the whole site down.
- **Show, don't describe.** When she asks how something looks, start the preview
  (`/preview-site`) rather than explaining it.
- If something goes wrong, fix it and tell her plainly. Don't hand her a stack trace.

---

## Architecture

| | |
|---|---|
| Generator | Hugo **extended** v0.153.1 (version pinned in `build.sh`) |
| Theme | [hugo-profile](https://github.com/gurusabarish/hugo-profile), a **git submodule** at `themes/hugo-profile` |
| Portfolio content | **All of it** lives in `hugo.yaml` under `params:` |
| Blog posts | `content/blogs/*.md` |
| Host | Cloudflare Workers (`wrangler.toml` → `build.sh`) |
| Timezone | Europe/Vienna. `+01:00` in winter (CET), `+02:00` in summer (CEST). |
| Spelling | **British.** organisation, analyse, realise, programme, centre. |

There is no `data/` directory and no per-section YAML files. Unlike most Hugo portfolios,
every section of this site (hero, about, experience, education, achievements, volunteering,
contact) is configured in the one 419-line `hugo.yaml`.

---

## File map

| To change... | Edit | Roughly at |
|---|---|---|
| Site title, domain, language | `hugo.yaml` | lines 1–4 |
| Nav bar menu items | `hugo.yaml` `Menus:` | 31–54 |
| Nav bar behaviour, which sections show | `hugo.yaml` `params.navbar` | 112–131 |
| Hero (name, tagline, intro text, photo, resume button, social links) | `hugo.yaml` `params.hero` | 133–156 |
| About text, skills list | `hugo.yaml` `params.about` | 158–185 |
| Jobs and roles | `hugo.yaml` `params.experience` | 187–237 |
| Degrees and qualifications | `hugo.yaml` `params.education` | 239–279 |
| Certifications | `hugo.yaml` `params.achievements` | 281–300 |
| Volunteering | `hugo.yaml` `params.volunteering` | 302–315 |
| Projects (currently `enable: false`) | `hugo.yaml` `params.projects` | 317–365 |
| Contact blurb | `hugo.yaml` `params.contact` | 367–378 |
| Footer, recent posts count | `hugo.yaml` `params.footer` | 380–389 |
| Blog post display, read time, share buttons | `hugo.yaml` `params.singlePages` | 395–404 |
| Site colours | `hugo.yaml` `params.color` (commented out at 89–110, uncomment to use) |
| A blog post | `content/blogs/<slug>.md` | |
| Photo gallery | `content/gallery.md` | |
| Custom styling | `static/style.css` | |
| Section HTML | `layouts/partials/sections/` | |
| Which sections appear, and in what order | `layouts/index.html` | |
| Images | drop in `static/images/`, reference as `/images/name.jpg` | |
| Resume PDF | replace `static/Nayana-Resume.pdf` (keep the filename) | |

Line numbers drift as the file is edited. Confirm with `grep -n` before an edit rather
than trusting the table.

---

## Deployment

```
git push origin main  →  Cloudflare Workers runs build.sh  →  live in ~2 minutes
```

`build.sh` installs Dart Sass, Go, Hugo extended and Node from scratch on every build, then
runs `hugo --gc --minify`. Output goes to `public/` (gitignored).

**A push is a publish.** Always confirm with Nayana first. Use `/publish-site`, which
builds, shows her exactly what will go live, and asks.

To roll back a bad deploy: `git revert HEAD` then push. The site returns to its previous
state in about two minutes.

---

## The Obsidian vault

`content/blogs/.obsidian/` is a real Obsidian vault with the **obsidian-git** plugin
installed. This is how Nayana writes posts by hand. The plugin auto-commits with messages
like `Update blog: 24-12-2025 13:43:32` — every recent commit in this repo came from it.

- Don't delete it, don't "clean it up", don't reorganise `content/blogs/`.
- `content/blogs/hugo.yaml` is a **symlink** to the real `hugo.yaml` at the repo root, so
  Obsidian can see it. Leave it alone; it's gitignored.
- **Before you commit anything, run `git status`.** Obsidian may have already staged or
  committed her edits. Never blow away her uncommitted work.

---

## Known state and gotchas

- **`themes/hugo-profile` may be empty.** It's a submodule that isn't always initialised
  locally. Local preview will fail with a missing-theme error until you run
  `git submodule update --init --recursive`. The live site is unaffected: Cloudflare
  fetches the submodule itself.
- **Hugo may not be installed on this Mac.** `/preview-site` handles installing it.
- **`content/blogs/building-your-portfolio-with-hugo.md` is authored by Puneeth Prakash**,
  Nayana's husband, and it is deliberately published on her site. **Do not remove or
  re-attribute it.** It is not a mistake and not a template for her voice.
- That post also carries a `menu.sidebar` frontmatter block. That's a Toha-theme feature
  from his site; it does nothing here. **Don't copy it into new posts.**
- `content/blogs/Template/blog-template.md` is a hand-written starter template. It will
  render as a page if it ever loses `draft: true` status. Prefer `archetypes/blogs.md`.
- `params.projects` is `enable: false` and still contains the theme's demo content
  (Hugo Profile, Image Converter). Not Nayana's work. Clear it before ever enabling it.
- `params.mathjax: false` but `static/images/mathjax.png` exists. Unused.
- `content/gallery.md` is full of stock-photo placeholder URLs from the theme demo.

---

## Skills

Four skills cover everything Nayana does. Use them rather than improvising.

| Skill | For |
|---|---|
| `/write-blog` | Turn a topic plus a brain-dump into a finished post in her voice |
| `/edit-portfolio` | Any change to the portfolio itself, described in plain English |
| `/preview-site` | See the site locally before anything goes live |
| `/publish-site` | The only thing that touches the live site |

There is also an `esg-researcher` subagent for checking current ESG regulatory facts.
`/write-blog` calls it automatically when a post makes regulatory claims.

**Her writing voice is documented in `.claude/skills/write-blog/voice.md`.** Read it before
writing *any* prose that appears on the site, including About text, section blurbs and
contact copy, not just blog posts. Most important rule: **no em dashes, ever.**

---

## About Nayana (for content generation)

**ESG & Sustainability Professional.** Based in Klaus, Vorarlberg, Austria.
Contact: nayanakeshavamurthy@gmail.com | LinkedIn: [@nayanakmurthy](https://linkedin.com/in/nayanakmurthy)

**Currently:** Sustainability Consultant at The Bartan Company (Oct 2025 – present).
Looking for ESG consulting, carbon accounting or sustainability reporting roles in the DACH
region.

**Qualifications:** MSc Global Strategy and Sustainability, University of Edinburgh
(Distinction, 2025) · Chartered Accountant, ICAI (2021) · B.Com International Business,
St Joseph's College of Commerce (8.6) · CSRD certified

**Career path:** PwC India (SAP FI/CO consulting, 2021–23) → IiAS (ESG/governance analyst,
150+ listed companies, 2023–24) → Balu and Anand CA (audit manager, 2024) → Edinburgh MSc →
South of Scotland Enterprise (Net Zero framework for tourism SMEs, 2025) → The Bartan
Company (event sustainability reporting).

The unusual thing about her background: **she came to sustainability through accounting.**
Chartered Accountant first, ESG second. That combination — audit rigour applied to
sustainability data — is her actual differentiator and the honest hook for a lot of her
writing.

**Expertise:** GRI, BRSR, CSRD/ESRS, GHG Protocol (Scope 1/2/3), double materiality,
corporate governance scorecards, ESG ratings, carbon accounting, Excel, Tableau.

**Volunteering:** SustainaPod events team (podcast, 10,000+ listeners) · U&I Trust
2020–2024, maths teacher then leader of 40+ volunteers across 3 centres, mentoring 100+
young people aged 14–18. She has volunteered since she was 10.

**Outside work:** badminton, volleyball, football, squash (the current obsession),
knitting, mystery novels, learning photography, making traditional Indian dolls, one
ill-advised attempt at surfing.

---

## Commands

```bash
git submodule update --init --recursive   # fetch the theme (needed once)
hugo server -D                            # local preview, drafts visible, port 1313
hugo --gc --minify                        # production build, use this to check for errors
```
