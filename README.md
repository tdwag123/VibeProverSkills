# VibeProverSkills

Lean 4 formalization, definition design, and theorem proving guidelines for AI coding agents.

Supports both **Claude** (`CLAUDE.md`) and **OpenAI Codex** (`AGENTS.md`). The two files share the same Lean 4 guidelines; only the agent-specific tool-check section differs.

`CLAUDE.md` is the source of truth for Claude. `AGENTS.md` mirrors it for Codex.

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

### Claude

Option A: Plugin marketplace (public distribution)

From Claude Code:

```text
/plugin marketplace add tdwag123/VibeProverSkills
/plugin install VibeProverSkills@lean4-guidelines
```

Option B: Per-project CLAUDE.md

Copy this repo's `CLAUDE.md` into your Lean project root (or merge it into your existing one).

### OpenAI Codex

Copy `AGENTS.md` into your Lean project root (or merge it into your existing one):

```sh
curl -O https://raw.githubusercontent.com/tdwag123/VibeProverSkills/main/AGENTS.md
```

Codex reads `AGENTS.md` automatically from the project root, the same way Claude reads `CLAUDE.md`.

## Tool Self-Check Behavior

Default behavior is self-check, not manual flags.

At the start of each Lean task, the agent should state what tools are available (Lean MCP, search, code execution, etc.) and use them before guessing symbol names or theorem statements.

If no tools are available, the agent should state that once and continue with best-effort Lean/Mathlib reasoning.

**Claude-specific:** Claude checks for Lean MCP availability and prefers MCP lookups.
**Codex-specific:** Codex checks for available search/execution tools and uses them equivalently.

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
- `CLAUDE.md` (source of truth for Claude)
- `AGENTS.md` (mirror for Codex — same Lean guidelines, adapted tool-check section)
- `.claude-plugin/skills/lean4-guidelines/SKILL.md` (plugin skill payload)
- `.claude-plugin/plugin.json` (plugin metadata)
