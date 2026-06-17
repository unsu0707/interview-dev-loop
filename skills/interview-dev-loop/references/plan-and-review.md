# Plan and review details

## Plan file

Use the repo's durable plan convention. If there is none, use:

```text
docs/plans/plan_YYYYMMDDHHMMSS.md
```

The plan should be concrete enough for a fresh implementation context to execute.

Include:

- goal
- current findings
- decisions
- open questions resolved
- scope
- steps
- validation
- risks

Keep the plan updated if scope changes after approval.

## Development pass

A development pass should receive only the approved plan and task-local context needed to execute it.

For fix passes, include the exact findings being fixed and require same-root-cause fixes across the approved scope.

## Review pass

Use a code-review stance. Findings should lead and should be ordered by severity.

Focus on:

- correctness bugs
- security or permission gaps
- behavioral regressions
- missing required tests
- stale durable docs or plans
- violations of local repo instructions

Avoid broad refactors and style preferences that do not affect behavior or maintainability.
