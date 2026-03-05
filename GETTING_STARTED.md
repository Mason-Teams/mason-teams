<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Getting Started with MASON

Let's get your team up and running. This guide walks you through setup, configuration, and your first project — step by step.

## Before You Start

You'll need:

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **Docker** | v24+ | Latest stable |
| **Docker Compose** | v2.20+ | Latest stable |
| **RAM** | 8GB | 16GB+ |
| **Disk** | 10GB free | 20GB+ |
| **OS** | macOS, Linux, or Windows (WSL2) | macOS or Linux |

You'll also need an API key for your preferred LLM provider (OpenAI, Anthropic, etc.). MASON agents need an LLM to think.

## Step 1: Get the Code

```bash
git clone https://github.com/Mason-Teams/mason-teams.git
cd mason-teams
```

## Step 2: Configure Your Environment

Copy the example environment file and fill in your details:

```bash
cp .env.example .env
```

Open `.env` in your editor and set:

```bash
# Your LLM API key (required)
LLM_API_KEY=your-api-key-here

# Which provider to use
LLM_PROVIDER=anthropic  # or "openai"

# Your project name (agents will use this)
PROJECT_NAME=my-project
```

That's the minimum. The defaults handle everything else.

## Step 3: Start Your Team

```bash
docker compose up -d
```

First run takes a few minutes — Docker needs to pull the images. After that, startup is fast.

Check that everything's running:

```bash
docker compose ps
```

You should see your agents coming online. Give them a minute to get settled.

## Step 4: Say Hello

MASON includes a built-in chat interface where you and your agents communicate. Open it at:

```
http://localhost:8065
```

You'll find your team already there. Connie, the concierge, will greet you and help you get oriented.

## Step 5: Give Them a Project

In the chat, tell Connie what you're working on. Be as specific or as broad as you like:

- "We're building a REST API for a todo app"
- "I need help refactoring this Python codebase"
- "Let's design a landing page for our new product"

Connie will figure out which agents to bring in and get them started. You can jump into any channel to talk directly with individual agents.

## Configuration

### Team Size

By default, MASON starts with a small team. To adjust:

```yaml
# In docker-compose.yaml
services:
  mason:
    environment:
      - MAX_AGENTS=5  # Free tier limit
```

### Connecting Your Code

Mount your project directory so agents can work on your actual codebase:

```yaml
# In docker-compose.yaml
services:
  mason:
    volumes:
      - ./my-project:/workspace
```

Agents will see your code in `/workspace` and can read, write, and commit.

### Customizing Agent Roles

Want different specialists? Edit the team configuration:

```yaml
# In docker-compose.yaml (or team-config.yaml)
team:
  - role: backend-engineer
    focus: "Python, FastAPI, PostgreSQL"
  - role: frontend-engineer
    focus: "React, TypeScript, Tailwind"
  - role: qa-engineer
    focus: "Testing, code review, security"
```

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

Try reducing `MAX_AGENTS` or increasing Docker's memory limit in Docker Desktop settings.

### API key errors

Double-check your `.env` file. The key should be the raw key string, no quotes needed:

```bash
LLM_API_KEY=sk-abc123...
```

### Port conflicts

If port 8065 is taken, change it in `docker-compose.yaml`:

```yaml
ports:
  - "9065:8065"  # Use port 9065 instead
```

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
