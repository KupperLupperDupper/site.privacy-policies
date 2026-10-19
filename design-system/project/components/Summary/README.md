# Summary

The "At a glance" box near the top of a policy page, which lists the main points as checks.

- Markup: `aside.summary` (`aria-labelledby` its title) with `h2.summary__title` ("At a glance" / "Kort fortalt") and `ul.checklist > li.checklist__item`.
- Use 3 to 5 items, each a short positive statement. The check icon comes from CSS, so don't add one in the markup.
- Background `color-accent-soft`, `radius-lg`, padding `space-5`. Use one summary per page.
