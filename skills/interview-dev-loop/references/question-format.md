# Clarification question format

Ask one question at a time in this format:

```text
Q1. Which behavior should we support first?

Current as-is:
The current code supports the existing behavior only.

This resolves:
Still Ambiguous #1.

Why user choice is needed:
This is a product direction choice because each option preserves a different user-visible behavior and changes rollout risk.

How this changes the plan:
The answer determines which workflow branches, tests, and rollout handling belong in scope.

A) Existing flow only
This keeps the current behavior as the first supported path. Implementation would focus on preserving existing routes, tests, and data handling while deferring new behavior.

B) New flow only
This makes the new behavior the only supported path. Implementation would replace the existing workflow branches and update validation around the new behavior, with higher migration or compatibility risk.

C) Both
This supports both behaviors during the transition. Implementation would add branching logic, tests for both paths, and rollout handling so existing users can continue while the new behavior is introduced.

Recommendation:
C) Both, because it preserves existing users while allowing the new behavior to ship. Do not choose it if the goal is intentionally to remove the existing flow in this change.

Please choose A, B, or C.
```

The question should unblock architecture, scope, validation, rollout, data handling, permissions, or user-visible behavior. Do not ask questions whose answer can be discovered from the repository.
