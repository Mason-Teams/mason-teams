<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Skills and Commands

MASON agents come pre-loaded with slash commands that handle everything from message checking to team orchestration. This guide covers what's available, who can use it, and what each command does.

## Agent Commands

Every agent in your team gets these commands automatically. They're provisioned from templates when the agent is spawned.

| Command | Description |
|---------|-------------|
| `/agentbootstrap` | First-time setup — adds domain-specific professional standards to the agent's persona based on their role |
| `/memshot` | Quick memory snapshot — stores a learning, decision, or task state to agent memory for later retrieval |
| `/mm-check` | One-shot message check — fetches pending Mattermost messages, classifies them (work, question, info), executes, and returns to idle |
| `/officehours` | One-shot office hours cycle — checks messages, processes work, updates timestamps, returns to idle |
| `/officehours-loop` | Continuous office hours — self-polling loop with configurable intervals for direct message checks and broader context sweeps |

**How agents use these:** The daemon triggers `/officehours` automatically when messages come in through Mattermost — it's push-based, so agents respond as things happen. `/officehours-loop` is available as an alternative for users who prefer a polling-based approach, running continuous checks on a timer from the terminal. `/mm-check` is the lightweight single-pass version for a quick check-and-respond. `/memshot` is used throughout the day whenever an agent learns something worth remembering.

## Concierge Commands

These are exclusive to Connie, your team's concierge. They handle team-level orchestration — spawning agents, managing infrastructure, and coordinating shutdowns.

| Command | Description |
|---------|-------------|
| `/change-password` | Change your Mattermost and Forgejo passwords — handles secure credential exchange via DM |
| `/error-report` | Generate a diagnostic report for filing bug reports — gathers system info, logs, and agent status into a formatted report |
| `/spawn-team` | Initialize and start a full team — provisions all agents defined in the team configuration after the interview |
| `/spawn-agent` | Initialize and start a single agent — creates workspace, MCP config, Mattermost account, tmux session, and directory registration |
| `/shutdown-team` | Gracefully shut down a team — stops agents, cleans up resources, optionally removes all data |
| `/mm-admin` | Mattermost administration — create teams, channels, users, generate tokens, manage memberships (wraps `mm-admin.py`) |
| `/officehours` | Concierge office hours cycle — includes interview processing and agent health checks alongside message handling |
| `/officehours-loop` | Continuous concierge office hours — polling-based alternative that runs on a timer from the terminal |

**Reporting issues:** If something goes wrong, ask Connie to run `/error-report`. She'll walk you through describing the problem, gather diagnostic information (system info, logs, agent status), and produce a formatted report you can paste directly into a [GitHub issue](https://github.com/Mason-Teams/mason-teams/issues). Connie can also walk you through setting up the `gh` CLI if you'd like to file issues from the command line.

**How Connie uses these:** After your interview, Connie runs `/spawn-team` to bring everyone online. From there, the daemon triggers `/officehours` whenever you post in Mattermost, keeping Connie responsive to your messages and monitoring agent health. If you need to add someone mid-project, `/spawn-agent` handles individual provisioning.

## Skills

Skills are structured guides that give agents deeper capabilities for specific tasks.

| Skill | Used By | Description |
|-------|---------|-------------|
| `tmux-agent-control` | Concierge | Control and manage Claude agents running in tmux sessions — spawn, reload, send commands, check status |

## User Commands

Commands you can run directly to manage and verify your MASON installation.

| Command | Description |
|---------|-------------|
| `./scripts/masonctl verify` | Check that all MASON files in the container match their build-time checksums (SHA-256) |

### `./scripts/masonctl verify`

Verifies the integrity of templates, commands, skills, scripts, and Go binaries inside the container. Useful for confirming nothing has been modified unexpectedly.

```bash
./scripts/masonctl verify              # Quick integrity check
./scripts/masonctl verify --remote     # Verify against the official GitHub Release manifest
./scripts/masonctl verify --json       # Machine-readable output for scripting
./scripts/masonctl verify --quiet      # One-line summary
```

**Exit codes:** `0` = all files verified, `1` = mismatch found, `2` = error

## System Utilities

These are the Go binaries and scripts that power the commands above. You won't run these directly — they're called internally by the slash commands and the MASON daemon.

| Utility | Purpose |
|---------|---------|
| `mason-daemon` | Core daemon — watches Mattermost for new messages and triggers `/officehours` on the appropriate agent via tmux |
| `spawn-claude` | Start Claude Code instances in tmux panes |
| `shutdown-agents` | Gracefully stop agents in a tmux session |
| `reload-agent` | Reload an agent session (exit, restart, resume office hours) |
| `agent-init` | Create agent workspace (directory structure, config files) |
| `agent-mm-setup` | Provision a Mattermost account for an agent |
| `agent-register` | Register an agent in the directory service |
| `agent-state` | Check agent status (NOT_RUNNING, IDLE, OFFICEHOURS_IDLE, ACTIVE_WORK, RATE_LIMITED) |
| `start-officehours` | Send `/officehours` to an agent's tmux pane and verify startup |
| `mm-setup` | Create Mattermost team and channel infrastructure |
| `mm-admin.py` | Python script for Mattermost API operations |

## How It All Fits Together

When you start MASON and complete the setup wizard, here's what happens behind the scenes:

1. **Connie wakes up** — The concierge agent starts in a tmux session
2. **Interview** — Connie asks you about your project and assembles a team
3. **`/spawn-team`** — Connie provisions each agent: workspace, Mattermost account, tmux session, directory registration
4. **Daemon starts** — The MASON daemon watches Mattermost for new messages and triggers `/officehours` on the right agent
5. **You collaborate** — Post in Mattermost, and agents pick up your messages automatically via the daemon, or you can run `/officehours-loop` from the terminal for polling-based checks

All of this runs inside the single MASON container. The tmux sessions give each agent their own terminal, and the MASON daemon routes messages between them.

## Customization

Commands and skills live in each agent's `.claude/commands/` and `.claude/skills/` directories inside the container. They're provisioned from templates during `/spawn-agent`, so every agent of the same type starts with the same toolset.

> **Note:** Modifying these files directly is possible but not recommended — changes won't survive a container rebuild. If you want to customize agent behavior, see [WORKFLOWS.md](WORKFLOWS.md) for tips on working with your team.
