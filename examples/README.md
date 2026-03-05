<!--
Copyright (c) 2025 Human Loop Ventures, LLC. All rights reserved.
Use of this source code is governed by the Business Source License 1.1
included in the LICENSE file at the root of this repository.
-->

# Examples

Optional reference files for users who want more control over their MASON setup.

**You probably don't need these.** The recommended way to run MASON is with `./scripts/masonctl start` — it handles everything for you. These files are here if you prefer Docker Compose workflows or want to see all the configurable settings in one place.

## What's Here

| File | What it's for |
|------|--------------|
| **docker-compose.yaml** | Run MASON via Docker Compose instead of masonctl |
| **.env.example** | Reference for all configurable environment variables |

## Docker Compose

If you prefer `docker compose up` over `masonctl`:

```bash
cd examples
cp .env.example .env    # optional — edit to customize
docker compose up -d
```

This starts the same single MASON container with the same defaults as masonctl. The compose file just gives you a declarative way to manage it.

## Environment Variables

The `.env.example` file documents every setting you can tweak — ports, resource limits, image version, and data volume. Copy it to `.env` and change only what you need. The defaults work for most setups.

## When to Use These

- You already have a Docker Compose workflow and want MASON to fit into it
- You want to pin a specific image version or customize resource limits
- You're curious what's configurable under the hood
