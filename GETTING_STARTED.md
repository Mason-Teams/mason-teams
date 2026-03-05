<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Getting Started with MASON

Let's get your team up and running. This guide walks you through setup and your first project — step by step.

## Before You Start

You'll need:

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **Docker** | v24+ | Latest stable |
| **Docker Compose** | v2.20+ | Latest stable |
| **RAM** | 8GB | 16GB+ |
| **Disk** | 10GB free | 20GB+ |
| **OS** | macOS, Linux, or Windows (WSL2) | macOS or Linux |

You'll also need an **Anthropic API key**. MASON agents are powered by Claude — grab your key from [console.anthropic.com](https://console.anthropic.com).

## Step 1: Get the Code

```bash
git clone https://github.com/Mason-Teams/mason-teams.git
cd mason-teams
```

## Step 2: Start Your Team

```bash
docker compose up -d
```

First run takes a few minutes — Docker needs to pull the images. After that, startup is fast.

Check that everything's running:

```bash
docker compose ps
```

## Step 3: Run the Setup Wizard

Open your browser and go to:

```
http://localhost:3000
```

The setup wizard walks you through everything:

1. **Accept the User Agreement** — Read through, check the three confirmation boxes
2. **Enter your Anthropic API key** — Or choose subscription-based authentication if you have one
3. **Name your project** — Tell MASON what you're working on

That's it. No config files to edit, no environment variables to set. The wizard handles the rest.

## Step 4: Meet Your Team

Once setup is complete, the wizard opens your team's chat interface (Mattermost) with login credentials pre-filled. You'll find your agents already there.

**Connie**, the concierge, will greet you and help you get oriented. She's your guide to everything MASON — think of her as the team coordinator who makes sure everyone's on the same page.

## Step 5: Give Them a Project

In the chat, tell Connie what you're working on. Be as specific or as broad as you like:

- "We're building a REST API for a todo app"
- "I need help refactoring this Python codebase"
- "Let's design a landing page for our new product"

Connie will figure out which agents to bring in and get them started. You can jump into any channel to talk directly with individual agents.

## Troubleshooting

### Agents aren't responding

Give them 30-60 seconds after startup. If still quiet:

```bash
# Check logs for errors
docker compose logs --tail=50

# Restart the stack
docker compose restart
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

If port 3000 is taken, check `docker-compose.yaml` for the port mapping and adjust as needed.

## Updating

```bash
git pull
docker compose pull
docker compose up -d
```

Check the [Changelog](CHANGELOG.md) for what's new.

## Getting Help

- **Issues**: [github.com/Mason-Teams/mason-teams/issues](https://github.com/Mason-Teams/mason-teams/issues)
- **Email**: [info@masonteams.com](mailto:info@masonteams.com)
- **Website**: [masonteams.com](https://masonteams.com)

## What's Next?

Once your team is running:

1. **Explore the channels** — Each agent has their own space, plus shared project channels
2. **Try a small task first** — Get a feel for how agents collaborate before going big
3. **Check in, don't hover** — Agents work best when you give direction and let them run. Pop in when you have questions or want to steer.

Welcome to MASON. Let's build something together.
