---
name: agent-ready
description: Audit a repository and scaffold what coding agents need to work in it alone (AGENTS.md, a private instance per worktree, a test skill, CI, delivery rules).
disable-model-invocation: true
argument-hint: "[path]"
---

# Agent-ready

Make a repository one where an agent, started in a fresh worktree with no
history, can build, run, check in a browser and deliver a change, without
asking the user how and without hurting anything. Scaffold only what this
project needs: a library needs no instance, a script needs no ADRs.

## 1. Audit

Read before asking. Note what exists and what is missing:

- The stack, package manager, entry points, and the commands that build, test,
  lint and run it. Run them once to learn what works.
- Existing agent files: AGENTS.md, CLAUDE.md, `.agents/skills`,
  `.claude/skills`, `.cursor`, `t3.json`.
- Docs, ADRs, CI, PR template, secret scanning.
- What the app needs to run (database, backend, auth, external APIs) and
  where its configuration and secrets come from.
- The past: the git log, and the agents' sessions in this repository if
  there are any (see the `retex` skill's `references/sessions.md`). Look for
  corrections the user repeated, setup steps every session rediscovered, and
  anything done to production by mistake.

## 2. Ask, once

One batch of questions, each with your proposed answer from the audit:

1. **Delivery**: how a change reaches users. Pull request merged by the
   agent when green, pull request merged only by the user, direct push to
   `main`, or a release tag? Who deploys, and to which environments?
2. **Review**: none, a review bot, or a review subagent?
3. **Languages**: the interface's, and the repository's (code, docs,
   commits).
4. **Hard limits**: production systems, shared services, other people's
   resources on the host, actions that cost money.
5. **The instance**: what an agent may run locally, and what demo data a
   user would recognise.

## 3. Scaffold

Write each file from its reference, filled with this project's facts. Never
leave a placeholder or a section that says nothing.

- `AGENTS.md`, with `CLAUDE.md` containing only `@AGENTS.md`:
  [references/agents-md.md](references/agents-md.md).
- `bun run dev:agent` (or the stack's equivalent), a private, seeded instance
  per worktree: [references/dev-agent.md](references/dev-agent.md). Skip it
  when nothing needs to run.
- `t3.json`, so T3 Code prepares each new worktree and offers a Dev action:
  [references/t3-json.md](references/t3-json.md).
- `.agents/skills/test-<app>/SKILL.md`, the project's data for the global
  `browser-verify` skill, and the relative symlink
  `.claude/skills -> ../.agents/skills`:
  [references/test-skill.md](references/test-skill.md). Only for a project
  with a UI.
- `.agents/skills/pull-request/SKILL.md` when delivery goes through pull
  requests: how to write one, attach captures, and handle its review and
  checks: [references/pull-request-skill.md](references/pull-request-skill.md).
- CI: the same gate as the local `verify` command, a secret scan over the
  whole history, and a dependency audit. A PR template with Problem, Change,
  Scope, Verification.
- `docs/adr/` with `0001` recording this setup, and `docs/operations/access.md`:
  which service is reached with which CLI, API or MCP and which credentials
  (variable names, never values), so agents do those steps themselves.

## 4. Deliver

Deliver the scaffold the way the user just chose, in one change. Then give the
user the commands to try it, and say what you left out on purpose.

## Rules for what you write

- A rule enters AGENTS.md only if an agent would get it wrong without it.
  Prefer a script, a lint or a test that enforces it over a sentence.
- No rule about how the agent should behave in general (tone, planning,
  orchestration). The global AGENTS.md covers what is personal.
- Facts the code states are not repeated. Link to the doc that owns a topic
  instead of copying it.
