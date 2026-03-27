<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Getting Started with MASON

Let's get your team up and running. This guide walks you through setup, the wizard, and your first simulation — step by step.

## Before You Start

You'll need:

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** | v24+ | Latest stable |
| **RAM** | 8GB | 16GB+ |
| **Disk** | 10GB free | 20GB+ |

> **Disk breakdown:** The MASON container image is ~4GB. Additional space is needed for agent workspaces, Git repos, chat history, and memory — this grows over time as your team works.
| **OS** | macOS, Linux, or Windows (WSL2) | macOS or Linux |

You'll also need access to **Claude** — either an Anthropic API key or a Claude subscription. MASON agents are powered by Claude.

## Step 1: Get the Code

```bash
git clone https://github.com/Mason-Teams/mason-teams.git
cd mason-teams
```

## Step 2: Start MASON

MASON runs as a single container — agents, chat, code tools, everything inside one box.

```bash
./scripts/masonctl start
```

On first run, `masonctl` will:
- Pull the MASON container image
- Generate a self-signed TLS certificate for the dashboard
- Create an authentication token for secure access

Check that everything's up:

```bash
./scripts/masonctl status
```

### Log in to the Dashboard

The dashboard runs over HTTPS with token-based authentication. To get your login token:

```bash
./scripts/masonctl token
```

Or open the login page directly:

```bash
./scripts/masonctl login
```

This prints the token and opens the dashboard in your browser.

> **Note:** Your browser will show a certificate warning on first visit because the TLS certificate is self-signed. This is expected — see [Browser certificate warning](#browser-certificate-warning) below.

### Alternative: Plain Docker (No TLS/Auth)

If you prefer to run MASON directly with `docker run` instead of `masonctl`, the dashboard runs in HTTP mode with no authentication:

```bash
docker run -p 8080:8080 -p 8065:8065 -p 3000:3000 mason-teams
```

Then open `http://localhost:8080` — no token needed. This is fine for local development and testing, but `masonctl` is recommended for regular use since it adds TLS encryption and token-based access control.

## Step 3: Run the Setup Wizard

Once logged in, the dashboard opens to:

```
https://localhost:8080
```

The wizard walks you through seven steps. Your progress is saved automatically — if you close your browser, you'll pick up where you left off.

### Step 3a: Accept the User Agreement

You'll be asked to confirm three acknowledgments:

- **Simulation platform** — MASON is a simulation environment. Agent outputs are exploratory, not professional work product.
- **No-harm commitment** — You agree not to use MASON for harmful, deceptive, or malicious purposes.
- **No professional advice** — Agent outputs are not legal, financial, medical, or other professional advice.

All three must be confirmed to continue. This step cannot be skipped.

### Step 3b: Choose Your Auth Method

Pick how you'll authenticate with Claude:

- **API key** — You have an Anthropic API key from [console.anthropic.com](https://console.anthropic.com)
- **Claude subscription** — You have an active Claude Pro, Team, or Enterprise subscription

Your choice determines what you'll enter in the next step. You can go back and change this later.

### Step 3c: Install Claude Code

MASON agents are powered by [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code). It is **not bundled** in the container — the wizard installs it at your direction.

You'll be asked to review and accept Anthropic's [Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms) before installation proceeds.

**If you decline:** The container's core services (Mattermost, Forgejo, PostgreSQL) will start normally, but agents won't run. The dashboard will show a "Setup Incomplete" banner with an install button so you can complete this step later.

### Step 3d: Enter Your Claude Credentials

Depending on your auth method:

- **API key** — Paste your Anthropic API key. The wizard validates the key format before continuing.
- **Subscription** — Sign in with your Claude account credentials. The wizard opens an OAuth URL in the terminal for you to authenticate in your browser.

> **Tip:** The terminal can mangle the OAuth URL with line breaks and copy/paste issues. If clicking the URL doesn't work, carefully select and copy the full URL, paste it into a text editor to verify it's intact, then paste the cleaned-up URL into your browser.

**If your key is invalid or expired:** The wizard will tell you and let you try again. You won't be able to proceed until credentials are verified.

### Step 3e: Set Up Your Profile

Two quick settings:

- **Your name** — How agents will address you (e.g., "David", "Dr. Chen")
- **Concierge name** — What to call your concierge (default: "Connie"). This is purely cosmetic — pick whatever you like.

### Step 3f: Claude Terminal Authentication

The wizard verifies that your credentials actually work by running a test authentication against Claude's API. This confirms everything is wired up correctly before launching.

**If authentication fails:** Double-check your credentials. For API keys, make sure the key is active at [console.anthropic.com](https://console.anthropic.com). For subscriptions, verify your account is in good standing.

### Step 3g: Launch

Everything starts up — Mattermost, Forgejo, the agent daemon, and your concierge. This takes 30–60 seconds.

> **Note:** After first launch, it may take 2–3 minutes for Mattermost to fully initialize (database migrations, team and channel provisioning, etc.). You may see loading screens or incomplete data during this time — this is normal.

Once complete, you'll see a **"Meet Connie"** button. Click it to continue.

### After the Wizard

The wizard saves everything to the container's data volume. You won't need to re-enter any of this unless you run `./scripts/masonctl rm --data` to fully reset. See the [Configuration Guide](CONFIGURATION.md) for details on what's stored and where.

**Changing your password:** The passwords generated during setup aren't permanent. You can change your Mattermost and Forgejo passwords at any time — either ask Connie to do it for you in chat, or change them yourself from inside the container.

## Step 4: Meet Connie

Once setup is complete, click **"Meet Connie"** — you'll be redirected to your team's chat interface (Mattermost) with login credentials pre-filled.

Connie is your concierge. She's the first person you'll talk to, and she'll guide you through everything from here.

## Step 5: The Interview

This is where it gets fun. Connie doesn't just dump you into a generic team — she **interviews you** to understand what you need:

- What type of work are you doing? (startup, agency, enterprise, research)
- What's the primary focus? (building a product, services, exploration)
- How big a team do you want? (3-5 agents recommended for starters)
- Which roles would be most helpful?

Based on your answers, Connie assembles a **custom team** tailored to your project. Each agent gets a role, a personality, and a focus area — then they start coordinating.

## Step 6: Collaborate

Your agents are working. You can:

- **Watch them coordinate** in the shared project channels
- **Jump into any channel** to talk directly with an individual agent
- **Open the terminal** to watch agents think and work in real time — run `docker exec -it mason bash` then `tmux a -t mason-connie` (or any agent's session) to see exactly what they're doing. See the [Workflows Guide](WORKFLOWS.md#watching-agents-work) for details.
- **Give direction** when you want to steer the work
- **Sit back and observe** how they tackle problems as a team

The simulation runs as long as the container is up. Agents keep working, collaborating, and iterating.

## ⚠️ Important: Protect Your Work

**MASON agents run with full permissions inside the container.** They can create, modify, and delete any file without asking. This is by design — it lets them work autonomously — but it means mistakes can happen. A misunderstood instruction, a bad refactor, or an overeager cleanup can destroy work.

**Treat the container as volatile. Back up what matters.**

### The Rules

1. **Commit early, commit often.** Agents use Forgejo (the built-in Git forge) for all code. Make sure your agents are committing their work to branches regularly. Code that's committed to Forgejo is safe — code that's only on disk can be lost.

2. **Snapshot before major changes.** If you're about to ask agents to restructure a project, refactor a codebase, or do anything destructive, take a snapshot first. No downtime required:
   ```bash
   # Snapshot the running container (no need to stop it)
   docker commit mason mason-snapshot-$(date +%Y%m%d)
   ```
   This saves the entire container state as a local image. To restore from a snapshot, stop the current container and start from the snapshot image instead.

3. **Don't mount your only copy of anything.** If you mount a host directory into the container (`--volume`), agents can modify those files too. Mount a copy, or make sure the original is in version control.

4. **Review before you trust.** Agents are capable but not infallible. Review important code changes before they're merged. Use Forgejo's pull request workflow — it exists for a reason.

5. **Use the built-in code review workflow.** Designate a merge manager (one of your agents) to review PRs before they're merged to the main branch. This catches mistakes before they become permanent.

> **The short version:** Git is your safety net. If it's committed, it's recoverable. If it's not committed, it's at risk. Make sure your agents are committing.

## Useful Commands

| Command | What it does |
|---------|-------------|
| `./scripts/masonctl start` | Start the MASON container |
| `./scripts/masonctl stop` | Stop the container (agents will need to be brought back — see [Restarting](#restarting-mason)) |
| `./scripts/masonctl restart` | Restart everything (only Connie auto-resumes — tell her to bring the team back) |
| `./scripts/masonctl status` | Check what's running |
| `./scripts/masonctl token` | Print your dashboard auth token |
| `./scripts/masonctl login` | Print token and open dashboard in browser |
| `./scripts/masonctl logs` | View logs |
| `./scripts/masonctl logs -f` | Follow logs in real time |
| `./scripts/masonctl pull` | Pull the latest MASON image from the registry |
| `./scripts/masonctl update` | Pull latest image and restart |
| `./scripts/masonctl rm` | Remove the container (keeps data) |
| `./scripts/masonctl rm --data` | Remove the container **and** all persistent data |
| `./scripts/masonctl verify` | Check file integrity inside the container |

For the full command reference, see [CONFIGURATION.md](CONFIGURATION.md#useful-commands).

## Restarting MASON

**After a stop/start or restart, only Connie (your concierge) comes back online automatically.** Your other agents don't auto-resume — you need to tell Connie to bring them back.

Open Mattermost and message Connie:

> "Hey Connie, bring the team back online"

Connie will restart your existing team from where they left off. All agent workspaces, memory, and project state are preserved in the Docker volume — only the running sessions need to be restarted.

> **Why doesn't the team auto-resume?** MASON preserves your data but doesn't automatically restart agent sessions. This gives you the choice to bring back the full team, a subset, or start fresh with a different team composition.

## Troubleshooting

### Agents aren't responding

**If you just started or restarted MASON**, remember that only Connie comes back automatically. Tell Connie to bring your team back online (see [Restarting MASON](#restarting-mason) above).

If Connie is online but agents aren't responding, give them 30-60 seconds. If still quiet:

```bash
# Check logs for errors
./scripts/masonctl logs --tail=50

# Restart
./scripts/masonctl restart
```

### Out of memory

Agents need room to think. If things are slow or crashing:

```bash
# Check resource usage
docker stats mason
```

Try increasing Docker's memory limit in Docker Desktop settings.

### Browser certificate warning

When you first open the MASON dashboard, your browser will show a security warning about an untrusted or self-signed certificate. This is normal — MASON generates a self-signed TLS certificate on first start to encrypt traffic between your browser and the local dashboard. Since the certificate isn't issued by a public certificate authority, your browser flags it.

**To proceed:** Click "Advanced" (or "Show Details" in Safari) and accept the certificate. You only need to do this once per browser. For full details on MASON's TLS setup and how to use your own certificate, see [SECURITY.md](SECURITY.md#tls).

### Authentication errors

If the wizard reports a credentials error, double-check your setup. For API keys, verify yours at [console.anthropic.com](https://console.anthropic.com). For Claude subscriptions, make sure you're signed in and your subscription is active.

### Port conflicts

If port 8080, 8065, or 3000 is already in use, set custom ports:

```bash
MASON_PORT_WEB=9080 MASON_PORT_MM=9065 MASON_PORT_FORGEJO=4000 ./scripts/masonctl start
```

Then open `https://localhost:9080` for the wizard. See [CONFIGURATION.md](CONFIGURATION.md#environment-variables) for all available port options.

## Updating

```bash
./scripts/masonctl update
```

Check the [Changelog](CHANGELOG.md) for what's new.

## Getting Help

**Something not working?** Ask Connie to run `/error-report` — she'll gather diagnostics and help you create a bug report you can file on GitHub.

- **Issues**: [github.com/Mason-Teams/mason-teams/issues](https://github.com/Mason-Teams/mason-teams/issues)
- **Email**: [info@masonteams.com](mailto:info@masonteams.com)
- **Website**: [masonteams.com](https://masonteams.com)

## What's Next?

Once your simulation is running:

1. **Explore the channels** — Each agent has their own space, plus shared project channels
2. **Start small** — Get a feel for how agents collaborate before going big
3. **Observe and participate** — Jump in when you have questions, or sit back and watch how the team works through problems together
4. **Level up** — Check out the **[Workflows Guide](WORKFLOWS.md)** for tips on running projects, managing your team, and getting the most out of MASON's tools

Welcome to MASON. Let's see what your team can do.
