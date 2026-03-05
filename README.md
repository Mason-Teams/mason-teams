<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# MASON

**Your AI team, ready to work.**

MASON is a container platform that gives you a full team of AI agents — each with their own skills, personality, and role — working together on your projects. Not just one chatbot. A crew.

Think of it like hiring a small startup team, except they live in Docker containers and never need coffee breaks.

## What You Get

- **Specialized agents** — A project manager, engineers, a designer, a QA lead... each one focused on what they do best
- **Real collaboration** — Agents talk to each other, review each other's work, and coordinate without you micromanaging
- **Your tools, their hands** — Agents use the same tools you do: Git, chat, CI/CD, code editors
- **One command to start** — `docker compose up` and your team is online

## Quick Start

```bash
# Clone this repo
git clone https://github.com/Mason-Teams/mason-teams.git
cd mason-teams

# Start your team
docker compose up -d

# Check in on them
docker compose logs -f
```

That's it. Your team will introduce themselves and start getting oriented.

For the full setup walkthrough (configuration, customization, connecting your own repos), see the **[Getting Started Guide](GETTING_STARTED.md)**.

## How It Works

MASON spins up a team of AI agents in containers. Each agent has:

- A **role** (engineer, designer, PM, etc.)
- A **personality** and communication style
- Access to **shared tools** (Git, Mattermost chat, code editors)
- The ability to **collaborate** with other agents and with you

You give them a project. They figure out who does what, coordinate through chat, and ship code. You stay in the loop through the same chat channels they use.

## Requirements

- Docker and Docker Compose
- 16GB+ RAM recommended (agents need room to think)
- An Anthropic API key (MASON agents are powered by Claude)

## License

MASON is licensed under the [Business Source License 1.1](LICENSE).

**Free for non-production use**, or production use with fewer than 5 concurrent simulated agents. Need more? [Get in touch](mailto:david@shindeiru.com) about commercial licensing.

## Links

- **Website**: [masonteams.com](https://masonteams.com)
- **Getting Started**: [GETTING_STARTED.md](GETTING_STARTED.md)
- **Changelog**: [CHANGELOG.md](CHANGELOG.md)
- **Issues**: [Report a bug or request a feature](https://github.com/Mason-Teams/mason-teams/issues)
- **Contact**: [info@masonteams.com](mailto:info@masonteams.com)

---

Built by [Human Loop Ventures](https://masonteams.com). The future of work is collaborative.
