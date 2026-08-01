---
name: duo-vert-sheets-tracking
description: Emile's plan for 3 Google Sheets (leads, expenses, clients/revenue) for Duo Vert, and the lead-tracking automation that's now live
metadata:
  type: project
  modified: 2026-08-01
---

Emile wants 3 Google Sheets (tabs, not website pages) to run [[duo-vert/company|Duo Vert]]'s back office:

1. **Leads sheet** — ✅ done, see below.
2. **Expenses sheet** — every purchase/money leaving the account. Not started.
3. **Clients/revenue sheet** — revenue per client. Not started.

**Why:** no structured tracking previously existed — quote requests only landed as emails, no lead pipeline, no expense or revenue tracking.

## Leads sheet — done and verified working (2026-07-30)

Sheet: "Duo Vert - Suivi Leads". 4 tabs: Pré-soumission, Post-soumission, Perdu, Tableau de bord (live dashboard formulas).

**Automation (Apps Script bound to the sheet):**
- Checking "Soumission envoyée" in Pré → row auto-moves to Post.
- Setting Résultat = "Perdu" in Post → row moves to Perdu, tagged Raison = Refus.
- Both follow-ups checked + 5 days no resolution → daily trigger auto-moves to Perdu, Raison = Sans réponse.
- Lead numbers auto-increment via a script property counter.

**Visual diagram (2026-08-01):** `Excalidraw/lead-webhook-pipeline.excalidraw.md` in this vault — a 4-box diagram (Netlify Form → Proxy → Apps Script → Google Sheet) of the exact flow described below. Open it in Obsidian's Excalidraw view.

**Website form → Sheet pipeline:** duovert.ca's two quote forms — `soumission-gratuite` on `/soumission/`, `soumission-accueil` on the homepage, both Netlify Forms with identical fields (nom, telephone, courriel, adresse, service, message, photo) — send a webhook on submission via Netlify's "Form submission notifications → HTTP POST request" feature.

**⚠️ Critical incompatibility (why a direct webhook doesn't work):** Google Apps Script Web Apps always respond via a 302 redirect to a `script.googleusercontent.com` URL to serve their actual output — unavoidable, baked into the platform. Netlify's webhook sender follows that redirect but preserves POST instead of downgrading to GET (which the redirect target requires), so it always gets **405 Method Not Allowed** back — even though the Apps Script `doPost()` already executed and wrote the row *before* the redirect was issued. After 6 consecutive perceived failures, Netlify **auto-disables the webhook**. This is structural, not a transient bug — pointing the webhook directly at the Apps Script `/exec` URL will always eventually break this way, and no Apps Script code change fixes it.

**Fix — the proxy, full detail:** a small serverless proxy function sits between Netlify and Apps Script. It receives the webhook, forwards it to Google using Node's native `fetch` (which correctly downgrades POST→GET on 302 per spec), and returns a clean `200` to Netlify. Live at **`https://elaborate-gumdrop-199ffb.netlify.app/.netlify/functions/lead-webhook`**, deployed as its own **separate, minimal Netlify site** — NOT part of duovert.ca's actual deploy/repo (`~/Documents/duovert-site`), so nothing in that repo controls or documents this proxy; it's a standalone Netlify site with its own source. Source: single file `netlify/functions/lead-webhook.js` + a `netlify.toml` with `[functions] directory = "netlify/functions"` (required — Netlify's drag-and-drop manual deploys don't auto-detect a functions folder without it). Both of duovert.ca's form webhooks point at this proxy URL, not at the Apps Script URL directly.

**Resolved 2026-08-01 — proxy source now recovered and stored locally.** Emile downloaded and provided the proxy's source (separate from the `duovert-site` transfer, as expected since it's a standalone Netlify site). Now lives at `~/Documents/duovert-lead-proxy` on this Mac: `netlify.toml` ([functions] directory = "netlify/functions") + `netlify/functions/lead-webhook.js`. Verified the code matches this doc's description exactly — a Node `fetch` call with `redirect: 'follow'` to the Apps Script exec URL, which correctly downgrades POST→GET on the 302 per spec (unlike Netlify's own webhook sender).

**Deliberately NOT recording the exact Apps Script exec URL here** — this vault's GitHub repo is public, and that URL is a live, unauthenticated POST endpoint into the leads sheet; publishing it would let anyone who finds the repo submit fake leads directly, bypassing the actual website. The real URL is in the `GAS_URL` constant inside `~/Documents/duovert-lead-proxy/netlify/functions/lead-webhook.js` on this Mac — check the file directly rather than looking for it here. (This repo isn't currently git-tracked/backed up anywhere else — worth doing at some point the same way `duovert-site` now is, but keep any future repo for it private, not public like this vault.)

The Apps Script has a dedup safeguard (`isDuplicateSubmission`, keyed by Netlify's submission payload) in case Netlify ever retries a delivery — prevents duplicate rows even if a retry occurs. Verified working via a real end-to-end test submission on 2026-07-29 (single row landed, no duplicates).

**Lesson learned:** never drag a folder onto an *existing* Netlify site's deploy area to test something unrelated — did this once and replaced the live duovert.ca site with a placeholder. Recovered via Netlify's Deploys tab (full deploy history is always recoverable), but always use "Add new site → Deploy manually" for anything not meant to replace the live site.

**2026-07-31 update — added "Date visite prévue" column:** Emile wanted the planned quote-visit date visible on Pré-soumission instead of buried in email/texts. Added a new column O ("Date visite prévue", plain date entry, no checkbox) between Follow-up 2 and Soumission envoyée (which shifted from O to P). Updated `Code.gs` accordingly: `PRE_HEADERS` gained the new header, `checkboxes` opt changed from `['M','N','O']` to `['M','N','P']`, and every hardcoded PRE-sheet column reference shifted by one — `handlePreEdit`'s `soumisCol` 15→16, and the `getRange(row,1,1,15)` reads in `moveRowPreToPost`/`moveRowPreToLost` → 16. `addLeadFromForm` and `promptManualLead`'s row-builder arrays each gained one blank/`''` entry before the trailing Soumission `false`. Column insert had a side effect worth remembering: inserting a column left of a checkbox column carries the checkbox *data validation* onto the new column too — had to manually strip validation from the new date column and re-add a checkbox rule to the shifted Soumission column afterward.

See also: [[duo-vert/company]], [[feedback/build-locally-not-live-browser]]
