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

Open **http://localhost:8080**. The sessions list is empty until you [push a session](pushing-sessions.md).

Point the `/push-trace` skill at this address (see [Getting started](getting-started.md#1-install-the-push-trace-skill)).

## Settings

Put these in a file named `.env` next to `docker-compose.yml`, or set them in your shell.

| Setting | Default | Meaning |
| --- | --- | --- |
| `PORT` | `8080` | The port the web interface listens on. |
| `MAX_UPLOAD_MB` | `200` | The largest upload accepted, in megabytes. |
| `LOG_FORMAT` | `json` | `json`, or `text` for easier reading. |

Only the web interface is exposed. The API is reachable only through it.

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

Session Atlas has **no login**. Anyone who can open the address can see every session, and sessions can contain secrets. Before you share it:

- Run it on a private network or VPN, **or**
- Put it behind a proxy that asks people to sign in, for example Caddy, Traefik, or oauth2-proxy, and serve it over HTTPS.

If the proxy sits in front of the web interface, make sure it allows uploads as large as `MAX_UPLOAD_MB`.

## Other ways to run it

- **Kubernetes:** ask whoever manages your cluster. The images above work as they are.
- **One container at a time:** the API listens on port 8080 and keeps data in `/data`. The UI needs `API_UPSTREAM` set to the API's address.

See [Troubleshooting](troubleshooting-and-privacy.md) if something does not work.
