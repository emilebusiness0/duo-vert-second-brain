---
name: duo-vert-website-build-overview
description: Website project status, source-of-file-truth location, and the launch checklist — migrated from the duo-vert Claude Code skill
metadata:
  type: project
  modified: 2026-08-01
---

Goal: 15+ page site (duovert.ca) to rank #1 for pavé-uni searches in Gatineau/Ottawa. See [[duo-vert/company]] for the business itself. **Correction (2026-08-01):** the actual project is Vite + React (`package.json` confirms `react`, `vite`), not plain static HTML as previously described here — each page under `public/` is still a static, pre-built `index.html` though, so the SEO-visible-content workflow is unaffected.

**Source of truth for the actual files (resolved 2026-08-01):** transferred from the old computer via Emile uploading to Google Drive, then Claude Code pulling it down directly through the Drive API, unzipping, and restoring. Now lives at `~/Documents/duovert-site` on this Mac (renamed from `duovert-site-fixed`). `node_modules` was deleted before the Drive zip export (Drive's zip has a file-count limit `node_modules` blew past) and rebuilt locally via `npm install` — fully safe, it's in `.gitignore` and reinstalls byte-identical from `package-lock.json`. The `.git` folder survived the round-trip but was empty (`objects`/`refs` never survived zip export since they were empty directories) — turned out there was no actual commit history to lose; ran `git init` to restore the skeleton and made a fresh root commit (109 files) as the new baseline.

**Workflow (as of 2026-07-18): edit locally, not in AI Studio/Gemini prompts.** Reason: AI Studio's own project storage repeatedly and spontaneously corrupted binary image files — a platform-level reliability bug, recurred multiple times even after fixes. Editing locally with git avoids the category entirely (images are just files on disk). AI Studio may still be referenced for historical workflow context (see [[duo-vert/ai-studio-playbook]]) but shouldn't be used for future edits unless explicitly requested.

**Local preview (corrected 2026-08-01):** this is a Vite + React project (`package.json` has `dev`/`build`/`preview` scripts via Vite), not a plain static server. Run `npm run dev` from `~/Documents/duovert-site` — it starts on **port 3000** (custom-configured in `vite.config.ts`, not Vite's default 5173). Vite's own `public/` convention correctly overlays `public/*` at the served root (`/logo-duovert.png`, `/restauration-pave-uni-gatineau/` etc. all resolve, confirmed 2026-08-01 via curl 200s). The stale `.claude/launch.json` in that repo (pointing `npx serve -p 5174 .`) does NOT apply the public/ overlay correctly — plain `serve` returns 404s for anything under `public/`; don't use it, use `npm run dev` instead. Verify image changes with a real fetch+decode test, not just file-exists.

**Deploy:** drag the entire `duovert-site-fixed` folder (not its unpacked contents) onto Netlify's manual deploy drop zone. Confirmed working. **Lesson:** never drag an unrelated test folder onto an *existing* site's deploy area — did this once, replaced the live site with a placeholder proxy folder; recovered via Netlify's Deploys tab → "Publish deploy" on the previous good deploy (full history always recoverable, but avoid it).

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

See also: [[duo-vert/design-system]], [[duo-vert/ai-studio-playbook]], [[duo-vert/photo-workflow]], [[duo-vert/seo-history]]
