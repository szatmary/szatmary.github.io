# Personal Blog — m3u8.com

## Overview

A personal blog hosted on GitHub Pages, built with Hugo. Serves as a mix of personal writing (thoughts, what I'm doing) and project showcases (what I've built). Markdown-first authoring with minimal ongoing maintenance.

**Repository:** `szatmary/szatmary.github.io` (default branch: `master`)
**Custom domain:** `m3u8.com`

## Content Model

### Post Types

All content lives in a single blog feed — no separate sections. Posts are differentiated by tags, not by category or type.

### Front Matter

```yaml
---
title: "Post Title"
date: 2026-04-14
tags: ["streaming", "hls"]
summary: "One-line description for the feed listing."
cover: "/images/optional-cover.png"
draft: false
---
```

- `cover` is optional. When present, displayed at the top of the post.
- `draft: true` excludes the post from production builds.
- `summary` is used in the feed listing. If omitted, Hugo auto-generates from the first ~70 words.

### Content Support

- **Text:** Standard Markdown.
- **Images:** Stored in `static/images/` or as Hugo page bundles (`content/posts/my-post/index.md` alongside image files).
- **Code:** Fenced code blocks with language annotation. Syntax highlighting via Hugo's built-in Chroma (zero JavaScript).
- **Video:** Embedded via raw HTML in Markdown — `<video>` tags for self-hosted, iframes for YouTube/Vimeo.

## Site Structure

| Path | Content |
|------|---------|
| `/` | Blog feed — reverse-chronological post list with tag filtering |
| `/posts/<slug>/` | Individual post page |
| `/about/` | About page — who I am, what I do |
| `/tags/` | Tag index — list of all tags |
| `/tags/<tag>/` | Posts filtered by tag |

## Visual Design

### Principles

- Minimal and clean — content-first, no decoration for decoration's sake.
- Must not look "AI generated" — no blue-grey/purple palette.
- Light and dark theme support.

### Color Palette

- **Base:** Zinc scale (`#fafafa` to `#18181b`) — neutral, warm grey.
- **Accent:** International Orange `#FF4F00` — used for links, tags, nav highlights, and interactive elements.
- **Dark mode:** Inverted zinc scale with adjusted orange for contrast/accessibility (lighter orange on dark backgrounds).

### Typography

- System font stack for body text (`system-ui, -apple-system, sans-serif`).
- Clean, readable sizing with generous line height.

### Layout

- Single column, max-width constrained for readability (~680px for prose).
- Header: site title (`m3u8.com`) + minimal nav (About, Tags).
- Footer: minimal — copyright, maybe social links.
- No sidebar.

## Theme Architecture

Custom Hugo theme built from scratch. No external theme dependency.

### Template Files

- `layouts/_default/baseof.html` — base layout (HTML shell, head, header, footer, dark mode toggle)
- `layouts/_default/list.html` — blog feed and tag listing pages
- `layouts/_default/single.html` — individual post page
- `layouts/_default/taxonomy.html` — tag index page
- `layouts/_default/terms.html` — all-tags listing
- `layouts/partials/head.html` — meta tags, CSS, favicon
- `layouts/partials/header.html` — site header with nav
- `layouts/partials/footer.html` — site footer
- `layouts/404.html` — custom 404 page

### Styling

- Single CSS file: `static/css/style.css`
- CSS custom properties for theming (light/dark mode)
- `prefers-color-scheme` media query for automatic detection + manual toggle via JS
- Dark mode toggle stored in `localStorage` for persistence
- Chroma syntax highlighting theme configured in `hugo.toml` (light and dark variants)

### JavaScript

Minimal — only for dark mode toggle. No frameworks, no build tools.

## Deployment

### GitHub Actions

A workflow (`.github/workflows/deploy.yml`) triggers on push to `master`:

1. Checkout repository
2. Install Hugo (extended edition)
3. Build site (`hugo --minify`)
4. Deploy to GitHub Pages via `actions/deploy-pages`

### Custom Domain

- `static/CNAME` file containing `m3u8.com`
- DNS configuration (user-managed): A records and/or CNAME pointing to GitHub Pages servers
- HTTPS provided automatically by GitHub Pages (Let's Encrypt)

## Hugo Configuration

`hugo.toml` at the repo root:

- `baseURL = "https://m3u8.com/"`
- `title = "m3u8.com"`
- `theme` not set (theme files live directly in the repo, not as a submodule)
- Taxonomies: tags only
- Chroma syntax highlighting enabled with style configured
- RSS feed enabled (built-in)
- Permalink structure: `/posts/:slug/`

## Directory Structure

```
szatmary.github.io/
├── .github/workflows/deploy.yml
├── hugo.toml
├── content/
│   ├── posts/
│   │   └── my-first-post.md
│   └── about.md
├── layouts/
│   ├── _default/
│   │   ├── baseof.html
│   │   ├── list.html
│   │   ├── single.html
│   │   ├── taxonomy.html
│   │   └── terms.html
│   ├── partials/
│   │   ├── head.html
│   │   ├── header.html
│   │   └── footer.html
│   └── 404.html
├── static/
│   ├── css/style.css
│   ├── images/
│   └── CNAME
└── docs/
```
