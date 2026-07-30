---
name: duo-vert-website-build-overview
description: Website project status, source-of-file-truth location, and the launch checklist — migrated from the duo-vert Claude Code skill
metadata:
  type: project
  modified: 2026-07-30
---

Goal: 15+ page static HTML site (duovert.ca) to rank #1 for pavé-uni searches in Gatineau/Ottawa. See [[duo-vert/company]] for the business itself.

**Source of truth for the actual files:** was `duovert-site-fixed` on the old computer's Downloads — **not yet transferred to the new Mac** as of 2026-07-30. Nothing local-file-editable can happen until Emile transfers it (AirDrop/external drive/cloud). Don't try to reconstruct it from the live duovert.ca deploy.

**Workflow (as of 2026-07-18): edit locally, not in AI Studio/Gemini prompts.** Reason: AI Studio's own project storage repeatedly and spontaneously corrupted binary image files — a platform-level reliability bug, recurred multiple times even after fixes. Editing locally with git avoids the category entirely (images are just files on disk). AI Studio may still be referenced for historical workflow context (see [[duo-vert/ai-studio-playbook]]) but shouldn't be used for future edits unless explicitly requested.

**Local preview:** static file server rooted at `duovert-site-fixed`, with `public/*` overlaid at the served root (Vite `public/` convention — `/logo-duovert.png` must resolve at root, not `/public/logo-duovert.png`). Verify image changes with a real fetch+decode test, not just file-exists.

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
