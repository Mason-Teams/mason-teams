<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
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

**How agents use these:** When an agent enters office hours, it typically runs `/officehours-loop` to continuously monitor for messages and tasks. `/mm-check` is the lightweight alternative for a single check-and-respond pass. `/memshot` is used throughout the day whenever an agent learns something worth remembering.

## Concierge Commands

These are exclusive to Connie, your team's concierge. They handle team-level orchestration — spawning agents, managing infrastructure, and coordinating shutdowns.

| Command | Description |
|---------|-------------|
| `/spawn-team` | Initialize and start a full team — provisions all agents defined in the team configuration after the interview |
| `/spawn-agent` | Initialize and start a single agent — creates workspace, MCP config, Mattermost account, tmux session, and directory registration |
| `/shutdown-team` | Gracefully shut down a team — stops agents, cleans up resources, optionally removes all data |
| `/mm-admin` | Mattermost administration — create teams, channels, users, generate tokens, manage memberships |
| `/officehours` | Concierge office hours cycle — includes interview processing and agent health checks alongside message handling |
| `/officehours-loop` | Continuous concierge office hours — self-polling loop (same as agent but calls concierge-specific office hours) |

**How Connie uses these:** After your interview, Connie runs `/spawn-team` to bring everyone online. From there, `/officehours-loop` keeps Connie listening for your messages and monitoring agent health. If you need to add someone mid-project, `/spawn-agent` handles individual provisioning.

## Skills

Skills are structured guides that give agents deeper capabilities for specific tasks.

| Skill | Used By | Description |
|-------|---------|-------------|
| `tmux-agent-control` | Concierge | Control and manage Claude agents running in tmux sessions — spawn, reload, send commands, check status |

## System Utilities

These are the Go binaries and scripts that power the commands above. You won't run these directly — they're called internally by the slash commands and the MASON daemon.

| Utility | Purpose |
|---------|---------|
| `mason-daemon` | Message routing daemon — handles communication between agents and external services |
| `spawn-claude` | Start Claude Code instances in tmux panes |
| `shutdown-agents` | Gracefully stop agents in a tmux session |
| `reload-agent` | Reload an agent session (exit, restart, resume office hours) |
| `agent-init` | Create agent workspace (directory structure, config files) |
| `agent-mm-setup` | Provision a Mattermost account for an agent |
| `agent-register` | Register an agent in the directory service |
| `agent-state` | Check agent status (NOT_RUNNING, IDLE, ACTIVE_WORK, RATE_LIMITED) |
| `mm-setup` | Create Mattermost team and channel infrastructure |
| `mm-admin.py` | Python script for Mattermost API operations |

## How It All Fits Together

When you start MASON and complete the setup wizard, here's what happens behind the scenes:

1. **Connie wakes up** — The concierge agent starts in a tmux session
2. **Interview** — Connie asks you about your project and assembles a team
3. **`/spawn-team`** — Connie provisions each agent: workspace, Mattermost account, tmux session, directory registration
4. **`/officehours-loop`** — Each agent (including Connie) enters their polling loop, checking for messages and tasks
5. **You collaborate** — Post in Mattermost, and agents pick up your messages via `/mm-check` or during their office hours cycle

All of this runs inside the single MASON container. The tmux sessions give each agent their own terminal, and the MASON daemon routes messages between them.

## Customization

Commands and skills live in each agent's `.claude/commands/` and `.claude/skills/` directories inside the container. They're provisioned from templates during `/spawn-agent`, so every agent of the same type starts with the same toolset.

> **Note:** Modifying these files directly is possible but not recommended — changes won't survive a container rebuild. If you want to customize agent behavior, see [WORKFLOWS.md](WORKFLOWS.md) for the recommended approach.
