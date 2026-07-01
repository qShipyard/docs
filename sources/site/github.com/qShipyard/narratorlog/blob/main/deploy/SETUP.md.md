# Source: https://github.com/qShipyard/narratorlog/blob/main/deploy/SETUP.md

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# SETUP.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/deploy/SETUP.md)

History

68 lines (48 loc) · 1.76 KB

 main

/

# SETUP.md

Copy path

Top

## File metadata and controls

- Preview
 
- Code
 
- Blame
 

68 lines (48 loc) · 1.76 KB

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/deploy/SETUP.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

# narratorlog — Self-Host Setup

> **Just want to try it locally?** Skip this entire guide and run one command from the repo root: `docker compose -f deploy/docker-compose.quickstart.yml up -d --build`, then open [http://localhost:3000](http://localhost:3000). To reset and try first-run setup again: `docker compose -f deploy/docker-compose.quickstart.yml down -v` then `up -d --build` again. The steps below are for a real production deployment behind nginx with TLS.

## Prerequisites

- Docker and Docker Compose
- A domain name pointed at your server

## 1\. Clone the repo

```shell
git clone https://github.com/qShipyard/narratorlog.git
cd narratorlog
```

## 2\. Configure environment

```shell
cp deploy/.env.example .env
```

Edit `.env` and fill in every value. Generate secrets with:

```shell
openssl rand -hex 32   # use twice — once for APP_SECRET, once for ENCRYPTION_KEY
```

## 3\. Connect a git provider

After logging in, go to **Settings → Sources** and paste a Personal Access Token for your git provider (GitHub, GitLab, or Bitbucket). No OAuth app registration required.

## 4\. Run migrations

This runs the bundled `migrate` service (golang-migrate) against the `postgres` service, applying every migration in `apps/api/internal/db/migrations`. It reads `DATABASE_URL` from your `.env`, so make sure that points at the `postgres` host (e.g. `postgresql://narratorlog:...@postgres:5432/narratorlog?sslmode=disable`).

```shell
docker compose -f deploy/docker-compose.yml run --rm migrate
```

## 5\. Start

```shell
docker compose -f deploy/docker-compose.yml up -d
```

## 6\. Verify

```shell
curl https://your-domain.com/health
# {"status":"ok","version":"0.1.0"}
```

## Updates

```shell
docker compose -f deploy/docker-compose.yml pull
docker compose -f deploy/docker-compose.yml up -d
```