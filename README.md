# Session Atlas documentation

Session Atlas turns a Claude Code session into an interactive map of what the model did: every prompt, every tool call, every shell command, and why. It also shows usage across all your sessions and flags risky actions, such as a force push or a read of your `.env` file.

These docs are for **people who use Session Atlas**. They cover using it, and running the server with Docker.

## Start here

| I want to… | Read |
| --- | --- |
| Run the server | [Setting up the server](docs/setup.md) |
| Get Session Atlas running and see my first session | [Getting started](docs/getting-started.md) |
| Send sessions to the server, one or all | [Pushing sessions](docs/pushing-sessions.md) |
| Understand a session and follow what the model did | [Exploring a session](docs/exploring-a-session.md) |
| Search every shell command the model ran | [Searching commands](docs/searching-commands.md) |
| See usage and trends across sessions | [Dashboard](docs/dashboard.md) |
| Get warned about risky actions | [Rules and alerts](docs/rules-and-alerts.md) |
| Fix a problem or check privacy | [Troubleshooting and privacy](docs/troubleshooting-and-privacy.md) |

## How it fits together

```
Claude Code ──/push-trace──▶ Session Atlas server ◀── your browser
```

1. You work with Claude Code as usual.
2. You run `/push-trace` to upload a session's transcript to the server.
3. You open the server's web address in a browser to explore it.

## Words used in these docs

| Term | Meaning |
| --- | --- |
| **Session** | One Claude Code conversation, from start to end. |
| **Prompt / turn** | One message from you and everything the model did in answer. |
| **Tool call** | One action the model took: running a command, reading or editing a file, searching, and so on. |
| **Subagent** | A helper the model started for part of the work. It has its own tool calls. |
| **Context** | How much text the model was given for a request. It grows as the session goes on. |
| **Rule** | A saved check that flags matching tool calls. |
| **Alert** | A tool call that a rule matched. |

## License

[MIT](LICENSE)
