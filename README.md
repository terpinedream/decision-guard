# decision-guard

A Cursor plugin that keeps recorded decisions alive during build. Before
non-trivial edits, the agent checks `.cursor/decisions.md` and hard
constraints in `.cursor/context.md`, pauses only on conflicts or new
high-impact forks, and stays silent when aligned.

## What it does

- Loads settled architecture Choices and Goal / Scope / Out of scope /
  Constraints
- Infers what the intended change set implies (stack, data, auth, APIs, scope)
- Pauses with a compact chooser on contradictions or significant undecided forks
- Proceeds with no user-facing checkpoint when work fits the rails
- Records overrides and new choices so the agent does not re-violate them

## Components

| Component | Path | Purpose |
| --------- | ---- | ------- |
| Rule | `rules/decision-guard.mdc` | Always-on trigger before non-trivial edits |
| Skill | `skills/decision-guard/SKILL.md` | Drift detection procedure and chooser |
| Command | `commands/decision-guard.md` | `/decision-guard` for on-demand audit or override |

## Persistence

| File | Role for this plugin |
| ---- | -------------------- |
| `.cursor/decisions.md` | Primary: architecture Choices to enforce or update |
| `.cursor/context.md` | Secondary: Goal, Scope, Out of scope, Constraints only |

Acceptance criteria in `context.md` are intentionally ignored (leave that to
a separate acceptance-check plugin).

## Install locally

Install this folder as a Cursor plugin (Plugins → add local path, or place it
under your local plugins directory). Ensure a project has
`.cursor/decisions.md` and/or `.cursor/context.md` (e.g. from autoscope), then
try implementing something that would contradict a recorded Choice, or run
`/decision-guard`.

## Note on autoscope / better-plan

Autoscope (and design-checkpoint / better-plan) elicit context and decisions
**before** plan/build. Decision-guard enforces those rails **during** build.
Safe to run alongside autoscope. Decision-guard does not replace
design-checkpoint.

## License

MIT
