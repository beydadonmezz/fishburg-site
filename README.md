# fishburg-site

Public website for **Fishburg** (the game; its App Store name is **"Fishburg: Aquarium"**): home page, Privacy Policy, Terms of Use and Support, in English and Turkish. Plain static HTML plus one CSS file. There is no build step, no JavaScript, no cookies, no trackers and no external requests; each page sets a strict Content-Security-Policy.

Live URL (after publishing): `https://beydadonmezz.github.io/fishburg-site/`

| Page | English | Turkish |
|---|---|---|
| Home | `/` | `/tr/` |
| Privacy Policy | `/privacy/` (Privacy Choices: `/privacy/#choices`) | `/tr/privacy/` |
| Terms of Use | `/terms/` | `/tr/terms/` |
| Support | `/support/` | `/tr/support/` |

## Developer name (filled 2026-09-28)

CRUI's rule: use the seller name exactly as App Store Connect shows it, **BEYDANUR DONMEZ**. Its first mention on each page adds the correctly spelled name once: "BEYDANUR DONMEZ (Beydanur Dönmez)". Every later mention on that page, including a later copyright line, is "BEYDANUR DONMEZ". The pages are already filled. To redo it from a template containing `DEVELOPER_NAME`:

```sh
./fill-developer-name.sh "BEYDANUR DONMEZ" "Beydanur Dönmez"
```

## Files that are NOT part of the published site

Don't copy these two files to the `fishburg-site` repo:

- `_user-site-snippet.html`: the Fishburg entry for the existing user site `beydadonmezz/beydadonmezz.github.io`. Add it to that repo's `index.html` next to LINGRID, and don't remove LINGRID. That repo's `app-ads.txt` already has the right AdMob publisher line (`pub-6308223268641775`), so it needs no change.
- `fill-developer-name.sh`: a local helper.

## Publish on GitHub Pages

1. Create the **public** repository `beydadonmezz/fishburg-site` with no README and no license, so it starts empty.
2. Copy the site into a fresh folder, without the two local files, and push:
   ```sh
   rsync -a --exclude '_user-site-snippet.html' --exclude 'fill-developer-name.sh' ./ ../fishburg-site-publish/
   cd ../fishburg-site-publish
   git init -b main && git add -A && git commit -m "site: initial legal site"
   git remote add origin https://github.com/beydadonmezz/fishburg-site.git
   git push -u origin main
   ```
3. On GitHub, open **Settings → Pages → Build and deployment**. Set **Source** to "Deploy from a branch", **Branch** to `main`, folder `/ (root)`, and click **Save**.
4. After a minute or two, check that these return 200: `/fishburg-site/`, `/privacy/`, `/terms/`, `/support/` and their `/tr/` versions. Also check that the language switch works.
5. App Store Connect already points at these URLs (docs/accounts.md §2.5):
   - Privacy Policy: `https://beydadonmezz.github.io/fishburg-site/privacy/`
   - Privacy Choices: `https://beydadonmezz.github.io/fishburg-site/privacy/#choices`
   - Support: `https://beydadonmezz.github.io/fishburg-site/support/`
   - Marketing: `https://beydadonmezz.github.io` (the user site, where app-ads.txt lives)
6. Then add the snippet to the user site (see above).

## Notes

- `404.html` uses absolute `/fishburg-site/...` links, because GitHub Pages serves it at any missing path. If the site ever moves to a custom domain or the repo is renamed, update those links.
- `.nojekyll` tells GitHub Pages to serve the files as they are, without Jekyll processing.
- The privacy policy states what Google AdMob collects, based on Google's current disclosure and the SDK privacy manifests. If you add an ad network, analytics, a server or cloud save, update the policy and the App Privacy answers in App Store Connect at the same time.
- Preview locally: run `python3 -m http.server 8000` in this folder, then open `http://localhost:8000/`. The 404 page's absolute links only work on GitHub Pages.
