---
name: feedback-fix-root-cause-not-just-instance
description: Standing instruction — whenever a problem is found and fixed, also fix whatever let it happen in the first place, not just the one instance
metadata:
  type: feedback
  modified: 2026-08-01
---

Emile said explicitly (2026-08-01): "chaque fois qu'on rencontre un problème, tu répares le problème, mais tu fais aussi en sorte que ça n'arrive plus." — every fix must include a recurrence-prevention step, not just resolve the immediate instance.

**Why:** the same day, a new memory file (`project_duovert_soumission_template.md`) got written as a real file directly inside `.claude/projects/.../memory/` instead of into the vault (`~/Documents/emile-secondbrain/`) with a symlink — breaking the pattern every other memory file follows (see [[duo-vert/memory-architecture]]). Fixing that one file wasn't enough on its own; the underlying cause (not checking the existing pattern before writing a brand-new memory file) needed a concrete counter-measure, or the same mistake happens on the next new file too.

**How to apply — concrete recurrence-prevention checklist, not just "be more careful":**
- **Vault file placement bug (this incident's fix):** before writing any *new* memory file, run `ls -la` on the `.claude/projects/.../memory/` directory first and confirm the pattern (existing entries are symlinks into the vault) — then create the real file inside `~/Documents/emile-secondbrain/<category>/<name>.md` and symlink it into the memory directory, never write directly into the memory directory.
- **General principle for any future bug:** after fixing the immediate instance, explicitly ask "what let this happen, and does the fix address that cause or just this occurrence?" If the cause is a missing check, a bad default, a step that's easy to skip, or an assumption that turned out false — change the process/tooling/code so that class of mistake can't recur silently, not just patch the symptom. Write the fix (a new step, a script safeguard, a code guard) into a memory file so it's actually followed next time, per [[feedback/proactive-vault-saving]].
- Examples already applied this way in the Duo Vert sheet work: `setup()` used to wipe data with no confirmation — fixed by adding a confirmation dialog AND an automatic backup (not just restoring the one wiped dataset); `onEdit` used to swallow errors silently — fixed by logging errors going forward (not just manually re-adding the two leads that got stuck).

See also: [[feedback/proactive-vault-saving]], [[duo-vert/memory-architecture]], [[duo-vert/soumission-template]]
