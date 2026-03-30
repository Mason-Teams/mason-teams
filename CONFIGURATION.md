<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Configuration

MASON is designed to work out of the box — the setup wizard handles most configuration for you. This guide covers what you can customize and where things live.

## Environment Variables

Set these before running `./scripts/masonctl start` to override defaults. For a complete reference, see [examples/.env.example](examples/.env.example).

| Variable | Default | Description |
|----------|---------|-------------|
| `ANTHROPIC_API_KEY` | *(none)* | Your Anthropic API key. Can also be entered through the setup wizard. |
| `MASON_PORT_WEB` | `8080` | Port for the MASON web UI (wizard and dashboard). |
| `MASON_PORT_MM` | `8065` | Port for Mattermost (team chat). |
| `MASON_PORT_FORGEJO` | `3000` | Port for Forgejo (Git forge). |

**Example — custom ports:**

```bash
MASON_PORT_WEB=9080 MASON_PORT_MM=9065 ./scripts/masonctl start
```

Then open `https://localhost:9080` for the setup wizard.

## Ports

MASON exposes these ports from the container:

| Port | Service | What it's for |
|------|---------|---------------|
| `8080` | Web UI | Setup wizard and dashboard (HTTPS, token auth) |
| `8065` | Mattermost | Team chat — where you talk to agents (HTTPS via `masonctl`, HTTP via plain `docker run`) |
| `3000` | Forgejo | Git repositories and code collaboration (HTTPS via `masonctl`, HTTP via plain `docker run`) |

All three can be customized with the `MASON_PORT_*` environment variables above.

### Advanced Ports

These are exposed for debugging and advanced use. Most users won't need them.

| Port | Service | What it's for |
|------|---------|---------------|
| `7681` | Web Terminal | Browser-based terminal access (ttyd) |
| `6333` | Qdrant | Vector database API for agent memory |
| `9090` | Daemon | Agent lifecycle API and Prometheus metrics |

## Data & Persistence

All persistent state lives in a single Docker volume mounted at `/data` inside the container. This means your data survives container restarts — but **not** `docker volume rm`.

| Path | What's stored |
|------|---------------|
| `/data/config/` | Setup wizard progress and user preferences |
| `/data/credentials/` | Auto-generated service tokens and admin passwords |
| `/data/mattermost/` | Mattermost configuration and uploaded files |
| `/data/forgejo/` | Forgejo configuration and Git repositories |
| `/data/postgres/` | All databases (Mattermost, Forgejo) |
| `/data/qdrant/` | Vector embeddings for agent memory |
| `/data/agents/` | Agent workspace directories |
| `/data/teams/` | Team workspace directories |

### Backing Up

To back up your MASON data:

```bash
# Stop the container first
./scripts/masonctl stop

# Copy the volume data (docker cp is needed — masonctl doesn't have a backup command)
docker cp mason:/data ./mason-backup
```

### Starting Fresh

To reset everything and start over:

```bash
./scripts/masonctl rm --data   # Removes container, data volume, trusted certs, and ~/.mason
./scripts/masonctl start       # Fresh start with the setup wizard
```

This is a full uninstall — it removes the container, the data volume (agent workspaces, repos, chat history), any MASON certificates from your system trust store, and the `~/.mason` directory (token and TLS files). Use `./scripts/masonctl rm` (without `--data`) to remove just the container while keeping everything else.

## Wizard Configuration

The setup wizard collects your preferences and stores them automatically. You don't need to edit any config files.

**What the wizard saves:**

| Setting | Where it's stored | Description |
|---------|-------------------|-------------|
| API key | `/data/credentials/credentials.json` | Your Anthropic API key |
| Auth method | `/data/config/setup-progress.json` | API key or subscription authentication |
| Concierge name | `/data/config/setup-progress.json` | Custom name for your concierge (default: "Connie") |
| Your display name | `/data/config/setup-progress.json` | How agents address you |
| Terms acceptance | `/data/config/setup-progress.json` | User Agreement acknowledgments and timestamps |

**Progress is saved automatically.** If you close your browser mid-setup, you'll resume where you left off when you come back. Going back to earlier steps preserves your previous inputs.

## API Documentation

MASON includes interactive API documentation via Swagger UI. Once the container is running, you can explore and test endpoints from your browser:

| Service | URL | Description |
|---------|-----|-------------|
| Mason Server | `https://localhost:8080/swagger/` | Dashboard and setup API |
| Agent Daemon | `http://localhost:9090/swagger/` | Agent lifecycle and messaging API |

> **Note:** The daemon API (port 9090) is internal to the container and not exposed to the host by default. Access it from inside the container or by adding `-p 9090:9090` to your `docker run` command.

## Claude Code Version

MASON ships with a certified version of Claude Code (currently **v2.1.77**) to ensure consistent behavior across installations. This version is installed automatically during the setup wizard.

If you decline the wizard install, the dashboard shows the certified version number with a manual install option. We recommend using the bundled version rather than installing the latest independently, as agent templates and commands are tested against the certified version.

## Agent Configuration

MASON pre-configures Claude Code with sensible defaults for agent workloads:

| Setting | Value | Description |
|---------|-------|-------------|
| `effortLevel` | `medium` | Thinking effort preset — balances quality and speed. Agents won't prompt you to choose on first start. |

## Advanced Configuration

### Resource Recommendations

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| RAM | 8 GB | 16 GB+ |
| Disk | 10 GB free | 20 GB+ |
| CPU | 2 cores | 4+ cores |

Agents share the container's resources with Mattermost, Forgejo, and other services. If things are slow, try increasing Docker Desktop's memory limit in settings.

### Server Flags

The MASON server binary accepts these flags (for advanced users running outside masonctl):

| Flag | Default | Description |
|------|---------|-------------|
| `--addr` | `:8080` | Web server listen address |
| `--data` | `/data` | Data directory for all persistent state |
| `--mattermost-url` | `http://127.0.0.1:8065` | Internal Mattermost API URL |
| `--config` | *(none)* | Path to a `mason.yaml` config file |

### Daemon Configuration

The agent daemon manages agent lifecycles, message routing, and health monitoring. Its config lives at `/data/daemon.yaml` and is auto-generated during setup.

**Timing settings** (can be tuned for responsiveness vs. resource usage):

| Setting | Default | Description |
|---------|---------|-------------|
| `poll_interval` | `1s` | How often the daemon checks agent status |
| `health_check_interval` | `30s` | How often services are health-checked |
| `stuck_threshold` | `5m` | Time before an unresponsive agent is flagged |
| `memshot_interval` | `10m` | How often agent memory snapshots are taken |

**Runtime changes:** The daemon supports live configuration updates — send `SIGHUP` to reload the config file without restarting.

## Useful Commands

| Command | What it does |
|---------|-------------|
| `./scripts/masonctl start` | Start MASON |
| `./scripts/masonctl stop` | Stop MASON (prompts for confirmation) |
| `./scripts/masonctl restart` | Restart everything (prompts for confirmation) |
| `./scripts/masonctl status` | Check what's running |
| `./scripts/masonctl token` | Print your dashboard auth token |
| `./scripts/masonctl login` | Print token and open dashboard in browser |
| `./scripts/masonctl logs` | View logs |
| `./scripts/masonctl logs -f` | Follow logs in real time |
| `./scripts/masonctl pull` | Pull the latest MASON image from the registry |
| `./scripts/masonctl update` | Pull latest image and restart (prompts for confirmation) |
| `./scripts/masonctl rm` | Remove the container, keeps data (prompts for confirmation) |
| `./scripts/masonctl rm --data` | Full uninstall — removes container, data volume, trusted certificates, and `~/.mason` directory (prompts for confirmation) |
| `./scripts/masonctl trust-cert` | Add MASON's TLS certificate to your system trust store (eliminates browser warnings) |
| `./scripts/masonctl untrust-cert` | Remove MASON's TLS certificate from your system trust store |
| `./scripts/masonctl verify` | Check file integrity inside the container |

> **Scripting tip:** Pass `--yes` or `-y` to skip confirmation prompts: `./scripts/masonctl stop --yes`

## Third-Party Dependencies

MASON requires [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code) by Anthropic to function. Claude Code is **not bundled** in the container image — it is installed during the setup wizard at the user's direction. Use of Claude Code is subject to [Anthropic's Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms).

For a full list of bundled third-party software and their licenses, see the [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES) file.

---

*For the full setup walkthrough, see the [Getting Started Guide](GETTING_STARTED.md).*
