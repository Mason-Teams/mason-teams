<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Getting Started with MASON

Let's get your team up and running. This guide walks you through setup, the wizard, and your first simulation — step by step.

## Before You Start

You'll need:

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **Docker** | v24+ | Latest stable |
| **RAM** | 8GB | 16GB+ |
| **Disk** | 10GB free | 20GB+ |
| **OS** | macOS, Linux, or Windows (WSL2) | macOS or Linux |

You'll also need an **Anthropic API key**. MASON agents are powered by Claude — grab your key from [console.anthropic.com](https://console.anthropic.com).

## Step 1: Get the Code

```bash
git clone https://github.com/Mason-Teams/mason-teams.git
cd mason-teams
```

## Step 2: Build and Start

MASON runs as a single container — agents, chat, code tools, everything inside one box.

```bash
# Build the container image (first time only)
masonctl build

# Start MASON
masonctl start
```

Check that everything's running:

```bash
masonctl status
```

## Step 3: Run the Setup Wizard

Open your browser and go to:

```
http://localhost:8080
```

The wizard walks you through six steps:

1. **Accept the User Agreement** — Read through and confirm the three acknowledgments (simulation platform, no-harm commitment, no professional advice)
2. **Choose your auth method** — API key or subscription-based authentication
3. **Enter your Claude credentials** — Paste your Anthropic API key
4. **Set up your profile** — Your name and what to call your concierge
5. **Claude terminal authentication** — Verifies your credentials work
6. **Launch** — Everything starts up

No config files to edit, no environment variables to set. The wizard handles it all.

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
- **Give direction** when you want to steer the work
- **Sit back and observe** how they tackle problems as a team

The simulation runs as long as the container is up. Agents keep working, collaborating, and iterating.

## Useful Commands

| Command | What it does |
|---------|-------------|
| `masonctl start` | Start the MASON container |
| `masonctl stop` | Stop the container |
| `masonctl restart` | Restart everything |
| `masonctl status` | Check what's running |
| `masonctl logs` | View logs |
| `masonctl logs -f` | Follow logs in real time |

## Troubleshooting

### Agents aren't responding

Give them 30-60 seconds after startup. If still quiet:

```bash
# Check logs for errors
masonctl logs --tail=50

# Restart
masonctl restart
```

### Out of memory

Agents need room to think. If things are slow or crashing:

```bash
# Check resource usage
docker stats
```

Try increasing Docker's memory limit in Docker Desktop settings.

### API key errors

If the wizard reports an API key error, double-check that you're using a valid Anthropic API key. You can verify your key at [console.anthropic.com](https://console.anthropic.com).

### Port conflicts

If port 8080 or 8065 is already in use, set custom ports:

```bash
MASON_PORT_WEB=9080 MASON_PORT_MM=9065 masonctl start
```

Then open `http://localhost:9080` for the wizard.

## Updating

```bash
git pull
masonctl build
masonctl restart
```

Check the [Changelog](CHANGELOG.md) for what's new.

## Getting Help

- **Issues**: [github.com/Mason-Teams/mason-teams/issues](https://github.com/Mason-Teams/mason-teams/issues)
- **Email**: [info@masonteams.com](mailto:info@masonteams.com)
- **Website**: [masonteams.com](https://masonteams.com)

## What's Next?

Once your simulation is running:

1. **Explore the channels** — Each agent has their own space, plus shared project channels
2. **Start small** — Get a feel for how agents collaborate before going big
3. **Observe and participate** — Jump in when you have questions, or sit back and watch how the team works through problems together

Welcome to MASON. Let's see what your team can do.
