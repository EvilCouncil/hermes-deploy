# 🚨 Hermes Agent Deployment 🚨

> The **Nous Research Hermes Agent** — self-hosted, dockerized, and alive. 🐙

<details>
<summary>📎 Tech Stack</summary>

| 🧱 Layer | Stack |
|----------|-------|
| Container Runtime | Docker 🐳 |
| Orchestration | Docker Compose (one service) 🧩 |
| Image | `nousresearch/hermes-agent:v2026.8.3` 📦 |
| Data Store | `/srv/hermes` bind-mounted → `/opt/data` 🧠 |
| Network | `hermes-net` (bridge) 🌐 |
| Port | `5100` (host) → `5000` (container) 🔌 |

</details>

## 🚀 Quick Start

```bash
# 1️⃣  Prepare the data directory
sudo mkdir -p /srv/hermes

# 2️⃣  Copy & configure environment
cp .env.example .env
# (edit .env to add HF_TOKEN if you need gated / private models — optional)

# 3️⃣  Bring it up
docker compose up -d
```

✅ The health endpoint lives at `http://<host>:5100/health` — **curl** can verify it. 🩺

## 📁 What Lives in `/srv/hermes`

All of Hermes' runtime state maps to `/opt/data` inside the container:

| 📂 Path | Contents |
|---------|----------|
| `.env` | API keys & secrets 🔑 |
| `config.yaml` | All Hermes configuration ⚙️ |
| `SOUL.md` | Agent personality / identity 🎭 |
| `sessions/` | Conversation history 💬 |
| `memories/` | Persistent memory store 🧠 |
| `skills/` | Installed skills 🔧 |
| `home/` | Per-profile HOME for tool subprocesses 🏠 |
| `cron/` | Scheduled job definitions ⏰ |
| `hooks/` | Event hooks 🪝 |
| `logs/` | Runtime logs 📜 |
| `skins/` | Custom CLI skins 🎨 |

## 🔄 Updating

```bash
docker compose pull
docker compose up -d
```

Your data is preserved — `git pull` a new image tag won't touch `/opt/data`. 🛡️

## 🛑 Resetting

```bash
sudo rm -rf /srv/hermes/*
docker compose down
docker compose up -d
# Re-run the setup wizard inside the fresh container
```

The first run drops you into the **setup wizard** 💫 — it prompts for API keys and writes them to `/opt/data/.env`. Only ever needed once.

## ⚠️ Gotchas

| 🚧 Issue | 💡 Fix |
|----------|--------|
| No `.env` exists by default | Run `cp .env.example .env` first |
| `~/.hermes` vs. `~/.hermes` | Use `/srv/hermes` on the host, it maps to `/opt/data` inside |
| Image tag is pinned | Bump `nousresearch/hermes-agent:v2026.8.3` manually |
| Healthcheck uses curl | The image ships with curl built-in ✅ |

## 🏗️ Structure

```
hermes-deploy/
├── docker-compose.yml     # single service + bridge network
├── .env.example           # HF_TOKEN placeholder
├── .gitignore             # blocks .env + data
├── AGENTS.md              # deployment instructions
└── LICENSE
```

## 🔗 Related

| 🔗 Resource | Link |
|-------------|------|
| 📖 Official Docs | https://hermes-agent.nousresearch.com |
| 🐙 GitHub | https://github.com/NousResearch/hermes-agent |
| 🌀 Docker Hub | https://hub.docker.com/r/nousresearch/hermes-agent |

---

<div align="center">

Made with 🍺 by the **EvilCouncil** 🤝

</div>
