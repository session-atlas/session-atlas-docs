# Pushing sessions

## Push the current session

```
/push-trace
```

The reply contains a link to the session. If any [rule](rules-and-alerts.md) matched, it also lists the alerts, for example `Alerts: Git force push [critical] ×1`.

Running the command again later uploads the session again and **replaces** the stored copy, so push a long session as often as you like.

## Use a different server for one push

```
/push-trace http://other-server:8080
```

## Which server is used

The first of these that is set wins:

1. The address you typed after `/push-trace`.
2. The `AGENT_TRACE_URL` environment variable.
3. The address saved when you installed the skill (in `~/.config/agent-trace/url`).
4. `http://localhost:8080`.

Always use the address of the **web interface**, the one you open in a browser.

## Which account is used

Uploads are made with your [access token](accounts.md#access-tokens). The first of these that is set wins:

1. The `AGENT_TRACE_TOKEN` environment variable.
2. The token saved when you installed the skill with `--token` (in `~/.config/agent-trace/token`).

Without a token the server refuses the upload and `/push-trace` tells you to create one under **Account**. Never paste a token into a chat with Claude.

## Import all your history

```
/push-trace all
```

Claude first shows a plan (how many sessions and how many MB), then uploads them and links to the dashboard.

| Add | Effect |
| --- | --- |
| `--since 30` | Only sessions changed in the last 30 days. |
| `--project ~/code/x` | Only one project. |
| `--force` | Upload again even if a session has not changed. |

Good to know:

- Sessions already uploaded and unchanged are **skipped**. Session Atlas remembers them in `~/.config/agent-trace/uploaded.tsv`.
- Empty sessions are skipped.
- You get **one** notification summarizing the alerts found, not one per session.

## Push without Claude Code

The upload script works from a terminal too:

```bash
push.sh                          # newest session for the current folder
push.sh <session-id>             # a session by its ID
push.sh path/to/session.jsonl    # a transcript file
push.sh --all --since 30         # import history
push.sh --all --dry-run          # show what would be uploaded
```

Add a server address as the last argument to override the default.

## Delete a session

Open the session and choose **Delete** at the top right. This removes it from the server only, not from your computer. You can also choose **Download JSONL** to get the original transcript back.
