# LanguageSwitcher

A pill with one link per language that switches to the same page in the other language.

- Markup: `nav.lang-switch` (`aria-label` "Language" or "Sprog") with two `a.lang-switch__link` elements, always in the order EN, DA.
- Mark the current language with `aria-current="page"`, which fills it with `color-accent` and `color-on-accent`.
- The other link points to the same page in that language and carries `hreflang` and `lang`.
- Every link is at least `tap-min` wide. Keep the switcher visible at every width.
