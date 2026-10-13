---
name: decision-guard
description: Audit intended work against recorded decisions and hard
  constraints now, or revisit and override a past decision
---

# Decision Guard

Use the `decision-guard` skill. Be thorough on analysis; keep the reply condensed.

1. Read `.cursor/decisions.md` and the Goal / Scope / Out of scope /
   Constraints sections of `.cursor/context.md` if they exist; summarize
   rails briefly.
2. Look at the user's goal, focus topic, or open intent; infer implications
   and find conflicts or new high-impact forks.
3. Present open items in the skill's condensed format (1–5 max). If
   aligned, say so in one line.
4. On override or new choice, update `.cursor/decisions.md` (architecture)
   and/or `.cursor/context.md` (scope/constraints) once the user confirms.
5. If the user wants to change a past entry, explain impact briefly, then
   update the relevant file once they confirm.

If the user passed arguments, treat them as the topic or decision to focus on.
