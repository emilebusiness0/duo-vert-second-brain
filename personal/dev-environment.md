---
name: personal-dev-environment
description: What's installed and authenticated on Emile's Mac for Claude Code to use directly, so future sessions don't assume tools are missing
metadata:
  type: user
  modified: 2026-08-01
---

**Homebrew**: installed 2026-08-01 (`/opt/homebrew/bin/brew`). Not yet on PATH in already-open Terminal windows/sessions — new windows pick it up automatically via `.zprofile`; from inside a Claude Code session, call it by full path (`/opt/homebrew/bin/brew`, `/opt/homebrew/bin/gh`) rather than assuming `brew`/`gh` resolve on PATH.

**GitHub CLI (`gh`)**: installed via Homebrew 2026-08-01, authenticated as `emilebusiness0` over HTTPS with scopes `gist, read:org, repo, workflow`. This means repo-level admin actions (rename, create, delete, PR/issue management) can be done directly via `gh` from Claude Code — no more walking Emile through the GitHub Settings UI manually for things `gh` can do. Reason it got installed: hit a wall renaming the `duo-vert-second-brain` → `emile-secondbrain` GitHub repo (see [[duo-vert/memory-architecture]]) with no CLI available; had to ask Emile to do it via browser.

**Existing git auth (separate from `gh`):** `git config credential.helper` returns `osxkeychain` — a cached credential already lets plain `git push`/`pull` work without prompts, independent of `gh`'s own auth. Both now work; `gh` adds account/repo-admin-level actions on top of what plain git already could do.

See also: [[duo-vert/memory-architecture]], [[feedback/rename-move-verification-checklist]]
