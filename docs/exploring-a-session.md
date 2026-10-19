# Exploring a session

Open **Sessions**, then click a session. Use the search box on the list to find one by title, prompt text or folder. The list shows when it ran, how long it took, the number of turns and tool calls, failures, and alerts.

A session page has three layers. Start at the top and drill down.

## 1. Overview

The summary shows duration, turns, tool calls, failed calls, subagents and tokens. Two panels, each with two tabs, go deeper:

| Tab | Shows |
| --- | --- |
| **Tools used** | Which tools were used and how often, with failures and time spent. Click a tool to filter the map to it. |
| **When tools were used** | When each call happened, one lane per subagent. Switch between **Call order** and **Wall-clock time**. |
| **Context & tokens** | Peak context and output tokens for each prompt. |
| **Commands** | Every shell command, grouped by program (`git`, `aws`, `rm`…). Sort by most used, A–Z, most failures or first used. Expand a program to see its subcommands and flags, and click any of them to filter. |

## 2. The flow map

Prompts run **left to right**. Each prompt card shows:

- the context size, from start to end of the prompt, with a bar against the session's peak
- output tokens and the number of model requests
- the most-used tools
- failures and alerts

Choose **Show calls** on a card to open its tool calls in a column underneath. Calls the model made together sit side by side. Failed calls are red, and each call shows the reason the model gave. When the model starts a subagent, its work opens as a branch to the right.

**Expand all** and **Collapse all** open or close every prompt at once.

A flag badge (⚑) on a call means a [rule](rules-and-alerts.md) matched it.

## 3. Call details

Click a prompt or a call to open the detail panel. For a call it shows:

| Section | Meaning |
| --- | --- |
| **Why** | The tool's description, the model's stated reason, and its thinking when recorded. |
| **Commands** | For shell calls, each command split into program, subcommand and flags. Click a part to filter by it. |
| **Input** | Exactly what was sent to the tool. |
| **Change** | For file edits, the diff. |
| **Output** | What came back. Very long output is shortened, and **Download JSONL** gives the full text. |
| **Alerts** | Rules this call matched. |

For a prompt it shows your message, the calls made, the final answer, and a **Context & tokens** summary.

Step through every call in order with **Prev** and **Next**, or press <kbd>j</kbd> / <kbd>k</kbd> (or <kbd>→</kbd> / <kbd>←</kbd>). Subagent calls are included.

### About context

Context is the amount of text, in tokens, that the model received for a request: your messages plus tool output plus cached material. It grows as tool output piles up and drops when the conversation is compacted. A very high context is a sign that a session is getting heavy.

## Filtering

Everything on the page can be narrowed. While a filter is on, only prompts with matches open, and calls that do not match fold into a grey summary card such as "⋯ 23 other calls · Bash 15 · Read 5". Click it to show or hide them.

| Filter | How |
| --- | --- |
| **Command** | Type in the box above the map, for example `git push --force`. See [Searching commands](searching-commands.md#query-syntax). |
| **Category** | Choose a chip such as shell, read, write, search, web, agent. |
| **Tool** | Click a tool in the overview. Click it again, or the chip, to clear. |
| **✕ Failed only** | Keeps failed calls. |
| **Alerts** | Keeps calls that matched a rule. |

Non-matching calls are dimmed on the map and the timeline, and **Prev/Next** steps through matches only.

## Share an exact step

The address in your browser keeps the selected call and the command filter. Copy it to send a colleague straight to the same step.
