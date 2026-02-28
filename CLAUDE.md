# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static single-page wedding website for Guadalupe & Ayrton. Everything lives in a single `index.html` file with embedded CSS and JavaScript. No build tools, no dependencies, no package.json.

**Live site:** https://chichex.github.io
**Language:** Spanish (Argentina/Uruguay audience)

## Development

Serve locally with any HTTP server:
```bash
python3 -m http.server 8000
# or
npx http-server
```

Deploy by pushing to `master` — GitHub Pages auto-deploys from the remote `https://github.com/chichex/chichex.github.io.git`.

There are no tests, linting, or build steps.

## Architecture (all in index.html)

**Structure:** `<style>` block → HTML body → `<script>` IIFE (bottom of file)

**Sections in order:**
1. **Hero** (`#hero`) — Torn paper frame (`papel rasgado doble más espacio - papel chico.png`) overlaying video/poster (`espejo-atardecer.png`). Title and phrase are positioned absolutely over the video inside `.hero__overlay-top` and `.hero__overlay-bottom` (z-index 4, above the torn paper overlay at z-index 3). Uses `aspect-ratio: 1724/3429` to match the paper image proportions. Video uses `object-fit: contain`.
2. **Timeline/Cronograma** (`#timeline`) — Wedding schedule with Google Maps links. Olive green card with paper texture. Dress code integrated inside the timeline track.
3. **Contact** (`#contact`) — WhatsApp deeplinks. White card, outline-style buttons.
4. **Gift** (`#gift`) — Mercado Pago alias with copy-to-clipboard. White card, outline-style buttons.
5. **Footer** — Olive green background with couple names and date.

**CSS design system** uses custom properties in `:root` for a warm/earthy palette (terracotta `#9C5540`, olive green `#6B705C`, paper-bg `#F5F0E8`, chalk-white `#FAF7F2`, gold `#C9A96E`). Typography: Boheme Floral (custom TTF for titles), Lavishly Yours (script), Cormorant Garamond (serif body), Lexend Zetta (sans labels). Uses `clamp()` for fluid scaling. Responsive breakpoints at 768px and 1024px.

**JavaScript features:** Intersection Observer scroll animations (`.fade-in`, `.fade-in-left`, `.fade-in-right`), clipboard copy with fallback, desktop dot navigation, reduced-motion support.

**Key assets:**
- `Boheme Floral.ttf` — Custom font for titles
- `espejo-atardecer.png` — Hero poster/fallback image
- `hero-video.mp4` — Hero background video
- `papel rasgado doble más espacio - papel chico.png` — Torn paper frame overlay for hero
- `papel fondo-01.png` — Paper texture used in timeline background

## Key Conventions

- Mobile-first responsive design
- Accessibility: ARIA labels, `aria-live` for dynamic content, `.sr-only` class, `prefers-reduced-motion` support, `:focus-visible` styling
- SVG icons are inlined directly in HTML
- Decorative paper-grain texture via SVG filter data URI on `body::after`
- Commit messages in Spanish
- Contact and gift cards use white (`#ffffff`) backgrounds with outline-style buttons
- Timeline card uses olive green with paper texture: `linear-gradient(rgba(107,112,92,0.75), ...), url('papel fondo-01.png')`
