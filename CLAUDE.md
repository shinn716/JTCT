# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Single-file static portfolio website for John.TC Tsai (Interactive Artist & Creative Technologist). The entire site lives in `index.html` — no build step, no package manager, no framework.

## Development

Open `index.html` directly in a browser, or serve it locally:

```sh
python3 -m http.server 8080
# then visit http://localhost:8080
```

## Architecture

Everything is in one file with three layers:

**CSS** (`<style>` block, ~90 lines) — Custom styles layered on top of Tailwind CDN (loaded via `<script src="https://cdn.tailwindcss.com">`). Custom classes: `.project-card`, `.project-overlay`, `.lightbox`, `.image-popup`, `.nav-link`, `.skill-tag`.

**Data** (`artworks` array in `<script>`) — All 14 projects are defined as JavaScript objects with fields: `id`, `title`, `year`, `medium`, `category`, `color` (Tailwind gradient), `image` or `videoEmbed`, `description`, plus optional `gallery`, `github`, `link`, `event`, `contribution`, `team`. This is the primary place to add/edit/remove projects.

**UI logic** (inline `<script>`) — Key functions:
- `renderProjects()` — builds the project grid from `artworks`; called once on load
- `openLightbox(index)` / `closeLightbox()` — full-screen project detail modal
- `openImagePopup(src, index, gallery)` / `closeImagePopup()` — image zoom modal with prev/next navigation
- `prevProject()` / `nextProject()` — navigate between projects within the lightbox
- `navScrollTo(e, id)` — smooth scroll to section without updating URL hash
- Scroll listener — toggles `.scrolled` on `#navbar` and highlights active `.nav-link`

## Thumbnails

For video-only projects, `getThumbnail(work)` auto-generates thumbnails:
- YouTube: `https://img.youtube.com/vi/{videoId}/maxresdefault.jpg`
- Vimeo: `https://vumbnail.com/{videoId}.jpg`

External images are hosted at `duk.tw`.

## Sections

`#hero` → `#about` (experience, education, skills, awards) → `#projects` (dynamically rendered grid) → `#contact` → `<footer>`
