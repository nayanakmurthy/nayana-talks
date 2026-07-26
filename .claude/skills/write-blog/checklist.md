# Pre-save checklist

Run this against the draft **before** you show Nayana anything. Actually run it, item by
item. Fix every failure. Don't report a post as done with known failures in it.

---

## Automated checks

Run these first. They catch the things that are objectively wrong.

```bash
# 1. Em dashes. Must return nothing.
grep -n '—' content/blogs/<slug>.md

# 2. American spellings. Must return nothing.
grep -nEi '\b(organiz|analyz|realiz|recogniz|prioritiz|categoriz|utiliz|behavior|center|favor|labor|program(s)? )' content/blogs/<slug>.md

# 3. Banned words. Every hit needs justifying or rewriting.
grep -nEi '\b(delve|leverag|robust|seamless|pivotal|underscore|foster|harness|unlock|holistic|paradigm|testament to|tapestry|game.chang|deep dive|move the needle|best.in.class|in today.s|furthermore|moreover|that said|it.s worth noting)' content/blogs/<slug>.md

# 4. The site still builds.
hugo --gc --minify
```

Check 1 is absolute. There is no acceptable em dash.

Check 3 will occasionally flag a legitimate use. "Robust" in a quoted standard is fine.
"Robust framework" in her own prose is not. Judge each hit; don't blanket-ignore them.

---

## Voice

- [ ] Read [voice.md](voice.md) again just now, including `## Learned corrections`
- [ ] At least one comma splice, and not more than one per two or three paragraphs
- [ ] No em dashes (check 1 above passed)
- [ ] British spelling throughout (check 2 passed)
- [ ] At least one specific number where a vaguer writer would say "many" or "several"
- [ ] Concrete over abstract in every section
- [ ] Understated, not hyped. No "incredible", "amazing", "transformative".
- [ ] Earnest but not preaching at the reader
- [ ] Sentence length actually varies. Read three paragraphs aloud in your head; if they
      all have the same rhythm, rewrite.
- [ ] At most one or two parenthetical asides, and they land
- [ ] Sentence case headings

## Substance

- [ ] **Every fact about her life, work, clients and numbers came from her dump.** Nothing
      invented. This is the one that matters most.
- [ ] Every regulatory claim with a date, threshold or "currently" was checked by
      `esg-researcher`, or is explicitly hedged
- [ ] The post takes a position somewhere. It isn't a neutral survey.
- [ ] There's at least one thing in it only she could have written
- [ ] Acronyms expanded on first use
- [ ] Any table's numbers add up
- [ ] Links use descriptive text, never a bare URL

## Shape

- [ ] The opening earns the second paragraph. No dictionary definition, no "In this post I
      will", no scene-setting about the planet.
- [ ] Paragraphs are 2 to 4 sentences
- [ ] Sections aren't all the same length
- [ ] No "Key Takeaways" box bolted on
- [ ] The ending is a thought, not a summary
- [ ] No closing CTA she wouldn't say out loud

## Technical

- [ ] Frontmatter matches [frontmatter.md](frontmatter.md) exactly
- [ ] **`draft: true`**
- [ ] Date has the right offset (`+02:00` summer, `+01:00` winter)
- [ ] Exactly one category, from the fixed list
- [ ] 3 to 6 tags, from the vocabulary
- [ ] No `menu.sidebar` block
- [ ] Filename is lowercase-hyphenated, no date, in `content/blogs/`
- [ ] Any `image:` path points at a file that actually exists in `static/images/`
- [ ] `hugo --gc --minify` passes (check 4 above)

---

## The read-aloud test

Read the first and last paragraphs as if saying them to someone.

If any sentence would make you self-conscious to say out loud, it's wrong. Her writing
sounds like a person who knows the subject talking normally. Not a report, not a LinkedIn
post, not a lecture.

If you can't tell whether a passage is hers or generic, it's generic. Rewrite it with a
specific number, a specific company, or a specific thing that went wrong.
