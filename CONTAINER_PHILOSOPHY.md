<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Container Philosophy

The MASON Teams container is the only artifact we ship. It bundles a chat server, a git forge, a vector database, agent tooling, and the MASON web UI into one image. This page explains why it's shaped the way it is, what we promise about the contents, and what we deliberately don't.

A sanitized reference of the build is in [`Dockerfile.reference`](./Dockerfile.reference).

## Why a container

The bar we set for ourselves is one command from "I want to try this" to "agents are talking." Everything else flows from that.

A container is the smallest unit that makes "one command" possible across macOS, Linux, and Windows hosts. It pins versions, isolates state, gives us a single rollback point, and lets us assume the runtime environment is exactly what we built against — no "works on my machine" failures from a slightly different Python, a missing libsecret, or a stale Node. Reinstalling MASON Teams is a `docker rm` away.

We treat the container as the product, not a packaging convenience. Everything we change is delivered through it.

## Why "bundled" services

MASON Teams runs Mattermost, Forgejo, ttyd, Qdrant, PostgreSQL, and our own services side-by-side in a single image. That's intentional, even though it makes the image larger than people sometimes expect.

The alternative — five separate containers wired together with a Compose stack — would force users to think about networks, depends_on, health gates, persistent volumes per service, and version compatibility between services. Each one of those is a place a new user gets stuck.

We pay the cost of a larger image so users don't pay the cost of integrating services that aren't really separable for our use case. Agents need chat to coordinate, git to commit, memory to recall, and a terminal to work in. Splitting those across containers makes the user assemble what we should be delivering.

This is also why we don't expose pluggability for the bundled services. You can't swap our Mattermost for Slack or our Forgejo for GitHub — the agents are wired to the bundle, and breaking that boundary is a separate, larger project.

## Ephemeral container, persistent volumes

The container itself is meant to be disposable. The volume isn't.

Everything stateful — Mattermost data, Forgejo repos, agent workspaces, generated credentials, the PostgreSQL cluster, the Qdrant vector store — lives under a single `/data` mount. When you `docker rm` the container and `docker run` a fresh one with the same volume, you pick up exactly where you left off. When you `docker rm -v`, you start clean.

That split is deliberate. The container is for the runtime: binaries, system packages, tmux config, fonts, the entrypoint script. The volume is for what you've built: messages, repos, agent state, memory. We can ship a new image without anyone's data migrating; you can wipe state without reinstalling.

If you're operating MASON Teams long-term, treat the volume as the only thing worth backing up.

## Agent-first design

MASON Teams is built so its agents are first-class operators of their own environment. Every workflow that exists for a human also exists as something an agent can execute: there are no GUI-only paths to managing the container, no manual steps wedged into otherwise automated flows.

Concretely, that means:

- The container exposes CLI tools (not just an API) for every lifecycle operation an agent or operator needs to perform.
- Configuration is files, not forms. Agents read and write files reliably; that's our most universal automation primitive.
- The web UI is a view onto the same state that agents manipulate. It never holds state the CLI can't reach.

This is the principle that constrains a lot of design choices that might otherwise drift toward "easier for the human, harder for the agent."

## Security boundaries

The container is designed as a **single-user, localhost-only** development environment. The threat model assumes the host is trusted and the container runs on Docker Desktop. Details on the auth and exposure model live in [SECURITY.md](./SECURITY.md); this section covers what the *container* contributes.

- **Non-root by default.** Every process inside the container runs as the unprivileged `mason` user (UID 1000). The image does not run as root.
- **`sudo` inside the container is intentional.** The `mason` user has passwordless sudo so agents can install packages they discover they need during a session. The container is ephemeral; that compromise of least-privilege is bounded by the container lifetime and by the host-only network exposure.
- **Three published ports.** Only `8080` (web UI), `8065` (Mattermost), and `3000` (Forgejo) are EXPOSEd. The terminal proxy, the vector database, and the daemon's control plane stay internal to the container.
- **Build provenance.** Since v1.5.x, every published image on Docker Hub ships with SBOM (Software Bill of Materials) and provenance attestations. You can audit what's in any tag with `docker buildx imagetools inspect masonteams/mason-teams:<tag> --format '{{json .Provenance}}'`.
- **Tini as PID 1.** Signal forwarding and zombie reaping are handled by `tini`, not bash. `docker stop` gives every internal service the chance to shut down cleanly.
- **Integrity manifest.** The image ships with a manifest of every binary it installs, generated at build time. The `mason-verify` tool inside the container compares the installed files against that manifest at startup — tampering with the image fails the verification step.

## What we promise, what we don't

This is the section that matters most if you're depending on MASON Teams for anything beyond exploration. We're still in beta. Read this before you build a workflow that breaks when we ship.

**What's stable across minor releases:**

- **Published port numbers** (`8080`, `8065`, `3000`). If we change these, it's a major-version event.
- **The `/data` volume layout at the top level** (`/data/mattermost`, `/data/forgejo`, `/data/agents`, etc.). Subtree contents may change as the underlying services upgrade.
- **The `masonctl start`-level interface** — the user-facing CLI commands, their flags, their exit codes. We treat these like a public API.
- **The dashboard auth model** (token at `~/.mason/token`, `mason_token` cookie). Documented in [SECURITY.md](./SECURITY.md); not silently changed.
- **The image tag scheme.** `masonteams/mason-teams:stable` always points at the latest production release. `:vX.Y.Z` tags are immutable.

**What's deliberately not stable and will change without ceremony:**

- **The internal binary layout.** Which Go binaries we build, what they're named, which paths they live at inside `/usr/local/bin`. If you script against the container's internals, expect breakage.
- **The persona, skill, and template content** baked into the image. These are how agents behave; we tune them constantly. They are not an API.
- **Service versions** (Mattermost, Forgejo, Qdrant, Node, Python). We pin and upgrade these as part of routine maintenance.
- **The internal port assignments** for non-EXPOSE'd services (Qdrant on 6333, daemon on 9090, ttyd on 7681). Don't rely on these from outside the container.
- **Anything inside the container's filesystem outside `/data`.** Treat the image as opaque except for what's documented.

**The general principle:** if you can reach it without `docker exec`, we treat it as an interface. If you have to `docker exec` to find it, treat it as an implementation detail.

If something on the "stable" list needs to change, it goes through a deprecation cycle with a `Deprecation` HTTP header (where applicable) and a CHANGELOG entry one minor version ahead of the actual change. If something on the "not stable" list breaks for you, please [open an issue](https://github.com/Mason-Teams/mason-teams/issues) — we want to know, but the fix may be "your workflow shouldn't have depended on that."

## Further reading

- [Dockerfile.reference](./Dockerfile.reference) — sanitized reference of the actual build
- [SECURITY.md](./SECURITY.md) — auth, exposure model, token handling
- [CONFIGURATION.md](./CONFIGURATION.md) — environment variables and runtime options
- [WORKFLOWS.md](./WORKFLOWS.md) — what agents actually do inside the container
