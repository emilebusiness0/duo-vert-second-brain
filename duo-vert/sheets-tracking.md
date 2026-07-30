---
name: duo-vert-sheets-tracking
description: Emile's plan for 3 Google Sheets (leads, expenses, clients/revenue) for Duo Vert, and the lead-tracking automation that's now live
metadata:
  type: project
  modified: 2026-07-30
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

**Website form → Sheet pipeline:** duovert.ca's two quote forms (Netlify Forms) send a webhook on submission. Apps Script Web Apps always 302-redirect their response, and Netlify's webhook sender preserves POST instead of downgrading to GET on that redirect — so pointing the webhook directly at Apps Script always eventually breaks (405, then Netlify auto-disables the hook after 6 failures). **Fix:** a small serverless proxy function sits between Netlify and Apps Script, using Node's native `fetch` (which correctly downgrades POST→GET on 302) — deployed as its own minimal Netlify site, separate from duovert.ca's actual deploy.

Full technical detail (exact URLs, source file, dedup safeguard) lives in the `duo-vert` Claude Code skill's "Lead Tracking System" section — not duplicated here.

**Lesson learned:** never drag a folder onto an *existing* Netlify site's deploy area to test something unrelated — did this once and replaced the live duovert.ca site with a placeholder. Recovered via Netlify's Deploys tab (full deploy history is always recoverable), but always use "Add new site → Deploy manually" for anything not meant to replace the live site.

See also: [[duo-vert/company]], [[feedback/build-locally-not-live-browser]]
