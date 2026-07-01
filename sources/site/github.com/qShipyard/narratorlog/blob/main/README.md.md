# Source: https://github.com/qShipyard/narratorlog/blob/main/README.md

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# README.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/README.md)

History

192 lines (135 loc) · 7.13 KB

 main

/

# README.md

Copy path

Top

## File metadata and controls

- Preview
 
- Code
 
- Blame
 

192 lines (135 loc) · 7.13 KB

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/README.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

# narratorlog

**Your codebase has a story. narratorlog tells it.**

A self-hosted, open-source engineering communication tool that reads your git commits, understands them in context, and delivers the right story to every audience — automatically, with human approval before anything is published.

[Why we built this](https://github.com/qShipyard/narratorlog/blob/main/docs/01-problem-statement.md) · [Documentation](https://github.com/qShipyard/narratorlog/blob/main/docs/INDEX.md) · [Contributing](https://github.com/qShipyard/narratorlog/blob/main/docs/contributing/08-contributing-guide.md)

---

## What it does

Engineering teams ship code every day. But the story of what they built rarely reaches the people who need to hear it — and when it does, it arrives too late, in the wrong language, or not at all.

narratorlog reads what your team shipped, understands it in the context of your codebase, and writes the right version of the story for every audience:

- **Developers** get a technical summary of what changed and what to watch out for
- **Product Managers** get a plain-English breakdown of what moved and what is ready to demo
- **Marketing** gets benefit-focused copy they can actually use
- **Executives** get a high-level narrative of what the team is building and shipping

Every draft goes through a human approval step before anything gets sent anywhere.

---

## How it works

```
Your repos → narratorlog reads commits → understands context → generates drafts per audience
          → team reviews and approves → delivers to Slack, Notion, email, and more
```

---

## Running narratorlog

narratorlog is open source and self-hosted. Your code, your data, your infrastructure.

The stack is a Go API + background worker, a Next.js web app, a Rust codebase reader, PostgreSQL, and Redis.

### Quickstart — one command

Just want to try it? With Docker running, from the repo root:

```shell
docker compose -f deploy/docker-compose.quickstart.yml up -d --build
```

This builds everything, runs the database migrations automatically, and starts the whole stack with safe localhost defaults — no `.env` and no config required. The first run downloads base images and compiles the API, worker, web app, and reader — expect several minutes before the stack is ready. When it finishes, open **[http://localhost:3000](http://localhost:3000)** — on a fresh database you land on the setup wizard first (workspace, admin account, then activation).

> The quickstart uses insecure placeholder secrets and plain HTTP for local trials only. For a real deployment, use the production setup below.

**Useful commands**

```shell
# Follow logs
docker compose -f deploy/docker-compose.quickstart.yml logs -f

# Stop the stack (keeps your database)
docker compose -f deploy/docker-compose.quickstart.yml down

# Fresh start — wipes the database and runs setup from scratch
# (use this if migrate fails or you want a clean first-run experience)
docker compose -f deploy/docker-compose.quickstart.yml down -v
docker compose -f deploy/docker-compose.quickstart.yml up -d --build
```

### Run it locally (development)

**Prerequisites:** Go 1.25+, Node 20+, pnpm 9+, Docker, and the [`migrate`](https://github.com/golang-migrate/migrate) CLI (`go install -tags 'postgres' github.com/golang-migrate/migrate/v4/cmd/migrate@latest`).

```shell
# 1. Install JS dependencies
pnpm install

# 2. Start Postgres + Redis
docker compose -f deploy/docker-compose.dev.yml up -d

# 3. Apply database migrations
export DATABASE_URL="postgresql://narratorlog:narratorlog@localhost:5433/narratorlog?sslmode=disable"
migrate -path apps/api/internal/db/migrations -database "$DATABASE_URL" up

# 4. Set the remaining required env (any 32+ char strings for the secrets)
export REDIS_URL="redis://localhost:6379"
export APP_URL="http://localhost:3000"
export APP_SECRET="$(openssl rand -hex 32)"
export ENCRYPTION_KEY="$(openssl rand -hex 32)"   # API and worker MUST share this value

# 5. Start the API, worker, and web app (separate terminals)
cd apps/api && go run ./cmd/api      # API on :8080
cd apps/api && go run ./cmd/worker   # background worker (processes scans + delivery)
cd apps/web && pnpm dev              # web app on :3000
```

Then open [http://localhost:3000](http://localhost:3000) and complete the first-run setup wizard.

> The Rust reader (`packages/reader`) is only needed for `deep` AI depth. Standard scans run without it.

### Self-hosting (Docker)

For a production deployment behind nginx with TLS, see **[deploy/SETUP.md](https://github.com/qShipyard/narratorlog/blob/main/deploy/SETUP.md)**. In short: copy `deploy/.env.example` to `.env`, fill in every value (generate `APP_SECRET` and `ENCRYPTION_KEY` with `openssl rand -hex 32` — the API and worker must share the same `ENCRYPTION_KEY`), apply migrations, then:

```shell
docker compose -f deploy/docker-compose.yml up -d
curl https://your-domain.com/health   # {"status":"ok","version":"0.1.0"}
```

### First-run setup

The first time you open the web app on a fresh database, you are sent to `/setup` to create your workspace and admin account, then `/activate` to connect git, AI, and your first story. After that, sign in at `/login` on future visits.

You can also configure AI, delivery, and privacy later under **Settings**.

---

## Bring your own AI

narratorlog works with any LLM:

- Anthropic Claude
- OpenAI GPT-4o
- Ollama (local — zero data leaves your machine)
- Groq
- Any custom HTTP endpoint

---

## Status

> Working end to end. The Go pipeline, background worker, web app, Docker setup, and team settings are implemented. **Sources:** GitHub, GitLab, and Bitbucket — connect each with a Personal Access Token in Settings (no OAuth app). **AI providers:** Anthropic, OpenAI, and Ollama (Groq coming soon). **Delivery:** Slack, Notion, Discord, Linear, and email.

We are building in public. Follow along, contribute, and help shape what this becomes.

---

## Contributing

narratorlog is built by and for the community. There is a meaningful contribution at every skill level — from writing output plugins (any TypeScript developer) to improving the core pipeline (Go) to extending the codebase reader (Rust).

Read the [Contributing Guide](https://github.com/qShipyard/narratorlog/blob/main/docs/contributing/08-contributing-guide.md) to get started.

---

## Documentation

| Doc | Description |
| --- | --- |
| [Problem Statement](https://github.com/qShipyard/narratorlog/blob/main/docs/01-problem-statement.md) | Why narratorlog exists |
| [Product Requirements](https://github.com/qShipyard/narratorlog/blob/main/docs/02-product-requirements.md) | What it does and for who |
| [Architecture](https://github.com/qShipyard/narratorlog/blob/main/docs/03-architecture.md) | How it is built |
| [Database Schema](https://github.com/qShipyard/narratorlog/blob/main/docs/04-database-schema.md) | Data model |
| [Pipeline Spec](https://github.com/qShipyard/narratorlog/blob/main/docs/05-pipeline-spec.md) | The 8-stage pipeline |
| [API Design](https://github.com/qShipyard/narratorlog/blob/main/docs/06-api-design.md) | REST API reference |
| [Plugin Interface](https://github.com/qShipyard/narratorlog/blob/main/docs/07-plugin-interface-spec.md) | How to build plugins |
| [Contributing](https://github.com/qShipyard/narratorlog/blob/main/docs/contributing/08-contributing-guide.md) | How to contribute |

---

## License

narratorlog is open source under the Apache 2.0 license with an attribution clause. Any output generated by narratorlog must retain the narratorlog attribution.

---

Built with intention by the narratorlog community.