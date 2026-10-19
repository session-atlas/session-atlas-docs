# Dashboard

The dashboard summarizes how you use Claude Code across **all** uploaded sessions.

## Choose what to look at

At the top, pick a time range (**7, 30, 90, 365 days** or **all time**) and a **project** or all projects. Everything below follows these two choices. Click a project in the table, or a program in the shell list, to filter by it.

If a range is empty, pick a longer one or [import your history](pushing-sessions.md#import-all-your-history).

## What you see

| Section | Tells you |
| --- | --- |
| **Totals** | Sessions, prompts, tool calls, failure rate, output and cache tokens, peak context, alerts. |
| **Activity** | Per day. Switch the measure: calls, sessions, tokens, failures or alerts. |
| **When you work with Claude** | A weekday by hour heatmap in your local time. |
| **Tool categories** | Shell, read, write, search, web, agent… and their share of calls. |
| **Tools** | Per tool: calls, failures, average and p95 duration, total time. *p95* means 95% of calls were faster than this. |
| **Projects** | Per folder: sessions, calls, failures, output tokens, alerts, last activity. |
| **Top shell programs** | The programs the model ran most. Click one to search it. |
| **Alerts by rule** | Which [rules](rules-and-alerts.md) matched how often. |
| **Peak context per session** | How large the context grew in each session. |
| **Models** | Sessions that used each model. A session can use several, for example for subagents. |
| **Biggest sessions** | Sort by tokens, calls, duration or context. Click to open. |

## Tips

- A rising **failure rate** or tool with many failures points to something to fix in your setup or instructions.
- Sessions with a very high **peak context** are slow and costly. Starting a fresh session sooner can help.
- Use **Alerts by rule** to see which risky action shows up most.
