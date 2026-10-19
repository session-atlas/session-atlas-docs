# Admin usage dashboard

The **Usage** page (top navigation) summarises how much every person and team uses Session Atlas. It is available in the self-hosted and SaaS editions; in the single-user edition there is nothing to summarise.

## Who can see it

- **Admins** see every team, and people who are in no team.
- **Team admins** see their own team only. An admin marks someone as team admin with the *Team admin* checkbox on the **Team** page. The person must already be in a team, and moving them to another team clears the flag.
- Everyone else does not see the page, and the server refuses the request.

## What it shows

Pick a range (7 days to all time). The page has:

- headline numbers: active users, sessions, prompts, tool calls, output tokens, alerts;
- sessions per day, with the number of active users;
- the top users by output tokens;
- a table per team. Expand a team to see each person: sessions, prompts, tool calls, failed calls, output and cache tokens, alerts, and when they were last active.

## What it does not show

Only aggregated numbers. It never shows prompts, session titles, working directories or transcripts, and admins still cannot open other people's sessions (see [Accounts](accounts.md#your-sessions-are-private)).

## Privacy

Per-person usage is information about people's work. Tell your team that admins and team admins can see these numbers before you hand out the team admin flag. See [Troubleshooting and privacy](troubleshooting-and-privacy.md).
