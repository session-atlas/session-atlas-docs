# Rules and alerts

A **rule** is a saved check. When a tool call matches it, that call becomes an **alert**: it is flagged on the map, counted on the dashboard and, if you want, sent as a notification.

## Built-in rules

A new server starts with six rules. You can edit, disable or delete them. Deleted ones do not come back.

| Rule | Severity | Flags |
| --- | --- | --- |
| Recursive delete | warning | `rm -r` outside temporary folders |
| Git force push | critical | `git push --force`, `-f`, `--force-with-lease` |
| Secret files touched | critical | Reading or writing `.env` files, keys, credentials (not `.env.example`) |
| Cloud or cluster resources deleted | critical | Delete or destroy commands for AWS, GCP, Azure, Kubernetes, Helm, Terraform, CDK |
| Denied by user | info | You rejected a tool call |
| Long-running call | info | One call took over 5 minutes |

Severity is **info**, **warning** or **critical**. It sets the colour of badges and how alerts are sorted.

## Where alerts show up

- A ⚑ badge on the call in the flow map, and an **Alerts** column in the session list.
- The **Alerts** filter in a session, to see only flagged calls.
- The **bell** in the top bar.
- **Alerts by rule** on the dashboard.

## Notifications

The bell shows unread alerts from new uploads and refreshes every 15 seconds. New ones also pop up as a message. Open the bell to:

- click an alert and jump to the exact call
- **Mark all read** or **Clear**
- **Turn on desktop notifications** (your browser asks permission)

Only matches found in **new uploads** notify. Saving a rule re-checks old sessions silently.

## Create or edit a rule

Open **Rules** and choose **+ New rule** (or **Edit** on an existing one). Use **+ Add include condition** and **+ Add exclude condition** to add conditions.

1. **Name** and **Severity**. Add a description saying why it matters.
2. **Enabled** and **Send a notification for new matches**.
3. **Include conditions**: what to catch. Choose whether **any** or **all** of them must match.
4. **Except calls that match any of these**: known-safe cases to skip.

Click **Preview matches** to test an unsaved rule against every stored session before you save it.

Select a rule in the list to see the calls it matched. Each rule has **Enabled** and **Notify** checkboxes, so you can disable a rule without losing it.

### Conditions

Each condition has three parts.

**Tool.** A tool name, a comma-separated list (`Write,Edit`), or a pattern (`mcp__*`). Leave empty for any tool.

**What to test:**

| Field | Tests |
| --- | --- |
| Tool name only | Just that the tool was used. |
| Shell command | The commands run, using the [query syntax](searching-commands.md#query-syntax) or text matching on each simple command. |
| Input parameter | One input, named by you, such as `file_path`, `url`, `pattern`. Nested values use dots: `todos.0.content`. |
| Status | `ok`, `error`, `denied`, `interrupted`, `pending`. |
| Output | The text the tool returned. |
| Category | `shell`, `read`, `write`, `search`, `web`, `agent`, `plan`, `skill`, `mcp`, `other`. |
| Duration | Milliseconds. |
| Scope | `main` or `subagent`. |
| Summary | The one-line description shown in the interface. |

**How to compare:** equals, contains, starts with, ends with, regex, glob, command query, exists (not empty), greater than, less than.

### Examples

**Production database access**
- Include: tool `Bash`, shell command, query `psql prod`.

**Any web fetch to an unknown site**
- Include: tool `WebFetch`, input `url`, regex `^https?://`.
- Except: tool `WebFetch`, input `url`, contains `github.com`.

**Edits to CI config**
- Include: tool `Write,Edit,MultiEdit`, input `file_path`, glob `*/.github/workflows/*`.

**A single call over a minute, not counting subagents**
- Include: duration greater than `60000`.
- Except: tool `Agent,Task`.

## Tips

- Start broad, **preview**, then add exclusions until the matches are the ones you care about.
- Keep **Notify** on only for rules that deserve interruption. Leave low-signal rules as info without notifications.
