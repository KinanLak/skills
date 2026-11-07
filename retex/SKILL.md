---
name: retex
description: >
  Use when the user agrees to keep what a piece of work taught, or types
  /retex, optionally naming other sessions, threads, a project or a period:
  turns durable lessons into reviewed edits to skills, AGENTS.md or docs,
  never into hidden memory.
argument-hint: "[thread link | project | period | topic]"
---

# Retex

A lesson is worth keeping only where the next agent will read it: in the
repository's skills, its AGENTS.md or its docs, or in the global AGENTS.md.
Never in an agent's private memory, which nobody reviews and other agents
cannot see. This skill finds the lessons, proposes where each one goes, and
writes nothing until the user approves.

## 1. Read the sessions

With no argument, the current session. Otherwise, what the argument names:

- A T3 Code thread link or title: `t3_thread_search`, then `t3_thread_read`.
- A project or a period ("the netplan threads this week"): search T3 Code,
  then the agents' own transcripts (see [references/sessions.md](references/sessions.md)).
- A topic ("the Reemo tickets"): search the transcripts for it.

Read the user's messages first: corrections, repeated requests and "I already
told you" are the strongest signal. For more than a few sessions, have a
subagent extract the candidates, with session ids and quotes as evidence.

## 2. Keep only what a future agent would get wrong

Keep:

- a correction the user made more than once, or made angrily;
- a pitfall that cost a round (a wrong endpoint, a tool that does not exist, a
  setup step everyone rediscovers);
- a procedure that worked and will recur;
- a fact about the environment that no file states (which host, which
  account, which site behaves differently).

Drop what the code, the git history or an existing doc already says, a detail
of this one task, the story of the session, and any secret value, always.

## 3. Choose the destination

The closest place the next agent will read when it needs it:

| The lesson | Goes to |
|---|---|
| A step or pitfall of one recurring task | that task's skill, or its `references/` |
| A recurring task with no skill, seen three times or more | a new skill (propose it, name and description) |
| A rule for every task in one repository | that repository's AGENTS.md, one line |
| A lasting product or architecture choice | an ADR or `docs/` |
| How to operate or reach a service | the repository's operations doc |
| A preference that holds across projects | the global `~/.agents/AGENTS.md` |

Before adding, read the destination. If it already says something on the
subject, change or delete that line instead of appending. A file that forbids
agent files (its AGENTS.md says so) gets nothing.

## 4. Propose, then apply

Show one short list, each item as: the lesson in one line, its evidence
(session and date), its destination, and the exact diff. Ask for approval of
the whole list in one question; the user may pick items.

Apply only what was approved. A repository edit follows that repository's
delivery rules (branch and pull request when it requires them). End with the
links to what changed.
