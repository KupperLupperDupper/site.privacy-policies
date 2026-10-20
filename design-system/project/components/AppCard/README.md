# AppCard

A link card on the front page for one app or system, which leads to its privacy policy.

- Markup: `ul.app-grid > li > a.app-card`. Inside the card: `span.app-card__icon` (the app name's first letter, `aria-hidden`), then a `div` holding `h2.app-card__title`, `p.app-card__desc` and `span.app-card__cta`.
- The whole card is one link. Keep the description to one short sentence.
- The CTA reads "Read privacy policy →" in English and "Læs privatlivspolitik →" in Danish.
- The grid has 1 column below 640px, 2 columns from 640px and 3 from 960px. Cards in a row share the same height.
- Cards use `color-surface`, `radius-lg` and `shadow-card`. On hover the border turns `color-accent`.
