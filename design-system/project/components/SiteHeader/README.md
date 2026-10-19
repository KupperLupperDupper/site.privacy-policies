# SiteHeader

The bar at the top of every page, with the brand on the left and the language switcher on the right.

- Markup: `header.site-header > div.container.site-header__inner > a.brand + nav.lang-switch`.
- The brand is `a.brand` with `span.brand__mark` (the shield SVG, `aria-hidden`) followed by the site name: "Privacy Policies" in English, "Privatlivspolitikker" in Danish.
- The brand links to the front page in the same language (`/` or `/da/`).
- Background `color-surface` with a `color-border` hairline below, minimum height `header-height`.
