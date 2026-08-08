---
name: feedback-vault-cross-reference-integrity
description: When a fact moves or a file gets added, propagate the change to everything that references it, in the same edit — don't leave stale pointers for a later consolidation pass
metadata:
  type: feedback
  modified: 2026-08-03
---

Caught during the 2026-07-31 memory consolidation: two files (`duo-vert/company.md`, `duo-vert/sheets-tracking.md`) still pointed to the old `.claude/skills/duo-vert/SKILL.md` as the source of full detail, even though that content was migrated into the vault on 2026-07-30 — a full day earlier, same session lineage. The vault's own `README.md` was also missing two feedback files that already existed and were already listed in `MEMORY.md`.

**Root cause:** writes happened in the one file a fact directly belongs to, without checking what else in the vault referenced the old location or needed a matching index entry. Nothing was structurally broken — it just wasn't caught until an explicit consolidation pass went looking.

**How to apply, going forward:**
1. **When content moves or a fact supersedes an old pointer** (e.g. a skill file gets migrated into the vault, a project changes location or owner) — before finishing that edit, search the vault for other files referencing the old location/fact and fix them in the same pass, not as a deferred cleanup.
2. **When a new file is added to `MEMORY.md`** (Claude Code's index), add the matching entry to the vault's own `README.md` in the same action. They're meant to mirror each other; treat a MEMORY.md-only add as incomplete.
3. **Don't rely on periodic consolidation to catch this.** Consolidation is for genuine staleness (dated info, retired projects) and overlap — not for basic index hygiene, which should never have drifted in the first place.

**Why:** the whole point of the vault is that a fact written once stays findable and accurate without a human re-auditing it. A stale pointer is worse than no pointer — it looks authoritative and sends the next session to a file that may no longer exist.

See also: [[duo-vert/memory-architecture]], [[feedback/proactive-vault-saving]], [[feedback/rename-move-verification-checklist]]
