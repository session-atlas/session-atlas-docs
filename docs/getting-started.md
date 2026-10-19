# Getting started

## What you need

- **A Session Atlas server.** Your team may already run one. Ask for its web address. To run your own, follow [Setting up the server](setup.md); it is served at `http://localhost:8080`.

## 1. Install the `/push-trace` skill

The server hands out the skill itself. Run this in a terminal, with your server's address:

```bash
curl -fsSL http://localhost:8080/api/skill/install.sh | bash
```

It installs the skill for you, in every project, and remembers the server address so you do not have to type it again. Restart Claude Code if it was already open.

To install it for one project only, or to skip remembering the address:

```bash
curl -fsSL http://localhost:8080/api/skill/install.sh | bash -s -- --project /path/to/repo
curl -fsSL http://localhost:8080/api/skill/install.sh | bash -s -- --no-save-url
```

You can read the script first by opening the same address in a browser. To update the skill later, run the command again.

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
