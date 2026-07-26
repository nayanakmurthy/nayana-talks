---
name: write-blog
description: Write a blog post for nayanamurthy.com in Nayana's own voice. Use when she gives a topic, a brain-dump of notes, a voice-memo transcript, a document, or a rough idea and wants it turned into a publishable post. Also use when editing or rewriting an existing post to sound more like her, or when writing any prose that appears on the site.
---

# Write a blog post

Turn whatever Nayana gives you — a topic, messy notes, a transcript, a PDF, or one sentence
— into a finished post at `content/blogs/<slug>.md`, in her voice.

**You save it as a draft. You never publish.** Publishing is `/publish-site`, and only when
she says so.

## Before you start

Read [voice.md](voice.md) in full. Every time, even if you think you remember it. The
`## Learned corrections` section at the bottom changes as she uses this, and it overrides
everything else.

Then read whichever of these apply:
- [post-types.md](post-types.md) — pick the shape of the post
- [frontmatter.md](frontmatter.md) — exact frontmatter, tags, categories, slugs
- [checklist.md](checklist.md) — run this before you show her anything

---

## Workflow

### 1. Work out what kind of post this is

Match her material against [post-types.md](post-types.md): **explainer**, **career
narrative**, **how-to**, or **opinion**. If it doesn't fit any of the four, that's fine —
write what the material wants to be and note the new shape at the bottom of post-types.md
afterwards.

Say which one you picked and why, in one line. If you've got it wrong she'll say so
immediately, and that's much cheaper than finding out after 2,000 words.

### 2. Ask at most three questions, and only useful ones

If her dump already has enough, ask nothing. Start writing.

If it's thin, ask questions that **unlock content she has and you don't**:

- What surprised you when you were doing this?
- Who is this for, someone starting out or someone who already does this work?
- Is there a specific company, project or number you can point at?
- What do most people get wrong about this?
- What do you still not know?

Never ask her about configuration. Not tags, not categories, not word count, not
frontmatter, not the filename. You decide those. She is not technical and those questions
are noise to her.

### 3. Check the facts if the post makes regulatory claims

If the post asserts anything current about **CSRD, ESRS, EFRAG, the Omnibus package, GRI,
BRSR, SEBI, ISSB/IFRS S1-S2, SFDR, CSDDD, the GHG Protocol, or the EU Taxonomy** —
deadlines, thresholds, which standards apply to whom, what changed recently — launch the
`esg-researcher` subagent first and write from what it returns.

ESG regulation moves faster than any model's training data, and Nayana's professional
credibility is on the line in a way that a wrong date genuinely damages. A post that
confidently states a superseded threshold is worse than no post.

Ordinary background (what double materiality means, how Scope 3 categories are structured)
doesn't need research. Anything with a date, a number, or a "currently" does.

### 4. Show her the outline

One screen. Working title, the shape, the main sections, the specific claim or story each
one carries, and roughly how long. Flag anything you need from her that you couldn't find.

Wait for a yes. Don't write 2,000 words on spec.

### 5. Write it

Follow the post type template loosely, not mechanically. The template is a starting shape,
not a form to fill in. If the material wants a different structure, use it.

While writing:
- One idea per paragraph. 2 to 4 sentences.
- Vary sentence length deliberately. Long, then short.
- Every claim about her own experience must come from her dump. **Never invent a story, a
  client, a project or a number.** If a section needs an anecdote she didn't give you,
  leave a marker and ask her afterwards.
- Concrete over abstract. Always.
- Sentence case headings.
- Tables are good for comparisons, standards, timelines. She's an accountant and tables
  suit her material.
- Code blocks are almost never right for her posts. Excel formulas and calculation
  worked-examples are, and those belong in tables or fenced blocks marked `text`.

### 6. Save it

Write to `content/blogs/<slug>.md` with **`draft: true`**. See
[frontmatter.md](frontmatter.md) for the exact block and the slug rules.

If the post wants a hero image, note what would suit it. Don't fabricate an image path that
doesn't exist in `static/images/`.

### 7. Self-check, then fix

Run [checklist.md](checklist.md) against what you wrote. Actually run it, line by line, and
**fix what fails before showing her.** The em dash check and the banned-words check are not
optional.

### 8. Report back in plain English

Tell her:
- The title and how long it is
- The shape you gave it and why
- Any claim you weren't certain about, and where it came from
- Anything you left a gap for, and what you need from her to fill it
- That it's saved as a draft and nothing is live

Then offer: *"Want to see it in the browser?"* → `/preview-site`.

---

## When she gives feedback

This is the part that makes the skill get better, so don't skip it.

When she rewrites one of your lines, or says "I wouldn't say it like that", or corrects a
fact about herself:

1. Fix the post.
2. **Append the correction to the `## Learned corrections` section of
   [voice.md](voice.md)** — what you wrote, what she changed it to, and the general rule
   you should extract from it.
3. Mention that you've noted it, in one short line. Don't make a production of it.

Corrections about *facts* (her job title, a date, a client) belong in `CLAUDE.md` under
"About Nayana", not in voice.md.

---

## Hard rules

1. **No em dashes.** Not one, anywhere. Grep for them before saving.
2. **`draft: true` always.** You do not publish.
3. **Never invent her experience.** No made-up clients, projects, colleagues, numbers or
   anecdotes. This is her professional reputation.
4. **British spelling** throughout.
5. **Never copy the structure or voice of
   `content/blogs/building-your-portfolio-with-hugo.md`.** It's Puneeth's post, on her site
   deliberately. Different person, different voice.
6. **No `menu.sidebar` in frontmatter.** That's a Toha-theme thing from his site and it
   does nothing here.
7. **Don't touch other posts** while writing a new one.
