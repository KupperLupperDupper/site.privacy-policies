# Design system

This site has a single look, and every page has to follow it. The look is defined in two files:

| File | Contents |
|---|---|
| [`css/tokens.css`](css/tokens.css) | Design tokens (CSS custom properties). This is the **only** file that contains raw values such as hex colors, px and rem. |
| [`css/site.css`](css/site.css) | Base styles and reusable components. Uses tokens only. |

[`styleguide.html`](styleguide.html) shows every token and component. Open it after you change anything in `css/`.

## Rules

1. **Only tokens and components.** Pages use the classes documented below and nothing else. Pages contain no `style="…"` attributes, no `<style>` blocks, and no page-specific CSS files.
2. **No raw values outside `tokens.css`.** `site.css` refers to `var(--token)` and never uses a literal color, font size, spacing or radius. Two exceptions are allowed. The `@media` breakpoints (`40rem`, `60rem`) are written literally, because custom properties can't be used inside media queries. The browser icons (`favicon.svg`, `apple-touch-icon.png`) hard-code the light `--color-accent` and `--color-on-accent`, because an icon file can't read CSS variables. If those tokens change, update the icons too.
3. **Need something new?** Add it to the design system first: a token in `tokens.css`, a component in `site.css`, a section in this file, an example in `styleguide.html`, and a matching entry under `design-system/project/` (tokens or a component folder). Then republish the design system artifact (see `CLAUDE.md`). Only after that, use it on a page.
4. **No external resources.** The site uses no web fonts, CDNs, analytics or tracking pixels. The site is about privacy, so pages make no third-party requests.
5. **Accessibility.** Keep the skip link, use one `<h1>` per page, give the language switcher `aria-current="page"` on the current language, and never remove the focus outline.
6. **Light and dark.** Colors switch automatically through `prefers-color-scheme`. Use only color tokens; never hard-code a color for one mode.

## Tokens

### Color

| Token | Light | Dark | Use |
|---|---|---|---|
| `--color-bg` | `#F7F7F4` | `#111315` | Page background |
| `--color-surface` | `#FFFFFF` | `#191C1F` | Header, cards |
| `--color-text` | `#1B1F24` | `#ECEDEE` | Body text, headings |
| `--color-text-muted` | `#5A636E` | `#A2A9B1` | Meta, descriptions, footer |
| `--color-border` | `#E3E3DD` | `#2A2F34` | Dividers, card borders |
| `--color-accent` | `#2D6A5A` | `#6CC3A6` | Links, active language, check icons |
| `--color-accent-hover` | `#24574A` | `#86D1B8` | Link hover |
| `--color-accent-soft` | `#E3EFEA` | `#1C2D28` | Summary box, app icon background |
| `--color-on-accent` | `#FFFFFF` | `#0E1A16` | Text on top of `--color-accent` |
| `--color-focus` | `#3B82F6` | `#60A5FA` | Keyboard focus ring |

### Typography

The site uses the system font stack (`--font-sans`) and loads no web fonts.

| Token | Value | Use |
|---|---|---|
| `--fs-sm` | 14px | Meta, labels, footer, card description |
| `--fs-base` | 16px | Body |
| `--fs-md` | 18px | Lead text, card titles |
| `--fs-lg` | 22px | Section headings |
| `--fs-xl` | 28px | Page title (mobile) |
| `--fs-2xl` | 36px | Page title (≥ 640px) |
| `--lh-tight` / `--lh-body` | 1.25 / 1.65 | Headings / body |
| `--fw-regular` / `--fw-medium` / `--fw-bold` | 400 / 500 / 650 | |

### Spacing (4px base)

`--space-1` 4 · `--space-2` 8 · `--space-3` 12 · `--space-4` 16 · `--space-5` 24 · `--space-6` 32 · `--space-7` 48 · `--space-8` 64

### Radii

`--radius-sm` 6px (focus ring, small marks) · `--radius-md` 10px (app icon) · `--radius-lg` 16px (cards, summary box) · `--radius-pill` (language switcher, round check icons)

### Sizes

`--page-width` 64rem · `--content-width` 44rem (reading column) · `--header-height` 60px · `--icon-size` 48px · `--tap-min` 40px

### Breakpoints

| Name | Min width | Changes |
|---|---|---|
| md | `40rem` (640px) | Gutter 16 → 32px, title 28 → 36px, 2-column app grid, footer in one row |
| lg | `60rem` (960px) | 3-column app grid |

The styles are written mobile first: the base styles target phones, and the media queries add to them.

## Components

### Page skeleton

Every page has this structure. Copy it from `_template/` rather than writing it from scratch.

```html
<body>
  <a class="skip-link" href="#main">Skip to content</a>
  <header class="site-header">…</header>
  <main id="main" class="site-main">
    <div class="container">…</div>
  </main>
  <footer class="site-footer">…</footer>
</body>
```

`.container` centers content at `--page-width`, with side gutters that match the breakpoints.

### Browser icon

Every page links the brand mark as its tab icon in `<head>`, before the stylesheets. Adjust the relative path to the page's folder depth (`../`, `../../`):

```html
<link rel="icon" href="favicon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="apple-touch-icon.png">
```

- `favicon.svg` is the header shield (`.brand__mark`) in `--color-on-accent` on a `--color-accent` square with `--radius-sm`-like rounding. It is used in browser tabs and bookmarks.
- `apple-touch-icon.png` (180 × 180) is the same mark with square corners, because iOS rounds the corners itself. It is used when the site is saved to a home screen.
- To regenerate the PNG after changing the SVG, remove `rx` from the rect and render it at 180 px, for example with `npx sharp-cli@5.1.0 --input square.svg --output apple-touch-icon.png --density 540 resize 180 180`.

### Header: `.site-header`

```html
<header class="site-header">
  <div class="container site-header__inner">
    <a class="brand" href="/">
      <span class="brand__mark" aria-hidden="true"><svg …shield…></svg></span>
      Privacy Policies
    </a>
    <nav class="lang-switch" aria-label="Language">…</nav>
  </div>
</header>
```

The brand text is "Privacy Policies" on English pages and "Privatlivspolitikker" on Danish pages. It links to the front page in the same language.

### Language switcher: `.lang-switch`

```html
<nav class="lang-switch" aria-label="Language">           <!-- "Sprog" on Danish pages -->
  <a class="lang-switch__link" href="/same-page/" aria-current="page" lang="en">EN</a>
  <a class="lang-switch__link" href="/da/same-page/" hreflang="da" lang="da">DA</a>
</nav>
```

- Both languages always appear, in the order EN, DA.
- The current language gets `aria-current="page"`, which renders it filled.
- The other link points to **the same page** in the other language and carries `hreflang`.

### Hero: `.hero`

Used only on the front page: `.hero__title` (the page's `<h1>`) and `.hero__lead`.

### App grid and app card: `.app-grid`, `.app-card`

```html
<ul class="app-grid">
  <li>
    <a class="app-card" href="app-slug/">
      <span class="app-card__icon" aria-hidden="true">W</span>
      <div>
        <h2 class="app-card__title">App name</h2>
        <p class="app-card__desc">One sentence about the app or system.</p>
        <span class="app-card__cta">Read privacy policy →</span>
      </div>
    </a>
  </li>
</ul>
```

- The whole card is a single link.
- `.app-card__icon` shows the first letter of the name.
- Keep the description to one short sentence.

### Policy page: `.policy`

```html
<article class="policy">
  <a class="back-link" href="../">← All policies</a>
  <h1 class="policy__title">App name</h1>
  <p class="policy__meta">Privacy policy · Last updated <time datetime="2026-10-19">19 October 2026</time></p>

  <aside class="summary" aria-labelledby="summary-title">
    <h2 class="summary__title" id="summary-title">At a glance</h2>
    <ul class="checklist">
      <li class="checklist__item">…</li>
    </ul>
  </aside>

  <section class="policy-section">
    <h2 class="policy-section__title">Heading</h2>
    <p>…</p>
  </section>
</article>
```

- `.summary` lists the main points, 3–5 short `.checklist__item` lines, each a positive statement.
- `.policy-section` blocks are separated by a thin rule. **Contact** is always the last section.
- Plain `<p>`, `<ul>` and `<a>` inside a section are styled automatically.

### Standalone policy page (in-app)

This is a variant of a policy page that an app opens in its own web view, at `<slug>/app/` and `da/<slug>/app/`. It uses the same components with two changes, so the reader can't navigate away from the policy:

- There is no `.back-link`.
- The brand is a `<span class="brand">` instead of a link. The language switcher stays and links to the other language's `app/` page.

In `<head>` it has `<meta name="robots" content="noindex">`, no `canonical`, and `hreflang` links that point to the two `app/` pages. Everything from `.policy__title` to `</article>` must be identical to the normal policy page. The deploy workflow checks this.

### Footer: `.site-footer`

```html
<footer class="site-footer">
  <div class="container site-footer__inner">
    <span>Questions? <a href="mailto:…">…</a></span>
    <span>© 2026</span>
  </div>
</footer>
```

### Utilities

- `.visually-hidden` hides content visually but keeps it available to screen readers.
- `.skip-link` must be the first element in `<body>`.

### Styleguide-only (`.sg-*`)

The `.sg-*` classes (swatches, bars, frames) exist only for `styleguide.html`. Don't use them on site pages.
