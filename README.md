<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
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

Then log in to the dashboard:

```bash
./scripts/masonctl login
```

This prints your auth token and opens **https://localhost:8080** in your browser. Bring your own Anthropic API key or use your Claude subscription — the wizard lets you choose. Configure your profile, and meet Connie — she'll take it from there.

For the full walkthrough, see the **[Getting Started Guide](GETTING_STARTED.md)**.

## How It Works

MASON runs everything inside a single container — agents, chat, code tools, the works. Each agent has:

- A **role** (engineer, designer, PM, etc.)
- A **personality** and communication style
- Access to **shared tools** (Git, Mattermost chat, terminal)
- The ability to **collaborate** with other agents and with you

You describe a project. Connie figures out what kind of team you need, spins them up, and they get to work. You observe and participate through the same chat channels they use.

> **Note**: MASON is a simulation platform. Agent outputs are exploratory, not professional work product. See the User Agreement for details.

## Security

MASON takes a secure-by-default approach to localhost development:

- **Token authentication** — Dashboard access requires a 32-character token generated on first start (`masonctl token` to retrieve)
- **TLS encrypted** — Dashboard serves HTTPS with an auto-generated certificate
- **Minimal exposure** — Only 3 ports are mapped to the host; all internal services are localhost-only inside the container
- **Read-only credentials** — Host credentials are mounted read-only into the container
- **Locked-down permissions** — Token and TLS key files are mode 0600

For the full security model, including token management, network details, and recommendations for networked environments, see **[SECURITY.md](SECURITY.md)**.

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (includes Docker CLI and everything you need)
- 16GB+ RAM recommended (agents need room to think)
- An Anthropic API key or Claude subscription (agents are powered by Claude)

MASON also requires [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code) by Anthropic, which is installed during the setup wizard. Claude Code is not bundled in the container — you install it at your direction, subject to Anthropic's [Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms).

## A Note on Quality and Compatibility

MASON is built and maintained by a single developer, with AI agents doing much of the heavy lifting — writing code, reviewing each other's work, and shipping features. It's a small operation building something ambitious, and I want to set honest expectations.

**What's been tested:**

- macOS on Apple Silicon (M4 Max MacBook Pro) with Docker Desktop
- That's the environment MASON was built in, and it's the only one I can confirm works

**What hasn't been tested:**

- Intel Macs, Windows, or Linux hosts
- Other container runtimes (Podman, Rancher Desktop, OrbStack, etc.)
- Cloud or remote Docker environments

It may work on other setups — the container itself is standard — but I haven't verified it and can't promise a smooth experience outside the tested environment.

**On bugs:**

There will be bugs. Both manual and automated testing have been done, but a project this size built by one person and a team of agents is going to have rough edges. If you hit something, please [open an issue](https://github.com/Mason-Teams/mason-teams/issues) — I'll do my best to address bugs in a reasonable timeframe. Clear reproduction steps help a lot.

**On scope:**

This is an early release. Features may change, APIs may shift, and some things might not work the way you expect yet. Your patience and feedback are genuinely appreciated — they make MASON better for everyone.

## License

MASON is licensed under the [Business Source License 1.1](LICENSE).

**Free for non-production use**, or production use with fewer than 5 MASON-orchestrated agents. For production deployments exceeding 5 agents, [contact us about a commercial license](mailto:info@masonteams.ai).

**Note:** MASON requires [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code) by Anthropic, which is installed separately at your direction during setup. Your use of Claude Code is governed by [Anthropic's Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms). MASON's license covers the orchestration platform only — not third-party dependencies.

## Links

- **Website**: [masonteams.com](https://masonteams.com)
- **Getting Started**: [GETTING_STARTED.md](GETTING_STARTED.md)
- **Configuration**: [CONFIGURATION.md](CONFIGURATION.md)
- **Skills and Commands**: [SKILLSANDCOMMANDS.md](SKILLSANDCOMMANDS.md)
- **Workflows**: [WORKFLOWS.md](WORKFLOWS.md)
- **Security**: [SECURITY.md](SECURITY.md)
- **Changelog**: [CHANGELOG.md](CHANGELOG.md)
- **Wiki**: [Guides, tips, and community knowledge](https://github.com/Mason-Teams/mason-teams/wiki)
- **Contributing**: [CONTRIBUTING.md](CONTRIBUTING.md)
- **Issues**: [Report a bug or request a feature](https://github.com/Mason-Teams/mason-teams/issues)
- **Contact**: [info@masonteams.com](mailto:info@masonteams.com)

---

Built by [Human Loop Ventures](https://humanloopventures.com). The future is together, not apart.
