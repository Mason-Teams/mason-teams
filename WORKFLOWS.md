<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
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

### How Agents Remember

MASON agents have a layered memory system that works behind the scenes. You don't need to manage it — but understanding it helps you get more from your team:

- **Project memory** — Each agent keeps notes about your project's architecture, conventions, and decisions. This persists across sessions.
- **Shared memory** — When one agent learns something useful (a gotcha, a workaround, a decision), it stores it where other agents can find it. Knowledge spreads across the team automatically.
- **Quick snapshots** — Agents periodically save their current state so they can recover if interrupted.

The practical effect: your agents get better at working on your project over time. They remember past decisions, avoid repeating mistakes, and share context without you having to repeat yourself.

### Scheduled and Recurring Tasks

Agents support a `/loop` command that runs any task on a timed interval. Tell an agent in Mattermost what you want repeated, and they'll set it up:

> "Hey Riley, run `/loop 5m /officehours`" — checks messages every 5 minutes
>
> "Run `/loop 30m` and check the deploy status each time" — polls every 30 minutes
>
> "Start a `/loop 10m` to run the test suite and report failures" — continuous integration

The default interval is 10 minutes if you don't specify one. The agent keeps running the loop in the background, and the MASON daemon can still deliver new messages to the agent — so you can continue chatting, give new instructions, or redirect the agent without interrupting the loop.

**Common uses:**
- **Monitoring** — Periodically check a service, deployment, or build pipeline
- **Polling** — Watch for changes in a repo, API, or external system
- **Recurring maintenance** — Run tests, lint checks, or health checks on a schedule

To stop a loop, just tell the agent to stop it in Mattermost.

### Agent Directory

MASON includes a built-in agent directory service that keeps track of every agent on the team. When Connie spawns an agent, it's automatically registered in the directory with its name, role, skills, status, and a unique identifier.

**What it's for:**
- **Discovery** — Agents can look up who's on the team and what they do, which helps them route questions to the right person
- **Status tracking** — The directory tracks whether an agent is online, busy, or offline, so other agents know who's available
- **Coordination** — Agents use the directory to find teammates by skill or role when they need help with something specific

**Agent email addresses:** Each agent gets an identifier in the format `name@mason.local` (e.g., `connie@mason.local`). These are **not real email addresses** — they're just unique identifiers used internally for directory lookups and agent-to-agent messaging within the container. No actual email is sent or received.

You don't need to interact with the directory directly — it runs in the background. But if you're curious about your team roster, ask Connie and she can list who's registered and their current status.

---

## 3. The Container and You

*MASON runs in a single container. Here's how to customize it, connect it to your environment, and get things in and out.*

### Default Ports

MASON exposes several ports out of the box:

| Port | Service | Purpose |
|------|---------|---------|
| 8080 | Web UI | Setup wizard and dashboard (HTTPS, token auth) |
| 8065 | Mattermost | Team chat — where you talk to your agents (HTTPS via `masonctl`, HTTP via plain `docker run`) |
| 3000 | Forgejo | Git forge — repos, issues, pull requests (HTTPS via `masonctl`, HTTP via plain `docker run`) |

Additional ports are available for advanced use — see [CONFIGURATION.md](CONFIGURATION.md#advanced-ports) for details.

If your agents spin up additional services inside the container (a dev server, an API, etc.), you'll need to expose those ports. You can do this when starting the container:

```bash
# Expose an additional port for an agent's dev server
./scripts/masonctl start --port 3001:3001
```

Or pass port mappings directly to Docker:

```bash
docker run -p 8080:8080 -p 8065:8065 -p 3000:3000 -p 3001:3001 ...
```

### Volume Mounts

By default, MASON uses a Docker volume to persist data across container restarts. If you want agents to work on code that lives on your machine, mount your project directory into the container:

```bash
# Mount a local project directory
./scripts/masonctl start --volume /path/to/your/project:/workspace/project
```

Or with Docker directly:

```bash
docker run -v /path/to/your/project:/workspace/project ...
```

Common things to mount:
- **Your project code** — so agents can read and edit your files directly
- **SSH keys** — if agents need to push to external Git repos
- **Config files** — custom settings or environment files your project needs

### Networking

The container has full outbound internet access — agents can clone repos, call APIs, and download packages. For connecting to services on your host machine:

- **macOS/Windows**: Use `host.docker.internal` to reach services running on your host
- **Linux**: Use `--network host` or the host's IP address

```bash
# Example: agent connecting to a database on your machine
# Inside the container, use:
postgres://host.docker.internal:5432/mydb
```

For external services (cloud APIs, remote servers), agents can reach them directly — no special configuration needed.

### Persistence

**What survives a container restart:**
- All data in the Docker volume (agent workspaces, Mattermost messages, Forgejo repos, memory)
- Anything mounted from your host machine

**What doesn't survive `./scripts/masonctl rm --data`:**
- The `--data` flag deletes the Docker volume — this wipes everything
- Use `./scripts/masonctl rm` (without `--data`) to remove the container while keeping data

**Backing up:** Your simulation state lives in the Docker volume. To back it up:

```bash
# Find your volume
docker volume ls | grep mason

# Back it up
docker run --rm -v mason_data:/data -v $(pwd):/backup alpine tar czf /backup/mason-backup.tar.gz /data
```

### Resource Tuning

MASON recommends 16GB+ RAM. Each agent runs a Claude Code session, and they add up. Signs you need more resources:

- Agents responding slowly or timing out
- Mattermost or Forgejo becoming unresponsive
- Container OOM kills (check `./scripts/masonctl logs`)

To adjust resource limits, you'll need to use Docker directly (masonctl doesn't expose resource flags):

```bash
# Give MASON more memory and CPU
docker run --memory=24g --cpus=8 -p 8080:8080 -p 8065:8065 -p 3000:3000 ...
```

**Tips for larger teams:**
- Start with 3–5 agents and scale up as needed
- Monitor with `docker stats` to see resource usage per container
- If running many agents, consider giving the container at least 4GB per agent

### Common Recipes

**Mount a local project so agents can edit your code:**
```bash
./scripts/masonctl start --volume ~/my-project:/workspace/my-project
```

**Expose an agent's dev server to your browser:**
```bash
./scripts/masonctl start --port 3001:3001
# Then open http://localhost:3001 in your browser
```

**Run MASON with custom resource limits** (requires Docker directly — `masonctl` doesn't expose resource flags):
```bash
docker run --memory=32g --cpus=12 -p 8080:8080 -p 8065:8065 -p 3000:3000 ...
```

---

## 4. Advanced — Mixed Control

*Terminal commands and Mattermost messages — two ways to talk to your team, each with strengths.*

### Terminal vs Mattermost — When to Use Which

- **Terminal** (direct Claude session) — Precise instructions, code-heavy work, debugging, when you need control
- **Mattermost** (chat) — Broad direction, team-wide announcements, async work, when you want to coordinate across agents
- **Mixing both** — Using terminal for the agent you're paired with, MM for the rest of the team

### When to Use Which

A general rule of thumb:

- **Terminal** when you want precision — debugging a specific issue, reviewing code, walking through a complex task step by step
- **Mattermost** when you want breadth — giving the team a direction, checking in on progress, letting agents coordinate on their own
- **Both** when you're actively working — terminal for your focus agent, Mattermost for the rest of the team

### Watching Agents Work

Every agent runs in its own tmux session inside the container. You can drop in at any time to watch them think, code, and collaborate in real time.

**Get into the container** (`masonctl` names the container `mason`):

```bash
docker exec -it mason bash
```

**List all agent sessions:**

```bash
tmux ls
```

You'll see sessions like `mason-connie`, `mason-kenji`, `mason-riley`, etc. — one per agent.

**Attach to a session to watch an agent:**

```bash
tmux a -t mason-connie
```

You're now watching Connie's terminal live. You'll see Claude Code running, the agent thinking, reading files, writing code, sending messages — everything in real time.

**Detach without stopping the agent:** Press `Ctrl+B` then `D`. This puts you back in the container shell. The agent keeps working.

**Switch between agents quickly:**

```bash
# Detach from current, attach to another
tmux a -t mason-kenji
```

**View multiple agents at once** (if you're familiar with tmux):

```bash
# From inside tmux, press Ctrl+B then S to see all sessions
# Use arrow keys to switch between them
```

**Exit the container** when you're done watching:

```bash
exit
```

> **Tip:** You can also access the container terminal from your browser via ttyd at `http://localhost:7681` — no `docker exec` required.

> **Note:** Watching is read-only observation. You won't interfere with the agent's work by attaching to their session. To actually communicate with agents, use Mattermost.

---

*More workflows and tips will be added in future releases.*
