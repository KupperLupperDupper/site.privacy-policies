# Privacy policies for my apps and systems

A static website with the privacy policies for all my apps, systems and tools (mobile apps, web apps, desktop tools and anything else I publish), in English and Danish. It's plain HTML and CSS with no build step, published with GitHub Pages.

Live site: https://kupperlupperdupper.github.io/site.privacy-policies/

| URL | File |
|---|---|
| `/` | `index.html`: English front page |
| `/<app-slug>/` | `<app-slug>/index.html`: English policy |
| `/da/` | `da/index.html`: Danish front page |
| `/da/<app-slug>/` | `da/<app-slug>/index.html`: Danish policy |
| `/styleguide.html` | All design tokens and components |

On the site now: **BetBook** (`/betbook/`, `/da/betbook/`) and **Ponvia** (`/ponvia/`, `/da/ponvia/`).

## Add a new app or system

Example: an app called "Habit Log". The steps are the same for a web app, a desktop tool or any other system.

1. **Pick the slug.** Take the app's display name, make it lowercase, replace spaces with hyphens and remove special characters (æ→ae, ø→oe, å→aa). "Habit Log" becomes `habit-log`.
2. **English page.** Copy `_template/en/index.html` to `habit-log/index.html`.
3. **Danish page.** Copy `_template/da/index.html` to `da/habit-log/index.html`.
4. **Fill in the placeholders** in both files:

   | Placeholder | Example |
   |---|---|
   | `{{APP_NAME}}` | `Habit Log` |
   | `{{APP_SLUG}}` | `habit-log` |
   | `{{LAST_UPDATED_ISO}}` | `2026-11-02` |
   | `{{LAST_UPDATED}}` | `2 November 2026` (EN) / `2. november 2026` (DA) |
   | `{{YEAR}}` | `2026` |

5. **Match the policy to what it really does.** Sections marked `OPTIONAL` / `VALGFRIT` (export, biometric lock, reminders, device backups) stay only if it really does that. The templates are written for an app installed on a phone. For anything else, rewrite the sections that don't fit. For example, a web app that runs in the browser stores data in the browser instead of on the device and is cleared by clearing site data instead of uninstalling, and a system with a server must describe what the server stores. Then delete the instruction comment at the top of each file.
6. **Add a card to both front pages.** Copy the Ponvia `<li>` in `index.html` and `da/index.html`, then change the link, the first letter in `app-card__icon`, the name and the one-sentence description. Keep the cards in alphabetical order.
7. **Check it locally** (see below), then push to `main`.

The deploy workflow refuses to publish if any `{{…}}` placeholder is left in a published page.

## Change the contact email

Each language has its own contact address, and it appears on every page in that language and in its template:

| Language | Address | Files |
|---|---|---|
| English | `support@stormitron.com` | root pages, `404.html`, `_template/en/` |
| Danish | `support@stormitron.dk` | everything under `da/`, `_template/da/` |

To change an address everywhere at once, replace `new@example.com` below with the new address:

```bash
grep -rl 'support@stormitron.com' --include=*.html . | xargs sed -i 's/support@stormitron\.com/new@example.com/g'
```

Use `support@stormitron.dk` in the command to change the Danish address. Contact details are part of the policy, so also update the "Last updated" date on each policy page.

## Follow the design system

All styling comes from the design system. Read [DESIGN.md](DESIGN.md) before you change anything.

- Pages link `css/tokens.css` and `css/site.css` and nothing else.
- Use only the documented components (`site-header`, `lang-switch`, `app-card`, `summary`, `policy-section`, …). Pages have no `style="…"` attributes, no `<style>` blocks and no extra CSS files.
- Raw values (colors, sizes, spacing) live only in `css/tokens.css`.
- To add something new, add it to the design system first: a token or component, a `DESIGN.md` entry and an example in `styleguide.html`.
- Open `/styleguide.html` to see every token and component.

The design system is also published as a Design System artifact in Claude, built from the same files: https://claude.ai/artifact/5mDkJMGYfxg6GQvHyCd3Pf. When you change `css/` or `DESIGN.md`, republish it as described in [CLAUDE.md](CLAUDE.md).

## Every page needs

- `<html lang="en">` or `<html lang="da">`.
- `hreflang` links (`en`, `da`, `x-default`) and a `canonical` link with absolute URLs.
- A language switcher that links to **the same page** in the other language, with `aria-current="page"` on the current language.
- Relative links for everything inside the site (`../css/site.css`, `../da/habit-log/`), so the site works under any base path. The one exception is `404.html`, which uses `/site.privacy-policies/…`.

## Run it locally

```bash
npx http-server@14.1.1 . -p 8080 -c-1
```

Then open http://localhost:8080/.

## Enable GitHub Pages

1. Push this repo to GitHub as `site.privacy-policies`. The repo must be public for Pages on a free account.
2. On GitHub, go to **Settings → Pages → Build and deployment → Source** and choose **GitHub Actions**.
3. Push to `main`, or run the **Deploy to GitHub Pages** workflow by hand under **Actions**.
4. The site goes live at `https://kupperlupperdupper.github.io/site.privacy-policies/`.

The workflow (`.github/workflows/deploy.yml`) publishes only the site: the pages, `css/`, `styleguide.html` and `404.html`. `_template/`, `design-system/` and the Markdown files are not published.

**If you rename the repo or add a custom domain**, update the absolute URLs in the `canonical` and `hreflang` links (search for `kupperlupperdupper.github.io`) and the `/site.privacy-policies/` paths in `404.html`.
