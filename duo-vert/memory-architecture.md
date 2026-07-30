---
name: duo-vert-memory-architecture
description: How this vault works, why curated notes over raw session dumps, and the real sync gap between Claude Code, Cowork, and claude.ai
metadata:
  type: project
  modified: 2026-07-30
---

**Setup (2026-07-30):** Emile migrated from an old computer to a new Mac, which surfaced that Claude Code's memory (`~/.claude/projects/.../memory/`) is local to one machine, and separate from Cowork's own skill storage and claude.ai's memory feature — three disconnected stores. Also discovered `duovert-site-fixed` (the actual website source files) never made it to the new Mac; the Netlify-deployed site's source still needs transferring from the old machine.

**This vault (`~/Documents/second-brain/`) is the fix for the memory-fragmentation part**, not the missing-site-files part. Structure: a small `README.md` index + one file per topic, cross-linked with `[[wikilinks]]` — same pattern Claude Code's memory already used, just relocated somewhere both Code and (once connected) Cowork can reach, and Obsidian-browsable.

**Curated, not raw-dump.** Considered and rejected a PDF-guide approach (an Instagram lead-magnet from @alex2learn) that parses every Claude Code `.jsonl` session file into one markdown file per session (raw transcripts, tool calls included). Rejected because: (1) it scales to tens of MB of mostly noise per year, (2) the guide's own docs flag a real risk of leaked API keys sitting in old raw sessions, (3) it only covers Claude Code, not Cowork or claude.ai, (4) it isn't actually automatic either — needs a manual `/slay` sync command. Curated notes avoid all four: small, nothing goes in that wasn't deliberately written, works the same for any product that can read a folder, and Obsidian's graph view is actually *more* useful over ~40 meaningful topic nodes than over hundreds of raw session dumps.

**Mechanics — what's automatic vs. not:**
- **Claude Code:** auto-reads this vault's index at the start of every session (via a symlink from the original `~/.claude/projects/.../memory/` path into this vault — see below). This part is native/automatic.
- **Cowork:** has no equivalent forced read. Only reads this vault if (a) the folder is explicitly connected to the session, and (b) a triggered skill's instructions say to read it first. This still needs to be written into Cowork's `duo-vert-ops` skill.
- **claude.ai chat:** no filesystem access at all — stays permanently manual (upload relevant files to Project knowledge).
- **Writing back:** neither product force-saves at session end. It only happens if the skill/instructions say to, and the update actually gets made before the session ends. Same risk as before, just relocated.

**Why the underlying Code memory files still physically live at `~/.claude/projects/.../memory/`:** that exact path is hardcoded into how Claude Code auto-injects memory at session start — not something that can be redirected from inside a session. So the real files live in this vault, and symlinks at the original path point back here, keeping the auto-load working without duplicating content.

**Full website-build history migrated in (2026-07-30, same session):** the entire ~717-line `duo-vert` Claude Code skill (design system, AI Studio prompt playbook, photo workflow + the image-corruption saga, SEO audit history) was split into 5 topic files and added to the vault + symlinked into Code's memory index — [[duo-vert/website-build-overview]], [[duo-vert/design-system]], [[duo-vert/ai-studio-playbook]], [[duo-vert/photo-workflow]], [[duo-vert/seo-history]]. Reason: the vault previously only had the small curated memory files, not 6 months of build decisions that actually live in the skill — Emile flagged this gap directly ("that's not enough context... werent you going to add all the duo vert skill content inside?").

**Confirmed working end-to-end this session:** Obsidian opened the vault correctly via "Open folder as vault" (the `obsidian://open` deep-link didn't work on first launch before any vault existed — needed the manual Open once; should work automatically after that). Graph view confirmed functional.

**Still outstanding, not done yet:**
1. Cowork's `duo-vert-ops` skill has read/write instructions pointing at this vault, but the vault folder itself has not been connected to a Cowork session yet — that's a UI action only Emile can do (no tool exists to do it remotely). Until connected, Cowork cannot actually reach these files.
2. `duovert-site-fixed` (actual website source) still not transferred from the old computer to this Mac.

**Going forward:** every future Code/Cowork session working on Duo Vert should read this vault's `README.md` first, then write a short dated update to the relevant file at session end — this is how "what did we do 2 weeks ago" will keep working. Confirmed with Emile this is the expected behavior (2026-07-30).

See also: [[duo-vert/company]], [[duo-vert/sheets-tracking]], [[duo-vert/website-build-overview]]
