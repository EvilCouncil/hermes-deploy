# hermes-deploy

Runs the **Nous Research Hermes Agent** via docker-compose. Part of the EvilCouncil infrastructure.

## Setup

1. Copy `.env.example` → `.env`.
2. `mkdir -p /srv/hermes`
3. `docker compose up -d`

**Web dashboard:** `http://<host>:5101` — Basic Auth (username `hermes`, set a password).

**API server:** `http://<host>:5100/v1/...` — requires `Authorization: Bearer <key>` header.

## External access

### Dashboard

Bound to `0.0.0.0:9119` with Basic Auth (`HERMES_DASHBOARD_BASIC_AUTH_USERNAME` / `_PASSWORD`). Access at port **5101**. The dashboards auth gate is mandatory on non-loopback binds (June 2026 hardening) — no escape hatch for unauthenticated public access. Set `HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` in `.env` before first deploy.

### API server

Requires `API_SERVER_ENABLED=true`, `API_SERVER_HOST=0.0.0.0`, and a non-empty `API_SERVER_KEY` (min 8 chars, generate with `openssl rand -hex 32`). Every call to `http://<host>:5100/v1/...` must include `Authorization: Bearer <key>`.

## Setup checklist

Before first deploy, your `.env` must have:

| Var | Required | Note |
|-----|----------|------|
| `API_SERVER_KEY` | ✅ | Generated via `openssl rand -hex 32` |
| `HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` | ✅ | At least 8 chars — auth gate is mandatory on non-loopback dashboard |
| `HF_TOKEN` | ❌ | Only for gated/private HF models |
| `HERMES_DASHBOARD_BASIC_AUTH_USERNAME` | ❌ | Defaults to `hermes` |

## Gotchas

- **Container setup wizard.** On first `docker compose up -d`, the container drops you into an interactive setup. Run `docker exec -it hermes-agent /bin/bash` then `$ hermes setup` to configure your first provider. `hermes setup --portal` does it via one OAuth token.
- **No `.env` = empty keys.** The container starts with `HF_TOKEN=` (optional) and empty `API_SERVER_KEY` — the API server refuses requests without a key.
- **Dashboard always prompts for credentials** on first access (Basic Auth login).
- **Healthcheck:** uses `curl -f http://localhost:8642/health`.
- Not a git repo — the source of truth is the host filesystem.

## Structure

```
docker-compose.yml  # single service (hermes-agent), bridge network (hermes-net)
.env.example        # HF_TOKEN placeholder
/srv/hermes/       # persisted app data (docker volume bind mount at /opt/data)
```
