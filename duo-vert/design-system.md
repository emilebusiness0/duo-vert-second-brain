---
name: duo-vert-design-system
description: The structural rules every Duo Vert page must follow — hero CSS values, service page structure, review inventory, verification checklist. Migrated from the duo-vert Claude Code skill.
metadata:
  type: project
  modified: 2026-08-03
---

Structural source of truth is `restauration-pave-uni-gatineau` for service pages, `/gatineau/` for city pages. See [[duo-vert/website-build-overview]] for overall status.

## Hero CSS reference values

| Property | Value |
|---|---|
| padding-top | 130px |
| padding-bottom | 200px |
| hero-content max-width | 1050px (Masson-Angers exception: 1250px, needs forced `<br>` in H1) |
| H1 font-size | `clamp(3rem, 7vw, 6rem)` → 96px at 1440px |
| H1 lines | 2 (keep H1 under ~35 chars) |

Hero height = padding-top + heroContentHeight + padding-bottom. Gatineau reference = 757px; service pages target ≥725px (shorter subtitles, acceptable).

## Service page structure (exact order)

1. **Hero** — dark bg, photo, H1, subtitle, ONE button → `/soumission/`. No emoji, no stats grid. `hero-overlay` div BEFORE `hero-bg-container`.
2. **Processus** — 4 `step-card` (num → h3 → p), no icons, no emoji, curved green SVG arrows between steps.
3. **Pourquoi choisir Duo Vert?** — `section-pourquoi-dark`, dark green `#0d1e0d`, 4 `.why-card` + testimonial (`.reveal.d5`) at the bottom of THIS section (not separate). Logo in card matches review source (Instagram for Jessy, Google for Jeff).
4. **Avant/Après slider** — `section-grey` — ONLY restauration + homepage, delete elsewhere.
5. **FAQ** — `class="section"` + `style="background-color: #eaf4ea;"` (NOT `section-grey`).
6. **CTA Final** — identical everywhere: blur bg, `/pave-uni-gatineau.jpg` bg, overlay, `btn-premium-green` → `/soumission/`. No phone numbers in buttons anywhere.

## City page structure (different — discovery pages, not service pages)

1. Hero — H1 with city name, 1 CTA
2. Section régionale — 1 prose paragraph, locally personalized (ranks better than cards for local SEO)
3. "Nos services à [Ville]" — 4 cards to the service pages
4. Quartiers desservis — neighborhoods list (hyperlocal search capture)
5. FAQ — 3-4 city-specific questions
6. CTA Final — identical to service pages

No "Pourquoi choisir" section (redundant with service pages), no avant/après slider.

Zones per city (exclude current city): each of Gatineau/Hull/Aylmer/Masson-Angers/Ottawa links to the other 4 (`/hull/`, `/aylmer/`, `/masson-angers/`, `/ottawa/`, `/gatineau/` as applicable).

## Review inventory

| Review | Source | Used on |
|---|---|---|
| Jessy — "Duo Vert came to clean our stone…" | Instagram | restauration, à-propos |
| Jeff — "Excellent travail de ces deux jeunes acharnés!…" | Google | nivelage, scellant (temp), à-propos |

Each page needs a unique testimonial in `section-pourquoi-dark` — don't reuse the same person across pages. À-propos shows both side by side (grid `1fr 1fr`, `align-items: stretch`).

## Navbar

Active-state handled by JS matching `a.pathname === window.location.pathname` (script duplicated in every HTML file, no shared component — sitewide nav changes need one big multi-file prompt listing every filename explicitly).

Zones dropdown is 2-level nested (`dropdown-nested`): "Gatineau" region (Gatineau, Aylmer, Hull, Masson-Angers, Buckingham) and "Ottawa" region (Ottawa général + 6 quartiers), each revealing a `.sub-dropdown-menu` on hover.

## Verification checklist (after any structural edit)

Compare against Gatineau (or restauration for service pages) — "exists" isn't enough, must match exactly:
- [ ] H1 = 96px
- [ ] H2 sections = 35.2px / `clamp(1.5rem, 3vw, 2.2rem)`, weight 900, letter-spacing -0.05em
- [ ] H2 CTA final = 56px / `clamp(2.25rem, 5vw, 3.5rem)` — intentional exception, stays bigger
- [ ] FAQ background = `rgb(234, 244, 234)` (mint), NOT grey
- [ ] FAQ `.faq-answer` overflow = `hidden` (answers invisible by default — CRITICAL, if `visible` they bleed out of the container)
- [ ] No English text leaking into French pages
- [ ] No parasite sections (standalone testimonial, stray stats grid)
- [ ] Section order correct: hero → régionale → services → zones → FAQ → CTA (city) or hero → processus → pourquoi-dark → avant/après → FAQ → CTA (service)

Force reveal-animations before measuring anything (`.reveal` elements sit at `opacity:0, translateY(30px)` until triggered) — set `opacity:1; transform:none; transition:none` on all `.reveal, .d1-.d5` first, or measurements will be wrong.

See also: [[duo-vert/ai-studio-playbook]], [[duo-vert/website-build-overview]]
