# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static single-page wedding website for Guadalupe & Ayrton. Everything lives in a single `index.html` file with embedded CSS and JavaScript. No build tools, no dependencies, no package.json.

**Live site:** https://chichex.github.io
**Language:** Spanish (Argentina/Uruguay audience)

## Development

Serve locally with any HTTP server:
```bash
python3 -m http.server 8888
# or
npx http-server
```

Deploy by pushing to `master` — GitHub Pages auto-deploys from the remote `https://github.com/chichex/chichex.github.io.git`.

There are no tests, linting, or build steps.

## Architecture (all in index.html)

**Structure:** `<style>` block → HTML body → `<script>` IIFE (bottom of file)

**Approach:** The entire design is a single full-height image with a video behind a transparent hole and invisible interactive buttons overlaid on the clickable areas of the image.

**HTML structure:**
```
<div class="site">                    (relative wrapper, max-width: 430px)
  <video>                             (z-index: 1, absolute, in transparent hole)
  <picture><img>                      (z-index: 2, pointer-events: none, full design)
  <a> Cómo llegar Ceremonia           (z-index: 3, invisible overlay button)
  <a> Cómo llegar Recepción           (z-index: 3, invisible overlay button)
  <button> Copiar ALIAS               (z-index: 3, invisible overlay button)
  <a> Confirmar a Guada               (z-index: 3, invisible overlay button)
  <a> Confirmar a Chiche              (z-index: 3, invisible overlay button)
</div>
```

**Key assets:**
- `assets/tarjeta resolucion 430 px_g_Mesa de trabajo 1.png` (1792x12281px) — Main design image for 430px screens
- `assets/tarjeta resolucion 375 px_-02-02.png` (1564x10974px) — Responsive variant for ≤375px screens
- `assets/hero-video.mp4` — Video visible through transparent hole in image
- `assets/espejo-atardecer.png` — Video poster/fallback

**Button positions (% of image height, centered horizontally at 35% width):**
| Button | Y center | Target |
|---|---|---|
| Cómo llegar (Ceremonia) | 46.7% | Google Maps: Iglesia Catedral San Luis |
| Cómo llegar (Recepción) | 53.2% | Google Maps: Mirna Eventos |
| Copiar ALIAS | 77.9% | Clipboard: `ayrtonmarini.mp` |
| Confirmar a Guada | 85.4% | WhatsApp: wa.me/5492664301974 |
| Confirmar a Chiche | 86.8% | WhatsApp: wa.me/598098145888 |

**Video hole position:** `top: 12.46%; height: 19.61%; width: 100%; object-fit: cover`

**JavaScript features:** Loader (spinner until `window.load`, 8s timeout), clipboard copy with navigator.clipboard API + fallback, toast feedback.

**Loading:** Full-page spinner (`#loader`) blocks rendering until `window.load` fires (8s safety timeout).

## Key Conventions

- Mobile-only layout (`max-width: 430px` on body)
- Responsive via `<picture>` with `<source media="(max-width: 375px)">`
- All visual content is in the image — buttons are invisible overlays (`background: transparent; border: none`)
- Accessibility: ARIA labels on all interactive elements, `aria-live` toast, `.sr-only` class, `:focus-visible` styling
- Commit messages in Spanish
- Filenames with spaces must be URL-encoded in `srcset` attributes (use `%20`)

## CSS Gotchas

- **Button positions**: All buttons use `position: absolute` with `top` in percentages relative to the image. If the design image changes, re-measure Y positions
- **Measuring transparent holes**: Don't estimate — render image to `<canvas>` and scan for alpha < 128 to find exact transparent pixel ranges. Estimated values can be off by ~1%
- **Z-index stack**: video (1) → image (2, pointer-events: none) → buttons (3)
- **Guada/Chiche overlap**: These buttons are only 1.4% apart; they use `min-height: 38px` (instead of 44px) to avoid overlap
- **srcset spaces**: Filenames with spaces break `srcset` parsing — always encode as `%20`
