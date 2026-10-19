# Searching commands

Session Atlas understands the shell commands the model ran. It splits each one, including pipes, `&&` chains and `sudo` or `npx` wrappers, into simple commands with a **program**, a **subcommand** and **flags**. Data such as heredoc text is not treated as a command.

## The Commands page

**Commands** lists every command across all sessions.

- **Programs** (left) are sorted by *Most used*, *A–Z*, *Most failures*, *Most sessions* or *Recently used*. Hover for counts, click for the subcommands and flags used most, and click one to search it.
- **Results** (right) are newest first. Each shows its status and the full command. Click one to open the call in its session, with your search already applied.

The search is kept in the address (`?q=`), so you can bookmark or share it.

## Query syntax

Separate parts with spaces. **Every part must match the same simple command.**

| You type | It finds |
| --- | --- |
| `aws s3` | Program `aws` whose subcommand or arguments contain `s3`. |
| `git push --force` | `git push` with the `--force` flag. |
| `rm -rf` | `rm` with both `-r` and `-f`. `-rf`, `-fr` and `-r -f` all match. |
| `--region` | Any program that used `--region`. |
| `"main.go"` | Commands containing exactly that text. |
| `is:failed` | Only calls that failed. Combine it: `npm is:failed`. |

Rules of thumb:

- The **first plain word** is the program. Case does not matter.
- It also matches through wrappers: `aws` finds `sudo aws …`.
- **Later plain words** match the subcommand or any argument, as part of a word.
- `-x` and `--name` are flags. Combined short flags are understood.
- Put text in **double quotes** to match it exactly as written.

A few programs use single-dash flags as whole words (`find -name`, `go test -run`, `terraform`, `ffmpeg`). Session Atlas knows this and does not split them into letters.

## Search inside one session

The box above the flow map uses the same syntax but only affects that session. See [Exploring a session](exploring-a-session.md#filtering).
