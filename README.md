# r-l-cog

A Hacker News-style personal blog built with Jekyll, hosted on GitHub Pages.

Live: https://rajan-chari.github.io/r-l-cog/

## Writing a post

Drop a Markdown file into `_posts/` named `YYYY-MM-DD-title.md` with frontmatter:

```yaml
---
layout: post
title: "Your title"
date: 2026-05-19
tags: [tag1, tag2]
---

Body in Markdown.
```

## Local preview

```
bundle install
bundle exec jekyll serve
```

Then open `http://127.0.0.1:4000/r-l-cog/`.

## Deploy

Push to `main`. GitHub Pages builds and serves automatically. Enable Pages in
repo settings → Pages → Source = `Deploy from a branch` → `main` / `(root)`.

## Layout

- `_config.yml` — site config
- `_layouts/` — HTML templates (`default`, `home`, `post`)
- `_posts/` — blog posts in Markdown
- `assets/style.css` — HN-inspired styling
- `index.html` — homepage (list of posts)
- `about.md` — about page
