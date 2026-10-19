# Getting started

## What you need

- **A Session Atlas server.** Your team may already run one. Ask for its web address. To run your own, follow [Setting up the server](setup.md); it is served at `http://localhost:8080`.
- **An account.** Open the server's address in a browser. On a new server, the first page creates the first account (the admin). On a team server, ask an admin for an **invite link**. See [Accounts](accounts.md).

If an admin sent you an invite link with a skill template, sign up with it and follow the **Welcome** page: it gives you one command that does step 1 and the token for you. Then skip to [step 2](#2-push-a-session).

## 1. Install the `/push-trace` skill

The server hands out the skill itself. Run this in a terminal, with your server's address:

```bash
curl -fsSL http://localhost:8080/api/skill/install.sh | bash -s -- --token YOUR_TOKEN
```

Create `YOUR_TOKEN` first: in the web interface, open **Account**, choose **Create token**, and copy it. The token lets `/push-trace` upload sessions as you.

The command installs the skill for you, in every project, and remembers the server address and your token so you do not have to type them again. Restart Claude Code if it was already open.

To install it for one project only, or to skip remembering the address:

```bash
curl -fsSL http://localhost:8080/api/skill/install.sh | bash -s -- --token YOUR_TOKEN --project /path/to/repo
curl -fsSL http://localhost:8080/api/skill/install.sh | bash -s -- --token YOUR_TOKEN --no-save-url
```

You can read the script first by opening the same address in a browser (it needs no sign-in). To update the skill later, run the command again.

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
- Set up your whole team the same way: [Skill templates](skill-templates.md).

## The pages at a glance

The top bar has pages, a bell for notifications, a theme button, and your email, which opens your **Account**.

| Page | Use it to |
| --- | --- |
| **Dashboard** | See usage across your sessions. |
| **Sessions** | Find and open a session. |
| **Commands** | Search every shell command the model ran. |
| **Rules** | Decide which actions raise an alert. |
| **Templates** | Build a set of skills that others install with one command. |
| **Team** | *(Admins only.)* Invite people and manage roles. |
| **Account** | Your access tokens and password; sign out. |

The **Theme** button cycles between system, light and dark.
