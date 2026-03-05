<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# MASON

**Explore what happens when AI agents work as a team.**

MASON is a simulation platform that lets you spin up a team of AI agents — each with their own skills, personality, and role — and watch them collaborate on a project together. It's a sandbox for exploring how autonomous agents coordinate, communicate, and solve problems as a group.

Think of it as a flight simulator, but for AI teamwork.

## What You Get

- **Specialized agents** — A project manager, engineers, a designer, a QA lead... each one focused on what they do best
- **Real collaboration** — Agents talk to each other, review each other's work, and coordinate without you micromanaging
- **Familiar tools** — Agents use the same tools real teams use: Git, chat, code editors
- **One command to start** — `docker run` and your team is online

## Quick Start

```bash
# Pull and run MASON
docker run -d -p 3000:3000 -p 8065:8065 ghcr.io/mason-teams/mason-teams:latest

# Open the setup wizard
open http://localhost:3000
```

The wizard walks you through setup — enter your Anthropic API key, name your project, and your team comes to life.

For the full walkthrough, see the **[Getting Started Guide](GETTING_STARTED.md)**.

## How It Works

MASON runs a team of AI agents inside a single container. Each agent has:

- A **role** (engineer, designer, PM, etc.)
- A **personality** and communication style
- Access to **shared tools** (Git, Mattermost chat, code editors)
- The ability to **collaborate** with other agents and with you

You describe a project. The agents figure out who does what, coordinate through chat, and work through it together. You observe and participate through the same chat channels they use.

> **Note**: MASON is a simulation platform. Agent outputs are exploratory, not professional work product. See the User Agreement for details.

## Requirements

- Docker
- 16GB+ RAM recommended (agents need room to think)
- An Anthropic API key (agents are powered by Claude)

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
