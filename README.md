# VibeProverSkills

A single `CLAUDE.md` file for Lean 4 formalization, definition design, and theorem proving.

`CLAUDE.md` is the source of truth. The plugin skill package mirrors it for public installation.

## What CLAUDE.md Covers

The current guidance in `CLAUDE.md` includes:
- MCP self-check at task start (available vs unavailable in-session).
- Clarify statement first (assumptions, typeclasses, interpretation).
- Reuse Mathlib first, with search tooling before new declarations.
- Do not hallucinate lemma names.
- Verify imports and references actually exist.
- No new axioms (except Lean's standard classical foundations).
- Do not suppress warnings or linter output.
- Prefer small proof steps and explicit intermediate goals.
- Check definitions carefully (types, instances, conflicts).
- Ensure every helper declaration is actually used.
- Keep strict `sorry` discipline with explicit resolution plans.
- Use goal-driven verification loops.
- Keep edits surgical.
- Report assumptions and remaining `sorry` count in outputs.

## Why this exists

General coding assistants can make proof work harder by:
- guessing definitions without checking existing library names,
- writing long brittle proofs instead of small compositional lemmas,
- skipping verification loops after each theorem change.

These guidelines push the assistant toward small, checkable proof steps.

## Install

Option A: Plugin marketplace (public distribution)

From Claude Code:

```text
/plugin marketplace add tdwag123/VibeProverSkills
/plugin install VibeProverSkills@lean4-guidelines
```

Option B: Per-project CLAUDE.md

Copy this repo's `CLAUDE.md` into your Lean project root (or merge it into your existing one).

## MCP Availability Behavior

Default behavior is self-check, not manual flags.

At the start of each Lean task, Claude should state whether Lean MCP is available in this session.
If available, it should use MCP lookups before guessing symbol names or theorem statements.

If MCP is unavailable, Claude should state that once and continue with best-effort Lean/Mathlib reasoning.

## Example prompt

```text
First, state whether Lean MCP is available in this session.

Formalize: "Every finite subgroup of a cyclic group is cyclic."

Requirements:
1) Reuse Mathlib lemmas where possible.
2) Add helper lemmas only if needed.
3) Keep proof readable and under 40 lines if possible.
4) After each attempt, report remaining goals and next step.
```

## License

MIT

## For Contributors

When you update guidance, keep these in sync:
- `CLAUDE.md` (source of truth)
- `.claude-plugin/skills/lean4-guidelines/SKILL.md` (plugin skill payload)
- `.claude-plugin/plugin.json` (plugin metadata)
