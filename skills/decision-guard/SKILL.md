---
name: decision-guard
description: Detect when intended edits contradict recorded decisions or hard
  constraints, or imply a significant undecided architecture fork. Use before
  non-trivial implementation when .cursor/decisions.md or .cursor/context.md
  exists.
---

# Decision Guard

Goal: keep recorded decisions and hard constraints alive during build. Be
thorough when finding drift; be silent when aligned; be condensed when asking.
The user resolves conflicts; the agent does not silently override rails.

**Presentation:** Prefer an interactive question tool when available. No
preamble, no plan or implementation in the same message as a conflict round.

## 1. Load settled rails

Check these sources, in order:

1. The user's current request (and any active plan)
2. `.cursor/decisions.md` — architecture and design Choices
3. `.cursor/context.md` — only **Goal**, **Scope**, **Out of scope / no-gos**,
   and **Constraints** (ignore Acceptance; that is out of scope for this skill)
4. The existing code only as needed to interpret implications

If neither log file exists, stop — nothing to guard.

## 2. Infer implications

From the intended change set (files, stack, data, auth, APIs, dependencies,
structure), list what the work *implies* architecturally and for scope.
Know why each implication matters — even if you never show it.

## 3. Compare

Classify each material implication:

| Class | When |
| ----- | ---- |
| **Conflict** | Contradicts a recorded `Choice`, or a hard constraint / no-go / out-of-scope item |
| **New fork** | Significant undecided decision (costly to reverse) not covered by Assumed or Defaults |
| **Aligned** | Fits settled Choices and constraints |

If everything is **Aligned**, proceed silently. Do not announce a guard pass.

## 4. Present (conflicts and new forks only)

Ask only **1–5** highest-impact open items. Prefer an interactive question
tool when available.

```markdown
**Rails:** `Decision A = X`; `Out of scope: Y`

### 1. <Conflict or new fork>?
*<one-line why this contradicts / matters>*
- **A)** Keep recorded choice — <adjust approach>
- **B)** Override decision — <update log; impact>
- **C)** other / explain more
→ **Suggest: A** — <one-line>
```

Per item:

- For **conflicts**, option A keeps the log and forces the agent to change
  approach; option B explicitly overrides and will update the log.
- For **new forks**, options are real architecture choices (2–4), same
  condensed one-line pro/con style as autoscope; include an other path.
- Recommend, but do not choose for the user on significant items.
- Expand only if the user asks for more detail.

## 5. Record answers

After the user answers (or explicitly defers):

1. **Keep recorded choice** — do not change the log; adjust the
   implementation approach to match.
2. **Override or new architecture choice** — append to `.cursor/decisions.md`
   (create if needed):

   ```markdown
   ## <Decision title>

   - **Choice:** <what the user picked>
   - **Date:** <YYYY-MM-DD>
   - **Rationale:** <why, in the user's words where possible>
   - **Alternatives considered:** <brief list>
   ```

3. **Scope or constraint change** — update `.cursor/context.md` (Goal /
   Scope / Out of scope / Constraints only).

For anything unanswered that you must proceed with, list it explicitly as
**Assumed X; change if wrong**—never silent contradiction of a recorded Choice.

## 6. Stop condition

Do not implement the conflicting path in the same message as the chooser.
After answers (or silent align), continue building against the updated rails.

Follow-up round only if answers open a new high-impact conflict or fork.
Same format, same 1–5 cap.

## When NOT to run

Skip when:

- Neither `.cursor/decisions.md` nor `.cursor/context.md` exists
- The change is small and local (rename, typo, single-function tweak, config
  bump, clear bug fix that does not touch architecture)
- Work is clearly inside settled Choices with no new forks (aligned → silent)

Do not use this skill for upfront elicitation or Plan-mode scoping — that is
autoscope / design-checkpoint.
