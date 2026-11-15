# Perth Paws

A small static website for two Perth pet brands, plus the brand kit behind them:

| Page | What it is | Live path |
| --- | --- | --- |
| `index.html` | **PawPing**: landing page for personalised QR pet tags with Apple Find My tracking | `/` |
| `pawfection.html` | **Pawfection**: George Andrew's dog services in Fremantle (a PawPing partner) | `/pawfection` |
| `styleguide.html` | **Brand kit**: logo, colour, type and voice guidelines | `/styleguide` |

It's plain HTML, CSS and JavaScript, so there's nothing to install or build. Open any `.html` file in a browser, or deploy the folder as-is.

---

## Folder structure

```
perth-paws/
├── index.html          PawPing landing page
├── pawfection.html     Pawfection dog services page
├── styleguide.html     Brand kit / style guide
├── assets/
│   ├── motion.css      Shared animation styles (all three pages)
│   └── motion.js       Shared animation behaviour (all three pages)
├── images/
│   ├── *.webp          Optimised images used by the pages
│   ├── *.png           Original full-size artwork (kept as source files)
│   ├── favicon-64.png, apple-touch-icon.png
│   └── og-*.jpg        Social share previews (1200×630)
├── vercel.json         Hosting config: clean URLs, caching, security headers
├── LICENSE             MIT
└── README.md
```

### How each page is built

- **Styling**: `index.html` and `pawfection.html` use [Tailwind CSS via the CDN script](https://tailwindcss.com/docs/installation/play-cdn). Each page sets its own brand colours and fonts in the `<head>` (`tailwind.config` on PawPing, CSS variables on Pawfection). `styleguide.html` uses hand-written CSS with CSS variables (`--green`, `--sand`, `--wood`, `--ocean`, `--sun`, `--bg`).
- **Page-specific CSS** lives in a `<style>` block in each page's `<head>`. **Page-specific JS** (mobile menus, navbar background on scroll) is in a `<script>` block at the end of each page's `<body>`.
- **Shared animation** comes from `assets/motion.css` and `assets/motion.js`, linked from every page's `<head>` (see below).
- **Icons**: PawPing uses inline SVGs. Pawfection uses [Bootstrap Icons](https://icons.getbootstrap.com/), [Lordicon](https://lordicon.com/) animated icons, and an inline SVG `<symbol id="paw">` that's reused with `<use href="#paw">`.

### Page sections

**PawPing (`index.html`)**: nav (with mobile drawer), hero with phone mock-up, scrolling "paw band", stats (count-up), features, products, how it works, testimonials, pricing, partners (links to Pawfection), contact, footer. Each section starts with a comment block describing it.

**Pawfection (`pawfection.html`)**: nav, hero, services marquee, services (7 cards + a booking card), shop, Meet George (photo collage), contact (email and phone), footer.

**Brand kit (`styleguide.html`)**: logo system, colour palette (click a swatch to copy its hex), typography, personality and voice, and example applications.

---

## Motion layer

`assets/motion.js` has no dependencies. It reads `data-` attributes, so you can add an effect to any element without writing JavaScript:

| Attribute | Effect |
| --- | --- |
| `data-reveal="up"` | Animates in when scrolled into view. Also `left`, `right`, `zoom`, `pop`, `tilt` |
| `data-stagger="pop"` *(on a parent)* | Reveals each child one after another. Set the gap with `data-stagger-step="80"` (ms) |
| `data-split` | Headline words bounce up in sequence |
| `data-count="10000" data-suffix="+"` | Number counts up when visible. Optional: `data-prefix`, `data-decimals="1"` |
| `data-tilt` or `data-tilt="5"` | Card tilts in 3D toward the mouse with a soft glare (the number is the max angle) |
| `data-magnetic` | Button drifts toward the mouse |
| `data-parallax="0.15"` | Moves at a different speed while scrolling (negative values move the other way) |
| `data-boop` or `data-boop="#fff,#f97316"` | Burst of paws and hearts on click, in optional colours |
| `data-copy="#5A7D3D"` | Copies the text to the clipboard and shows a toast |
| `.pp-marquee > .pp-marquee-track` | Infinite scrolling band. Speed in seconds via `data-speed="34"` |

These run automatically on every page:

- **Intro curtain**: paw prints stamp across the screen on the first visit of a session, then the curtain lifts.
- **Page transitions**: links to the other pages wipe the curtain up before navigating.
- **Paw-print cursor trail**: mouse only. Turn it off for a page with `<body data-no-trail>`.
- **Back-to-top button** with a ring that fills as you scroll.
- **Reduced motion**: if the visitor's device asks for less motion, everything is shown immediately and nothing moves.

Each page sets the motion colours in its own CSS:

```css
:root {
  --pp-accent: #FF6B4A; /* paw trail, progress ring, focus outline */
  --pp-ink:    #2C1810; /* curtain + back-to-top background */
  --pp-paper:  #FFF8F0; /* back-to-top icon */
}
```

A one-line script at the top of each `<head>` adds `class="js"` to `<html>` (so hidden "before" states only apply when JavaScript runs) and paints the intro curtain before the first frame.

---

## Working on it

```bash
# preview locally with any static server, e.g.
npx serve .
# or
python3 -m http.server 8000
```

**Adding images**: keep the original in `images/` and save a `.webp` copy (around 800px wide for cards, 1400px for heroes). Reference the `.webp` with `width`, `height` and `loading="lazy"`.

---

## Deployment (Vercel)

The site deploys to Vercel as a static site, with no build command and the repository root as the output directory.

- `vercel.json` turns on **clean URLs** (`/pawfection` instead of `/pawfection.html`), caches `images/` for 30 days and `assets/` for an hour, and adds basic security headers.
- With the GitHub repo connected to the Vercel project, every pull request gets a preview URL and merging to `main` deploys to production.

---

## Next steps

- Swap the Tailwind CDN script for a compiled stylesheet (Tailwind CLI) to remove the runtime and the console warning.
- Replace placeholder contact details (`0400 000 000`, social links set to `#`) with real ones.
- Fill in `[Brand Name]` in the brand kit once the dog services name is final.
