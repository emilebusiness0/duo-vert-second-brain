---
name: duovert-soumission-template
description: "Confirmed Google Doc template/workflow for Duo Vert client soumissions (quotes), including pricing rules and a Drive API gotcha"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-01T19:52:07.533Z
  originSessionId: 42ce4579-01c7-4e2a-959b-56b7c2d869a8
---

Emile confirmed a working template for individual client soumission (quote) docs, built from the original doc `https://docs.google.com/document/d/122GpPX5hk4L-gUrfROxLdHhSXwNsZ_EH-dcqQNZwpZc/edit` (soumission #35, Carrie Laflamme). Final approved example: soumission #36, Grace, 10 Eliza Simon — `https://docs.google.com/document/d/1vQV7J5XN23RqY6Lv-BbO8YIX-UQcliqsmmVVYw2HsAU/edit`.

**Header block (top of doc):** "DUO VERT" (H1 bold) then a single line with `email | phone | website` — `duo.vert.gatineau@gmail.com | 819 328-2129 | duovert.ca`. Emile explicitly asked for phone + website to be added; the original template only had the email.

**Structure (in order):** H1 "DUO VERT" + contact line → H1 "SOUMISSION – PAVÉ UNI / INTERLOCK" → client fields (Date, No de soumission, Nom du client, Adresse du projet, Téléphone) each **bold label**, one per line → H2 "Description des travaux proposés" (bullet list) → Notes supplémentaires → H2 "Coût de la main-d'œuvre" (itemized lines, then bold total, bold rabais, bold final total) → H2 "Matériaux" → H2 "Total estimé du projet" → H2 "Conditions importantes" (4 standard bullets, reused verbatim across quotes) → H2 "Acceptation de la soumission" (signature/date blanks).

**Pricing rules confirmed by Emile:**
- Rabais avant+arrière (front+back discount) = 15% off total main-d'œuvre when both areas are being worked on — applies as a flat discount line, not per-item.
- Use non-round numbers for line items (e.g. 2931$, 597$, 512$ instead of 2900/600/500) — Emile's stated reason: round numbers "make it look suspicious" (less like a real itemized cost, more like an obviously made-up estimate).
- **Materials are NOT estimated with a dollar figure** — this was a deliberate change from the original template (which had an itemized "Matériaux estimés" section with a subtotal). Emile wants a single line stating materials are billed separately at actual cost at project end, since exact costs aren't knowable upfront. Don't reintroduce a materials estimate subtotal for future quotes unless he asks.

**Workflow gotcha — no Google Docs edit-in-place tool available.** Only `create_file` (new file) and `copy_file` (duplicate) exist in the connected Drive MCP tools — there is no update/patch tool for an existing Doc's content. Every revision required creating a brand-new file, which caused real confusion this session (multiple same-titled files existed simultaneously, Emile opened a stale one and thought numbers were wrong). **Always create a new file with the SAME title** (overwriting the visual identity, not the file) and explicitly tell Emile which old copies to delete — do not assume this is understood, state it every time until confirmed handled.

**Markdown → Google Doc conversion gotcha:** `create_file` with `contentMimeType: text/markdown` correctly converts `#`/`##` headers and `**bold**` into real Google Docs formatting (verified via `read_file_content` showing back the same markdown-style output, matching how the original human-made template reads). But **a single `\n` between lines does NOT create a paragraph break** — markdown needs a full blank line (`\n\n`) between every line meant to render as its own paragraph, or lines silently merge into one run-on paragraph. This bit us twice (once on bullet lists, once on the client-info field block). Always double-check every field block and bullet list uses blank-line separation before sending a soumission draft.

See also: [[duo-vert/company]], [[duo-vert/sheets-tracking]]
