# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Project Is

A Jekyll-based slideshow built on [reveal.js](https://github.com/hakimel/reveal.js/), used as a GitHub Learning Lab course repository. Slides are authored as Jekyll posts in `_posts/` and rendered through reveal.js via the Jekyll build pipeline.

## Commands

**Setup (first time):**
```bash
./script/setup
```
Installs rbenv, Ruby, bundler, and all gem dependencies.

**Local development server:**
```bash
./script/server
```

**Build and validate (what CI runs):**
```bash
./script/cibuild
```
Builds the Jekyll site then runs `htmlproofer` to validate HTML and links. Pass `--empty-alt-ignore` is already included.

**Staging deploy:**
```bash
./script/stage [repo-name]
```

## Architecture

The presentation system works as follows:

- **Slides** are Markdown files in `_posts/` named `YYYY-MM-DD-slug.md`. Each post becomes one slide. The date ordering controls slide sequence.
- **`_layouts/presentation.html`** is the outer shell — loads reveal.js, CSS, and wraps everything.
- **`_layouts/slide.html`** is applied per-post and delegates to `_includes/slide.html` for rendering the slide content within the reveal.js `<section>` structure.
- **`_includes/script.html`** initializes reveal.js with settings from `_config.yml` under the `reveal` key.
- **`_config.yml`** contains both standard Jekyll config and reveal.js presentation settings (theme, transitions, dimensions, etc.) under the top-level keys directly consumed by `_includes/script.html`.

## Key Conventions

- **Indentation:** 2 spaces for HTML/CSS/JS/YML; 4 spaces for Markdown; tabs for everything else (per `.editorconfig`).
- **Line endings:** LF everywhere.
- **Slide content:** Written in Kramdown-flavored Markdown. Emoji are supported via the `jemoji` plugin.
- **No Node.js build step** — reveal.js assets are pre-built and committed under `node_modules/`. Do not run `npm install` or modify those files.
- **Ruby dependency management:** Use `bundle exec` prefix when running Jekyll commands manually (the scripts handle this).
