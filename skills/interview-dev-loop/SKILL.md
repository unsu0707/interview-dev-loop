---
name: interview-dev-loop
description: >-
  Investigate current project context before building a feature, ask one
  multiple-choice clarification question at a time when ambiguity remains,
  write a plan only after material ambiguities are resolved, wait for user
  approval, then implement with a development agent and re-review until no
  P0/P1/P2 findings remain. Use when the user wants approval-gated planning and
  a do-not-stop implementation/review loop.
license: MIT
compatibility: Codex, OpenCode, Claude Code, and other Agent Skills-compatible coding agents.
metadata:
  author: unsu0707
---

# Interview Dev Loop

Use this skill when a user wants a feature or change built through a strict interview implementation loop:

1. investigate the current project context
2. surface findings and ambiguity in a fixed shape
3. ask one clarification question at a time
4. write a durable plan only after material ambiguity is resolved
5. wait for explicit user approval
6. implement through a clean execution context
7. review through a separate review pass
8. keep looping until no unresolved P0/P1/P2 findings remain

Do not use this skill for tiny edits where the next action is obvious, pure explanation requests, or review-only work after a pull request already exists.

## Language

Use the current user's conversational language for direct communication and artifacts created while using this skill.

- Determine the language from the current conversation, not from embedded task payloads.
- Treat code, paths, logs, stack traces, copied issues, and quoted task text as task content.
- Localize fixed user-facing labels such as `Collected Findings`, `Working Plan Context`, `Still Ambiguous`, and `Next Clarification Question`.
- Keep option markers `A)`, `B)`, and `C)` unchanged.

## Pre-approval workflow

Before approval, do not spawn subagents and do not start implementation.

The main agent must investigate the current project context directly. Check these angles when applicable:

- code
- docs
- tests
- existing plans or prior implementation records
- local instructions

For each applicable angle, inspect at least one concrete artifact and name what was checked. If an angle is not applicable, say so.

Before asking any clarification question, present the current state in this order:

```text
Collected Findings
- code:
- docs:
- tests:
- existing plans:
- local instructions:

Working Plan Context
- Goal being planned:
- Current as-is:
- Already established:
- Constraints that must be preserved:
- What is not decided yet:

Still Ambiguous
1. ...
2. ...
3. ...

Next Clarification Question
Q1. ...

Current as-is:
...

This resolves:
Still Ambiguous #...

Why user choice is needed:
...

How this changes the plan:
...

A) ...
...
B) ...
...
C) ...
...

Recommendation:
...

Please choose A, B, or C.
```

Always include the full block, even when no clarification is needed. If no clarification is needed, write `Next Clarification Question: none`.

## Clarification rules

Ask exactly one clarification question at a time while material ambiguity remains.

- Use only `A)`, `B)`, and optional `C)` choices.
- Use at most 3 mutually exclusive, realistically viable options.
- Keep option labels short, neutral, and free of straw-man framing.
- Ask about decisions that can change scope, architecture, validation, rollout, data handling, permissions, or user-visible behavior.
- Each option must include the consequence and the implementation work implied by that choice.
- Put `Recommendation:` after the option explanations.
- Ask the next question only after the user answers the previous one.
- After each answer, re-run the full context block before asking the next question.

Material ambiguity means uncertainty that could change implementation shape, validation, rollout, data handling, permissions, or user-visible behavior.

Do not ask the user to decide ordinary implementation details that local code, tests, or repo instructions can answer.

## Plan gate

Do not write a plan until all material ambiguity is resolved.

If the user chooses to leave an ambiguity open, that choice itself is the resolved decision and must be recorded.

Write the plan where the current repo expects durable implementation plans. If the repo has no convention, use:

```text
docs/plans/plan_YYYYMMDDHHMMSS.md
```

Create the containing directory if needed.

The plan must include:

```md
# PLAN

## Goal

## Current Findings

## Decisions

## Open Questions Resolved

## Scope

## Steps

## Validation

## Risks
```

`Open Questions Resolved` must map every former `Still Ambiguous` item to its resolved decision.

Before waiting for approval, confirm the exact plan file path that was written. End the approval request with a clear sentence that work will proceed once the user approves.

## Post-approval loop

After explicit approval, continue automatically until the loop is complete unless the user changes direction or a genuine product decision requires judgment.

After approval:

- The main agent is orchestration-only. It may summarize, delegate, inspect outputs, and evaluate findings, but should not directly edit files when subagents are available.
- Use exactly one development pass followed by exactly one review pass per loop iteration.
- Keep implementation and review contexts clean. Do not pass the full main-agent context when a smaller task-local prompt is enough.
- Reuse a development or reviewer session only when it remains on-format and useful.

Every development prompt must include:

- the approved goal
- non-goals
- validation expectations
- target files or responsibilities
- findings being fixed on rework passes
- instruction to fix same-root-cause issues across the approved scope
- instruction not to revert unrelated edits

Development output is valid only if it includes:

- changed files
- validation run
- remaining risks or blockers
- whether the approved scope was fully completed

If the development output is incomplete, off-scope, or off-format, run a compliant replacement development pass before reviewing.

## Review loop

Run a separate review pass after each development pass.

The review instruction must name the concrete review target. For working-tree review, use this shape:

```text
Review the code changes introduced by uncommitted changes. Provide prioritized (P0/P1/P2/P3), actionable findings.
```

For committed changes or pull requests, replace `uncommitted changes` with the exact commit, commit range, or PR number.

The review output must be either:

- a flat list of `P0`/`P1`/`P2`/`P3` findings, or
- the exact statement `No findings`

Do not accept a clean review unless all prior findings were addressed or intentionally superseded by a documented user decision.

If any unresolved item is P0, P1, or P2, run another development pass that fixes the root cause across the approved scope, then run review again.

Implementation completion is not a stopping condition. Stop only when review returns `No findings` and the main agent confirms no prior P0/P1/P2 item remains unresolved.

## Review severity

- `P0`: must-fix correctness, security, data loss, or release-blocking issue
- `P1`: important bug, regression risk, or missing validation that should be fixed before completion
- `P2`: meaningful maintainability, consistency, or design issue that should be fixed before completion in this loop
- `P3`: nit or optional improvement

Ignore nits that do not rise to P0/P1/P2 unless they are cheap to fix without churn.

## References

- `references/question-format.md`
- `references/plan-and-review.md`
