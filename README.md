# Site

Personal blog built with Hugo (tested on v0.123.7 extended). The layout lives
directly in `layouts/` — there is no external theme to update or fight with.

## Run it

```bash
hugo server -D      # http://localhost:1313, live reload, shows drafts
hugo --gc --minify  # production build into public/
```

## Write a post

```bash
hugo new posts/my-writeup.md
```

Front matter that the layouts understand:

```yaml
---
title: "Blind SSRF to RCE"
date: 2026-09-12
description: "One sentence — used in listings, <meta description> and RSS."
tags: ["web", "ssrf", "htb"]
draft: false
toc: true          # optional, default true; a TOC shows from 3 headings up

# Optional writeup fields. The metadata box at the top of the post only
# appears if at least one of them is set, so normal articles stay clean.
platform: "Hack The Box"
machine: "Cascade"
ctf: "picoCTF 2026"
category: "Active Directory"
difficulty: "Medium"
os: "Windows"
solved: 2026-09-12
---
```

Use fenced code blocks with a language tag (` ```bash `) — syntax highlighting
follows the site theme in both light and dark mode.

## Make it yours

| What | Where |
| --- | --- |
| Site title, description, author | `hugo.toml` under `[params]` |
| `baseURL` (required before deploying) | `hugo.toml`, line 1 |
| Nav items | `hugo.toml`, `[menu]` |
| Footer links | `hugo.toml`, `[[params.social]]` |
| Home page intro | `content/_index.md` |
| About page | `content/about.md` |
| Colors, spacing, fonts | `assets/css/main.css`, the `:root` block at the top |
| Favicon | `static/favicon.svg` |

The accent color is one token: change `--accent` in both the light and the two
dark blocks of `assets/css/main.css`.

## Layout map

```
layouts/
  index.html              home: intro + latest posts
  404.html
  _default/
    baseof.html           page shell
    single.html           a post or page
    list.html             /posts/, grouped by year
    terms.html            /tags/
    taxonomy.html         /tags/<tag>/
  partials/
    head.html             meta, Open Graph, CSS, no-flash theme script
    header.html           brand, nav, theme toggle
    footer.html
    post-meta.html        date · reading time · tags
    post-list-item.html   one row in a listing
    writeup-box.html      optional CTF metadata box
assets/
  css/main.css            all styles, light/dark tokens at the top
  js/site.js              theme toggle, code copy buttons, table scroll
```

## Theme switching

Light and dark are defined as CSS custom properties. With no stored choice the
site follows the reader's OS setting; the header button overrides it and the
choice is kept in `localStorage`. A small inline script in `<head>` applies the
stored theme before first paint, so there is no flash on load.

## Deploy

Set `baseURL` in `hugo.toml`, then build and publish `public/`. On GitHub Pages,
Netlify or Cloudflare Pages the build command is `hugo --gc --minify` and the
output directory is `public`.
