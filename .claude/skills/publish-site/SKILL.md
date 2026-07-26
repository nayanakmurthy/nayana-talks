---
name: publish-site
description: Put changes live on nayanamurthy.com. Use when Nayana says publish, go live, push it, make it public, or that a draft blog post is ready. This is the only skill that touches the live site. Also handles taking something back down.
---

# Publish to nayanamurthy.com

This is the only skill that changes what the public sees. Everything else stops at a local
draft.

```
git push origin main  →  Cloudflare builds  →  live in about 2 minutes
```

**A push is a publish.** Never do it without her explicit yes on this specific change.

---

## Steps

### 1. See what's actually changed

```bash
git status
git diff
```

Run `git status` **first, every time.** Her Obsidian vault auto-commits blog edits with
messages like `Update blog: 24-12-2025 13:43:32`, so there may be work in flight that you
didn't make. Never publish changes you haven't looked at, and never discard hers.

### 2. Flip any drafts she's approved

Blog posts are saved with `draft: true`. Change to `draft: false` **only** for the specific
post she named. Don't sweep up every draft in the folder.

If she says "publish the post" and there's more than one draft, ask which. Don't guess.

### 3. Build it

```bash
hugo --gc --minify
```

This must pass. If it fails, stop, fix it, and tell her what was wrong. **Never push a
build that doesn't pass** — Cloudflare runs the same command, so a local failure is a
guaranteed broken deploy.

Worth a glance at the output: it prints how many pages were built. If a post you expected
isn't in the count, its `draft:` is probably still `true`.

### 4. Show her exactly what goes live, then ask

Plain English list. Not a diff.

> Ready to publish:
> - New blog post: "What double materiality actually means"
> - Updated your contact section
>
> This goes live at nayanamurthy.com in about two minutes. Publish?

Then **wait for a yes.** Not silence, not "ok sounds good" to a different question. An
actual yes to this.

### 5. Commit and push

Match the existing commit style in this repo. It's short and plain, no Conventional
Commits, no scopes:

```bash
git add -A
git commit -m "Add post: what double materiality actually means"
git push origin main
```

Only `git add -A` when you've reviewed everything `git status` showed. Otherwise stage the
specific files.

Don't add a Claude co-author trailer here. Her repo history is hers, and it's written as if
she wrote it.

### 6. Tell her it's on its way

> Pushed. Cloudflare is building now, give it about two minutes then check
> https://nayanamurthy.com/blogs/what-double-materiality-means/

Offer to check the URL for her afterwards with a fetch, rather than making her refresh.

---

## Taking something back down

**A bad change that's already live:**

```bash
git log --oneline -10
git revert <hash>
git push origin main
```

Two minutes later the site is back to how it was. Reverting is safer than deleting, because
it keeps the history and can itself be undone.

**Unpublishing a post:** set `draft: true` again, then commit and push. The page disappears
from the live site. Its URL will 404 for anyone who bookmarked it, which is worth
mentioning if it's been up a while.

**Something is badly broken and she's worried:** revert first, explain second. The site
being right matters more than diagnosing it immediately.

---

## Never

- Push without an explicit yes on this specific change.
- Push a build that failed.
- `git add -A` without reading `git status` first.
- Flip a draft she didn't name.
- Force-push, rewrite history, or `git reset --hard` on anything that's been pushed.
- Delete a published post's file rather than setting `draft: true`. Deleting loses the
  writing; drafting just hides it.
- Rename a published post's file. That breaks its live URL.

---

## If the deploy fails

Cloudflare logs are in the Workers dashboard, which she'd have to open herself. Before
sending her there, check the obvious things locally:

1. Does `hugo --gc --minify` pass on a clean checkout?
2. Is the theme submodule reference committed? Cloudflare fetches it from
   `.gitmodules`, so a local-only submodule change won't be there.
3. Did anything in `build.sh` change? It pins Hugo 0.153.1, Go 1.25.5, Node 24.12.0 and
   Dart Sass 1.97.1. If a pinned version was bumped, that's the first suspect.

The previous deploy stays live while a new build fails, so a failed build is not an
outage. Say that, so she isn't panicking while you look.
