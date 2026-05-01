# John.TC Tsai — Portfolio

Personal portfolio website for John.TC Tsai, Interactive Artist & Creative Technologist.

**Live site:** https://shinn716.github.io/JTCT/

## Stack

- Single `index.html` — no build step, no framework
- [Tailwind CSS](https://tailwindcss.com/) via CDN
- Vanilla JavaScript
- Hosted on GitHub Pages

## Local Development

```sh
python3 -m http.server 8080
# visit http://localhost:8080
```

## Structure

All content lives in `index.html` with three layers:

| Layer | Location | Purpose |
|-------|----------|---------|
| Styles | `<style>` block | Custom CSS on top of Tailwind |
| Data | `artworks` array in `<script>` | Project definitions |
| Logic | Inline `<script>` | Rendering, lightbox, navigation |

### Adding a Project

Edit the `artworks` array in `index.html`. Each project object supports:

```js
{
  id: 'unique-id',
  title: 'Project Title',
  year: 2024,
  medium: 'Unity / XR',
  category: 'Interactive Installation',
  color: 'from-purple-900 to-blue-900',   // Tailwind gradient
  image: 'https://...',                    // or use videoEmbed
  videoEmbed: 'https://...',
  description: 'Project description.',
  gallery: ['https://...'],               // optional extra images
  github: 'https://github.com/...',       // optional
  link: 'https://...',                    // optional
  event: 'Exhibition Name',              // optional
  contribution: 'Role / contribution',   // optional
  team: 'Collaborators'                  // optional
}
```

## Deployment

Pushes to `main` automatically deploy via GitHub Actions → GitHub Pages.

## License

[MIT](LICENSE)
