# Teams

A **team** groups people who work together. Each team has its own skill template and its own dashboard.

## Rules

- Joining is by invitation only. An admin creates the team and puts people in it; nobody can join a team on their own.
- A person is in **one team** at most. Moving someone to another team takes them out of the first.
- Sessions stay private to the person who pushed them. A team dashboard shows the team's *numbers* (sessions, tool calls, tokens, alerts), not its members' transcripts.

## Create a team (admins)

1. Open **Team** in the top bar and use **Teams** to give the team a name and, optionally, a [skill template](skill-templates.md).
2. Put people in it with the **Team** column of the **People** table, or create an invite for the team: choose the team under **Invites**. Whoever signs up with that link joins the team, and the invite comes with the team's skill template unless you pick another one.

Deleting a team does not remove its members; they simply have no team.

## Team skills

The skill template of a team is the set of skills all its members use. Members find its install command on their team page. Changing the template of a team changes what new installs get; people who already installed need to run the command again.

## Team dashboard

Members see **My team** in the top bar. Admins open any team from the **Teams** list. The page shows the same charts as the [dashboard](dashboard.md), over all sessions pushed by the team's members, with the same time range and project filters. Below them:

- **Saved history**: one snapshot of the team's all-time totals per day, taken when a member pushes a session and when the page is opened. Snapshots are kept even if sessions are deleted later or a member leaves, so the history does not shrink.
- **Members** and the team's **skill template** with its install command.

Sessions pushed before accounts existed belong to the first admin and do not count for any team until that admin joins one.
