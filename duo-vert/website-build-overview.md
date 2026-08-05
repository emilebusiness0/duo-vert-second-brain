---
name: duo-vert-website-build-overview
description: Website project status, source-of-file-truth location, and the launch checklist — migrated from the duo-vert Claude Code skill
metadata:
  type: project
  modified: 2026-08-05
---

Goal: 15+ page site (duovert.ca) to rank #1 for pavé-uni searches in Gatineau/Ottawa. See [[duo-vert/company]] for the business itself. **Correction (2026-08-01):** the actual project is Vite + React (`package.json` confirms `react`, `vite`), not plain static HTML as previously described here — each page under `public/` is still a static, pre-built `index.html` though, so the SEO-visible-content workflow is unaffected.

**Source of truth for the actual files (resolved 2026-08-01):** transferred from the old computer via Emile uploading to Google Drive, then Claude Code pulling it down directly through the Drive API, unzipping, and restoring. Now lives at `~/Documents/duovert-site` on this Mac (renamed from `duovert-site-fixed`). `node_modules` was deleted before the Drive zip export (Drive's zip has a file-count limit `node_modules` blew past) and rebuilt locally via `npm install` — fully safe, it's in `.gitignore` and reinstalls byte-identical from `package-lock.json`. The `.git` folder survived the round-trip but was empty (`objects`/`refs` never survived zip export since they were empty directories) — turned out there was no actual commit history to lose; ran `git init` to restore the skeleton and made a fresh root commit (109 files) as the new baseline.

**Workflow (as of 2026-07-18): edit locally, not in AI Studio/Gemini prompts.** Reason: AI Studio's own project storage repeatedly and spontaneously corrupted binary image files — a platform-level reliability bug, recurred multiple times even after fixes. Editing locally with git avoids the category entirely (images are just files on disk). AI Studio may still be referenced for historical workflow context (see [[duo-vert/ai-studio-playbook]]) but shouldn't be used for future edits unless explicitly requested.

**Local preview (corrected 2026-08-01):** this is a Vite + React project (`package.json` has `dev`/`build`/`preview` scripts via Vite), not a plain static server. Run `npm run dev` from `~/Documents/duovert-site` — it starts on **port 3000** (custom-configured in `vite.config.ts`, not Vite's default 5173). Vite's own `public/` convention correctly overlays `public/*` at the served root (`/logo-duovert.png`, `/restauration-pave-uni-gatineau/` etc. all resolve, confirmed 2026-08-01 via curl 200s). The stale `.claude/launch.json` in that repo (pointing `npx serve -p 5174 .`) does NOT apply the public/ overlay correctly — plain `serve` returns 404s for anything under `public/`; don't use it, use `npm run dev` instead. Verify image changes with a real fetch+decode test, not just file-exists.

**Deploy:** drag the entire `duovert-site` folder (renamed from `duovert-site-fixed`, not its unpacked contents) onto Netlify's manual deploy drop zone. Confirmed working. **Lesson:** never drag an unrelated test folder onto an *existing* site's deploy area — did this once, replaced the live site with a placeholder proxy folder; recovered via Netlify's Deploys tab → "Publish deploy" on the previous good deploy (full history always recoverable, but avoid it).

**Exact-match verification (2026-08-01):** did a full byte-level diff of every page's raw HTML (all 25 top-level pages + `soumission/merci` + `reference/merci`, i.e. every folder under `public/`) between the restored local copy and live duovert.ca. Result: 26/27 were already byte-identical; 3 forms (`soumission-accueil` on homepage, `soumission-gratuite`, `programme-reference`) had `data-netlify="true" netlify-honeypot="bot-field"` attributes in the local source that live did NOT have — removed to match live exactly (see git commit `19a2280`).

**Netlify Forms gotcha this surfaced — important if the site is ever redeployed to a *new* Netlify site:** Netlify detects/registers a form by scanning for `data-netlify="true"` **at deploy time**. The 3 forms above no longer have that attribute in source, which is fine for continuing to deploy to the *same, already-registered* Netlify site (forms stay registered once seen, even after the attribute is later removed) — but if this repo is ever deployed fresh to a **different** Netlify site, those 3 forms will silently fail to register/submit until `data-netlify="true"` is temporarily added back for one deploy, then can be removed again to match live's current markup.

**Also verified, not a bug:** HTTP response headers differ between local (`npm run dev`) and live (Netlify's CDN: `server: Netlify`, `cache-status`, `strict-transport-security`, etc.) — expected, since those come from Netlify's edge/platform layer at deploy time, not from anything in the source files. `netlify.toml`'s only custom rule is 1-year immutable caching for `.jpg/.png/.webp`, already correctly configured, nothing to change.

## Page status (as of last full audit)

All 27 pages done except items below were still 🔄/⏳ at last check:
- **tarifs** — done (unique design, own page structure)
- **avant-apres** — done (H1 fixed, hero photo swapped from stock to site photo, overlay darkness fixed)
- **soumission, référence, faq, conditions-utilisation, conditions-contrat, politique-confidentialite** — legal/utility pages, done, sober design, no design-system checklist applies

Everything else (restauration, nettoyage, scellant, nivelage, all 5 Gatineau-region city pages, all 6 Ottawa quartier pages, homepage, à-propos, contact) — ✅ done, see [[duo-vert/design-system]] for the structural rules they all follow.

**Golden rule:** `restauration-pave-uni-gatineau` is the source of truth. Every service page must be structurally identical to it (text content and the avant/après slider being the only allowed differences). Pages marked "page unique" (tarifs, à-propos, contact, faq) are exempt — they have their own accepted designs.

## Launch checklist

**Blocking (must be done before publish):** all hero/step-card/service-card/FAQ photos across every page ✅ done as of last check, but subject to a known deferred-corruption bug (see [[duo-vert/photo-workflow]]) — always re-verify with the `checkValidJpeg` script before considering a page "done," especially after a pause/new session. Hero parallax effect (present on restauration) needs copying to other service pages if not already done. SEO visible content (H1/subtitles/paragraphs with keywords). Mobile/tablet check after photos+content final. 3 legal pages complete.

**Recommended order:** photos → SEO visible content → mobile → legal pages → launch.

**Post-launch (not blocking):** Meta Pixel in `<head>` sitewide. Google Tag/conversion tracking. Google Business Profile completion. 5-10 Google reviews before running ads. Meta Business Manager account. Full text-content rewrite with SEO keywords (see [[duo-vert/seo-history]]). Buckingham page (zero competition — done, but flagged as a good early post-launch win historically). Blog (15 topic ideas exist in SEO resources, see [[duo-vert/seo-history]]).

## Site outage incident + fix (2026-08-02)

**Found:** while investigating why leads weren't converting, discovered `duovert.ca` and `www.duovert.ca` were both live-serving a placeholder — `<html><body>Duo Vert lead proxy — nothing to see here.</body></html>` — instead of the real site. Confirmed via `curl`, not visible from a quick glance since the domain resolved fine.

**Root cause:** `~/Documents/duovert-lead-proxy` (a small, legitimate, separate Netlify site — see below) sat directly next to `~/Documents/duovert-site` and got dragged onto **duovert.ca's** Netlify manual-deploy drop zone by mistake instead of `duovert-site`. This is the second time this exact category of mistake happened (first time noted in the "Exact-match verification" section above, 2026-08-01) — manual drag-and-drop deploy is a standing risk, not a one-off.

**Fixed:** ran `npm run build` in `duovert-site` (Vite outputs to `dist/`, confirmed 27 pages present) and dragged `dist/` onto Netlify. Verified all pages return 200 and correct content. Moved `duovert-lead-proxy` → `~/Documents/duovert-infra/duovert-lead-proxy` so it's no longer sitting next to `duovert-site` and can't be grabbed by accident again.

**Not yet done (real fix, still open):** switch `duovert-site`'s Netlify deploy from manual drag-and-drop to Git-based continuous deployment. This is the actual root-cause fix — manual deploy is what caused the mistake twice, moving the folder only reduces the odds, doesn't eliminate them. Repo has git already initialized (`~/Documents/duovert-site`, commit `19a2280` as of last check).

**What `duovert-lead-proxy` actually is (don't delete it):** a standalone Netlify site with one serverless function, `netlify/functions/lead-webhook.js`, that forwards POST bodies to a Google Apps Script URL (`script.google.com/macros/s/...`) which writes into the leads Google Sheet (see [[duo-vert/sheets-tracking]]). It is **not** referenced anywhere in `duovert-site`'s own code — the site's 3 forms (`soumission-accueil`, `soumission-gratuite`, `programme-reference`) use plain Netlify Forms (`method='POST'`, no JS), so this webhook is wired up via Netlify's Forms → Outgoing Webhook notification setting (a dashboard setting on the `duovert-lead-proxy` Netlify site, not in either repo's code) — confirm this is still configured correctly if leads ever stop showing up in the Sheet despite the site itself working.

## Analytics setup (2026-08-02)

**GA4:** brand-new property created this session (previously never installed — confirms "Google Tag/conversion tracking" from the launch checklist above was correctly flagged as not-yet-done). Final Measurement ID **`G-T9BWBD62N6`** (an earlier ID, `G-5XLLMFJRF8`, got swapped out by GA4's own setup wizard mid-flow after Emile refreshed the page and it detected "existing tags" — not a mistake on our end, just how the wizard behaves). Installed via **Netlify → Site configuration → Build & deploy → Post processing → Snippet injection** (before `</head>`, all paths) rather than editing the 27 static HTML files directly — this survives independently of deploys since it's a site-level Netlify setting. Confirmed working via GA4 Realtime report after the site-outage fix above (couldn't detect anything while the site was serving the placeholder).

**Search Console:** property is `duovert.ca` under his **duovert** Google account (`duo.vert.gatineau@gmail.com`, separate from his personal Google/Claude accounts).

## GSC lead-conversion analysis + sitemap fix (2026-08-02)

**Pulled via Playwright MCP** (see [[personal/dev-environment]]): Performance report, Queries + Pages tabs, last 3 months (11 May–2 Aug). Headline finding: 86 of 88 total clicks (98%) came from the homepage alone — every other page (15+ service/city pages) combined generated 2 clicks in 3 months. Only real organic wins are branded queries ("duo vert", "duovert", position ~1-2.5). Non-branded/service/city queries either rank poorly (core term "pavé uni gatineau" sits at position 25-28) or rank well but get zero clicks despite good position (e.g. "installation pavé-uni aylmer" — position 2.0, 204 impressions, 0 clicks).

**Root cause found via URL Inspection:** `/aylmer/` showed "URL is not on Google... unknown to Google," despite the page existing on disk and being correctly listed in `sitemap.xml`. Checked GSC's Sitemaps report directly — **`0-0 of 0`, no sitemap had ever been submitted to Search Console.** This explains why `/aylmer/`, `/hull/`, `/ottawa-carlingwood-woodroffe/`, `/ottawa-island-park/`, `/ottawa-mechanicsville/` never appeared in the Pages report at all — Google had no reliable path to discover them (no sitemap, few backlinks, some not linked prominently from the homepage).

**Fixed:** submitted `https://duovert.ca/sitemap.xml` via GSC Sitemaps report. **Gotcha:** the submission field rejected a relative path (`sitemap.xml`) with "Invalid sitemap address" — needs the full absolute URL for this domain-property. Verified the file itself is healthy first (200, `content-type: application/xml`, served correctly by Netlify) before submitting. Status showed "Couldn't fetch" immediately after submission — normal transient state before Google's first crawl attempt, not an error.

**Not yet done:** once the sitemap shows processed (check back in a few days), use URL Inspection → Request Indexing on the previously-undiscovered pages individually to speed things up. Also still open from the original analysis: rewrite title/meta on the remaining top-3-position-but-0%-CTR pages (interlock stones/driveway gatineau — scellant hull and pavage gatineau were fixed the same day, see the follow-up session below); consolidate "pavé uni gatineau" spelling variants and push internal linking toward that phrase; GBP completion + reviews still not done (see launch checklist above) and likely matters more than more content right now given how brand-dependent traffic still is.

## Follow-up session (2026-08-02, same day): title/meta fixes + GBP services + GBP posts

**Requested indexing** on the 5 pages found missing from Performance earlier: `/aylmer/` and `/hull/` were genuinely undiscovered ("URL is not on Google") and got Request Indexing submitted. `/ottawa-carlingwood-woodroffe/`, `/ottawa-island-park/`, `/ottawa-mechanicsville/` turned out to already be indexed — they were just getting zero impressions (a ranking problem, not a discovery problem), so no action needed there.

**Title/meta edits made and deployed:** `/hull/index.html` title now includes "Scellant" (was missing despite meta description already having it) + added "scellant pavé uni Hull" to keywords. `/gatineau/index.html` title/description/keywords now lead with "Pavage & Pavé‑Uni Gatineau" instead of just "Pavé‑Uni" to match the "pavage gatineau" query it ranks position 5 for. Built with `npm run build` (27 pages confirmed in `dist/`), Emile deployed via Netlify drag-and-drop, verified live on duovert.ca via curl same session — both titles confirmed updated on the real site.

**GBP services translated to French** (via business.google.com panel embedded in Google Search results, edited live — no local file involved): all 5 services were in English ("Paver Restauration," "High-Pressure Cleaning," etc.) despite the whole business being French-first. Now: Restauration de pavé-uni, Nettoyage haute pression, Application de scellant, Nivelage de pavé-uni, Soumission gratuite. Confirmed saved via "Your service details have been updated" toast.

**GBP Posts — native scheduling exists** (a "Schedule this post" toggle with date/time picker, in the Add Post dialog) — corrects an earlier wrong claim in this same session that GBP posts couldn't be scheduled. Gotcha: scheduling for a same-day past time (e.g. 9am when it's already past 9am) throws "Scheduled date must be in the future" silently and leaves the dialog in a broken retry state — close without saving and start a fresh Add Post rather than trying to fix in place.

**Rejection pattern encountered:** submitted 6 posts (1 team-intro + 5 project-themed), rejected repeatedly and unpredictably:
- Team-intro post (photo of Émile & Beckett) — rejected 3x across 2 different photos and 2 different wordings (with/without "pas une franchise, pas de centre d'appel" competitor-comparison language). Consistent enough to conclude photos of their faces specifically get flagged by Posts review — dropped the concept entirely, do not retry with people-photos in Posts without testing on a low-volume day first.
- Scellant/gel-dégel seasonal post — rejected 4x across 2 photos and 2 wordings (with/without an "il reste encore quelques places" urgency line), including one retry using the exact photo (`sablage-polymere-pave-uni.jpg`) that succeeded fine on a *different* post the same session. This rules out that specific photo and suggests either the "scellant" content itself or — more likely given it was the 6th–9th post submitted within ~45 minutes — Google's Posts review rate-limiting/spam-flagging bursty submission volume rather than reacting to content. **Left unresolved, Aug 23 slot empty.** Next attempt should happen on a quiet day, not stacked behind other submissions.

**Final live/scheduled state as of this session:** Aug 9 avant/après scheduled; Aug 16 sable polymère educational scheduled; Aug 30 nivelage scheduled; Sep 6 avis-client scheduled. Aug 23 (scellant) still empty — Emile tried manually too and also got rejected, so it's not just a bulk-submission artifact; cause still unknown, needs another attempt later with fresh wording.

**Late addition same session — Book button + consistent CTA:** Emile asked to add a "Réserver" (Book) button linking to `duovert.ca/soumission/` on each post, and replace phone-number/inconsistent endings with "Réservez votre soumission gratuite ici!" for consistency. Edited all 4 scheduled posts (Aug 9, 16, 30, Sep 6) via the "Modifier" option on each (found under the 3-dot menu — scheduled/pending posts are editable, published ones likely are not, never tested). **Important gotcha:** editing a scheduled post resubmits it for review — all 4 flipped from "Scheduled"/"Programmé" back to "En attente" (pending) after the edit. Given the day's rejection pattern on the scellant post, there's real risk some of these could still come back rejected; wasn't confirmed clean before the session ended — check status next session before assuming they're live.

See also: [[duo-vert/design-system]], [[duo-vert/ai-studio-playbook]], [[duo-vert/photo-workflow]], [[duo-vert/seo-history]], [[duo-vert/sheets-tracking]], [[personal/dev-environment]]
