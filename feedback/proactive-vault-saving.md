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

**Also push to GitHub automatically, no Terminal step needed from Emile (confirmed working 2026-07-30):** `git config credential.helper` returns `osxkeychain` on this Mac — meaning Emile's GitHub token was cached in macOS Keychain the first time he entered it manually, and Claude Code can commit AND push using that cached credential without any prompt or Emile involvement. After every vault write, run `git add -A && git commit -m "..." && git push` in `~/Documents/emile-secondbrain` — don't leave commits unpushed waiting on Emile to run it himself, that defeats the point. Only fall back to asking Emile to push manually if a push ever actually fails/prompts for credentials again (e.g. token expired or revoked).

**Failure caught 2026-07-30 — pending action items and TODOs specifically kept slipping through.** Twice in one session, real pending tasks existed only in conversation and were never written to the vault: (1) the fact that Cowork's Instructions field needed pasting into 8 projects, (2) an unconfirmed "check for a global memory setting" task. Both got caught only because Emile explicitly asked "are you sure you saved everything" and forced a re-check — that should never be necessary. **Root cause:** action items and "still outstanding" lists are easy to mentally track as "I'll mention this at the end" and then just... not, especially in a long session. **Fix:** the moment ANY pending task, TODO, or "you still need to do X" is stated — to Emile or about Emile's own next steps — write it to the vault immediately (usually `duo-vert/memory-architecture.md`'s outstanding-items section, or wherever the topic lives), not just say it out loud and trust it'll be remembered. Treat "I told the user about a pending task" and "I wrote the pending task down" as two different, both-required actions — saying it is not the same as saving it.

**Also: periodically re-read a file's "still outstanding" / status section before trusting it, don't just trust your own memory of what you last wrote.** The same investigation found a stale outstanding-items list claiming Cowork wasn't connected, well after it actually was — an old status line that never got updated once the situation changed. When resolving something that a file's status section describes, update that section in the same edit, don't leave it to a future pass.

This mirrors the same instruction given to Cowork (see `personal/about-emile.md` and the Cowork `duo-vert-ops` skill for the Cowork-side version, which has to ask Emile to relay instead of writing directly, since Cowork lacks write access).

See also: [[duo-vert/memory-architecture]], [[personal/about-emile]]
