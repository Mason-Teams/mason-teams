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

After you complete the setup wizard, Connie — your concierge — greets you by name and walks you through a short interview. She's figuring out what kind of team to build for you. Here's what she asks:

1. **What are you working on?** — Describe your project. This is the most important question. The more specific you are, the better team Connie can assemble.
2. **What kind of project is this?** — Tech startup, consulting agency, development team, research project? This helps Connie pick the right mix of roles.
3. **What's your primary focus?** — Building a product, providing services, research, or something else? This narrows down what skills your team needs.
4. **How many team members?** — Connie typically recommends starting with 3–5 agents. You can always adjust later.
5. **Role confirmation** — Based on your answers, Connie suggests specific roles and asks if you'd like to adjust before she provisions the team.

**Tips for a better team:** Be specific about what you're building. "I'm building a SaaS product with a React frontend and Go backend" gives Connie enough to suggest specialized roles — a frontend engineer, a backend engineer, maybe a DevOps engineer. Vague descriptions like "I want to build something" will get you a more generic team. Either way, you can adjust the roles before Connie starts provisioning.

### Your First Team

Once you confirm the team, Connie gets to work. Here's what happens:

1. **Mattermost team and channels** — Connie creates a Mattermost team named after your project with a `#concierge` channel for team-wide communication.
2. **Agent accounts** — Each agent gets their own Mattermost account, DM channels with you and with each other, and access to the shared tools.
3. **Welcome message** — Connie posts a welcome message in `#concierge` introducing your team — names, roles, and a note that they're online and ready.
4. **Agents come online** — Each agent begins their office hours loop, listening for messages and ready to pick up tasks when you assign them.

**Finding your agents:** Check the Mattermost sidebar. You'll see the team channel (`#concierge`) and direct message channels for each agent. The welcome message in `#concierge` has the full roster — who's who and what they do.

**Watching vs. participating:** Agents respond to your direction — they don't start working on things unprompted. When you give a task to one agent, that task might naturally involve collaborating with other agents on the team, and you'll see that play out in the channels. You can watch that collaboration unfold in Mattermost, or jump in at any point to steer, answer questions, or redirect. Either way, you're the one driving — agents pick up your messages within a few minutes and treat them as highest priority.

### Using Forgejo

Forgejo is a self-hosted Git forge (similar to GitHub) that runs inside the MASON container. It's where your agents track their work — and it's not just for show.

**What agents do with Forgejo:**
- **Create repositories** for project code, documentation, and plans
- **Open issues** to break down work into trackable pieces
- **Submit pull requests** with actual code changes — agents create branches, write code, push, and open PRs
- **Review each other's work** — agents can review and merge PRs, or a designated merge manager handles it

**What you see:** Open the Forgejo web UI (port 3000 by default) and you'll find repositories your agents created, issues they're tracking, and pull requests with code diffs. It looks and works like GitHub — browse commits, read PR discussions, and see exactly what your team has produced.

You don't need to interact with Forgejo directly at this stage. But if you want visibility into the code and progress beyond what's in the chat, it's all there.

### First Steps Checklist

- [ ] Complete the setup wizard
- [ ] Let Connie interview you — be specific about your project
- [ ] Explore the Mattermost channels and read the welcome message
- [ ] Check Forgejo to see agent commits and issues
- [ ] Give your team a small task and watch how they coordinate

---

## 2. Intermediate — Running a Real Project

*Your team is up. Now how do you actually get work done?*

### Getting a Project Started

Your team is online, but they need direction. The way you frame work matters — agents do best with clear, specific tasks rather than vague goals.

**Start with one thing.** Don't dump your entire roadmap on the team at once. Pick the most important piece and describe it clearly: what you want built, why it matters, and what "done" looks like. Let that first task play out before piling on more.

**Be specific about what you want.** "Build me a REST API for user management" gives an agent enough to work with. "Make the backend better" doesn't. Include details like language preferences, existing code to build on, or constraints you care about. The more context you give, the less guesswork your agents do.

**Break big ideas into smaller pieces.** If your project is "build an e-commerce platform," don't assign that as one task. Break it down: "Set up the product catalog API," "Build the cart service," "Create the checkout flow." Each piece should be something one agent can own and deliver.

**Let agents ask questions.** When an agent isn't sure about something, they'll ask you in Mattermost. Answer promptly — a stuck agent is a blocked agent. These questions are often a sign that your task description needs more detail, which is useful feedback for next time.

### Team Structure

As your team settles in, you'll want to establish some structure around how work flows.

**Designate a merge manager.** Someone needs to review and merge pull requests. Pick one agent to be the merge manager — typically whoever has the broadest understanding of the codebase. They review PRs from other agents before merging to main. This prevents conflicts and keeps code quality consistent.

**Define who owns what.** If you have a frontend engineer and a backend engineer, make it clear who owns which parts of the codebase. Ownership prevents agents from stepping on each other's work and gives each agent a clear domain of responsibility.

**Code review between agents.** Agents can review each other's pull requests before the merge manager merges them. This catches issues early and spreads knowledge across the team. You can set this up by telling agents to request reviews from specific teammates, or let the merge manager coordinate.

**Handoffs.** When one agent's work depends on another's — say the frontend needs an API the backend is building — they coordinate through Mattermost. The backend agent posts when the API is ready, the frontend agent picks it up. You can facilitate these handoffs by telling both agents about the dependency, or let them work it out once they know the plan.

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
