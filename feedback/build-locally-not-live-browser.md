---
name: feedback-build-locally-not-live-browser
description: Prefer generating files locally for the user to import/paste themselves over driving a live browser session to build things in Google Sheets/Apps Script/etc.
metadata:
  type: feedback
  modified: 2026-07-30
---

When building something that will live in an external web app (Google Sheets + Apps Script, a config file, etc.), prefer writing the deliverable to a local file and handing it to the user to import/paste themselves, rather than driving a live browser automation session to click through the UI.

**Why:** confirmed directly by Emile (2026-07-30) — on a previous device, this was the standard workflow and he noted it was faster, used far fewer tokens, and produced a cleaner result. Live browser automation for this kind of task is slow, token-heavy, and fragile — hit real problems doing it live: hung Apps Script "Run" executions, an OAuth consent popup requiring manual clicks, and a nasty Google Sheets API bug (`getLastRow()` false-positive from checkbox/validation formatting on empty rows, causing `appendRow()` to silently write data hundreds of rows below the visible area).

Second incident (2026-07-31): while driving the Apps Script code editor live to *inspect* it (not even to build), keyboard shortcuts I sent didn't behave like shortcuts — `Ctrl+F` and `Page_Down` were typed as literal text directly into the live script instead of opening find/scrolling. Had to undo/backspace it out under time pressure, and a safety classifier then blocked further delete/backspace keystrokes in that tab, stalling recovery. Confirms the code editor's shortcut surface is unreliable for automation, not just risky for building.

**How to apply:** When the deliverable is something like an Apps Script project, a spreadsheet structure, or similar — write the complete file(s) locally (e.g. a `Code.gs` script), give it to the user, and brief them on the single manual step (e.g. "paste this into Extensions > Apps Script, save, run setup()"). Only fall back to live browser-driving when the task genuinely requires interacting with existing live state that can't be reconstructed from a file. This applies to *inspecting* the Apps Script editor too, not just writing to it — prefer read-only extraction (see [[feedback/browser-verification-token-cost]]) over clicking/typing inside that editor even just to look.

See also: [[duo-vert/sheets-tracking]], [[feedback/browser-verification-token-cost]]
