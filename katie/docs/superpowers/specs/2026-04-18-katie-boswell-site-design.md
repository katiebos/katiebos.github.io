# Katie Boswell Consulting — Static Site Design

**Date:** 2026-04-18
**Status:** Approved (brainstorm) — ready for implementation plan

## Goal

Rebuild Katie Boswell's freelance-consultant website as a static, single-page
site on top of a Start Bootstrap template. Reuse her existing copy and imagery
from the current Webflow site, and re-theme the template with her brand
palette and editorial typography.

## Non-goals

- Custom logo / favicon work (bootstrap default placeholder kept)
- Analytics, tracking, cookie banners
- Blog or writing section
- Per-service sub-pages
- CMS or build system (plain HTML/CSS/JS)
- Contact form infrastructure (see "Contact" below)

## Architecture

Single-file static site served from `katie/`:

```
katie/
├── index.html          # one page, anchor-scrolled sections
├── css/styles.css      # Bootstrap overrides + brand theming
├── js/scripts.js       # empty; Bootstrap bundle handles nav
├── assets/
│   ├── favicon.ico
│   └── img/
│       ├── hero.jpg
│       ├── katie.avif
│       ├── systems-thinking.jpg
│       ├── strategy.jpg
│       └── impact-learning.jpg
└── docs/superpowers/specs/
    └── 2026-04-18-katie-boswell-site-design.md
```

Bootstrap 5.3 loaded from CDN (as in the starter template). Google Fonts
(Fraunces + Inter) loaded from CDN. No build step.

## Page structure (top to bottom)

1. **Sticky navbar** — plain-text "Katie Boswell" brand on the left; anchor
   links on the right: *About · Services · Contact*. Bootstrap's
   `navbar navbar-expand-lg` with custom colour overrides.
2. **Hero** (`#top`) — headline *"See the whole. Shift the system."*,
   intro paragraph, two pill buttons (*About me* → `#about`,
   *Get in touch* → `#contact`). Hero image beside text on desktop,
   stacked on mobile. Bootstrap `row` with `col-lg-6` halves.
3. **About** (`#about`) — section heading *"About me"*, headshot on the
   left (`col-lg-5`), bio paragraphs on the right (`col-lg-7`). Reversed
   column order vs hero for visual rhythm. Mobile: stacked.
4. **Services** (`#services`) — heading *"Services"*, short approach
   paragraph, then 3 Bootstrap cards in a `row row-cols-1 row-cols-md-3 g-4`.
   Each card: image on top, service name, short blurb, full
   detail paragraph below. All visible — no collapse/expand.
5. **Contact** (`#contact`) — heading *"Get in touch"*, intro paragraph,
   email shown as a large burgundy link (`katie@katieboswell.co.uk`),
   plus a pill *Email Katie* button (`mailto:` link).
6. **Footer** — cream-on-charcoal strip: copyright line + email repeated
   as `mailto:` link.

## Visual design

### Palette

| Token | Hex | Use |
|---|---|---|
| `--cream` | `#f2efe9` | Page background, light section background |
| `--charcoal` | `#1c1c20` | Body text, footer background |
| `--burgundy` | `#8b3a4a` | Primary buttons, links, accent |
| `--forest` | `#3d6b5e` | Secondary accents, hover states |
| `--stone` | `#9a958d` | Muted text, subtle borders |

These override the relevant Bootstrap tokens (`--bs-primary`, `--bs-body-bg`,
`--bs-body-color`, `--bs-link-color`, etc.).

### Typography

- **Headings:** Fraunces, 700 for h1/h2, 600 for h3. `letter-spacing: -0.01em`
  on h1. Loaded from Google Fonts with `display=swap`.
- **Body:** Inter, 400 regular, 500 for buttons/nav. 17px base font-size on
  desktop (up from Bootstrap's default 16px) to suit the editorial feel.
- **Fallbacks:** `Georgia, serif` for Fraunces; `system-ui, sans-serif` for
  Inter.

### Components

- **Buttons:** fully rounded pill (`border-radius: 999px`), generous
  horizontal padding (~1.5rem). Primary is filled burgundy with cream text;
  secondary is charcoal outline. Hover: darken ~10%, subtle
  `translateY(-1px)` lift.
- **Nav:** transparent background on cream; burgundy hover underline. Burger
  menu on mobile uses Bootstrap's default collapse.
- **Cards:** Bootstrap default with cream background, soft 8px radius on
  images, no heavy shadow — just a faint 1px stone-coloured border.
- **Section rhythm:** alternating plain cream (`#f2efe9`) and a slightly
  warmer/darker cream (`#ebe6dc`) band for visual separation. Vertical
  padding via a `.section` helper:
  `padding-block: clamp(4rem, 8vw, 7rem)`.
- **Content width:** Bootstrap's default `container` (max ~1140px).

## Bootstrap theming strategy

Keep Bootstrap 5.3 intact via CDN. Override via a small custom stylesheet
loaded *after* Bootstrap. `css/styles.css` contains only:

1. Google Fonts `@import` or `<link>` in HTML head
2. `:root` overrides of Bootstrap CSS variables (`--bs-primary`,
   `--bs-primary-rgb`, `--bs-body-bg`, `--bs-body-color`,
   `--bs-body-font-family`, `--bs-link-color`, `--bs-link-hover-color`,
   `--bs-border-color`) to brand values
3. Custom brand tokens (`--cream`, `--burgundy`, etc.) for clarity
4. Heading font rule: `h1,h2,h3,.navbar-brand { font-family: 'Fraunces', … }`
5. `.btn` pill override + hover transform
6. `.section` spacing helper
7. Minor nav hover polish

Target: under ~150 lines. No custom grid, no custom responsive breakpoints,
no custom component rewrites.

## Content mapping

All copy is verbatim from Katie's existing Webflow site
(`/home/shaun/Downloads/katies-site/`). No rewrites in v1.

| Section | Source file |
|---|---|
| Hero headline + intro | `Katie Boswell Consulting.html` |
| About bio (3 paragraphs) | `About.html` |
| Services intro | `Katie Boswell Consulting.html` |
| Systems thinking card blurb | home page card text |
| Systems thinking card detail | `Systems thinking.html` body |
| Strategy card blurb | home page card text |
| Strategy card detail | `Strategy.html` body |
| Impact & learning card blurb | home page card text |
| Impact & learning card detail | `Impact & learning.html` body |
| Contact intro | `Katie Boswell Consulting.html` |

Images copied into `assets/img/` with clearer names:

| New filename | Source |
|---|---|
| `hero.jpg` | `69dfa92f0a2bf4ea867c5505_pexels-ethan-sees-1319959-2853432.jpg` |
| `katie.avif` | `69dfa951b18552d426ec5e32_KB headshot 2026.avif` (from About_files) |
| `systems-thinking.jpg` | `69dfb50959d2fcdccc252de2_pexels-victor-s-349386206-14173845.jpg` |
| `strategy.jpg` | `69dfb75559d2fcdccc25cec6_pexels-alexasfotos-2277784.jpg` |
| `impact-learning.jpg` | `69dfb78ae1a4a2283fa1e6c4_pexels-ketut-subiyanto-4623496.jpg` |

All images get short descriptive `alt` text (her existing site has empty
alts; this is an accessibility improvement).

## Contact

No form. Email shown prominently as a burgundy link, with a pill
*Email Katie* button using a `mailto:` link:

```html
<a class="btn btn-primary btn-lg"
   href="mailto:katie@katieboswell.co.uk">Email Katie</a>
```

Rationale: Katie's visitors (senior staff at mission-driven orgs) are
comfortable with email, a visible address builds trust, zero infrastructure.
If spam becomes an issue later, swapping in Formspree is a ~10-minute
change.

## Accessibility

- Semantic landmarks (`<nav>`, `<main>`, `<section>`, `<footer>`)
- Skip-to-content link at top
- All images get meaningful `alt` text
- Colour contrast: burgundy on cream and charcoal on cream both clear WCAG
  AA (verify during implementation)
- Focus rings retained (no `outline: none` without replacement)
- Nav works with keyboard (Bootstrap default)

## Responsive behaviour

Rely entirely on Bootstrap breakpoints:

- **< 768px (mobile):** single-column everywhere, hamburger nav,
  images above text in hero and about, cards stack full-width.
- **≥ 768px (tablet):** hero and about go to two-column; services grid
  becomes 3-column (`row-cols-md-3`) — Katie's service cards are short
  enough to read well at tablet width.
- **≥ 992px (desktop):** full layout as designed; container caps at
  ~1140px.

## Testing / verification

- Open `index.html` in a browser (local file or simple static server) and
  visually confirm each section renders correctly on desktop and mobile
  widths (use devtools responsive mode).
- Tab through the page to confirm keyboard focus is visible and ordered
  logically.
- Click anchor links — each should smooth-scroll to the right section.
- Click the email button — should open the default mail client.
- Validate HTML via the W3C validator (optional sanity check).
- Confirm no console errors.

## Out of scope

- SEO / meta-tag tuning beyond a sensible `<title>` and `<meta
  name="description">`
- Open Graph / Twitter card tags
- Sitemap / robots.txt
- Performance optimisation (images are already reasonably sized from the
  source)
- Dark mode
