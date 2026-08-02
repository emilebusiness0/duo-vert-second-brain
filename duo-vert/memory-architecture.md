---
name: duo-vert-memory-architecture
description: How this vault works, why curated notes over raw session dumps, and the real sync gap between Claude Code, Cowork, and claude.ai
metadata:
  type: project
  modified: 2026-08-02
---

**Setup (2026-07-30):** Emile migrated from an old computer to a new Mac, which surfaced that Claude Code's memory (`~/.claude/projects/.../memory/`) is local to one machine, and separate from Cowork's own skill storage and claude.ai's memory feature — three disconnected stores. Also discovered `duovert-site-fixed` (the actual website source files) never made it to the new Mac; the Netlify-deployed site's source still needs transferring from the old machine.

**This vault (`~/Documents/emile-secondbrain/`) is the fix for the memory-fragmentation part**, not the missing-site-files part. Structure: a small `README.md` index + one file per topic, cross-linked with `[[wikilinks]]` — same pattern Claude Code's memory already used, just relocated somewhere both Code and (once connected) Cowork can reach, and Obsidian-browsable.

**Curated, not raw-dump.** Considered and rejected a PDF-guide approach (an Instagram lead-magnet from @alex2learn) that parses every Claude Code `.jsonl` session file into one markdown file per session (raw transcripts, tool calls included). Rejected because: (1) it scales to tens of MB of mostly noise per year, (2) the guide's own docs flag a real risk of leaked API keys sitting in old raw sessions, (3) it only covers Claude Code, not Cowork or claude.ai, (4) it isn't actually automatic either — needs a manual `/slay` sync command. Curated notes avoid all four: small, nothing goes in that wasn't deliberately written, works the same for any product that can read a folder, and Obsidian's graph view is actually *more* useful over ~40 meaningful topic nodes than over hundreds of raw session dumps.

**Mechanics — what's automatic vs. not:**
- **Claude Code:** auto-reads this vault's index at the start of every session (via a symlink from the original `~/.claude/projects/.../memory/` path into this vault — see below). This part is native/automatic.
- **Cowork:** has no equivalent forced read. Only reads this vault if (a) the folder is explicitly connected to the session, and (b) a triggered skill's instructions say to read it first. This still needs to be written into Cowork's `duo-vert-ops` skill.
- **claude.ai chat:** no filesystem access at all — stays permanently manual (upload relevant files to Project knowledge).
- **Writing back:** neither product force-saves at session end. It only happens if the skill/instructions say to, and the update actually gets made before the session ends. Same risk as before, just relocated.

**Why the underlying Code memory files still physically live at `~/.claude/projects/.../memory/`:** that exact path is hardcoded into how Claude Code auto-injects memory at session start — not something that can be redirected from inside a session. So the real files live in this vault, and symlinks at the original path point back here, keeping the auto-load working without duplicating content.

**Full website-build history migrated in 2026-07-30:** the old `duo-vert` Claude Code skill (design system, AI Studio prompt playbook, photo workflow, SEO audit history) was split into 5 topic files, now the source of truth — [[duo-vert/website-build-overview]], [[duo-vert/design-system]], [[duo-vert/ai-studio-playbook]], [[duo-vert/photo-workflow]], [[duo-vert/seo-history]].

**Still outstanding (Tasks-plugin format, added 2026-08-01 — installed the Obsidian Tasks plugin, converting this list to real trackable checkboxes instead of prose):**
- [x] `duovert-site-fixed` transferred from old computer ✅ 2026-08-01 — Emile uploaded to Google Drive; Claude Code pulled it via the Drive API, restored to `~/Documents/duovert-site`, rebuilt `node_modules`, verified local preview (`npm run dev`, port 3000). Full detail in [[duo-vert/website-build-overview]].
- [ ] Paste the drafted read-bias + proactive-flagging Instructions text into all 8 of Emile's Cowork projects (separate from the `duo-vert-ops` skill file) — only the Duo Vert project has it applied so far via the GitHub-Context connection.
- [ ] Check whether Cowork has an account-level (not per-project) Memory/Personalization setting that could cover all 8 projects without connecting the GitHub repo to each individually — check under `Emile · Pro` account settings / Customize.

**Going forward:** every future Code/Cowork session working on Duo Vert should read this vault's `README.md` first, then write a short dated update to the relevant file at session end — this is how "what did we do 2 weeks ago" will keep working. Confirmed with Emile this is the expected behavior (2026-07-30).

## Cowork read/write — resolved (2026-07-30 – 2026-08-01)

Vault connected to Cowork via the public GitHub repo `emilebusiness0/emile-secondbrain` (pushed manually via `git push` from Terminal — Cowork can't handle credentials).

**Read confirmed reliable** — works via a different available integration ("vibiz"), not Cowork's own web-fetch tool (which returned empty/failed). Verified genuine live pulls (not cache/hallucination) across 3 separate tests, including one where Cowork correctly reported facts added to the vault in that same session.

**Write confirmed impossible** — Cowork has no sandbox/shell environment (its "Code" tab is just a shortcut into Claude Code, not independent execution), so it can't `git push`. The in-chat folder-connect tool also doesn't persist across sessions.

**Resolution:** Cowork's `duo-vert-ops` skill (Cowork's own local copy, separate from this vault) drafts a copy-pasteable update in the chat reply at session end instead of attempting to write anywhere; Emile relays it into a Code session or edits the GitHub file directly — only needed for sessions that actually produced something worth remembering.

See also: [[duo-vert/company]], [[duo-vert/sheets-tracking]], [[duo-vert/website-build-overview]]
