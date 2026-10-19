# Getting started

## What you need

- **A Session Atlas server.** Your team may already run one. Ask for its web address. To run your own, follow [Setting up the server](setup.md); it is served at `http://localhost:8080`.

## 1. Install the `/push-trace` skill

From the `agent-trace-skill` folder:

```bash
./install.sh --url http://localhost:8080
```

Replace the address with your server's. It is saved, so you do not have to type it again.

| Option | Effect |
| --- | --- |
| *(none)* | Installs for you, in every project. |
| `--project /path/to/repo` | Installs for one project only. |
| `--url <address>` | Saves the server address. |

Restart Claude Code if it was already open.

## 2. Push a session

In Claude Code, run:

```
/push-trace
```

Claude uploads the current session and replies with a link. You can also say "push this session to Session Atlas".

## 3. Open it

Click the link, or open your server's address and choose the session from the **Sessions** list.

## What next

- Bring in your old sessions too: [Pushing sessions](pushing-sessions.md#import-all-your-history).
- Learn to read the map: [Exploring a session](exploring-a-session.md).
- Get warned about risky actions: [Rules and alerts](rules-and-alerts.md).

## The pages at a glance

The top bar has four pages, a bell for notifications, and a theme button.

| Page | Use it to |
| --- | --- |
| **Dashboard** | See usage across all sessions. |
| **Sessions** | Find and open a session. |
| **Commands** | Search every shell command the model ran. |
| **Rules** | Decide which actions raise an alert. |

The **Theme** button cycles between system, light and dark.
