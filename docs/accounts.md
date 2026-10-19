# Accounts and sign-in

Session Atlas asks everyone to sign in. Each person has their own account, and scripts such as `/push-trace` use a personal **access token** instead of a password.

## The first account

The first time you open a new server it shows **Set up Session Atlas**. Create the first account there. It becomes the **admin**, who invites everyone else.

Until that first account exists, nobody can use the server. If the server can be reached by people you do not trust, ask whoever runs it to set `SETUP_TOKEN` (see [Setting up the server](setup.md#settings)); the setup page then asks for it, so a stranger cannot claim the server first.

## Invite people

Admins open **Team** and choose **Create invite link**.

- The link works **once** and expires after the number of days you pick (1, 7 or 30).
- The link is shown **only once**. Copy it then. A lost link cannot be shown again, so revoke it and create a new one.
- Optionally attach a [skill template](skill-templates.md). The new person then lands on a welcome page with the command that sets up their skills.

The person opens the link, enters an email and a password (at least 10 characters), and is signed in.

Servers can also allow anyone to create an account without an invite, with `ALLOW_SIGNUP=true`. Leave it off unless the server is on a private network.

## Roles

| Role | Can |
| --- | --- |
| **Admin** | Everything a member can, plus: invite people, change roles, remove people. |
| **Member** | Use every page, create skill templates, and manage their own tokens. |

There is always at least one admin: the last admin cannot be removed or demoted. Removing a person also deletes their access tokens and their skill templates (so those templates' install links stop working).

## Sessions are shared

Signing in keeps **outsiders** out. It does not separate people from each other: every signed-in person sees **all** sessions, commands and alerts on the server, whoever pushed them. Treat a server as one shared workspace, and only invite people who may see everything in it. See [Privacy](troubleshooting-and-privacy.md#privacy).

## Access tokens

`/push-trace` runs on your computer and cannot use your browser login, so it uses an access token.

1. Open **Account** (your email at the top right) and choose **Create token**.
2. Copy the token. It is shown **once**.
3. Give it to the skill in one of these ways. The first that is set wins:
   - the `AGENT_TRACE_TOKEN` environment variable
   - the file `~/.config/agent-trace/token`

Installing a skill template with `--token` writes the file for you, and the welcome page of an invite can include a token in the install command.

Tokens do not expire. Revoke one on the **Account** page when a laptop is lost or a token was shared by mistake; it stops working immediately. Create a separate token per computer, so you can revoke one without touching the others.

A token acts as you: it can do everything your account can. Keep it as private as a password, and do not paste it into a chat with Claude.

## Change your password

On **Account**, use **Change password**. Every other browser you were signed in on is signed out. Your access tokens keep working.

## Sessions and security

- Browsers stay signed in for 30 days, or until you sign out.
- After 10 wrong passwords for an account (or from one address), sign-in is blocked for 15 minutes.
- Passwords are stored as salted hashes. Access tokens and invite links are stored only as hashes, which is why they can be shown once.
- Serve the server over **HTTPS** if it is reachable from anywhere other than a private network, because passwords and tokens travel in every request.

## Forgot the admin password

There is no email reset yet. Another admin can remove your account and invite you again. If you were the only admin, the person who runs the server has to reset the accounts by running `DELETE FROM users;` on the database file (`traces.db` in the `api-data` volume) while the API is stopped. The next visit shows the setup page again. Sessions are not touched, but access tokens, invites and skill templates are deleted with the accounts.
