---
name: duo-vert-ai-studio-playbook
description: How to prompt Gemini/AI Studio effectively for Duo Vert page edits — patterns that work, common mistakes, and the exact city-page prompt sequence. Migrated from the duo-vert Claude Code skill; historical/legacy workflow, current workflow is local file editing (see website-build-overview.md).
metadata:
  type: project
  modified: 2026-07-30
---

**Note: as of 2026-07-18 the active workflow is local file editing, not AI Studio.** This playbook is kept for historical reference and in case Emile explicitly asks to go back to it. See [[duo-vert/website-build-overview]].

## Core rules that made prompts actually work

- **Never paste HTML in prompts** — Gemini produces broken output.
- **Use CAPS for emphasis** (CRITICAL, MUST, ALL, IDENTICAL, DO NOT) — Gemini responds much better to caps than polite phrasing.
- **Always add** "Keep ALL structure, ALL text sizes, ALL spacing, ALL fonts, ALL classes IDENTICAL. It should look IDENTICAL." to any copy/replace prompt — otherwise Gemini silently changes sizes/spacing.
- **For structural fixes: select the SOURCE element on the reference page, say "copy this to X"** — never hand Gemini class names/inline styles to add manually.
- **One fix at a time, short and focused.** Never bundle multiple concerns in one prompt.
- **All pages already exist — never ask Gemini to create a new file.**
- **FAQ copy prompt — never simplify.** Shorter versions consistently failed; use the full CAPS version every time (see Prompt 5 below).
- **Gemini resists modifying the restauration page** (treats it as the reference) — for restauration fixes, select the source element on another page and say "add this to restauration replacing the existing one."

## Prompt pattern reference

| Fix needed | How to prompt |
|---|---|
| Section wrong/broken | Select it → "This section should be an exact copy of the restauration page version" |
| Delete a section | Select it → "Delete this section entirely" |
| Match hero | Select it → "Match this hero exactly to the [reference] page hero — same structure, same font sizes, one button only, no stats grid, hero-overlay before hero-bg-container" |
| Match any section | Select SOURCE on reference page → "Copy this entire section to [target], replacing the existing [section]. Keep ALL structure/sizes/spacing/fonts/classes/buttons IDENTICAL." |
| Fix FAQ accordion (overflow visible) | No selection — "In [page].html, `<style>` block, find `.faq-answer {` and add `overflow: hidden;` and `opacity: 0;`" |
| Fix FAQ background to mint | No selection — "In [page].html, find the FAQ section and add `style=\"background-color: #eaf4ea;\"`" |
| Invisible code (schema/meta/head) | No selection — reference file directly: "In [filename].html, find [element] and change [X] to [Y]" |
| H1 too small after hero copy | No selection → add `style="font-size: clamp(3rem, 7vw, 6rem);"` |

## City page — complete prompt sequence

All prompts select source on Gatineau unless noted. Run in this order:

1. **Hero** (select hero on Gatineau) → copy to [ville].html, change only H1/subtitle, keep everything else identical. H1 under ~35 chars (long names like Masson-Angers → "Pavé-uni à Masson-Angers" instead of full service name).
2. **Regional section** (select mint section on Gatineau) → copy, change only H2 to "Pourquoi le pavé-uni se dégrade à [Ville] ?" (always ends with `?`) and the paragraph. Follow with a no-selection H2 font-size fix.
3. **Services** (select services section) → copy, change only the title to "Nos services à [Ville]". Follow with H2 font-size fix.
4. **Zones** (select zones section) → copy, update the 4 links to exclude the current city, update subtitle.
5. **FAQ** (select FAQ on Gatineau) → full CAPS prompt, never simplified: "Copy this entire FAQ section to [ville].html replacing the existing FAQ section. CRITICAL: the accordion MUST work exactly like this one — answers COMPLETELY HIDDEN by default. DO NOT change accordion behavior/overflow/grid properties. Keep ALL classes/inline styles/structure IDENTICAL. Replace ONLY question/answer text. Keep section title 'Questions fréquentes — [Ville]'. H2 needs font-size: clamp(1.5rem, 3vw, 2.2rem), font-weight: 900, letter-spacing: -0.05em as inline style if needed."
6. **CTA** (select CTA on Gatineau) → copy identically.
7. **Delete parasite sections** — any extra testimonial/stats grid left over.

## Common mistakes catalog

- Hero overlay order matters: `hero-overlay` must come BEFORE `hero-bg-container`.
- ALL H2s need explicit inline style after any section copy (Gemini doesn't copy CSS) — standard is 35.2px at 1440px.
- Service pages originally had `hero-content: 820px` vs city pages' `1050px` — always verify alignment, it's per-file embedded `<style>`, not external CSS.
- Screenshots via computer-use are unreliable for AI Studio comparison — the foreground window shows the app chrome, not the navigated page; use JS measurements instead.
- Titles over ~35 chars get shortened (e.g. scellant's H1 went from 47 to 31 chars).
- Copying the footer between pages doesn't work (Gemini resists) — fix CSS overrides in the `<style>` block directly instead.
- `tarifs`, `à-propos`, `contact`, `faq` are each "page unique" — don't apply the service/city-page checklist to them, they have their own accepted designs (documented per-page in [[duo-vert/website-build-overview]]).

See also: [[duo-vert/design-system]], [[duo-vert/photo-workflow]]
