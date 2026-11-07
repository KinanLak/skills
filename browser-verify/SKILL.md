---
name: browser-verify
description: >
  Use before calling a user-visible change done or marking a pull request
  ready, and whenever asked to test, check, screenshot or review a screen,
  page or flow in a browser. Hands the check to a fresh GPT-6.1 Terra
  subagent at high effort instead of doing it yourself.
compatibility: Needs T3 Code (delegate_task, preview tools) and a Codex provider.
---

# Browser verify

The author of a change is the worst judge of it: you test the path you built.
A verifier with no context but a brief tests what a user would see. So you
never drive the browser yourself to verify your own change. You prepare the
instance, write the brief, delegate, and act on the verdicts.

## 1. Gather the project's test data

Read the project's test skill (`test-<app>`, named in AGENTS.md) before
writing the brief. It holds what this skill does not: how to start the
instance, its URL, the accounts and where their passwords live, the demo data,
and the known noise and pitfalls. If the project has none, take what AGENTS.md
says; if the instance or an account is still missing, tell the user that
`/agent-ready` would add it, and ask once.

## 2. Make the instance ready

The verifier shares your checkout (`isolation: "shared"`) and your running
instance. Before delegating:

- The instance runs this checkout's latest code, with the demo data the checks
  need. Reseed if the seed changed. A verifier on a stale build reports
  everything as unverifiable.
- Nothing will reload under it: do not edit files while it runs.
- Any overlay that blocks clicks in development (React Scan, devtools
  badges) is off, or the brief says how to close it.

## 3. Delegate

Call `orchestrator_capabilities` once per thread, then `delegate_task`:

```json
{
  "title": "Verify: <change>",
  "role": "test",
  "isolation": "shared",
  "target": {
    "providerInstanceId": "codex",
    "model": "gpt-6.1-terra",
    "options": { "reasoningEffort": "high" }
  },
  "task": "<brief>"
}
```

- Always pass `reasoningEffort: "high"`; the model's default is low.
- If Codex refuses the task (quota, unavailable), run the same brief on
  `claudeAgent` / `claude-opus-5-5` with `effort: "high"` and say so in your
  message. Never fall back to checking it yourself.
- Several independent areas: one delegate each, in parallel, each told to open
  its own tab (`reuseExistingTab: false`).
- Then follow the tool's `next` and end the turn; do not poll.

## 4. The brief

The delegate knows nothing. Write the brief in full, every time, from
[references/brief.md](references/brief.md). Never put a secret in it: name the
file or variable that holds the password.

## 5. Act on the report

- Fix what is `FAIL` or `REGRESSION`, then verify again with a new
  `delegate_task` that lists only those checks. Never re-check what passed, and
  never use `t3_thread_send` for a new round.
- `UNVERIFIABLE` means the setup failed, not the change: fix the setup (data,
  account, instance) before the next round.
- Put the verdicts and the screenshots that show the change in the pull
  request's verification section. Remarks outside the checks go to the user as
  a short list; do not fix them unasked.

## Review mode

For a UI review rather than the check of one change, the brief lists screens
and asks for findings ranked P0 to P2 (P0 broken or blocking, P1 wrong or
confusing for a real user, P2 polish), each with the screen, the viewport, the
steps and a screenshot. Same delegate, same effort.
