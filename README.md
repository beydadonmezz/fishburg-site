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

## Effective date placeholder (round 11, friends feature): fill on deploy day

**Filled and deployed on 2026-10-13 (round 12): the four pages say 13 October 2026 / 13 Ekim 2026 and carry no placeholder. The text below is how it was done; for the next change of the legal text, edit the date in the four `<time>` lines by hand.**

The four legal pages (`privacy/index.html`, `terms/index.html`, `tr/privacy/index.html`, `tr/terms/index.html`) carry the placeholders `EFFECTIVE_DATE_ISO` and `EFFECTIVE_DATE_TEXT` in the "Effective date" line under the page title (the pages that are live today say 2026-10-12). **Do not publish while they are there.** On the day you deploy, from this folder (example for 14 October 2026):

```sh
perl -pi -e 's/EFFECTIVE_DATE_ISO/2026-10-14/; s/EFFECTIVE_DATE_TEXT/14 October 2026/' privacy/index.html terms/index.html
perl -pi -e 's/EFFECTIVE_DATE_ISO/2026-10-14/; s/EFFECTIVE_DATE_TEXT/14 Ekim 2026/' tr/privacy/index.html tr/terms/index.html
grep -rn EFFECTIVE_DATE . --include='*.html'   # must print nothing
```

Before that deploy, check the friends texts against the build that ships (docs/STORE.md "App Privacy" §F lists the statements to check) and do the Supabase steps listed there (region; the Data Processing Addendum needs no separate acceptance, owner's statement of 2026-10-13, STORE.md F.5 step 2).

## Update the published site (after the first publish)

Round 13 (2026-10-14): the privacy and support pages (en + tr) state the server rules for notifications about friends (quiet hours, grouping, three a day, 12 h), the in-game Notifications switch and the stored time zone; the privacy pages' effective date is 14 October 2026 / 14 Ekim 2026 (the terms stay 13 October). `src/i18n/__tests__/site-facts.test.ts` ties those numbers to `supabase/migrations/20261014000100_social_push_rules.sql`.

Round 12b (2026-10-13): the privacy and support pages (en + tr) say that notifications about friends are sent as push notifications through Expo and Apple. **Deploy them only after the phone test of push has passed**, because they describe the build with push switched on. The effective date stays 13 October 2026 / 13 Ekim 2026.

```sh
git clone https://github.com/beydadonmezz/fishburg-site.git /Applications/Apps/fishburg-site-publish
rsync -a --delete --exclude '.git' --exclude '.DS_Store' --exclude '_user-site-snippet.html' --exclude 'fill-developer-name.sh' /Applications/Apps/fishburg/site/ /Applications/Apps/fishburg-site-publish/
git -C /Applications/Apps/fishburg-site-publish status --short
git -C /Applications/Apps/fishburg-site-publish add -A
git -C /Applications/Apps/fishburg-site-publish commit -m "site: notifications about friends are push notifications (Expo, Apple)"
git -C /Applications/Apps/fishburg-site-publish push origin main
```

`status --short` should list only the files that changed (round 12b: `privacy/index.html`, `support/index.html`, `tr/privacy/index.html`, `tr/support/index.html`, `README.md`). If it lists a deletion you did not expect, stop and look before `add`. If the folder `fishburg-site-publish` already exists, use `git -C /Applications/Apps/fishburg-site-publish pull` in place of the clone. GitHub Pages rebuilds in a minute or two; then check that `/fishburg-site/privacy/#notifications` and `/fishburg-site/tr/privacy/#notifications` show the new text.

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
- The privacy policy states what Google AdMob collects, based on Google's current disclosure and the SDK privacy manifests. Since round 11 it also states what the friends feature stores on Supabase, and since round 12b that notifications about friends go through Expo's push service to Apple. If you add an ad network, analytics, another server or cloud save, or change what the friends feature stores, update the policy and the App Privacy answers in App Store Connect at the same time.
- Preview locally: run `python3 -m http.server 8000` in this folder, then open `http://localhost:8000/`. The 404 page's absolute links only work on GitHub Pages.
