# fishburg-site (old address, redirects only)

The Fishburg pages moved on 2026-10-28 to **https://beydadonmezz.github.io/fishburg/** (source: `site/` in the game's repo, published from the user-site repo `beydadonmezz/beydadonmezz.github.io` as `public/fishburg/`).

This repo stays published **forever**: App Store Connect, Play Console, AdMob's consent message and app builds made before the move link to its pages. Never delete, rename or unpublish it, and never remove a page from it.

Each old page redirects to the matching new page:

| Old | New |
|---|---|
| `/fishburg-site/` | `/fishburg/` |
| `/fishburg-site/privacy/` (also `#choices` and every other anchor) | `/fishburg/privacy/` |
| `/fishburg-site/terms/` | `/fishburg/terms/` |
| `/fishburg-site/support/` | `/fishburg/support/` |
| `/fishburg-site/delete-account/` | `/fishburg/delete-account/` |
| `/fishburg-site/tr/…` (the same five) | `/fishburg/tr/…` |
| any other path (`404.html`) | `/fishburg/` |

Each page has a meta refresh, a one-line script that keeps the `#anchor`, `rel="canonical"` to the new page and a visible link. The source of these files is `site-legacy/` in the game's repo; publish with the commands in its `site/README.md`.
