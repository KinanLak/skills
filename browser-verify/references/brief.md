# Verification brief

Fill every section. Leave none implied: the delegate has no history.

```markdown
You verify a change in a running web app, as its user would. You do not
change anything.

## The change
<What changed, for whom, and why, in two or three sentences. The branch and
its head commit.>

## The app
- URL: <the instance's URL, from the banner or `--status`>
- It is already running; never start, stop, restart or reseed it.
- Account: <email>, role <role>. The password is in <file / variable>: read
  it inside the command that uses it, never print it.
- Other accounts for other roles, if a check needs them: <…>
- Data: <what the demo data contains that the checks rely on>
- Known noise, not to report: <console warnings, the expected "not
  configured" error…>

## Checks
For each: where to go, the steps, what must be true. State the viewport
(390 and 1440 px wide unless said otherwise) and the theme (light and dark
when colours changed).
1. <…>
2. <…>
Also check the reverse path (undo, close, back) and an empty state when the
change touches one.

## How
- T3 Code preview tools: `preview_open` (with `reuseExistingTab: false`),
  `preview_snapshot`, `preview_act` (click, type, press, drag, scroll…),
  `preview_resize`, `preview_set_appearance`, `preview_evaluate`,
  `preview_wait_for`. <Or the project's Playwright helper, if the test skill
  names one.>
- Screenshots: `preview_snapshot` with `save: true`; keep their paths.
- Do not edit, commit or install anything. If a check is blocked by the
  setup, try the fixtures and accounts above before giving up.
- Close the tabs you opened before you finish.

## Report
One line per check: its number, a verdict, what you saw, and a screenshot
path.
- PASS: works as described.
- PARTIAL: works on some viewports, themes or paths; say which fail.
- FAIL: does not work; say what happens instead.
- REGRESSION: something that worked before is now broken.
- UNVERIFIABLE: the setup prevented the check; say what you tried.
Then, separately, up to five remarks outside the checks, ranked by how much
they would bother a user.
```
