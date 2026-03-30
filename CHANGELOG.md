<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Changelog

All notable changes to MASON will be documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.4] - 2026-03-30

### Changed
- Add auto-generated changelog to release pipeline
- Fix: rename GITHUB_PAT to GH_PAT in workflow (Forgejo blocks GITHUB_ prefix)
- Add automated CI/CD release pipeline

## [1.3.1] - 2026-03-30

### Fixed
- **`untrust-cert` reliability** — Now scans keychain for Mason certificates instead of assuming a fixed name
- **`rm --data` cleanup** — Removes trusted certificates when deleting Mason data
- **Help text formatting** — Aligned help output for trust-cert commands

## [1.3.0] - 2026-03-30

### Added
- **`masonctl trust-cert`** — Adds MASON's self-signed TLS certificate to your system trust store, eliminating browser security warnings. Works on macOS (System Keychain) and Linux (Debian/Ubuntu + RHEL/Fedora). Idempotent — safe to re-run after regenerating certificates.
- **`masonctl untrust-cert`** — Removes MASON's certificate from the system trust store. Clean reversal of `trust-cert`, also idempotent.
- **TLS cert warning on start** — `masonctl start` now displays a note about the browser certificate warning with instructions to proceed and a pointer to `masonctl trust-cert`.

### Changed
- **Certificate CN** — Self-signed certificate common name changed from `localhost` to `MASON Local` for precise identification in the system keychain.

## [1.2.2] - 2026-03-28

### Fixed
- **Agent restart reliability** — Agents now resume existing sessions (`-c` flag) instead of starting fresh on container restart
- **Home directory persistence** — `/home/mason` data (credentials, settings) survives container restarts
- **Permission handling** — Claude bypass permissions written to correct settings file
- **Daemon stability** — Fixed false-positive busy detection and stale notification state

### Changed
- **Pinned ditops-memory dependency** to `<=0.2.6` for compatibility

## [1.2.0] - 2026-03-27

### Changed
- **masonctl safety prompts** — All destructive commands (stop, restart, rebuild, update, rm) now prompt for y/N confirmation before executing. Pass `--yes` or `-y` to skip for scripting.
- **Mattermost upgraded to 11.5.1** (was 9.11.0)
- **Forgejo upgraded to 14.0.3** (was 9.0.0)

## [1.1.0] - 2026-03-27

### Added
- Single-container simulation platform with AI agent teams
- Concierge (Connie) — interviews you and assembles a custom team
- Setup wizard — guided onboarding with Claude Code installation
- TLS encryption and token authentication for all services
- Mattermost for team chat and collaboration
- Forgejo for Git repositories and code review
- Agent memory system with shared knowledge and context
- Agent directory service for team discovery and coordination
- tmux-based agent terminals with live observation
- `masonctl` CLI for container management
- `masonctl verify` for file integrity checks
- `/error-report` command for diagnostic report generation
- Interactive API documentation via Swagger UI
- Docker Compose support as an alternative to masonctl

---

*For the latest updates, visit [masonteams.com](https://masonteams.com).*
