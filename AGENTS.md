# hermes-deploy

Runs the **Nous Research Hermes Agent** via docker-compose. Part of the EvilCouncil infrastructure.

## Setup

1. Copy `.env.example` → `.env`. Add `HF_TOKEN` if you need gated/private HF models (otherwise optional).
2. `docker compose up -d`

**Exposed port:** `5100` (inside container: `5000`).

## Gotchas

- **No `.env` exists by default.** The container starts with `HF_TOKEN=` if you skip this step.
- **Data volume:** `/srv/hermes` on the host maps to `/opt/data` inside the container. This is the official Hermes path — all config, sessions, skills, memory, cron, and logs live here. Create it (`mkdir -p /srv/hermes`) before first deploy. To reset, wipe the directory and re-run setup.
- **Image tag:** pins `nousresearch/hermes-agent:v2026.8.3`. Bump manually.
- **Healthcheck:** uses `curl -f http://localhost:5000/health`. The image must contain curl (it does).
- Not a git repo — the source of truth is the host filesystem.

## Structure

```
docker-compose.yml  # single service (hermes-agent), bridge network (hermes-net)
.env.example        # HF_TOKEN placeholder
/srv/hermes/       # persisted app data (docker volume bind mount at /opt/data)
```
