# Lannco Tree Service (formerly Jed's Tree Service LLC) — Runbook

## Status

| Field | Value |
|---|---|
| **Status** | Active — rebrand complete, live on CF Pages |
| **Next** | Awaiting Jed's reply to FB DM (sent 2026-10-14) → if no response in 7-10 days, phone fallback (715) 530-4922 |
| **Blocker** | None |
| **Last touched** | 2026-10-14 (FB DM sent; canonical pitch saved to `~/.hermes/cron/output/lannco-jed-pitch-sent.md`) |
| **Repo** | https://github.com/thecarter34/jedstreeservice.git (repo name still says jedstreeservice — brand mismatch) |
| **Live URL** | https://jedstreeservice.pages.dev |
| **Stale URL** | https://jedstreeservice.com + https://www.jedstreeservice.com (still old WordPress site) |

## Business

- **Brand:** Lannco Tree Service (rebrand from Jed's Tree Service LLC)
- **Service area:** Strum, Altoona, Osseo, Menomonie, Chippewa Falls, Mondovi, Augusta, Eau Claire (Chippewa Valley, WI)
- **Phone:** (715) 530-4922
- **Email:** lannco@lanncotreeservice.com (placeholder — verify)
- **FB:** facebook.com/jedstreeservice (still old handle, needs Lannco FB page)
- **Site:** Static single-page (index.html + style.css + 404.html), CF Pages deploy from GitHub master

## Branding

- **Primary green:** `#2c4e34` (deep forest — wordmark color)
- **Forest hierarchy:** `#1a3322` deepest → `#2c4e34` deep → `#4a7c5e` rich
- **Sage/Mint:** `#88a89b` soft → `#b8ccc1` fresh
- **Tan accent:** `#b08c68` (extracted from logo oak illustration)
- **CTA:** `#d4a017` (warm honey amber — restored in polish commit 2a02e92)
- **Warm bg:** `#fdf6ed` (reviews + form section bg — added in polish)
- **Logo SVG:** branding/lannco-tree-service-logo.svg (1280×1280 viewBox)
- **Logo description:** Oak tree silhouette in tan (#b08c68) with visible roots, deep-green wordmark "LANNCO" where the "A" is letterformed as a small evergreen; an arborist mid-canopy on a rope; tagline "TREE SERVICE" tucked between root and canopy.

## Deploy flow

```
local edit → git commit (single commit) → git push origin master
  → CF Pages auto-build → served at https://jedstreeservice.pages.dev
```

GitHub repo has branch-protection rule (advisory) suggesting changes go through a pull request, but direct push to master is currently allowed (admin override).

**Verify both URLs after every push:**
```bash
for url in "https://jedstreeservice.pages.dev" "https://jedstreeservice.com" "https://www.jedstreeservice.com"; do
  code=$(curl -sk -o /dev/null -w "%{http_code}" --max-time 8 -L "$url")
  echo "$code  $url"
done
```

## Change Log

| Date | Change |
|---|---|
| 2026-10-13 | Rebrand Jed's Tree Service LLC → Lannco Tree Service. New logo SVG (deep green + warm tan), `--forest-deep` and `--forest-deepest` shifted to Lannco greens, brand copy + schema + email updated. Single commit `c6efef1`. |
| 2026-10-13 | Polish pass: `--citrus-burst` restored to `#d4a017` (was invisible-aliased to forest-deep on dark section); hero padding tightened so CTA sits above fold at desktop 1280×800; reviews + form section bg → `#fdf6ed` warm tan. Shipped in two commits (`723e165` + `2a02e92` fixup). |
| 2026-10-13 | File-mode noise cleared (`git config core.fileMode false`); old `logo.png` (Jed's PNG) removed as orphaned brand-ident asset. |
| 2026-10-13 | Live verified at jedstreeservice.pages.dev showing Lannco rebrand. Custom domains jedstreeservice.com + www.jedstreeservice.com still serve old WordPress site — DNS / CF Pages custom-domain binding not updated. |

## Open questions / next actions

- [ ] **DNS / CF Pages custom-domain binding** — point `jedstreeservice.com` and `www.jedstreeservice.com` at the new CF Pages project. Without this, anyone using the custom domain sees the old site.
- [ ] **GitHub repo rename** — repo name `jedstreeservice` no longer matches the brand. Renaming would change `github.com/thecarter34/jedstreeservice` to something like `lannco-tree-service`. Existing redirect behavior should be verified.
- [ ] **FB handle** — facebook.com/jedstreeservice still; needs to match once Lannco FB page exists.
- [ ] **Email verification** — confirm lannco@lanncotreeservice.com is the right address; current repo only has the placeholder.
- [x] **FB outreach send** — DONE 2026-10-14 via Facebook Messenger. Canonical pitch saved to `~/.hermes/cron/output/lannco-jed-pitch-sent.md`. Awaiting Jed's reply (7-10 days).
- [ ] **Logo PNG (`logo.png`) deleted** — restore with `git checkout HEAD~1 -- logo.png` if archival is desired.

## Verification recipe

```bash
# 1. Confirm no Akash- brand stragglers on live page
curl -sL https://jedstreeservice.pages.dev | grep -oE '<title>[^<]+</title>'
# Expected: <title>Lannco Tree Service | Tree Removal & Trimming in Chippewa Valley, WI</title>

# 2. Confirm logo SVG loads
curl -sk -o /dev/null -w "%{http_code}\n" https://jedstreeservice.pages.dev/branding/lannco-tree-service-logo.svg
# Expected: 200

# 3. Verify hero CTA visibility at 1280×800
# (use dev-browser Playwright script — see client-website-maintenance skill Step 4)
```