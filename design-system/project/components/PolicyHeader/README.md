# PolicyHeader

The top of a policy page: a link back to the front page, the app name as `h1`, and the last-updated line.

- Markup inside `article.policy`: `a.back-link` ("← All apps" / "← Alle apps"), then `h1.policy__title`, then `p.policy__meta` with a `<time datetime="YYYY-MM-DD">`.
- `article.policy` limits the reading column to `content-width`.
- Write dates out in full: "19 October 2026" in English, "19. oktober 2026" in Danish.
