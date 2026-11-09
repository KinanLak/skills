# The pull-request skill

`.agents/skills/pull-request/SKILL.md`, only when delivery goes through pull
requests. AGENTS.md keeps the delivery rules every session needs (never on
`main`, draft after the first commit, what must be green, who merges and
how); this skill holds the how-to, read when a pull request is opened,
written, updated or reviewed.

Fill it from the audit and the user's answers; drop a section that does not
apply (no review bot, no screenshots for a library).

```markdown
---
name: pull-request
description: >
  Use when opening, writing or updating a pull request of <project>, adding
  screenshots or a video to one, or handling its review and checks.
---

# Pull requests in <project>

## Writing it

- <Language>, title and body. Title: a conventional commit subject that
  states the effect, not the edit: `<a real example from this project>`.
- Body: the problem (what was wrong or missing, for whom), what changed and
  why this approach, what was left out on purpose, then verification: what
  was run, and what was checked where, with which account or device. A
  simple change is a few sentences; headings only for a long body. Never
  narrate the diff or list the files.
- Steps outside the code (a dashboard setting, a command after the release)
  get their own numbered list, in the order to run them.
- One concern per pull request. If the body needs an "also", split or stack.
- A decision that must outlive the pull request goes in `docs/`; the body
  links to it. `Closes #N` when an issue exists.

## Screenshots and video

- A visible change carries one before/after pair for the main change, and
  another screen only for a distinct effect; a phone capture when the mobile
  layout differs. Motion or timing needs a short video, not stills.
- Upload, never commit: draft the body with `![alt](./file.png)` alone in its
  paragraph, run `gh pr edit <n> --body-file <draft> --attach <file>`, read
  the URLs back with `gh pr view <n> --json body --jq .body`, swap them for
  thumbnails (`<a href="URL"><img src="URL" alt="…" width="320"></a>`) and
  publish with `gh pr edit <n> --body-file <final>`. 10 MB per file.
- <How to shrink a video, if the project has a script for it.>

## Review and checks

- <How the review starts (marking ready, a command), how it reports
  (author, check title, priorities), how to re-run it, and how to tell that
  the head itself was reviewed.>
- Check each finding against the source: fix the real ones, answer false
  positives in their thread with the reason.
- Wait for checks and reviews with T3 Code's `watch_pull_request`, then end
  the turn; never poll with loops or sleeps. A subagent without the tool
  hands its pull request back once ready.
- <What must be green before asking to merge or merging, and how stacks are
  merged.>
```
