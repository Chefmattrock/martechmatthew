# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static personal site for Matthew Stein (martechmatthew.com), hosted on GitHub Pages, deployed straight from `main` (root) — no build step, no framework, no package manager. Every page is hand-authored HTML that duplicates the same header/nav/footer markup inline.

## Local preview

```bash
python3 -m http.server 8080
```
Then open http://localhost:8080. There is no lint or test command — verify changes by eyeballing them in the browser.

## Deployment

Pushing to `main` deploys automatically via GitHub Pages ("Deploy from branch: main / (root)"). `.nojekyll` disables Jekyll processing, so files are served as-is. `CNAME` pins the canonical domain to `www.martechmatthew.com` — the apex/www redirect behavior depends on this file and DNS config, so don't change it casually (see the "Use www as the canonical domain to fix redirect loop" commit for why).

## Structure

- Each route is a real directory with an `index.html` (e.g. `/blog/easy-is-hard/index.html`), giving clean URLs under GitHub Pages without any server config.
- `contact-me/index.html` is a legacy URL kept alive as a meta-refresh + canonical-link redirect to `/contact/` — this is the pattern to copy if another old URL ever needs preserving.
- `assets/css/style.css` is the single stylesheet for the whole site, organized by section via comments (`/* Layout */`, `/* Header */`, `/* Buttons */`, `/* Homepage */`, `/* Marketing page */`, `/* Blog */`, `/* Contact */`, `/* Footer */`, `/* Responsive */`). Add new component styles under the matching section rather than creating new stylesheets.
- `assets/js/nav.js` is the only script in the codebase: a small IIFE that toggles the mobile nav (`.nav-toggle` / `.site-nav.is-open`) and closes it on outside click. There is no other client-side JS.
- All internal links and asset references use root-relative paths (`/assets/...`, `/blog/...`), consistent with GitHub Pages custom-domain hosting.

## Page template

Every page repeats the same shell: `<head>` (charset, viewport, title, description, OG tags, favicon, Google Fonts preconnect + Source Sans 3, stylesheet link) → `.site-wrapper` containing `.site-header` (logo + `.nav-toggle` + `.site-nav` with the same 5 nav links) → `<main>` with page content inside `.container` (or `.container--narrow` for text-heavy pages like blog/contact) → `.site-footer` with the same social-links SVG icon list → `nav.js` script tag.

When adding or editing a page, copy header/nav/footer from an existing page of the same kind (e.g. `contact/index.html` for a narrow content page) rather than writing it from scratch, and mark the current page's nav link with `aria-current="page"`.

The contact form is a third-party HubSpot embed (`js.hsforms.net` script + `.hs-form-frame` div with portal/form IDs) — treat it as opaque; don't try to replicate its markup by hand elsewhere.

## Adding a blog post

1. Create `blog/<slug>/index.html` using an existing post (e.g. `blog/easy-is-hard/index.html`) as the template — same head/header/footer shell, with post body in `.container--narrow`.
2. Add a corresponding `<li class="blog-list__item">` entry to `blog/index.html`'s `.blog-list`, including image, title link, `<time>` with ISO `datetime`, excerpt, "Read more" link, and tags — following the existing two entries' structure.
3. Add any post image to `assets/images/blog/`.
