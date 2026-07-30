---
name: duo-vert-seo-history
description: SEO audit findings and fixes, content expansion rounds, mobile audit findings, and the prepared keyword/competitor research docs. Migrated from the duo-vert Claude Code skill.
metadata:
  type: project
  modified: 2026-07-30
---

## SEO resources (for future content rewrites)

Two Google Docs with full analysis, prepared in advance — consult when reworking page text content:
- **Keyword research:** `https://docs.google.com/document/d/1OGya1kHlaT77CyMYYI2AESyezTb_APeftGbv0I9CEz8/edit` — 36 keywords with volume/difficulty/intent (FR+EN), 20 long-tail phrases, 18 FR + 13 EN "People Also Ask" questions (map directly onto the 5 FAQ categories), seasonal calendar, 15 blog topic ideas.
- **Competitive analysis:** `https://docs.google.com/document/d/1z-DeLLfyLLMVydLjtEEcDKVo5_-j2YznvtBoHyGJsQQ/edit` — 7 competitors scored with weaknesses, 8 FAQ questions competitors don't answer (direct opportunity), page recommendations (Buckingham = zero competition), minimal recommended page structure.

## Full SEO audit findings (9 parallel subagents: technical, content, schema, sitemap, performance, visual, geo, local, sxo)

**Verification caveat:** 2 of the audit's findings were false positives, only caught by manually re-checking with curl — always double-check an automated SEO audit's specific claims before acting on them, don't trust the tool blindly.

**Fixed:** removed `Google-Extended: Disallow` from robots.txt (was blocking Google AI Overviews/Gemini grounding). Added a general price-range disclosure to `/tarifs/` ("600$-4000$ en général"). Fixed duplicate H1 between `/gatineau/` and `/restauration-pave-uni-gatineau/`. Fixed homepage schema `aggregateRating.reviewCount` (1 → 12, matching actual testimonial-card count). Unified opening hours to "Mo-Su 08:00-19:00" (confirmed real hours: 8h-19h every day). `/installation-pave-uni-gatineau/` 404s and doesn't exist in the codebase — likely an orphaned URL from Google Business Profile, flagged for Emile to check GBP directly, not a code fix.

**Confirmed reusable facts (safe to use in content without re-asking):** scellant brand = Techniseal; pressure washer = 3600 PSI / 2.5 GPM / Honda industrial engine; always protect surrounding lawn/plants + full site cleanup; no subcontracting, always Emile & Beckett personally; season June-September (possible window May-October); polymeric sand ~5 years, pavers themselves 25-50 years (two different numbers, don't conflate); pricing "generally 600$-4000$" for a restoration project, not per-service.

**Still open (prioritized in full report, not yet actioned):**
- Content depth — homepage (~1250-1463 words) and 4 service pages (~1139-1407 words) still below the 1500-1600 word target.
- BreadcrumbList schema site-wide missing; `@id` entity consolidation missing (every page re-declares an anonymous LocalBusiness instead of one canonical entity).
- Hero images unoptimized (~200KB JPEG, no WebP, missing width/height on some pages) — LCP/CLS risk.
- Missing security headers (X-Frame-Options, CSP) via Netlify `_headers` file.
- 12 city/quartier pages are 85-97% templated — real content-uniqueness risk if local FAQ/detail sections ever shrink further.
- The 6 Ottawa quartier pages are confirmed near-duplicate templates (verified by diffing Glebe vs Mechanicsville) — the 5 Gatineau-side city pages are genuinely unique, not a problem. Rewriting the 6 Ottawa pages with real local detail is still open.

**Content expansion round 2 results:** word counts pushed up (homepage 1463, restauration 1407, scellant 1240, nivelage 1176, nettoyage 1139) — still short of the 1500-1600 target. Added BreadcrumbList JSON-LD to all 4 service pages, "Zones desservies" sections with real internal links, "Pourquoi ne pas attendre" urgency sections with the actual $600-4000 figure. SEO health score progressed 63 → 68 → 71/100 across that session.

**Established content process (apply to any future content addition):** identify what facts are needed → ask Emile to confirm those facts FIRST → only then draft → show for approval → apply. Never draft with generic/competitor-researched placeholder numbers first, risks a rewrite once real facts differ.

**Google indexing requests:** Search Console "Request Indexing" caps at ~10-12/day — batch large URL sets across multiple days.

## Mobile audit findings

- Hamburger menu accordion bug: first `<li>` in `.mobile-dropdown-menu` collapsed to height:0 (grid 0fr/1fr trick misapplied to a multi-item list) — fixed via `max-height` transitions instead.
- Mobile nav was missing Buckingham + all 6 Ottawa quartier links (desktop had them) — added to both.
- Mobile logo size: only homepage had the correct smaller mobile size — other 26 pages needed the same `@media` overrides copied in.
- "White logo on white" on load: a mobile-only `body{padding-top}` rule pushed the hero down past the transparent header — fixed with `margin-top: -64px` on `.hero`/`.hero-home-new` inside the same media query. Was present on 18/27 pages when found.
- Hero photos are wide/desktop-oriented (~2:1) and crop tightly on mobile via `object-fit: cover` — only real fixes are shrinking mobile `min-height` (partial, applied sitewide) or generating true portrait-composed photos.
- Non-breaking hyphens (U+2011) used in "pavé‑uni" and short place names to prevent mid-word line breaks — only for short compounds; long hyphenated names (14+ chars, e.g. Carlingwood-Woodroffe) overflow if forced this way, reverted to normal breakable hyphens for those.
- CTA final section stray `!important` padding override found only on homepage stripped its side margins — check for this pattern if a CTA card looks full-bleed on one page but not others.

See also: [[duo-vert/website-build-overview]], [[duo-vert/design-system]]
