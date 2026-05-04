# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A Jekyll-based slideshow site that uses [reveal.js](https://github.com/hakimel/reveal.js/) for in-browser presentation rendering. It was created as a GitHub Learning Lab exercise. Slides are authored as Jekyll posts and assembled into a single reveal.js presentation at build time.

## Development commands

```sh
script/setup        # Install Ruby gems and init git submodules
script/server       # Run local dev server (wraps: bundle exec jekyll serve)
script/cibuild      # Build site and run htmlproofer validation
```

The site is served at `http://localhost:4000` by default.

## Architecture

### How slides become a presentation

1. Each slide is a Markdown file in `_posts/` with `layout: slide` front matter. Jekyll post filenames must follow the `YYYY-MM-DD-title.md` convention — the date controls ordering.
2. `index.html` (layout: `presentation`) iterates over `site.posts` in reverse order and includes `_includes/slide.html` for each post, wrapping it in a `<section class="step slide">` element.
3. `_layouts/presentation.html` wraps the assembled slides in the reveal.js `.reveal > .slides` DOM structure.
4. `_includes/head.html` loads reveal.js CSS from `node_modules/reveal.js/`.
5. `_includes/script.html` initializes `Reveal.initialize()` with markdown, highlight, and notes plugins.

### Configuration

`_config.yml` controls two layers of config:
- Standard Jekyll settings (timezone, markdown engine, permalink style, plugins).
- A `reveal:` key whose values are passed directly into `Reveal.initialize()` — edit this to change transitions, controls, progress bar, slide dimensions, etc.
- A `solarized.theme` key (`dark`/`light`) sets the HTML class on `<html>` for the color scheme.

### reveal.js dependency

reveal.js lives in `node_modules/reveal.js/` (installed via npm, tracked in `package-lock.json`). It is referenced directly by path in the Liquid templates — there is no asset pipeline or bundler.

## Adding slides

Create a new file in `_posts/` with front matter:

```markdown
---
layout: slide
title: "My Slide Title"
---

Slide content here (Markdown supported).
```

Use a date earlier than the intro slide (`0000-01-01`) if it should appear before it, or a later date to append after.

Optional front matter fields:
- `slide-id` — sets the HTML `id` on the `<section>`
- `classes` — list of extra CSS classes added to the section
- `data` — key/value pairs rendered as `data-*` attributes (used by reveal.js for per-slide settings)

## EditorConfig conventions

- Most files: tabs, size 4
- JSON, JS, CSS, SCSS, YAML, HTML: spaces, size 2
- Markdown: spaces, size 4; trailing whitespace preserved; final newline required
