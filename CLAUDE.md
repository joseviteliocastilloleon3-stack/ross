# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file static website (`index.html`) for **Zenith-KO**, a Peruvian chicken wings restaurant in Trujillo, Peru. Everything — HTML markup, CSS styles, and JavaScript logic — lives in one file (~900 lines). There is no build system, no package manager, and no backend.

## Development

**To preview:** Open `index.html` directly in a browser. No server required.

There are no build, lint, or test commands. The file is the deployable artifact.

**External dependencies (CDN only):**
- Google Fonts: `Bebas Neue`, `DM Sans`, `DM Mono`

All product images are base64-encoded and embedded directly in `index.html`, which is why the file is ~1MB despite being a single HTML file.

## Architecture

The file is organized into sequential `<style>`, `<body>`, and `<script>` blocks. CSS is grouped by section with comment headers (e.g., `/* ── NAV ── */`, `/* ── HERO ── */`).

**Page sections (in DOM order):** Nav → Hero → Carta (menu) → Galería → Cómo Pedir → Sobre → Testimonios → Contacto → Footer. Floating elements (cart FAB, WhatsApp button, cart drawer) live outside the main flow.

### Core data structures

Product catalog — a JS array at the top of the script block:
```js
const SABORES = [{ n, desc, p, img, badge }, ...];  // 7 wing flavors at S/ 20 each
```
Image map — base64 JPEG blobs keyed by short name (`bbq`, `bufalo`, `acevi`, `parri`, `extra`, `marac`, `honey`).

Cart state — module-level `let cart = []` where each item is `{ n, p, qty }`.

Order history persisted to `localStorage` under the key `zk_pedidos`.

### Ordering flow

`addCart(index)` → updates `cart` → `renderCart()` redraws the slide-out panel → `pedirWA()` formats the order as a WhatsApp message and opens `wa.me`.

### CSS conventions

- Custom properties: `--or` (orange `#E8773A`), `--bk` (black), `--wh` (white), `--g3`–`--g6` (grayscale scale), `--rad` (border radius).
- Short class names tied to sections: `.scard` (menu card), `.stit` (section title), `.stag` (section tag), `.cgrid` (menu grid), `.tcard` (testimonial card).
- Responsive breakpoints: 900px and 480px.
- Scroll-reveal pattern: elements get a `.rev` class; an `IntersectionObserver` in `observeRev()` adds `.visible` to trigger CSS transitions.

### JavaScript conventions

Object property shorthand is used aggressively (`n` = name, `p` = price, `qty` = quantity, `ex` = existing cart item). Keep this style when modifying data structures.
