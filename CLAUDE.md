# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Single-file static portfolio website for John.TC Tsai (Interactive Artist & Creative Technologist). Nearly everything lives in `index.html` — no framework, no JS bundler. Styling is a precompiled static `styles.css` (Tailwind), not the Tailwind CDN script, for load performance.

## Development

Open `index.html` directly in a browser, or serve it locally:

```sh
python3 -m http.server 8080
# then visit http://localhost:8080
```

### Regenerating styles.css

If you add a Tailwind utility class that isn't already used elsewhere in `index.html`, it won't appear in `styles.css` until regenerated:

```sh
npx tailwindcss -i ./input.css -o ./styles.css --minify --content ./index.html
```

(`input.css` just needs the three `@tailwind base/components/utilities` directives; regenerate ad hoc if missing.)

## Deployment

Pushes to `main` auto-deploy via GitHub Actions (`.github/workflows/deploy.yml`) to GitHub Pages at `https://shinn716.github.io/JTCT/`. Requires GitHub Pages source set to **GitHub Actions** in repo settings.

## Architecture

Everything is in one file with three layers:

**CSS** (`<style>` block) — Custom styles layered on top of Tailwind CDN (`<script src="https://cdn.tailwindcss.com">`). Custom classes: `.project-card`, `.project-overlay`, `.lightbox`, `.image-popup`, `.nav-link`, `.skill-tag`, `.hero-bg`.

**Data** (`artworks` array in `<script>`) — All 14 projects are JavaScript objects. This is the primary place to add/edit/remove projects. Required fields: `id`, `title`, `year`, `medium`, `category`, `color` (Tailwind gradient), `tags` (array, up to 3 shown in grid), `description`. Media: use `image` (URL) or `videoEmbed` (iframe src). Optional: `video` (watch link), `gallery` (array of image URLs), `github`, `link`, `event`, `contribution`, `team`, `direction`.

**UI logic** (inline `<script>`) — Key functions:
- `renderProjects()` — builds the project grid from `artworks`; called once on load
- `openLightbox(index)` / `closeLightbox()` — full-screen project detail modal
- `openImagePopup(src, index, gallery)` / `closeImagePopup()` — image zoom modal with prev/next navigation; `gallery` may be a JSON string (from HTML attribute) or array
- `prevProject()` / `nextProject()` — navigate between projects within the lightbox
- `navScrollTo(e, id)` — smooth scroll to section without updating URL hash
- Scroll listener — toggles `.scrolled` on `#navbar` and highlights active `.nav-link`

Global state: `currentIndex` (active lightbox project), `currentGallery` (array), `currentGalleryIndex`.

Keyboard shortcuts active when image popup is open: `←` / `→` navigate gallery, `Escape` closes popup.

## Thumbnails

For video-only projects, `getThumbnail(work)` auto-generates thumbnails:
- YouTube: `https://img.youtube.com/vi/{videoId}/maxresdefault.jpg`
- Vimeo: `https://vumbnail.com/{videoId}.jpg`

External images are hosted at `duk.tw`.

## Sections

`#hero` → `#about` (experience, education, skills, awards) → `#projects` (dynamically rendered grid) → `#contact` → `<footer>`
