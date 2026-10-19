# Troubleshooting and privacy

## Privacy

A transcript holds everything in the session: your prompts, the model's replies, command output and file contents. That can include **passwords, tokens and other secrets**.

- Only push to a server you control or trust.
- Session Atlas has **no login**. Anyone who can open the address can see every session. Run it on a private network or behind a sign-in proxy.
- Deleting a session removes it from the server. It does not change files on your computer.
- Rules such as *Secret files touched* help you see when the model handled sensitive files.

## Troubleshooting

### `/push-trace` is not found
Run `./install.sh` again and restart Claude Code. If you installed with `--project`, you only have the skill inside that project.

### The upload fails or cannot connect
- Check the server address: `echo $AGENT_TRACE_URL` and `cat ~/.config/agent-trace/url`.
- Open the same address in a browser. If it does not load, the server is down or unreachable (VPN, firewall).
- Use the web interface address, not the backend's.

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
