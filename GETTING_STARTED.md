<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Getting Started with MASON

Let's get your team up and running. This guide walks you through setup and your first simulation — step by step.

## Before You Start

You'll need:

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **Docker** | v24+ | Latest stable |
| **RAM** | 8GB | 16GB+ |
| **Disk** | 10GB free | 20GB+ |
| **OS** | macOS, Linux, or Windows (WSL2) | macOS or Linux |

You'll also need an **Anthropic API key**. MASON agents are powered by Claude — grab your key from [console.anthropic.com](https://console.anthropic.com).

## Step 1: Start MASON

MASON runs as a single container — everything lives inside it (agents, chat, code tools).

```bash
docker run -d -p 3000:3000 -p 8065:8065 ghcr.io/mason-teams/mason-teams:latest
```

First run takes a minute while the container initializes. Check that it's healthy:

```bash
docker ps
```

## Step 2: Run the Setup Wizard

Open your browser and go to:

```
http://localhost:3000
```

The setup wizard walks you through everything:

1. **Accept the User Agreement** — Read through, check the three confirmation boxes (simulation platform acknowledgment, no-harm commitment, no professional advice)
2. **Enter your Anthropic API key** — Or choose subscription-based authentication if you have one
3. **Name your project** — Tell MASON what you want to explore

That's it. No config files to edit, no environment variables to set. The wizard handles the rest.

## Step 3: Meet Your Team

Once setup is complete, the wizard opens your team's chat interface (Mattermost) with login credentials pre-filled. You'll find your agents already there.

**Connie**, the concierge, will greet you and help you get oriented. She's your guide to everything MASON — think of her as the team coordinator who makes sure everyone's on the same page.

## Step 4: Start a Simulation

In the chat, tell Connie what you'd like to explore. Be as specific or as broad as you like:

- "Let's simulate building a REST API for a todo app"
- "I want to see how a team would approach refactoring this Python codebase"
- "Explore designing a landing page for a new product"

Connie will figure out which agents to bring in and get the simulation started. You can jump into any channel to observe and participate alongside individual agents.

## Troubleshooting

### Agents aren't responding

Give them 30-60 seconds after startup. If still quiet:

```bash
# Check logs for errors
docker logs <container-id> --tail=50

# Restart
docker restart <container-id>
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

If port 3000 or 8065 is already in use, map to different ports:

```bash
docker run -d -p 3001:3000 -p 9065:8065 ghcr.io/mason-teams/mason-teams:latest
```

Then open `http://localhost:3001` for the wizard.

## Updating

```bash
docker pull ghcr.io/mason-teams/mason-teams:latest
# Stop and remove old container, then run the new image
```

Check the [Changelog](CHANGELOG.md) for what's new.

## Getting Help

- **Issues**: [github.com/Mason-Teams/mason-teams/issues](https://github.com/Mason-Teams/mason-teams/issues)
- **Email**: [info@masonteams.com](mailto:info@masonteams.com)
- **Website**: [masonteams.com](https://masonteams.com)

## What's Next?

Once your simulation is running:

1. **Explore the channels** — Each agent has their own space, plus shared project channels
2. **Try a small project first** — Get a feel for how agents collaborate before going big
3. **Observe and participate** — Jump in when you have questions, or sit back and watch how the team works through problems together

Welcome to MASON. Let's explore what's possible.
