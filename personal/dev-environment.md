---
name: personal-dev-environment
description: What's installed and authenticated on Emile's Mac for Claude Code to use directly, so future sessions don't assume tools are missing
metadata:
  type: user
  modified: 2026-08-01 (Obsidian plugin decision added)
---

**Homebrew**: installed 2026-08-01 (`/opt/homebrew/bin/brew`). Not yet on PATH in already-open Terminal windows/sessions — new windows pick it up automatically via `.zprofile`; from inside a Claude Code session, call it by full path (`/opt/homebrew/bin/brew`, `/opt/homebrew/bin/gh`) rather than assuming `brew`/`gh` resolve on PATH.

**GitHub CLI (`gh`)**: installed via Homebrew 2026-08-01, authenticated as `emilebusiness0` over HTTPS with scopes `gist, read:org, repo, workflow`. This means repo-level admin actions (rename, create, delete, PR/issue management) can be done directly via `gh` from Claude Code — no more walking Emile through the GitHub Settings UI manually for things `gh` can do. Reason it got installed: hit a wall renaming the `duo-vert-second-brain` → `emile-secondbrain` GitHub repo (see [[duo-vert/memory-architecture]]) with no CLI available; had to ask Emile to do it via browser.

**Existing git auth (separate from `gh`):** `git config credential.helper` returns `osxkeychain` — a cached credential already lets plain `git push`/`pull` work without prompts, independent of `gh`'s own auth. Both now work; `gh` adds account/repo-admin-level actions on top of what plain git already could do.

**Obsidian community plugins:** Emile evaluated a batch on 2026-08-01 (Excalidraw, Templater, Dataview, Tasks, Git, Calendar, Style Settings, Kanban, Iconize, Remotely Save, QuickAdd, Minimal Theme Settings, Editing Toolbar, Omnisearch, Copilot, Claudian, Smart Connections, Tag Wrangler, Linter, Recent Files, and more) — decided only **Smart Connections** was worth installing (local semantic search/related-notes, no API key needed); installed it. Everything else rejected as not useful for this vault's size/use case, including Copilot and Claudian (both redundant with the REST API MCP connection already giving Claude direct vault access — see [[duo-vert/memory-architecture]] — and Claudian is explicitly unreviewed by Obsidian staff).

See also: [[duo-vert/memory-architecture]], [[feedback/rename-move-verification-checklist]]
