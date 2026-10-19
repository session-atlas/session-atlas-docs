# Troubleshooting and privacy

## Privacy

A transcript holds everything in the session: your prompts, the model's replies, command output and file contents. That can include **passwords, tokens and other secrets**.

- Only push to a server you control or trust.
- You must sign in, but **every signed-in person sees every session**, whoever pushed it. Only invite people who may read everything on the server, and serve it over HTTPS (see [Accounts](accounts.md)).
- Install links of [skill templates](skill-templates.md) need no sign-in: anyone with the link can install the template and read its written-here skills. They never expose sessions.
- Deleting a session removes it from the server. It does not change files on your computer.
- Rules such as *Secret files touched* help you see when the model handled sensitive files.

## Troubleshooting

### `/push-trace` is not found
Run the install command again and restart Claude Code. If you installed with `--project`, you only have the skill inside that project.

### The install command fails or shows the wrong address
- `curl: (7)` or a timeout means the server cannot be reached from your machine. Open the address in a browser to check.
- If the saved address is wrong (for example an internal name), ask the person running the server to set `PUBLIC_URL`, then install again.
- If it says it cannot determine the server address, `PUBLIC_URL` is needed too.

### The upload fails or cannot connect
- Check the server address: `echo $AGENT_TRACE_URL` and `cat ~/.config/agent-trace/url`.
- Open the same address in a browser. If it does not load, the server is down or unreachable (VPN, firewall).
- Use the web interface address, not the backend's.

### The upload says the server needs an access token
The server needs to know who is pushing. Create a token under **Account** in the web interface, then either install again with `--token YOUR_TOKEN` or put the token in `~/.config/agent-trace/token`. If you did, the token may have been revoked: check the **Account** page.

### I cannot sign in
- *Wrong email or password*: check both; the email is not case sensitive.
- *Too many failed attempts*: wait 15 minutes, or ask an admin.
- You were signed out suddenly: your password was changed, or an admin removed the account. Sign in again.
- The invite link says it is not valid: invite links work once and expire. Ask an admin for a new one.

### A template's install command reports a skill that was not installed
The other skills were installed. For a git skill, check the repository address and that `git` works on your computer, and for a private repository that your credentials are set up. The folder you named must contain a `SKILL.md`. See [Skill templates](skill-templates.md).

### The upload is rejected as too large
The default limit is 200 MB per upload. Ask whoever runs the server to raise it, or push a smaller session.

### A session is missing from the list
- Search is by title, prompt and folder. Clear the search box.
- Empty sessions are not uploaded.
- With `push-trace all`, sessions outside `--since` or `--project` are skipped. Add `--dry-run` in a terminal to see the plan.

### `push-trace all` skipped sessions I changed
Unchanged sessions are skipped by design. Use `--force` to upload everything again.

### The session looks out of date
Push it again. The new upload replaces the old one.

### The dashboard is empty
Widen the time range, set the project to *All projects*, or import your history.

### A rule matches too much, or nothing
Open **Rules**, select it, and check the matching calls. Use **Preview matches** while editing, and add an exclude condition for the false positives. Also check the rule is enabled.

### I changed a rule but old alerts did not update
Saving a rule re-checks every stored session, which can take a moment on large collections. Refresh the page.

### No desktop notifications
Open the bell and choose **Turn on desktop notifications**, then allow them in your browser.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| <kbd>j</kbd> or <kbd>→</kbd> | Next call |
| <kbd>k</kbd> or <kbd>←</kbd> | Previous call |
