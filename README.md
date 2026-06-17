# Interview Dev Loop

Interview Dev Loop is an Agent Skill for the workflow we call an "interview implementation loop".

The agent investigates first, asks one multiple-choice clarification question at a time, writes a plan only after material ambiguity is resolved, waits for approval, then runs implementation and review until no P0/P1/P2 findings remain.

The internal Japanese name is インタビュー実装ループ.

This repository is a public, agent-agnostic version of the original `interview-plan-implement-review-loop` skill. Product-specific repo rules have been generalized, but the core workflow is the same.

## What it does

- Investigates code, docs, tests, existing plans, and local instructions before planning.
- Surfaces findings in a fixed shape before asking questions.
- Asks one A/B/C clarification question at a time.
- Writes a durable plan only after material ambiguity is resolved.
- Waits for user approval before implementation.
- Runs development and review as a loop.
- Continues until no unresolved P0/P1/P2 findings remain.

## When to use it

Use this for feature work, bug fixes, and product tasks where the request is real but underspecified.

Good fit:

- "Add this integration."
- "Implement this ticket."
- "Build this feature, but ask before making product-scope decisions."
- "Plan first, then implement after approval."

Poor fit:

- Tiny edits where the next action is obvious.
- Pure explanation or research tasks.
- PR-only review loops after a pull request already exists.
- Tasks where the human has already supplied a complete implementation plan.

## Install

Codex can use Agent Skills directly, but reusable distribution is best packaged as a plugin.

Add this repository as a plugin marketplace:

```bash
codex plugin marketplace add unsu0707/interview-dev-loop
```

Then install `interview-dev-loop` from the Codex plugin directory.

For other coding agents, ask the agent you use:

```text
Install the [unsu0707/interview-dev-loop](https://github.com/unsu0707/interview-dev-loop) skill.
```

## Usage

Explicitly invoke the skill when starting a coding task:

```text
Use interview-dev-loop for this task: add support for ...
```

Or mention it by name in tools that support skill mentions:

```text
$interview-dev-loop Add support for ...
```

The first useful response should be an investigation summary and, if needed, one clarification question. It should not jump directly into implementation.

## Repository layout

```text
.
├── .codex-plugin/plugin.json
├── .agents/plugins/marketplace.json
├── marketplace.json
├── skills/interview-dev-loop/SKILL.md
├── skills/interview-dev-loop/agents/openai.yaml
├── skills/interview-dev-loop/references/
└── examples/
```

## Compatibility

This repository follows the open Agent Skills format:

- one directory per skill
- `SKILL.md` with `name` and `description`
- lowercase hyphenated skill name
- optional `references/` for progressive disclosure

It is intended for Codex, OpenCode, Claude Code, and other Agent Skills-compatible coding agents.

## License

MIT
