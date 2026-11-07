# t3.json

T3 Code reads `t3.json` at the repository root. A script with
`runOnWorktreeCreate` runs when T3 Code creates a worktree; the others appear
as actions in the thread.

```json
{
  "$schema": "https://t3.codes/schema/t3.json",
  "scripts": [
    {
      "name": "Setup Worktree",
      "command": "bun scripts/setup-worktree.ts",
      "icon": "configure",
      "runOnWorktreeCreate": true
    },
    {
      "name": "Dev",
      "command": "bun run dev:agent",
      "icon": "play"
    }
  ]
}
```

The setup script runs without a terminal: install dependencies, prefetch
what the first `dev:agent` would otherwise wait for (a container image, a
toolchain), and start nothing. A worktree that never runs the app must cost
nothing. Keep it a script in the repository rather than a long inline
command, so it is versioned and testable.
