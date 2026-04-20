<!--
Copyright (c) 2025-2026 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Security

MASON Teams is designed as a **single-user, localhost-only** development environment. Security controls protect against accidental exposure and unauthorized local access. The threat model assumes the host machine is trusted and the container runs on Docker Desktop.

## Authentication

### Dashboard

The dashboard requires token-based authentication.

- A 32-character hex token is generated on first `./scripts/masonctl start` and stored at `~/.mason/token` (mode 0600)
- The token is mounted read-only into the container
- Authenticate via the login page or programmatically with `POST /api/auth/login`
- Sessions use an HTTP-only cookie (`mason_token`) with 30-day expiry, SameSite=Lax

**Retrieving your token:**

```bash
./scripts/masonctl token
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

MASON Teams generates a self-signed TLS certificate on first start:

| Property | Value |
|----------|-------|
| Certificate | `~/.mason/tls/mason.crt` |
| Private key | `~/.mason/tls/mason.key` (mode 0600) |
| Subject | `CN=MASON Local` |
| SAN | `DNS:localhost, IP:127.0.0.1` |
| Validity | 365 days |
| Algorithm | RSA 2048 |

Browsers will show a self-signed certificate warning on first visit — this is expected for localhost development.

**Trusting the certificate:** To eliminate the browser warning permanently, add MASON Teams' certificate to your system trust store:

```bash
./scripts/masonctl trust-cert
```

This works on macOS (adds to System Keychain) and Linux (Debian/Ubuntu + RHEL/Fedora system CA stores). You'll be prompted for your system password. Safe to re-run after regenerating certificates — any previous MASON Teams cert is automatically replaced.

To remove the certificate from your trust store:

```bash
./scripts/masonctl untrust-cert
```

`untrust-cert` scans your system keychain for all MASON Teams certificates and offers to remove them in one step. It works even if `~/.mason` has already been deleted — it scans the keychain directly rather than depending on the local certificate file.

> **Note:** You don't need to run `untrust-cert` separately before a full uninstall — `./scripts/masonctl rm --data` automatically checks for and removes any trusted MASON Teams certificates from your keychain.

**Replacing with a custom certificate:** Place your own `mason.crt` and `mason.key` in `~/.mason/tls/` before starting the container.

**Renewal:** Delete `~/.mason/tls/` and restart — `./scripts/masonctl start` regenerates automatically. If you previously ran `trust-cert`, run it again after renewal to trust the new certificate.

## Network Exposure

### Exposed Ports

Three primary ports are mapped from the container to the host by default:

| Port | Service | Protocol | Auth |
|------|---------|----------|------|
| 8080 | Web UI | HTTPS (TLS) | Token required |
| 8065 | Mattermost | HTTPS (TLS) | MM login required |
| 3000 | Forgejo | HTTPS (TLS) | Forgejo login required |

All three services share the same self-signed TLS certificate. When TLS certificates are present (`~/.mason/tls/`), all services run HTTPS automatically. Without certificates (e.g., plain `docker run`), all services fall back to HTTP.

All other internal services bind to `127.0.0.1` inside the container and are **not accessible from the host**.

> **Note:** If you're using `curl` against any MASON Teams service with self-signed certs, add the `-k` flag to skip certificate verification: `curl -k https://localhost:8080/...`

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
| `./scripts/masonctl rm --data` | Token removed (cleans up `~/.mason/` and trusted certs) |

## Trust Model

| Boundary | Trust Level |
|----------|-------------|
| Host machine | Fully trusted |
| Container | Semi-trusted (can read token, can't modify host files) |
| Network | Untrusted beyond localhost |
| User | Single-user (no multi-tenancy, no RBAC) |

## Recommendations for Networked Environments

MASON Teams is designed for local development. If deploying in a shared or networked environment:

1. Replace the self-signed certificate with CA-signed certificates for all services
2. Use a firewall to restrict port access
3. Rotate tokens regularly
4. Consider adding RBAC if multiple users need access
5. Enable Mattermost/Forgejo audit logging

## Reporting Security Issues

If you discover a security vulnerability, please email [security@masonteams.com](mailto:security@masonteams.com) rather than opening a public issue. We take security reports seriously and will respond promptly.
