# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static HTML/CSS website for **Voltura Power Group**, a licensed electrical contractor (License #3001608) based in Colorado Springs, CO. No build system, no framework, no npm — everything is plain HTML, CSS, and vanilla JS. Deployed via GitHub Pages (see `CNAME`: `www.volturapower.energy`).

## Architecture

**Single shared stylesheet:** `styles/main.css` — all pages reference this one file. Changes here affect every page.

**Page types:**
- `index.html` — main homepage with all major sections (hero, services, reviews, FAQ, contact, photo gallery, booking modal)
- Service pages (`ev-charger-install.html`, `panel-upgrade.html`, `lighting.html`, etc.) — individual service detail pages
- Location pages (`colorado-springs.html`, `falcon.html`, `pueblo.html`, etc.) — SEO-targeted area pages
- `contact.html` / `contact-n8n.html` — contact forms (n8n variant has its own inline styles)
- `thank-you.html` — form submission redirect target

**Form submissions** go to an n8n webhook. The URL is defined as `WEBHOOK_URL` in an inline `<script>` block at the bottom of each page that has a booking form. The production webhook is at `wards-electrical.app.n8n.cloud`.

**Photo gallery** (`assets/photos/`): currently `photo-1.jpg` through `photo-6.jpg` plus `logo.jpg`. The lightbox on the homepage is driven by vanilla JS in `index.html`'s inline script.

## Design system (CSS variables in `main.css`)

"Silver & Light" theme — light cream background, dark text, gold accent.

| Variable | Value | Role |
|---|---|---|
| `--bg` | `#F0EFEB` | Page background (light cream) |
| `--bg-alt` | `#E8E6E0` | Alternate section background |
| `--bg-dark` | `#1C1C1C` | Dark sections (trust bar, reviews, footer) |
| `--gold` / `--gold-deep` | `#C9A227` / `#8B6914` | Primary accent |
| `--text` | `#1C1C1C` | Primary text |
| `--text-mid` / `--text-lt` | `#534F48` / `#6E6A62` | Secondary text (keep dark enough for WCAG AA on cream) |
| `--serif` / `--sans` | Cormorant Garamond / DM Sans | Display / body fonts |

Key CSS patterns:
- `.section-alt` / `.section-dark` — alternate/dark section backgrounds
- `.modal.is-open` / `.lightbox.is-open` — toggled by JS to show modals; `body.modal-open` disables scroll
- `.mobile-call-bar` — sticky bottom bar on mobile (`__btn` call link + `__book` quote button); shown ≤768px
- Form inputs are 16px+ on purpose — prevents iOS auto-zoom on focus
- Breakpoints: `1024px`, `768px` (mobile nav + call bar), `480px`

## Adding a new page

Copy an existing service or location page as a template. Every page should include:
1. The `<link rel="stylesheet" href="styles/main.css" />` reference (adjust path depth if in a subdirectory)
2. The sticky action bar (`.action-bar`) and site header (`.site-header`) markup
3. JSON-LD `LocalBusiness` schema in `<head>`
4. The booking modal markup + inline JS with `WEBHOOK_URL` set to the production n8n webhook
5. The mobile call bar (`#mobileCallBar`) at the bottom of `<body>`
6. A footer with the `#year` span (auto-filled by JS)

## Adding project photos

Add images to `assets/photos/` (naming convention: `photo-N.jpg`). Then add a `<figure class="photoCard">` block in the photoGrid in `index.html` and a matching `ImageObject` entry in the JSON-LD `ImageGallery` schema block.

## Contact info (do not change without instruction)

- Phone: `915-487-0660`
- Email: `service@volturapower.energy`
- License: `#3001608`
- Domain: `www.volturapower.energy`
