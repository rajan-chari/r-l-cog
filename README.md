# r-l-cog

A personal blog with a Hacker News-style layout, built with [Jekyll](https://jekyllrb.com/)
and hosted on GitHub Pages.

**Live site:** https://rajan-chari.github.io/r-l-cog/

---

## TL;DR — how to publish a post

1. Create a Markdown file in `_posts/` named `YYYY-MM-DD-some-slug.md`.
2. Add the frontmatter block (see below).
3. Write your post in Markdown.
4. `git add`, `git commit`, `git push`.
5. GitHub Pages rebuilds in ~30-60 seconds. Refresh the live URL.

There is **no build step on your machine**. GitHub runs Jekyll for you.

---

## How the site is built

You write files, GitHub does the rest. The pipeline:

```
  your files (Markdown + layouts)
              │
              ▼
       push to main
              │
              ▼
  GitHub Pages runs Jekyll
              │
              ▼
    static HTML in _site/
              │
              ▼
  served at rajan-chari.github.io/r-l-cog/
```

Jekyll is a static site generator. It reads your Markdown posts, runs them
through the layouts in `_layouts/`, and outputs plain HTML. GitHub Pages does
this automatically on every push — you don't install Jekyll, don't run any
commands, don't manage a server.

---

## File-by-file reference

| Path | What it does |
|------|--------------|
| `_config.yml` | Site-wide settings: title, base URL, permalinks, plugins. Changes here require a push to take effect. |
| `_layouts/default.html` | The outer page shell — orange topbar, footer, `<head>` tags. Every page is wrapped in this. |
| `_layouts/home.html` | The homepage layout — renders the numbered list of posts. Uses `default` as its parent. |
| `_layouts/post.html` | The single-post layout — title, metadata, body, back link. Uses `default` as its parent. |
| `_posts/` | One Markdown file per post. Filename **must** be `YYYY-MM-DD-slug.md`. |
| `index.html` | The homepage. Just frontmatter pointing at the `home` layout. |
| `about.md` | The about page at `/about/`. |
| `assets/style.css` | All styling (HN palette, typography, post list). |
| `Gemfile` | Ruby dependencies. Only needed for local preview. |
| `.gitignore` | Files to skip. Notably ignores `_site/` (the build output) and `.github/copilot/`. |

---

## Writing a post

### Filename rules

- Must live in `_posts/`.
- Must be named `YYYY-MM-DD-slug.md` (Jekyll parses the date from the filename).
- `slug` becomes part of the URL. Keep it lowercase, hyphen-separated.

Example: `_posts/2026-06-01-thoughts-on-rust.md` → `/2026/06/01/thoughts-on-rust/`

### Frontmatter

Every post starts with a YAML block between two `---` lines:

```yaml
---
layout: post
title: "Your title here"
date: 2026-05-19 08:00:00 -0700
tags: [tag1, tag2]
---
```

| Field | Required | Notes |
|-------|----------|-------|
| `layout` | yes | Always `post` for blog entries. |
| `title` | yes | Wrap in quotes if it contains a colon or other YAML-special char. |
| `date` | optional | If omitted, Jekyll uses the date from the filename. Include if you want a specific time. **Convert to UTC mentally and make sure it's not in the future** (or `future: true` in `_config.yml` is set — it is). |
| `tags` | optional | YAML list. Rendered as small badges on the homepage. |

### Body

Just write Markdown after the closing `---`. Headings, lists, code blocks, links,
blockquotes — all standard.

````markdown
## A section

Some prose with **bold** and *italics* and `inline code`.

- bullet
- list

```python
def hello():
    print("world")
```

> A blockquote.
````

Code blocks get syntax highlighting via Rouge (built into Jekyll).

---

## Editing layouts or styling

- **Site title / nav / footer:** `_layouts/default.html`
- **Homepage post list (numbering, metadata shown):** `_layouts/home.html`
- **Single post page (title size, back link):** `_layouts/post.html`
- **Colors / fonts / spacing:** `assets/style.css`

Layouts use [Liquid](https://shopify.github.io/liquid/) templating — that's the
`{{ ... }}` and `{% ... %}` syntax. The main things you'll see:

- `{{ site.title }}` — pulls from `_config.yml`
- `{{ page.title }}` — pulls from the current page's frontmatter
- `{% for post in site.posts %}` — loops over all posts
- `{{ content }}` — renders the page body inside a layout

Push any change to a layout or CSS file, and the rebuild picks it up.

---

## Local preview (optional)

You don't need this — push-to-deploy works fine. But if you want to see changes
before pushing:

**Requires:** Ruby 3.x + Bundler. On Windows, install via
[RubyInstaller](https://rubyinstaller.org/).

```
bundle install
bundle exec jekyll serve
```

Then open http://127.0.0.1:4000/r-l-cog/. Edits to Markdown files auto-reload.

---

## Common gotchas

### "My new post doesn't show up"

Most likely the post's `date:` is in the future relative to UTC at build time.
Either:
- Set the date earlier (remember `-0700` is 7 hours behind UTC), or
- Leave `future: true` in `_config.yml` (already set — but double-check it's still there).

### "I changed something but the live site looks the same"

GitHub Pages caches. Wait 60 seconds and hard-refresh (Ctrl+Shift+R). You can
also check the build status:

```
gh api repos/rajan-chari/r-l-cog/pages/builds/latest
```

Look for `"status":"built"` and `"error":{"message":null}`.

### "The build failed"

The same API call above will show the error message. Common causes:
- Bad YAML in frontmatter (unmatched quotes, wrong indentation).
- Liquid syntax error in a layout (`{{` without closing `}}`).
- Filename in `_posts/` doesn't match `YYYY-MM-DD-slug.md`.

### "Permalinks look weird"

Permalink format is controlled by `_config.yml`:
```yaml
permalink: /:year/:month/:day/:title/
```
Change once, push, all posts rebuild with the new pattern.

---

## Repo settings

- **Pages source:** `main` branch, `/` (root). Settings → Pages.
- **Custom domain:** not configured. Add via Settings → Pages → Custom domain
  if/when you get one.
- **Theme:** none. All HTML/CSS is in the repo so you have full control.

---

## What lives where (mental model)

- **Content** = `_posts/`, `about.md`, `index.html` — the words and structure.
- **Presentation** = `_layouts/`, `assets/style.css` — how it looks.
- **Wiring** = `_config.yml`, `Gemfile` — what plugins run, where files go.

If you only ever touch `_posts/`, the site keeps working. The rest is for when
you want to change the look or behavior.
