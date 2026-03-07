<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# MASON

**Your AI team, ready to simulate.**

MASON is a simulation platform that lets you spin up a team of AI agents — each with their own skills, personality, and role — and put them to work on a project together. Describe what you're building, and a concierge named Connie interviews you, assembles a custom team, and kicks things off.

Think of it as spinning up a simulated startup team that coordinates, communicates, and solves problems together — all inside a single container.

## What You Get

- **A concierge who gets it** — Connie interviews you about your project, then assembles the right team for the job
- **Specialized agents** — Engineers, a designer, a PM, a QA lead... each one focused on what they do best
- **Real collaboration** — Agents talk to each other, review each other's work, and coordinate without you micromanaging
- **Familiar tools** — Agents use the same tools real teams use: Git, chat, terminal
- **One command to start** — `./scripts/masonctl start` and you're up

## Quick Start

```bash
# Clone and start
git clone https://github.com/Mason-Teams/mason-teams.git
cd mason-teams
./scripts/masonctl start
```

Then open **http://localhost:8080** to run the setup wizard. Bring your own Anthropic API key or use your Claude subscription — the wizard lets you choose. Configure your profile, and meet Connie — she'll take it from there.

For the full walkthrough, see the **[Getting Started Guide](GETTING_STARTED.md)**.

## How It Works

MASON runs everything inside a single container — agents, chat, code tools, the works. Each agent has:

- A **role** (engineer, designer, PM, etc.)
- A **personality** and communication style
- Access to **shared tools** (Git, Mattermost chat, terminal)
- The ability to **collaborate** with other agents and with you

You describe a project. Connie figures out what kind of team you need, spins them up, and they get to work. You observe and participate through the same chat channels they use.

> **Note**: MASON is a simulation platform. Agent outputs are exploratory, not professional work product. See the User Agreement for details.

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (includes Docker CLI and everything you need)
- 16GB+ RAM recommended (agents need room to think)
- An Anthropic API key or Claude subscription (agents are powered by Claude)

MASON also requires [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code) by Anthropic, which is installed during the setup wizard. Claude Code is not bundled in the container — you install it at your direction, subject to Anthropic's [Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms).

## License

MASON is licensed under the [Business Source License 1.1](LICENSE).

**Free for non-production use**, or production use with fewer than 5 concurrent simulated agents. Need more? [Get in touch](mailto:info@masonteams.com) about commercial licensing.

## Links

- **Website**: [masonteams.com](https://masonteams.com)
- **Getting Started**: [GETTING_STARTED.md](GETTING_STARTED.md)
- **Configuration**: [CONFIGURATION.md](CONFIGURATION.md)
- **Workflows**: [WORKFLOWS.md](WORKFLOWS.md)
- **Changelog**: [CHANGELOG.md](CHANGELOG.md)
- **Issues**: [Report a bug or request a feature](https://github.com/Mason-Teams/mason-teams/issues)
- **Contact**: [info@masonteams.com](mailto:info@masonteams.com)

---

Built by [Human Loop Ventures](https://masonteams.com). The future of work is collaborative.
