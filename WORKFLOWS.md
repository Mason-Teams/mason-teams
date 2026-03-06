<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# MASON Workflows

A guide to getting the most out of MASON — from your first conversation with Connie to running a full agent team like a seasoned operator.

---

## 1. Basic — Your First Simulation

*Getting started: meet Connie, spin up a team, and find your way around.*

### The Interview

- What Connie asks and why it matters
- How your answers shape the team she builds
- Tips for describing your project (specific vs broad, what works best)

### Your First Team

- What to expect when agents come online
- How to find your agents (channels, DMs, who's who)
- The difference between watching and participating

### Using Forgejo

- What Forgejo is and why it's in the container
- How agents track changes to docs and plans
- Browsing the repo: seeing what your team has produced
- Issues as a lightweight way to track decisions and progress

### First Steps Checklist

- [ ] Complete the setup wizard
- [ ] Let Connie interview you
- [ ] Explore the Mattermost channels
- [ ] Check Forgejo to see agent commits
- [ ] Give your team a small task and watch how they coordinate

---

## 2. Intermediate — Running a Real Project

*Your team is up. Now how do you actually get work done?*

### Getting a Project Started

- How to frame a project for your agents
- Breaking big ideas into agent-sized pieces
- When to let agents self-organize vs giving explicit direction

### Team Structure

- Identifying a leader agent (who merges PRs, who manages)
- Defining responsibilities — who owns what
- How agents hand off work to each other
- The role of code review between agents

### Memory Systems — When to Use What

- **User memory** (`~/.claude/`) — Your preferences, global settings, things that apply everywhere
- **Project memory** (`.claude/CLAUDE.md`) — Architecture, conventions, team norms for this project
- **Memshot** (`/memshot`) — Quick snapshots of current state, for crash recovery or handoffs
- **Agent memory** (`memory_store`) — Learnings and gotchas that other agents should know
- **Obsidian diary** — Session logs for human review

### Decision Tree

```
Where does this info belong?
├─ Only this project? → Project CLAUDE.md
├─ All my sessions? → User CLAUDE.md
├─ Another agent needs it? → memory_store
├─ I need to recover from a crash? → memshot
└─ Human needs to review later? → Obsidian diary
```

---

## 3. Advanced — Mixed Control

*Terminal commands and Mattermost messages — two ways to talk to your team, each with strengths.*

### Terminal vs Mattermost — When to Use Which

- **Terminal** (direct Claude session) — Precise instructions, code-heavy work, debugging, when you need control
- **Mattermost** (chat) — Broad direction, team-wide announcements, async work, when you want agents to self-organize
- **Mixing both** — Using terminal for the agent you're paired with, MM for the rest of the team

### My Preference

<!-- dpark: fill in your actual workflow preferences here -->

- When I reach for terminal vs when I use MM
- How I switch between them during a session
- Why I prefer [X] for [Y] situations

### Agent Lifecycle Management

- Starting and stopping individual agents
- Reloading an agent when context gets stale
- Office hours mode — polling, monitoring, autonomous work
- Beepme — getting an agent's attention when you need them in a channel

### Tmux Session Management

- How agents live in tmux panes
- Sending commands to specific agents
- Monitoring multiple agents at once
- When to kill and respawn vs reload

---

## 4. The Container and You

*MASON runs in a single container. Here's how to customize it, connect it to your environment, and get things in and out.*

### Exposing Ports

- How to expose additional ports when agents spin up services inside the container
- Default ports (8080, 8065, 3000, 7681) and what they're for
- Adding custom port mappings for agent-built services (web apps, APIs, dev servers)
- When to use `masonctl` flags vs Docker port arguments

### Volume Mounts

- Mounting local directories into the container (sharing code, data, configs)
- The default data volume and what it persists
- Injecting files agents need (SSH keys, config files, datasets)
- Patterns for sharing a local project directory so agents can work on your code

### Networking

- How the container talks to your host machine
- Connecting MASON to local services (databases, APIs running on your machine)
- Connecting MASON to external services (cloud APIs, remote servers)
- DNS and hostname considerations

### Persistence

- What survives a container restart vs what doesn't
- Backing up your simulation state
- Moving a simulation between machines

### Resource Tuning

- Adjusting memory and CPU limits for larger teams
- Signs your container needs more resources
- Performance tips for running many agents simultaneously

### Common Recipes

<!-- Practical examples people can copy-paste -->

- Recipe: Mount a local project so agents can edit your code
- Recipe: Expose an agent's dev server to your browser
- Recipe: Connect MASON to a local database
- Recipe: Run MASON with custom resource limits
- Recipe: Persist simulation data across container rebuilds

---

## 5. Beyond — The Full Toolkit

*How an experienced operator uses MASON's tools together to work on real tasks and ideas.*

### The Operator's Workflow

<!-- dpark: this section is your voice — how YOU actually work with agents -->

- How I think about tasks before assigning them
- My process for going from idea → spec → implementation
- How I use agents differently for exploration vs execution
- When I do it myself vs when I delegate to agents

### Tools in Concert

- **Speckit workflow** — constprep → specify → clarify → plan → tasks → implement
- **Autopilot** — When to let agents run the full workflow autonomously
- **Code review loops** — How agents review each other's work before merging
- **Cross-agent collaboration** — Memchat, beepme, shared channels

### Patterns I've Found Useful

<!-- dpark: real examples from your experience -->

- Pattern: "Scout then build" — use one agent to explore, another to implement
- Pattern: "Terminal for precision, MM for direction"
- Pattern: "Let them argue" — when agent disagreement produces better results
- Pattern: [more to come]

### Anti-Patterns

- What doesn't work well
- Common mistakes when starting out
- When to pull the plug and redirect

---

*This guide is a work in progress. It'll grow as we learn more about what works.*
