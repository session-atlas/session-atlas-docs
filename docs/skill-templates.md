# Skill templates

A **skill template** is a named list of Claude Code skills that one command sets up on a person's computer. Use it to onboard new people: instead of "install these five things", you send one command or one invite link.

Open **Templates** in the top bar to see and create them.

## Create a template

Choose **New template**, name it (for example *Backend onboarding*), and add skills. Each skill comes from one of three places:

| Source | Use it for | You provide |
| --- | --- | --- |
| **Built in** | Skills the server ships. Today that is `push-trace`, so people can push sessions to this server. | Pick it from the list. |
| **From a git repository** | A skill that lives in a repository, such as your team's skills repo. | The `https://` address, the folder inside the repository (empty if `SKILL.md` is at the root), and optionally a branch, tag or commit. |
| **Written here** | A short skill that only exists in this template, such as house rules. | A folder name, one line saying when Claude should use it, and the instructions. |

Every skill has a **folder name**: lowercase letters, digits and hyphens. It is installed as `~/.claude/skills/<name>`, and a name can appear only once in a template.

**Pin git skills** to a tag or commit (`v1.2.0`) so everyone gets the same version. Without one, each person gets the repository's newest default branch when they run the command.

Git skills are downloaded on the person's computer, so they need `git` there, and for a private repository they need access already set up (a credential helper). Private repositories fail with a clear message instead of asking for a password.

## Share a template

Open a template and choose **Install command**:

```bash
curl -fsSL https://atlas.example.com/api/templates/<id>/install.sh | bash
```

Send that line to your teammates. Running it:

1. downloads the built-in and written-here skills from the server, and fetches the git skills,
2. installs each into `~/.claude/skills/`, replacing an older copy,
3. if the template includes `push-trace`, remembers the server address so `/push-trace` knows where to upload.

Restart Claude Code afterwards. Running the command again **updates** the skills to what the template says now.

Options go after `bash -s --`:

| Option | Effect |
| --- | --- |
| `--project /path/to/repo` | Install into that repository's `.claude/skills` instead of for the whole user. |
| `--token TOKEN` | Save an [access token](accounts.md#access-tokens) so `/push-trace` can upload. |
| `--no-save-url` | Do not remember the server address. |

If one skill cannot be installed (for example a repository that is not reachable), the others are still installed, the command lists what failed, and it exits with an error.

### The link is the key

The install command does **not** need an account, so it works on a brand-new laptop. In exchange, **anyone who has the link can install the template and read the instructions of its written-here skills.** The part of the link after `/templates/` is random and cannot be guessed, but treat it like a link to a shared document: send it to people you trust, not to a public channel. The link never gives access to sessions.

To stop a link from working, delete the template. (Create a new one for the same skills to get a new link.)

## Onboard a new person

1. In **Team**, create an invite link and pick the template.
2. Send the link. The person creates their account.
3. They land on a **Welcome** page with the install command for the template. **Include my access token** adds a personal token to the command, so `/push-trace` works right after installing. That command carries their token, so they should run it themselves and not share it.

## Who can change a template

Everyone who is signed in can see all templates and use their install commands, and can create their own. Only the template's owner or an admin can edit or delete it. Deleting a person deletes their templates.

## Keep in mind: skills run on people's computers

A skill tells Claude what to do and may include scripts, so installing one means trusting whoever wrote it. Only add skills you wrote or reviewed, and pin git skills to a reviewed commit. Anyone signed in can create a template, so treat a template from someone you do not know like any other script you are asked to run.
