# Personal Blog (m3u8.com) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a Hugo-based personal blog deployed to GitHub Pages at m3u8.com with a custom minimal theme featuring zinc base colors, international orange accents, and light/dark mode.

**Architecture:** Hugo static site with a custom theme built directly in the repo (no theme submodule). GitHub Actions builds on push to `master` and deploys to Pages. CSS custom properties handle light/dark theming. Minimal JS for dark mode toggle only.

**Tech Stack:** Hugo (static site generator), GitHub Actions, GitHub Pages, HTML/CSS/JS (no frameworks)

---

## File Map

| File | Responsibility |
|------|---------------|
| `hugo.toml` | Site configuration — base URL, title, taxonomies, permalinks, syntax highlighting |
| `layouts/_default/baseof.html` | HTML shell — doctype, head/body structure, dark mode script |
| `layouts/partials/head.html` | `<head>` contents — meta tags, CSS link, favicon |
| `layouts/partials/header.html` | Site header — title, nav links, dark mode toggle |
| `layouts/partials/footer.html` | Site footer — copyright |
| `layouts/_default/list.html` | Homepage feed and tag-filtered post lists |
| `layouts/_default/single.html` | Individual post page — title, date, tags, reading time, content |
| `layouts/_default/terms.html` | All-tags index page |
| `layouts/404.html` | Custom 404 page |
| `static/css/style.css` | All styling — CSS custom properties, light/dark themes, layout, typography |
| `static/css/syntax-light.css` | Chroma syntax highlighting for light mode (generated) |
| `static/css/syntax-dark.css` | Chroma syntax highlighting for dark mode (generated, scoped to `[data-theme="dark"]`) |
| `static/CNAME` | Custom domain for GitHub Pages |
| `content/about.md` | About page content |
| `content/posts/hello-world.md` | Seed post to verify the build |
| `.github/workflows/deploy.yml` | GitHub Actions workflow — build Hugo, deploy to Pages |
| `.gitignore` | Ignore Hugo build output and local files |

---

### Task 1: Initialize Repository and Hugo Project

**Files:**
- Create: `.gitignore`
- Create: `hugo.toml`

- [ ] **Step 1: Initialize git repo and clone remote**

```bash
cd /Users/matthewszatmary/Projects/blog
git init
git remote add origin git@github.com:szatmary/szatmary.github.io.git
```

- [ ] **Step 2: Create `.gitignore`**

```gitignore
/public/
/resources/_gen/
/.hugo_build.lock
.DS_Store
.superpowers/
```

- [ ] **Step 3: Create `hugo.toml`**

```toml
baseURL = "https://m3u8.com/"
languageCode = "en-us"
title = "m3u8.com"

[taxonomies]
  tag = "tags"

[permalinks]
  posts = "/posts/:slug/"

[markup]
  [markup.highlight]
    noClasses = false
    lineNos = false
    guessSyntax = false

[params]
  description = "Personal blog of Matthew Szatmary"
```

- [ ] **Step 4: Create Hugo directory structure**

```bash
mkdir -p content/posts layouts/_default layouts/partials static/css static/images .github/workflows
```

- [ ] **Step 5: Verify Hugo recognizes the project**

Run: `hugo version && hugo config | head -5`
Expected: Hugo version output and config showing `baseurl = "https://m3u8.com/"`

- [ ] **Step 6: Commit**

```bash
git add .gitignore hugo.toml
git commit -m "feat: initialize Hugo project with config"
```

---

### Task 2: CSS — Light and Dark Themes

**Files:**
- Create: `static/css/style.css`

- [ ] **Step 1: Create `static/css/style.css` with CSS custom properties and light theme**

```css
/* ===== Reset ===== */
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* ===== Theme tokens ===== */
:root {
  --color-bg: #fafafa;
  --color-bg-alt: #f4f4f5;
  --color-text: #18181b;
  --color-text-secondary: #52525b;
  --color-text-muted: #a1a1aa;
  --color-border: #e4e4e7;
  --color-accent: #FF4F00;
  --color-accent-hover: #E64600;
  --color-accent-subtle: #FFF1EB;
  --color-code-bg: #f4f4f5;
  --font-body: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --font-mono: ui-monospace, "SF Mono", Menlo, Consolas, monospace;
  --max-width: 680px;
  --header-max-width: 880px;
}

[data-theme="dark"] {
  --color-bg: #18181b;
  --color-bg-alt: #27272a;
  --color-text: #fafafa;
  --color-text-secondary: #a1a1aa;
  --color-text-muted: #71717a;
  --color-border: #3f3f46;
  --color-accent: #FF7A3D;
  --color-accent-hover: #FF935E;
  --color-accent-subtle: #2A1A10;
  --color-code-bg: #27272a;
}

/* ===== Base ===== */
html {
  font-family: var(--font-body);
  font-size: 17px;
  line-height: 1.7;
  color: var(--color-text);
  background: var(--color-bg);
  -webkit-font-smoothing: antialiased;
}

body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

a {
  color: var(--color-accent);
  text-decoration: none;
}

a:hover {
  color: var(--color-accent-hover);
  text-decoration: underline;
}

img {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
}

/* ===== Layout ===== */
.site-header {
  max-width: var(--header-max-width);
  width: 100%;
  margin: 0 auto;
  padding: 2rem 1.5rem 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.site-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--color-text);
  text-decoration: none;
}

.site-title:hover {
  color: var(--color-text);
  text-decoration: none;
}

.site-nav {
  display: flex;
  gap: 1.5rem;
  align-items: center;
}

.site-nav a {
  color: var(--color-text-secondary);
  font-size: 0.9rem;
}

.site-nav a:hover {
  color: var(--color-accent);
  text-decoration: none;
}

main {
  max-width: var(--max-width);
  width: 100%;
  margin: 0 auto;
  padding: 1rem 1.5rem 3rem;
  flex: 1;
}

.site-footer {
  max-width: var(--header-max-width);
  width: 100%;
  margin: 0 auto;
  padding: 2rem 1.5rem;
  border-top: 1px solid var(--color-border);
  color: var(--color-text-muted);
  font-size: 0.85rem;
}

/* ===== Dark mode toggle ===== */
.theme-toggle {
  background: none;
  border: none;
  cursor: pointer;
  color: var(--color-text-secondary);
  font-size: 1.1rem;
  padding: 0.25rem;
  line-height: 1;
  transition: color 0.2s;
}

.theme-toggle:hover {
  color: var(--color-accent);
}

/* ===== Post list ===== */
.post-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
}

.post-item-title {
  font-size: 1.3rem;
  font-weight: 600;
  line-height: 1.3;
}

.post-item-title a {
  color: var(--color-text);
}

.post-item-title a:hover {
  color: var(--color-accent);
  text-decoration: none;
}

.post-item-meta {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  margin-top: 0.25rem;
}

.post-item-summary {
  color: var(--color-text-secondary);
  margin-top: 0.5rem;
}

/* ===== Tags ===== */
.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  list-style: none;
}

.tag-list a {
  color: var(--color-accent);
  font-size: 0.85rem;
  font-weight: 500;
}

.post-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.post-tags a {
  color: var(--color-accent);
  font-size: 0.8rem;
  font-weight: 500;
}

.post-tags a::before {
  content: "#";
}

/* ===== Single post ===== */
.post-header {
  margin-bottom: 2rem;
}

.post-title {
  font-size: 2rem;
  font-weight: 700;
  line-height: 1.2;
}

.post-meta {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  margin-top: 0.5rem;
}

.post-meta span + span::before {
  content: " · ";
}

.post-cover {
  margin-bottom: 2rem;
}

/* ===== Post content (prose) ===== */
.post-content h2 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-top: 2.5rem;
  margin-bottom: 0.75rem;
}

.post-content h3 {
  font-size: 1.2rem;
  font-weight: 600;
  margin-top: 2rem;
  margin-bottom: 0.5rem;
}

.post-content p {
  margin-bottom: 1.25rem;
}

.post-content ul,
.post-content ol {
  margin-bottom: 1.25rem;
  padding-left: 1.5rem;
}

.post-content li {
  margin-bottom: 0.25rem;
}

.post-content blockquote {
  border-left: 3px solid var(--color-accent);
  padding-left: 1rem;
  color: var(--color-text-secondary);
  margin-bottom: 1.25rem;
}

.post-content code {
  font-family: var(--font-mono);
  font-size: 0.9em;
  background: var(--color-code-bg);
  padding: 0.15em 0.35em;
  border-radius: 3px;
}

.post-content pre {
  background: var(--color-code-bg);
  padding: 1rem 1.25rem;
  border-radius: 6px;
  overflow-x: auto;
  margin-bottom: 1.25rem;
}

.post-content pre code {
  background: none;
  padding: 0;
  font-size: 0.85rem;
  line-height: 1.6;
}

.post-content hr {
  border: none;
  border-top: 1px solid var(--color-border);
  margin: 2rem 0;
}

.post-content a {
  text-decoration-color: var(--color-accent-subtle);
  text-underline-offset: 2px;
}

.post-content a:hover {
  text-decoration-color: var(--color-accent);
}

.post-content video,
.post-content iframe {
  max-width: 100%;
  margin-bottom: 1.25rem;
  border-radius: 4px;
}

/* ===== Tags index page ===== */
.tags-index {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.tags-index a {
  font-size: 1rem;
  font-weight: 500;
}

.tags-index .tag-count {
  color: var(--color-text-muted);
  font-size: 0.85rem;
  font-weight: 400;
}

/* ===== 404 ===== */
.not-found {
  text-align: center;
  padding: 4rem 0;
}

.not-found h1 {
  font-size: 3rem;
  font-weight: 700;
  color: var(--color-accent);
}

.not-found p {
  color: var(--color-text-secondary);
  margin-top: 0.5rem;
}

/* ===== Page title (about, tags index) ===== */
.page-title {
  font-size: 1.8rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
}
```

- [ ] **Step 2: Verify the file is syntactically valid CSS**

Run: `python3 -c "open('static/css/style.css').read(); print('CSS file reads OK')"` (or just confirm the file exists and is non-empty)
Expected: File reads without error.

- [ ] **Step 3: Commit**

```bash
git add static/css/style.css
git commit -m "feat: add CSS with light/dark theme and international orange accents"
```

---

### Task 3: Base Layout and Partials

**Files:**
- Create: `layouts/_default/baseof.html`
- Create: `layouts/partials/head.html`
- Create: `layouts/partials/header.html`
- Create: `layouts/partials/footer.html`

- [ ] **Step 1: Create `layouts/partials/head.html`**

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{ if not .IsHome }}{{ .Title }} — {{ end }}{{ .Site.Title }}</title>
<meta name="description" content="{{ with .Params.summary }}{{ . }}{{ else }}{{ .Site.Params.description }}{{ end }}">
<link rel="stylesheet" href="/css/style.css">
<link rel="alternate" type="application/rss+xml" title="{{ .Site.Title }}" href="/index.xml">
```

- [ ] **Step 2: Create `layouts/partials/header.html`**

```html
<header class="site-header">
  <a href="/" class="site-title">{{ .Site.Title }}</a>
  <nav class="site-nav">
    <a href="/about/">About</a>
    <a href="/tags/">Tags</a>
    <button class="theme-toggle" id="theme-toggle" aria-label="Toggle dark mode">◐</button>
  </nav>
</header>
```

- [ ] **Step 3: Create `layouts/partials/footer.html`**

```html
<footer class="site-footer">
  &copy; {{ now.Year }} {{ .Site.Title }}
</footer>
```

- [ ] **Step 4: Create `layouts/_default/baseof.html`**

```html
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}">
<head>
  {{- partial "head.html" . -}}
  <script>
    (function() {
      var theme = localStorage.getItem('theme');
      if (theme === 'dark' || (!theme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
        document.documentElement.setAttribute('data-theme', 'dark');
      }
    })();
  </script>
</head>
<body>
  {{- partial "header.html" . -}}
  <main>
    {{- block "main" . }}{{ end -}}
  </main>
  {{- partial "footer.html" . -}}
  <script>
    document.getElementById('theme-toggle').addEventListener('click', function() {
      var html = document.documentElement;
      var isDark = html.getAttribute('data-theme') === 'dark';
      if (isDark) {
        html.removeAttribute('data-theme');
        localStorage.setItem('theme', 'light');
      } else {
        html.setAttribute('data-theme', 'dark');
        localStorage.setItem('theme', 'dark');
      }
    });
  </script>
</body>
</html>
```

- [ ] **Step 5: Commit**

```bash
git add layouts/_default/baseof.html layouts/partials/head.html layouts/partials/header.html layouts/partials/footer.html
git commit -m "feat: add base layout with head, header, footer partials and dark mode"
```

---

### Task 4: Homepage / List Template

**Files:**
- Create: `layouts/_default/list.html`

- [ ] **Step 1: Create `layouts/_default/list.html`**

```html
{{ define "main" }}
  {{ if .IsHome }}
    <ul class="post-list">
      {{ range where .Site.RegularPages "Section" "posts" }}
      <li>
        <h2 class="post-item-title"><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
        <div class="post-item-meta">
          <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "Jan 2, 2006" }}</time>
          · {{ math.Round (div (countwords .Content) 200.0) }} min read
        </div>
        {{ with .Params.summary }}<p class="post-item-summary">{{ . }}</p>{{ end }}
        {{ with .Params.tags }}
        <div class="post-tags">
          {{ range . }}<a href="/tags/{{ . | urlize }}/">{{ . }}</a>{{ end }}
        </div>
        {{ end }}
      </li>
      {{ end }}
    </ul>
  {{ else }}
    <h1 class="page-title">{{ .Title }}</h1>
    <ul class="post-list">
      {{ range .Pages }}
      <li>
        <h2 class="post-item-title"><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
        <div class="post-item-meta">
          <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "Jan 2, 2006" }}</time>
          · {{ math.Round (div (countwords .Content) 200.0) }} min read
        </div>
        {{ with .Params.summary }}<p class="post-item-summary">{{ . }}</p>{{ end }}
      </li>
      {{ end }}
    </ul>
  {{ end }}
{{ end }}
```

- [ ] **Step 2: Verify Hugo builds without error**

Run: `hugo --buildDrafts 2>&1 | tail -5`
Expected: Build succeeds (no pages yet, but no template errors)

- [ ] **Step 3: Commit**

```bash
git add layouts/_default/list.html
git commit -m "feat: add homepage and list template"
```

---

### Task 5: Single Post Template

**Files:**
- Create: `layouts/_default/single.html`

- [ ] **Step 1: Create `layouts/_default/single.html`**

```html
{{ define "main" }}
  <article>
    <header class="post-header">
      <h1 class="post-title">{{ .Title }}</h1>
      <div class="post-meta">
        <span><time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "Jan 2, 2006" }}</time></span>
        <span>{{ math.Round (div (countwords .Content) 200.0) }} min read</span>
      </div>
      {{ with .Params.tags }}
      <div class="post-tags">
        {{ range . }}<a href="/tags/{{ . | urlize }}/">{{ . }}</a>{{ end }}
      </div>
      {{ end }}
    </header>

    {{ with .Params.cover }}
    <img class="post-cover" src="{{ . }}" alt="{{ $.Title }}">
    {{ end }}

    <div class="post-content">
      {{ .Content }}
    </div>
  </article>
{{ end }}
```

- [ ] **Step 2: Commit**

```bash
git add layouts/_default/single.html
git commit -m "feat: add single post template"
```

---

### Task 6: Tags Index and 404

**Files:**
- Create: `layouts/_default/terms.html`
- Create: `layouts/404.html`

- [ ] **Step 1: Create `layouts/_default/terms.html`**

```html
{{ define "main" }}
  <h1 class="page-title">Tags</h1>
  <ul class="tags-index">
    {{ range .Pages }}
    <li>
      <a href="{{ .RelPermalink }}">{{ .Title }}</a>
      <span class="tag-count">({{ len .Pages }})</span>
    </li>
    {{ end }}
  </ul>
{{ end }}
```

- [ ] **Step 2: Create `layouts/404.html`**

```html
{{ define "main" }}
  <div class="not-found">
    <h1>404</h1>
    <p>Page not found. <a href="/">Go home</a>.</p>
  </div>
{{ end }}
```

- [ ] **Step 3: Commit**

```bash
git add layouts/_default/terms.html layouts/404.html
git commit -m "feat: add tags index and 404 page"
```

---

### Task 7: Syntax Highlighting CSS

**Files:**
- Create: `static/css/syntax-light.css`
- Create: `static/css/syntax-dark.css`
- Modify: `layouts/partials/head.html`

Hugo's Chroma generates class-based syntax highlighting when `noClasses = false`. We need CSS for both themes.

- [ ] **Step 1: Generate light syntax theme CSS**

Run: `hugo gen chromastyles --style=monokailight > static/css/syntax-light.css`

- [ ] **Step 2: Generate dark syntax theme CSS**

Run: `hugo gen chromastyles --style=monokai > static/css/syntax-dark.css`

- [ ] **Step 3: Scope dark syntax CSS under `[data-theme="dark"]`**

Prepend `[data-theme="dark"] ` to every selector in `syntax-dark.css`. This can be done by wrapping the file content:

Replace the contents of `static/css/syntax-dark.css` with:

```css
[data-theme="dark"] .chroma { /* ... */ }
```

The simplest approach: use `sed` to prepend the scope:

Run: `sed -i '' 's/^\./@DARK@./g' static/css/syntax-dark.css && sed -i '' 's/@DARK@/[data-theme="dark"] /g' static/css/syntax-dark.css`

Verify: `head -3 static/css/syntax-dark.css` should show selectors like `[data-theme="dark"] .chroma { ... }`

- [ ] **Step 4: Add syntax CSS to `layouts/partials/head.html`**

Add these two lines after the main stylesheet link:

```html
<link rel="stylesheet" href="/css/syntax-light.css">
<link rel="stylesheet" href="/css/syntax-dark.css">
```

- [ ] **Step 5: Commit**

```bash
git add static/css/syntax-light.css static/css/syntax-dark.css layouts/partials/head.html
git commit -m "feat: add light/dark syntax highlighting CSS"
```

---

### Task 8: Seed Content

**Files:**
- Create: `content/about.md`
- Create: `content/posts/hello-world.md`

- [ ] **Step 1: Create `content/about.md`**

```markdown
---
title: "About"
---

This is the personal blog of Matthew Szatmary.
```

- [ ] **Step 2: Create `content/posts/hello-world.md`**

```markdown
---
title: "Hello World"
date: 2026-04-14
tags: ["meta"]
summary: "First post — testing the blog setup."
---

Welcome to m3u8.com. This is a test post to verify everything works.

## A Code Block

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world!")
}
```

## A Quote

> The best way to predict the future is to implement it.

More content to come.
```

- [ ] **Step 3: Build and verify**

Run: `hugo --buildDrafts && echo "Build OK" && ls public/posts/hello-world/index.html && ls public/about/index.html`
Expected: Build succeeds, both HTML files exist.

- [ ] **Step 4: Run Hugo dev server and spot-check**

Run: `hugo server --buildDrafts &` then open `http://localhost:1313`
Expected: Homepage shows "Hello World" post. Clicking it shows full content with syntax-highlighted code. About link works. Dark mode toggle works. Kill the server after verification.

- [ ] **Step 5: Commit**

```bash
git add content/about.md content/posts/hello-world.md
git commit -m "feat: add seed content — about page and hello world post"
```

---

### Task 9: GitHub Actions Deployment

**Files:**
- Create: `.github/workflows/deploy.yml`
- Create: `static/CNAME`

- [ ] **Step 1: Create `static/CNAME`**

```
m3u8.com
```

(Single line, no trailing newline.)

- [ ] **Step 2: Create `.github/workflows/deploy.yml`**

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [master]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 3: Commit**

```bash
git add static/CNAME .github/workflows/deploy.yml
git commit -m "feat: add GitHub Actions deployment and CNAME"
```

---

### Task 10: Push and Verify Deployment

- [ ] **Step 1: Push to GitHub**

```bash
git push -u origin master
```

- [ ] **Step 2: Enable GitHub Pages**

Go to https://github.com/szatmary/szatmary.github.io/settings/pages and set source to "GitHub Actions" (it may already be auto-detected).

- [ ] **Step 3: Verify the Actions workflow runs**

Run: `gh run list --limit 1`
Expected: A workflow run in progress or completed for "Deploy to GitHub Pages."

- [ ] **Step 4: Verify the deployed site**

Run: `gh run watch` (wait for completion), then `curl -sI https://szatmary.github.io | head -10`
Expected: HTTP 200 response.

- [ ] **Step 5: Configure custom domain DNS**

The user needs to configure DNS for `m3u8.com`:
- A records pointing to GitHub Pages IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Or CNAME record for `www.m3u8.com` pointing to `szatmary.github.io`

After DNS propagates, enable "Enforce HTTPS" in the repo Pages settings.
