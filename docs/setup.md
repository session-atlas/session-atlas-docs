# Setting up the server

Session Atlas runs as two containers: the **API** (stores and analyzes sessions) and the **UI** (the web interface you open in a browser). Both are published on Docker Hub in one repository, [`bosheto/sessionatlas`](https://hub.docker.com/r/bosheto/sessionatlas), and told apart by tag.

| Container | Image |
| --- | --- |
| API | `bosheto/sessionatlas:api-latest` (or `api-0.1.0`) |
| UI | `bosheto/sessionatlas:ui-latest` (or `ui-0.1.0`) |

You need [Docker](https://docs.docker.com/get-docker/) with Compose. No other software is required.

## Start it

Create a folder with a file named `docker-compose.yml`:

```yaml
name: session-atlas

services:
  api:
    image: bosheto/sessionatlas:api-latest
    environment:
      MAX_UPLOAD_MB: ${MAX_UPLOAD_MB:-200}
      LOG_FORMAT: ${LOG_FORMAT:-json}
      PUBLIC_URL: ${PUBLIC_URL:-}
      ALLOW_SIGNUP: ${ALLOW_SIGNUP:-false}
      SETUP_TOKEN: ${SETUP_TOKEN:-}
    volumes:
      - api-data:/data
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "/server", "healthcheck"]
      interval: 15s
      timeout: 5s
      start_period: 5s
      retries: 3

  ui:
    image: bosheto/sessionatlas:ui-latest
    environment:
      API_UPSTREAM: http://api:8080
      MAX_UPLOAD_SIZE: ${MAX_UPLOAD_MB:-200}m
    ports:
      - "${PORT:-8080}:8080"
    depends_on:
      api:
        condition: service_healthy
    restart: unless-stopped

volumes:
  api-data:
```

Then run:

```bash
docker compose up -d
```

Open **http://localhost:8080**. The first page asks you to **create the first account**, which becomes the admin (see [Accounts](accounts.md)). Do this right away: until the first account exists, whoever opens the address first can create it. On a server others can reach, set `SETUP_TOKEN` before you start it, so the page asks for that secret.

The sessions list is empty until you [push a session](pushing-sessions.md).

Next, set up your team's skills. In the web interface, **Templates** builds a ready-made set of skills with one install command (see [Skill templates](skill-templates.md)), and **Team** invites people. The command that installs only the push skill is `curl -fsSL http://localhost:8080/api/skill/install.sh | bash`, with your server's address; see [Getting started](getting-started.md#1-install-the-push-trace-skill).

## Settings

Put these in a file named `.env` next to `docker-compose.yml`, or set them in your shell.

| Setting | Default | Meaning |
| --- | --- | --- |
| `PORT` | `8080` | The port the web interface listens on. |
| `MAX_UPLOAD_MB` | `200` | The largest upload accepted, in megabytes. |
| `LOG_FORMAT` | `json` | `json`, or `text` for easier reading. |
| `PUBLIC_URL` | *(automatic)* | The address people use to reach the server, such as `https://atlas.example.com`. Only needed if the skill install command shows the wrong address (see below). |
| `ALLOW_SIGNUP` | `false` | `true` lets anyone who can open the address create an account. Otherwise people need an invite link from an admin. Keep it `false` unless the server is on a private network. |
| `SETUP_TOKEN` | *(none)* | A secret that the first-account page asks for. Set it on a server that others can reach, so a stranger cannot create the first (admin) account. Not needed after setup. |

Only the web interface is exposed. The API is reachable only through it.

## Skill install address

The skill install script writes the server's address into itself, taken from the address you used to open it. If you reach the server through a proxy that does not pass the original host on (`X-Forwarded-Host`), the script may show the internal address. Set `PUBLIC_URL` to the public address to fix that.

## Choose a version

`latest` always follows the newest release. To stay on one version, use the numbered tags:

```yaml
image: bosheto/sessionatlas:api-0.1.0
image: bosheto/sessionatlas:ui-0.1.0
```

Use the same version number for both containers.

## Update

```bash
docker compose pull
docker compose up -d
```

Your data is kept in a volume, so it survives updates. After an update, stored sessions are re-analyzed in the background if the analysis has changed, so numbers can shift briefly.

## Stop, restart, logs

```bash
docker compose stop          # stop, keep data
docker compose start
docker compose logs -f api   # follow the API log
docker compose down          # remove containers, keep data
docker compose down -v       # remove containers AND all stored sessions
```

## Back up and restore

Everything lives in the `api-data` volume. Stop the API before copying so the database is consistent.

```bash
docker compose stop api
docker run --rm -v session-atlas_api-data:/data -v "$PWD":/backup alpine \
  tar czf /backup/session-atlas-backup.tgz -C /data .
docker compose start api
```

To restore, stop the API, then unpack the archive into the same volume:

```bash
docker compose stop api
docker run --rm -v session-atlas_api-data:/data -v "$PWD":/backup alpine \
  sh -c "rm -rf /data/* && tar xzf /backup/session-atlas-backup.tgz -C /data"
docker compose start api
```

The volume is named after the folder name in `name:`, so it is `session-atlas_api-data` with the file above.

## Running on a server others can reach

Everyone signs in with an account (see [Accounts](accounts.md)), but every signed-in person sees **all** sessions, and sessions can contain secrets. Passwords and access tokens travel with each request, so:

- Serve the web interface over **HTTPS**, using a reverse proxy such as Caddy or Traefik, unless it is on a private network or VPN.
- Set `SETUP_TOKEN` before the first start, and keep `ALLOW_SIGNUP` off.
- Only invite people who may see every session.

The proxy must pass the original host on (`X-Forwarded-Host`) and, for HTTPS, `X-Forwarded-Proto: https`, so login cookies are marked secure. It must also allow uploads as large as `MAX_UPLOAD_MB`.

## Upgrading from a version without sign-in

After the update, the first visit shows the account setup page. Create the first account and invite the others. Your stored sessions are kept and become visible to everyone with an account.

Anyone who pushed sessions before needs an [access token](accounts.md#access-tokens): until they add one, `/push-trace` answers that the server needs a token. The quickest way is to send them the install command of a [skill template](skill-templates.md) that includes `push-trace`, or tell them to create a token under **Account** and run the install command again with `--token`.

Accounts, tokens and templates live in the same database as the sessions, so the [backup](#back-up-and-restore) covers them.

## Other ways to run it

- **Kubernetes:** ask whoever manages your cluster. The images above work as they are.
- **One container at a time:** the API listens on port 8080 and keeps data in `/data`. The UI needs `API_UPSTREAM` set to the API's address.

See [Troubleshooting](troubleshooting-and-privacy.md) if something does not work.
