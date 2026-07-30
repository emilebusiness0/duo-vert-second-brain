---
name: duo-vert-photo-workflow
description: Photo generation/upload workflow, naming conventions, and the AI Studio image-corruption saga (root cause, workaround, final resolution). Migrated from the duo-vert Claude Code skill.
metadata:
  type: project
  modified: 2026-07-30
---

## Workflow basics

**Compression:** squoosh.app → MozJPEG → quality 100 → under 300kb (rarely needs dropping below 100% to hit that).
**Naming:** write filename WITHOUT `.jpg` when renaming on Windows (it auto-appends, avoids `.jpg.jpg`).
**Upload:** directly to project root in AI Studio (same level as other images), not a subfolder.
**Gemini can overwrite images it shouldn't** — always end any image-touching prompt with "DO NOT delete, move, generate, or replace any existing image files. Only edit the HTML."
**No people/hands in any photo** — Gemini generates humans badly, hurts realism.

**Hero image prompt (3 steps in 1):**
> Generate a photorealistic wide-angle photo of [description, pavé color, mood, light, suburban Quebec context]. Landscape 1400x800px. Save as `hero-[page].jpg`. Then in [page].html: (1) set `<img class="hero-bg">` src, (2) `.hero-bg{}` opacity→1, filter→none, (3) `.hero-overlay` background-color→`rgba(0,0,0,0.45)`. DO NOT delete/move/generate/replace any other image files.

**Naming convention:** service hero = `hero-[service]-pave-uni-gatineau`; city hero = `hero-pave-uni-[ville]`.

## Photo specs by section type

- **Step-card (process, 4 per service page):** portrait 800x1000px, medium shot, no people, `background-image + cover + center`, dark overlay `rgba(0,0,0,0.55)` via `multiply` blend.
- **Service-card (4 per city page, "Nos services à [Ville]"):** same 4 files reused across ALL city pages (`service-card-restauration.jpg` etc.), landscape 1200x600px, real `<img>` not background-image, no overlay, `object-fit: cover` + hover zoom.
- **FAQ background:** `linear-gradient(rgba(234,244,234,0.65), rgba(234,244,234,0.65)), url('/faq-bg-[page].jpg')` — mint overlay at 0.65 everywhere (uniformized). Different photo per page; keep pavé line/surface small in the photo — Gemini struggles with large paver surfaces.

## Known darkness bugs (check these two spots whenever a hero "isn't showing")

1. `.hero-bg` opacity/filter directly — correct values `opacity: 1; filter: none;` (a bug value of `opacity: 0.16` made heroes nearly invisible, hit on Buckingham once).
2. `.hero-overlay` background-image gradient — correct range `rgba(6,10,6, 0.45–0.55)`; found as high as 0.85–0.97 (near-opaque) on à-propos and avant-apres, both also still using external Unsplash stock photos at the time — both issues fixed by swapping to real site photos with the correct overlay range.

Check via `getComputedStyle(img).opacity` (want `1`) and `getComputedStyle(overlay).backgroundImage` (want a gradient in the 0.45-0.55 range, not 0.8+).

## The AI Studio image-corruption saga — summary and resolution

**Symptom:** regenerated hero/FAQ images would render fine immediately after generation, then silently break hours later or after rapid back-to-back regenerations — `naturalWidth: 0` despite a valid HTTP 200, and reading the file's first bytes showed `EF BF BD` (UTF-8 replacement char) instead of a valid JPEG header (`FF D8`) — a binary-to-text corruption on Gemini's/AI Studio's storage side, not a browser cache issue.

**What didn't cause it:** page structure (all Ottawa quartier pages share structure with unaffected Buckingham). What correlated: pages regenerated 2-3+ times in the same session. Account was confirmed on AI Studio's free tier (Chat Settings → Usage) — likely tier-related throttling/degradation contributed.

**Final, reliable fix: stop generating new images for pages hit by this bug — reuse existing stable photos instead.** The original 6 photos (Gatineau, Hull, Aylmer, Masson-Angers, Buckingham, Ottawa — never regenerated after initial creation) never corrupted. Pointing the 6 problem Ottawa quartier pages' `src`/`background-image` at these stable files instead of generating dedicated ones eliminated the problem entirely (no new file write = no corruption risk).

**Rule for any future city/quartier page:** don't attempt a dedicated hero/FAQ photo generation first — reuse a stable existing photo from creation, skip the generate→corrupt→clean→regenerate cycle entirely.

**Lesson on prompt style:** simple prompts outperformed elaborate ones on the free tier — heavily detailed "4K, hyper-detailed, cinema-quality" prompts gave *worse* results than a short plain prompt. Winning pattern reused successfully across pages:
> Generate a photorealistic photo of a backyard patio with interlocking pavers (pavé-uni), outdoor furniture, and garden landscaping, natural daylight. NO PEOPLE. Landscape orientation, 1600x900px. Save it as `[filename].jpg`. In `[page].html`, update the hero background image src to `/[filename].jpg`. DO NOT touch the H1 text or any other content.

**Standard verification script (run after any image generation, and again before declaring any page "done" — especially after a session gap):**
```js
async function checkValidJpeg(url) {
  const res = await fetch(url, {cache:'no-store'});
  if (!res.ok) return 'HTTP-' + res.status;
  const buf = await res.arrayBuffer();
  const bytes = new Uint8Array(buf.slice(0,4));
  return (bytes[0] === 0xFF && bytes[1] === 0xD8) ? 'OK' : 'CORRUPT';
}
```

## Google 5-star badge

Stable Maps link (not the "leave a review" link, which forces writing one): `https://maps.app.goo.gl/n6eG9kMzdxVsEfNK7`. Style: replicate homepage's `.social-promo-wrap` structure exactly. Text: "5.0 ★★★★★ — Noté 5 étoiles sur Google" (no exact review count, changes over time). Google logo recolored to solid black/dark-green outline (`#2c3e2c`, `fill="none"`) to match the Facebook/Instagram icon style nearby — not the official multicolor logo.

See also: [[duo-vert/design-system]], [[duo-vert/ai-studio-playbook]]
