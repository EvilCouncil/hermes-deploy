# hermes-deploy

Runs the **Nous Research Hermes Agent** via docker-compose. Part of the EvilCouncil infrastructure.

## Setup

1. Copy `.env.example` → `.env`. Add `HF_TOKEN` if you need gated/private HF models (otherwise optional).
2. `docker compose up -d`

**Exposed port:** `5100` — maps to the Hermes API server on `8642` (gateway + OpenAI-compatible endpoint).

## External access

Requires `API_SERVER_ENABLED=true`, `API_SERVER_HOST=0.0.0.0`, and a non-empty `API_SERVER_KEY` (min 8 chars, generate with `openssl rand -hex 32`). The key is required even on a trusted home network — every API call to `http://<host>:5100/v1/...` must include the `Authorization: Bearer <key>` header.

## Gotchas

- **No `.env` exists by default.** The container starts with `HF_TOKEN=` and `API_SERVER_KEY=` if you skip this step — the API server will bind but refuse requests without a key.
- **`HF_TOKEN` is optional** (for gated/private HF models only).
- **Generated `.env`** after copy: see `.env.example` for all vars.
- **Healthcheck:** uses `curl -f http://localhost:8642/health`.
- Not a git repo — the source of truth is the host filesystem.

## Structure

```
docker-compose.yml  # single service (hermes-agent), bridge network (hermes-net)
.env.example        # HF_TOKEN placeholder
/srv/hermes/       # persisted app data (docker volume bind mount at /opt/data)
```
