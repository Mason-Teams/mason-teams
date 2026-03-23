<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Security

MASON is designed as a **single-user, localhost-only** development environment. Security controls protect against accidental exposure and unauthorized local access. The threat model assumes the host machine is trusted and the container runs on Docker Desktop.

## Authentication

### Dashboard

The dashboard requires token-based authentication.

- A 32-character hex token is generated on first `masonctl start` and stored at `~/.mason/token` (mode 0600)
- The token is mounted read-only into the container
- Authenticate via the login page or programmatically with `POST /api/auth/login`
- Sessions use an HTTP-only cookie (`mason_token`) with 30-day expiry, SameSite=Lax

**Retrieving your token:**

```bash
masonctl token
# or directly:
cat ~/.mason/token
```

**Public paths (no auth required):**

- `/health` — health check
- `/setup`, `/api/setup/*` — first-run setup wizard
- `/static/*` — CSS/JS/images
- `/guide` — getting started page
- `/docs/*` — documentation
- `/api/auth/login` — login endpoint
- `/api/status/*` — service status

### Mattermost & Forgejo

These bundled services use their own authentication:

- **Mattermost**: Username/password created during setup wizard
- **Forgejo**: Username/password created during setup wizard
- Both run on localhost-only ports, not exposed to the network by default

## TLS

MASON generates a self-signed TLS certificate on first start:

| Property | Value |
|----------|-------|
| Certificate | `~/.mason/tls/mason.crt` |
| Private key | `~/.mason/tls/mason.key` (mode 0600) |
| Subject | `CN=localhost, O=MASON` |
| SAN | `DNS:localhost, IP:127.0.0.1` |
| Validity | 365 days |
| Algorithm | RSA 2048 |

Browsers will show a self-signed certificate warning on first visit — this is expected for localhost development.

**Replacing with a custom certificate:** Place your own `mason.crt` and `mason.key` in `~/.mason/tls/` before starting the container.

**Renewal:** Delete `~/.mason/tls/` and restart — `masonctl start` regenerates automatically.

## Network Exposure

### Exposed Ports

Only three ports are mapped from the container to the host:

| Port | Service | Protocol | Auth |
|------|---------|----------|------|
| 8080 | Dashboard | HTTPS (TLS) | Token required |
| 8065 | Mattermost | HTTP | MM login required |
| 3000 | Forgejo | HTTP | Forgejo login required |

All other internal services bind to `127.0.0.1` inside the container and are **not accessible from the host**.

### Adding TLS to Mattermost & Forgejo

Mattermost and Forgejo serve HTTP by default. For HTTPS:

- Use a reverse proxy (nginx, caddy, traefik) on the host
- Or configure their native TLS settings via their admin UIs
- For localhost-only use, HTTP is acceptable

## File Permissions

| File | Mode | Purpose |
|------|------|---------|
| `~/.mason/token` | 0600 | API authentication token |
| `~/.mason/tls/mason.key` | 0600 | TLS private key |
| `~/.mason/tls/mason.crt` | 0644 | TLS certificate (public) |

The `~/.mason/` directory is mounted **read-only** into the container, preventing the container from modifying host credentials.

## Token Management

| Event | What Happens |
|-------|--------------|
| First `masonctl start` | Token generated, stored at `~/.mason/token` |
| Subsequent starts | Existing token reused |
| Token rotation | Delete `~/.mason/token`, restart container |
| Container rebuild | Token persists (lives on host, not in image) |
| `masonctl rm --data` | Token persists (not in data volume) |
| Full reset | Delete `~/.mason/` directory |

## Trust Model

| Boundary | Trust Level |
|----------|-------------|
| Host machine | Fully trusted |
| Container | Semi-trusted (can read token, can't modify host files) |
| Network | Untrusted beyond localhost |
| User | Single-user (no multi-tenancy, no RBAC) |

## Recommendations for Networked Environments

MASON is designed for local development. If deploying in a shared or networked environment:

1. Replace self-signed certs with CA-signed certificates
2. Configure TLS for Mattermost and Forgejo
3. Use a firewall to restrict port access
4. Rotate tokens regularly
5. Consider adding RBAC if multiple users need access
6. Enable Mattermost/Forgejo audit logging

## Reporting Security Issues

If you discover a security vulnerability, please email [security@masonteams.com](mailto:security@masonteams.com) rather than opening a public issue. We take security reports seriously and will respond promptly.
