---
name: preview-site
description: Show Nayana her portfolio site running locally, before anything goes live. Use when she wants to see how a change or a draft blog post looks, asks "can I see it", "how does that look", or wants to check the site on her own machine. Handles installing Hugo and fetching the theme on first run.
---

# Preview the site

Get the site running locally and rendered in the browser pane, so Nayana can see a change
before it goes anywhere near the live site. She never types a URL or looks at a terminal.

Drafts are visible in the preview and invisible on the live site, so this is how she reads
a new blog post before deciding to publish it.

---

## Steps

### 1. Is Hugo installed?

```bash
hugo version
```

If that fails, install it. Tell her what you're doing in one line first: *"Hugo is the
programme that builds your site. I need to install it once, it takes about a minute."*

```bash
brew install hugo
```

If Homebrew itself is missing, `brew` will fail too. That install needs her Mac password,
so you can't do it silently:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Give her that command in a block, explain it's a one-time setup that will ask for her Mac
password, and wait.

**It must be Hugo extended.** `hugo version` shows `+extended` when it is. The Homebrew
build is extended by default. Without it, the theme's SCSS won't compile.

### 2. Is the theme there?

```bash
ls themes/hugo-profile
```

If it's empty, the theme hasn't been fetched:

```bash
git submodule update --init --recursive
```

This is the most common reason a preview fails. It only needs doing once per machine, and
it doesn't affect the live site at all.

### 3. Start it

Use `preview_start` with the `hugo-dev` config in `.claude/launch.json`, so the site opens
in the browser pane rather than sending her to a terminal.

That runs `hugo server -D --disableFastRender` on port 1313. The `-D` is what makes drafts
visible.

If `preview_start` isn't available, fall back to running it in the background and give her
`http://localhost:1313`:

```bash
hugo server -D --disableFastRender
```

### 4. Take her to the right page

Don't leave her on the home page if she's here to see something specific.

| To see | Go to |
|---|---|
| A blog post | `http://localhost:1313/blogs/<slug>/` |
| All posts | `http://localhost:1313/blogs/` |
| Portfolio changes | `http://localhost:1313/` |
| The gallery | `http://localhost:1313/gallery/` |

Tell her the site updates by itself when something changes, so she can ask for a tweak and
watch it happen without restarting anything.

---

## Checking it yourself first

Before handing it over, verify it actually worked. Use `read_page` or a screenshot to
confirm the page rendered with styling. Don't ask her whether it looks right when you can
look yourself.

If she asks how it looks on a phone, use `resize_window` with the `mobile` preset. Her CSS
has specific breakpoints at 768px and 992px for the hero title, and those are worth
checking after any hero change.

Dark mode is worth checking too. The theme has a toggle in the nav bar, and her custom CSS
in `static/style.css` uses theme variables that behave differently in each mode.

---

## When it fails

| What you see | What it means |
|---|---|
| `module "hugo-profile" not found` or an unstyled page | Theme submodule not fetched. Step 2. |
| `TOCSS ... this feature is not available` | Hugo isn't the extended build. `brew reinstall hugo`. |
| `failed to unmarshal YAML` | A syntax error in `hugo.yaml`. The message gives a line number. |
| `Error: ... address already in use` | A server is already running. Use it, or `pkill -f "hugo server"` and restart. |
| Page loads but changes don't appear | Hard refresh, or restart with `--disableFastRender`. |

Fix it and tell her what happened in plain English. Don't paste her a stack trace.

---

## Stopping

```bash
pkill -f "hugo server"
```

Or `preview_stop` with the server ID. It's harmless to leave running, but stop it when
she's finished so the port is free next time.

---

## Important

The preview is **only on her Mac**. Nothing here touches nayanamurthy.com. Say that
explicitly the first few times, so she can click around without worrying she'll break
something public.

To actually publish: `/publish-site`.
