---
name: feedback-proactive-vault-saving
description: Standing instruction to proactively write significant facts/lessons/preferences to the vault during or at the end of a session, without waiting to be asked
metadata:
  type: feedback
  modified: 2026-07-30
---

Emile explicitly asked (2026-07-30) that this not be a Cowork-only behavior — Claude Code should hold itself to the same standard, and since Code has real write access to the vault (unlike Cowork), the bar is higher: don't just notice something is worth saving, actually save it.

**Why:** Emile uses Claude 3-4 hours a day and does not want to be the one responsible for remembering to ask "did you save that." The system should catch it even if he'd have completely forgotten to bring it up himself.

**What counts as worth saving** (same bar as the Cowork instructions, for consistency):
- A new fact about Emile, Duo Vert, or another project that would still be true next week
- A mistake that got corrected, where the correction is a lesson worth not repeating
- A preference Emile stated or confirmed about how he wants work done
- A real decision that was made, with the reasoning behind it

**What NOT to save:** routine back-and-forth, one-off details that won't matter again, anything already captured, raw conversation content (see [[duo-vert/memory-architecture]] — curated facts only, never transcripts).

**How to apply:** don't wait until the very end of a session or until explicitly asked. When a save-worthy moment happens, either write it to the vault right then, or clearly note to self that it's pending and follow through before the session ends — never let it depend on Emile remembering to ask. If genuinely uncertain whether something crosses the bar, lean toward saving a short version rather than skipping it — a slightly-too-cautious note costs little; a silently lost fact costs a repeated conversation later.

**Always announce it (added 2026-07-30, per direct request):** whenever a file in the vault gets written or updated, say so explicitly in the reply — which file, roughly what was added. Never save silently. Emile caught this being skipped once already (the reasoning-and-pushback preference wasn't saved until he questioned it) — visibility is the whole point, don't let saves happen invisibly again.

This mirrors the same instruction given to Cowork (see `personal/about-emile.md` and the Cowork `duo-vert-ops` skill for the Cowork-side version, which has to ask Emile to relay instead of writing directly, since Cowork lacks write access).

See also: [[duo-vert/memory-architecture]], [[personal/about-emile]]
