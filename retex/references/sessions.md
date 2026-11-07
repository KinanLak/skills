# Where past sessions live

Open every store read-only. They hold secrets users pasted: never quote one.

- **T3 Code threads**: `t3_thread_search` (by text, project, date), then
  `t3_thread_read`. The most complete source when the user works through
  T3 Code, across providers.
- **Claude Code**: `~/.claude/projects/<cwd with / replaced by ->/*.jsonl`, one
  file per session; worktrees get their own folder (`-home-u--t3-worktrees-<repo>-…`).
  User turns are lines with `"type":"user"` whose content is text, not a
  `tool_result`. Old sessions are purged after `cleanupPeriodDays`.
- **Codex**: `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`; the first line
  carries the `cwd`. Skip the approval "guardian" sub-sessions.
- **OpenCode**: `~/.local/share/opencode/opencode.db` (SQLite, open with
  `file:<path>?mode=ro`). Sessions in `session_v2` (`directory`, `title`),
  messages in `session_message`, parts in `session_part`; older installs
  use `session`, `message`, `part`.

Filter by the project's directory, and by its worktrees' directories, before
reading messages.
