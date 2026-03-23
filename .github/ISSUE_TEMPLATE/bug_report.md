---
name: Bug Report
about: Report something that isn't working as expected
title: "[Bug] "
labels: bug
assignees: ''
---

## What happened?

A clear description of what went wrong. Include any error messages you saw — exact text is more helpful than paraphrasing.

## Steps to reproduce

1. Start MASON with `./scripts/masonctl start`
2. ...
3. ...
4. See error

The more specific you can be, the faster we can track it down.

## Expected behavior

What you expected to happen instead.

## Screenshots

If applicable, add screenshots or screen recordings of the issue. These are especially helpful for:
- UI problems in the dashboard or Mattermost
- Browser console errors (right-click → Inspect → Console tab)
- Unexpected agent behavior in chat

> **Tip:** On macOS, press Cmd+Shift+5 to capture a screenshot or screen recording.

## Environment

Please fill in all of these — issues often come down to environment differences.

- **OS and version**: (e.g., macOS 15.1, Ubuntu 24.04, Windows 11 23H2)
- **Architecture**: (e.g., Apple Silicon / ARM64, Intel / x86_64)
- **Docker version**: (run `docker --version`)
- **Docker runtime**: (e.g., Docker Desktop, OrbStack, Podman, Rancher Desktop)
- **MASON version**: (run `./scripts/masonctl status` or check the image tag)
- **Browser** (if UI issue): (e.g., Chrome 120, Safari 18, Firefox 130)

## Logs

Paste relevant output from `./scripts/masonctl logs --tail=100` below. If the output is long, attach it as a file instead.

```
Paste logs here
```

## Additional context

Anything else that might help — custom configuration, whether it worked before, unusual setup, etc.
