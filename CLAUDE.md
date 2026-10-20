# Notes for Claude

## Design system lives in two places — keep them in sync

The repo is the source of truth. The same design system is also published as a Design System artifact:
https://claude.ai/artifact/5mDkJMGYfxg6GQvHyCd3Pf

Whenever you change any of these, republish the artifact in the same turn:

| Repo file | Artifact path |
|---|---|
| `css/site.css` | `project/components/bundle.css` (published straight from the repo file) |
| `DESIGN.md` | `project/README.md` (published straight from the repo file) |
| `css/tokens.css` | update `design-system/project/tokens.json` by hand to the same values, then publish it |
| `favicon.svg`, `apple-touch-icon.png` | upload as assets (`asset: true`), then update `assetGroups.Logos` in `design-system/project/design-system.json` with the new blob ids and sizes |
| a new or changed component in `css/site.css` | add or update `design-system/project/components/<Name>/preview.html` and `README.md`, and `styleguide.html` |

How to publish: use the Artifact tool, `url` = the link above, `root` = `design-system`, and `files` mapping each changed `project/...` path. For the two repo files, use `{"from": "<absolute path>"}`. `file_path` must be a file inside `design-system/`, so pass `design-system/project/tokens.json` even if only a repo file changed. Send `project/design-system.json` (the index) last, and only when you change one of its keys; update its `lastChange` when you do.

Also follow `DESIGN.md`: pages use only tokens and components, with no one-off styles.
