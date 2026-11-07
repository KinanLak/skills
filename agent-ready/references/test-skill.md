# The project's test skill

`.agents/skills/test-<app>/SKILL.md` holds this project's data for the global
`browser-verify` skill, which owns the method (delegation, brief, verdicts).
Never repeat the method here, and never tell the reader to drive the browser
itself.

```markdown
---
name: test-<app>
description: >
  Use when checking a <App> change in a browser or writing a verification
  or UI-review brief: how to run this worktree's instance, its URL, the test
  accounts and data, and the known pitfalls.
---

# Testing <App> in a browser

Verification itself goes through the `browser-verify` skill; this is the
data its brief needs.

## Instance
How to start it, how to read its URL, how to reseed. Link AGENTS.md for the
rest.

## Accounts
One line per role: email, what it can see, where the password lives.

## Data
What the seed contains that a check can rely on, and how to reach each
screen (routes, preview pages that render without signing in).

## Known noise
What a verifier will see and must not report.

## Pitfalls
Each: the symptom, the cause, the remedy. Only ones that cost a real round.
```

Add a pitfall the first time a verifier loses a round to it.
