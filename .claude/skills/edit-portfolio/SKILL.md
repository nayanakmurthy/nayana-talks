---
name: edit-portfolio
description: Make changes to Nayana's portfolio site at nayanamurthy.com. Use when she wants to add or update a job, add a certification or volunteering entry, change her hero text or photo, rewrite her About section, update the contact blurb, show or hide a section, change colours, replace her resume, add images, or fix anything on the site. Handles the plain-English request end to end, verifies the site still builds, and stops short of publishing.
---

# Edit the portfolio

Nayana describes a change in plain English. You make it, prove the site still builds, and
show her. **You do not publish.** That's `/publish-site`, and only when she says so.

## The one thing to remember

She is not a developer. Never tell her to open a file or run a command. Never show her raw
YAML unless she asks. Describe what you're about to change the way you'd describe it to a
colleague, make the change, and show her the result.

---

## Sequence (don't skip steps)

1. **Say what you're going to do**, in plain English, before touching anything.
   > "I'll add The Bartan Company role above South of Scotland Enterprise, with the two
   > bullet points you gave me. Your other jobs stay as they are."

   For anything bigger than a sentence-level tweak, wait for a yes.

2. **Find the exact spot.** `grep -n` for it. Don't trust the line numbers in `CLAUDE.md`,
   they drift.

3. **Make the edit.** Match the surrounding style exactly: same quote style, same
   indentation, same bullet punctuation.

4. **Prove it builds.**
   ```bash
   hugo --gc --minify
   ```
   If that fails, fix it before saying anything. A YAML indentation error takes the whole
   site down.

5. **Offer to show her.** `/preview-site`. For anything visual, don't ask, just start it.

6. **Stop.** Nothing is live. Say so explicitly, and tell her `/publish-site` is what makes
   it live.

---

## Where everything lives

Almost the entire portfolio is in **one file**: `hugo.yaml`, under `params:`. There are no
per-section data files on this site.

| She says | You edit |
|---|---|
| "add my new job" / "update my role" | `params.experience.items` |
| "add a certification" / "I did a new course" | `params.achievements.items` |
| "add this volunteering" | `params.volunteering.items` |
| "change my tagline" / "the text at the top" | `params.hero` (`subtitle`, `content`) |
| "change my main photo" | `params.hero.image` |
| "rewrite my About" | `params.about.content` |
| "add a skill" | `params.about.skills.items` |
| "add my degree" / "education" | `params.education.items` |
| "change the contact bit" | `params.contact.content` |
| "hide the projects section" | `params.projects.enable` **and** `params.navbar.menus.disableProjects` |
| "add a LinkedIn/social link" | `params.hero.socialLinks` and `params.footer.socialNetworks` |
| "update my CV" | replace `static/Nayana-Resume.pdf`, keep the filename |
| "change the colours" | `params.color` (currently commented out, lines ~89–110) |
| "show more recent posts in the footer" | `params.footer.recentPosts.count` |
| "change the site name in the tab" | `title:` line 3, and `params.navbar.brandName` |
| "the gallery" | `content/gallery.md` |
| Spacing, fonts, sizes, anything visual not in config | `static/style.css` |
| Section HTML structure | `layouts/partials/sections/` |
| Which sections appear and in what order | `layouts/index.html` |

Full structure and line ranges: `CLAUDE.md`.

---

## Recipes

### Add a job

`params.experience.items` is a list of **companies**, each with a `jobs` list. Newest
company first. If she's been promoted at the same company, add a second entry to that
company's existing `jobs` list rather than duplicating the company.

```yaml
      - company: 'Company Name'
        companyUrl: 'https://example.com'      # use '#' if there isn't one
        jobs:
          - name: 'Job Title'
            date: 'October 2025 - Present'      # or 'Jan 2025 - April 2025'
            content: |
              - First achievement, with a number in it if there is one.
              - Second achievement.
```

Two-space indent per level, `content: |` block, bullets start with `- `. Her existing
bullets are written in a specific style: action verb, what she did, quantified outcome.
Match it. Read [../write-blog/voice.md](../write-blog/voice.md) before writing any prose.

### Add a certification

`params.achievements.items`:

```yaml
      - title: Certification Name
        content: One or two sentences, ending with the month and year in brackets (June 2026).
        image: /images/cert-name.png
```

The image must already exist in `static/images/`. If she hasn't given you one, ask for it
or omit the `image:` line rather than pointing at a file that isn't there.

### Add volunteering

`params.volunteering.items`, same shape as achievements but with longer, warmer `content`.
Look at the two existing entries: they're first-person mini-stories, not bullet points.
Match that.

### Change the hero

```yaml
  hero:
    intro: "Hi, I'm"
    title: 'Nayana Keshava Murthy'
    subtitle: 'ESG & Sustainability Professional'
    content: "The paragraph under her name."
    image: /images/hero.jpeg
```

`static/style.css` forces the hero title onto one line at large screen sizes. A much longer
title will need that CSS adjusting too.

### Show or hide a section

Two switches, and **both** need setting or the nav bar links to a section that isn't there:

```yaml
  projects:
    enable: false          # renders the section
  navbar:
    menus:
      disableProjects: true   # the nav bar link
```

Sections available: About, Experience, Education, Achievements, Volunteering, Projects,
Contact.

### Change the colours

`params.color` is commented out at roughly lines 89–110, with both light and dark mode
blocks. Uncomment the whole block and change the hex values. Every hex needs quotes,
because `#` starts a comment in YAML:

```yaml
    textColor: "#343a40"     # correct
    textColor: #343a40       # breaks: YAML reads this as an empty value
```

Change light and dark together, and always preview both.

### Add an image

Put the file in `static/images/`, reference it as `/images/filename.jpg` (leading slash, no
`static/`). Lowercase filenames, hyphens, no spaces. Check the file is actually there
before referencing it.

### Replace the resume

Overwrite `static/Nayana-Resume.pdf`, keeping that exact filename. The hero button and any
external links point at it.

---

## YAML rules that will bite you

`hugo.yaml` is 419 lines and one mistake breaks the entire site.

1. **Two-space indentation.** Never tabs. Match the surrounding block exactly.
2. **Quote anything with `#`, `:`, `&`, or a leading `*`.** Hex colours especially.
3. **Apostrophes inside single quotes must be doubled**, or use double quotes:
   `'3+ years'' experience'` or `"3+ years' experience"`. Her `params.description` already
   does this.
4. **`|` keeps line breaks, `|-` strips the trailing newline.** She uses both. Keep
   whichever the block already uses.
5. **List items line up.** A `- company:` at the wrong indent silently attaches to the
   wrong parent.
6. **Never reformat the file.** No re-quoting, no re-indenting, no reordering keys, no
   "tidying". Change only what she asked for, so the git diff stays readable.
7. **Build after every edit.** Always.

---

## Undoing things

She might say "undo that", "put it back", or "I liked it how it was".

**Not yet pushed:**
```bash
git diff                    # see what changed
git checkout -- hugo.yaml   # throw away the change
```

**Already published:**
```bash
git log --oneline -10       # find it
git revert <hash>           # then push to redeploy
```

Before discarding anything, run `git status` first. Her Obsidian vault auto-commits blog
edits, and you must not throw away her writing while reverting a config change. If both are
in play, discard only the specific file.

Tell her plainly what you undid and what state the site is in now.

---

## Things not to do

- Don't touch `content/blogs/` from this skill. Blog posts are `/write-blog`.
- Don't delete `content/blogs/building-your-portfolio-with-hugo.md`. It's Puneeth's post,
  on her site on purpose.
- Don't enable `params.projects` without clearing it first: it still holds the theme's demo
  content (Hugo Profile, Image Converter) which is not her work.
- Don't edit files inside `themes/hugo-profile/`. It's a submodule and changes there are
  lost on rebuild. Override in `layouts/` or `static/style.css` instead.
- Don't upgrade the theme, Hugo, or anything in `build.sh` unless she asks. It works.
- Don't commit or push. Ever, from this skill.
