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

**Obsidian community plugins (2026-08-01):** Emile browsed the community plugin store and screenshotted ~20 options. Claude only gave a reasoned opinion on 5 of them: recommended **Smart Connections** (local semantic search/related-notes, no API key) as worth considering, noted **Tag Wrangler**/**Linter** as nice-to-have but not urgent, and advised skipping **Copilot**/**Claudian** (redundant with the REST API MCP already giving Claude direct vault access — see [[duo-vert/memory-architecture]] — Claudian is also explicitly unreviewed by Obsidian staff). The other ~15 plugins shown (Excalidraw, Templater, Dataview, Tasks, Git, Calendar, Kanban, Iconize, Remotely Save, QuickAdd, etc.) were never evaluated by Claude at all. Emile installed only Smart Connections and judged the rest not worth it himself — that's his own call, not a Claude assessment, so don't treat this as "Claude reviewed and rejected 20 plugins."

See also: [[duo-vert/memory-architecture]], [[feedback/rename-move-verification-checklist]]
