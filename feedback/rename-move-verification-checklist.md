---
name: feedback-rename-move-verification-checklist
description: When renaming or moving something (a folder, repo, vault), verify both the functional layer and any separate identity-registry layer before declaring it done
metadata:
  type: feedback
  modified: 2026-08-03
---

Caught during the 2026-08-01 vault rename (`second-brain` → `emile-secondbrain`): after renaming the local folder and GitHub repo, verification checked the REST API responded and file listing was correct, then declared it "all clear." But Obsidian tracks vault identity in its own separate config (`~/Library/Application Support/obsidian/obsidian.json`), independent of the filesystem — that registry still pointed at the old, now-nonexistent path. The API kept working only because the already-running Obsidian process had the old folder handle open in memory, which masked the stale registry underneath. Emile had to ask "does Obsidian show the new name" before this surfaced.

**Why this matters:** a running process can keep functioning on a stale reference for a while after a rename, making "it still works" a false signal that everything is consistent. The real risk shows up later — on next app restart, next scheduled run, next fresh session — when the stale reference finally gets read cold.

**How to apply:** whenever renaming or moving something with any persistent identity (a vault, a repo, a scheduled job's target, an app's tracked folder) — check two separate things, not just one:
1. **Does it still function right now** (API responds, files list correctly) — the easy, obvious check.
2. **Does every place that persists the old identifier independently of the live process know about the change** — config files, registries, other apps' bookmarks, scheduled job configs. This is the one that's easy to skip because nothing visibly breaks until a fresh read happens.

Concretely for this vault: a rename touches at minimum — the folder itself, Claude Code's memory symlinks, the GitHub repo name + remote URL, the nightly consolidation routine's git source, Obsidian's `obsidian.json` vault registry, and any in-file text mentioning the old name. Don't declare a rename complete after checking only the first two or three of these.

**Also:** don't say "I'll fold this into a checklist" or "I'll remember this for next time" in chat without actually writing it down in the same turn — see [[feedback/proactive-vault-saving]] for why that specific failure mode (saying it instead of saving it) keeps recurring.

See also: [[feedback/proactive-vault-saving]], [[duo-vert/memory-architecture]], [[feedback/vault-cross-reference-integrity]]
