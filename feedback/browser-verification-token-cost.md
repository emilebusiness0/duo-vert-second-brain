---
name: browser-verification-token-cost
description: Prefer text-based browser extraction (get_page_text, javascript_exec, read_page) over screenshots for verifying browser state — screenshots dominate token cost in browser-automation sessions.
metadata:
  type: feedback
  modified: 2026-07-31
---

When checking whether a browser action succeeded or reading content from a live page, default to text-based tools (`get_page_text`, `javascript_exec` reading `document.body.innerText`, `read_page`) instead of `computer{action:"screenshot"}` — unless the actual visual layout/rendering is what needs checking (e.g. confirming a checkbox vs. a dropdown rendered correctly, or a design/CSS review).

**Why:** flagged by Emile (2026-07-31) after a session where token usage felt very high for the amount of actual work done. Root cause: a code-editor mishap (see [[feedback/build-locally-not-live-browser]]) triggered ~30+ screenshot round-trips for recovery and verification. Each screenshot costs roughly a page of text in tokens — 10-50x a text-extraction call for the same information. Separately, verifying a small piece of a long page by scrolling 10 ticks at a time and screenshotting each step (to visually search for a function/string) was very wasteful — a single `javascript_exec` regex match over `document.body.innerText` finds it in one call regardless of scroll position.

**How to apply:**
- Confirming an edit landed / text is present → `get_page_text` or `javascript_exec` grep, not a screenshot.
- Searching for a specific string/element inside a long or virtualized page (code editors, infinite-scroll feeds) → `javascript_exec` over `document.body.innerText` or `read_page`, not scroll+screenshot loops.
- Only screenshot when the *visual* result is the thing being verified (layout, color, whether a checkbox vs. plain cell rendered, a design review) — and even then, prefer one targeted `zoom` over a full screenshot when checking a specific region.
- When a verification loop is about to become "scroll, screenshot, repeat" more than 2-3 times, stop and switch to a text-search approach instead.

See also: [[feedback/build-locally-not-live-browser]]
