# Agent Guidelines — OSS Guard

This file provides context for AI agents (SuperPlane built-in agent, Cursor, or any external agent) operating on this canvas.

## What this app does

OSS Guard checks open-source license compatibility on every pull request. It runs osv-scanner against the PR head, publishes commit statuses, and comments on failures. It also tracks dependency license statistics on the console.

## Flows

### PR check flow
```
On Pull Request → Run License Check (Python runner) → Has Violations?
  → (yes) Publish Failure Status → Comment on PR
  → (no) Publish Success Status
```

### Setup flow
```
Run Setup (manual) → Save Repository to Memory
```

### Main scan flow
```
Scan Main (manual) → Read Repository from Memory → Run License Scan (Python runner)
  → Save Stats to Memory → Console dashboard updates
```

## Memory

- `oss_guard_config` — stores the repository (`owner/repo`) set during setup
- `oss_guard_stats` — scan results from the main branch (licenses in use, unapproved deps)

## What's safe to change

- **License policy** — the Python scanner script determines which licenses are approved. Edit the runner node's script to change the policy.
- **Commit status context** — the `context` field on the commit status nodes (default: "OSS Guard")
- **PR comment format** — the `github.createIssueComment` node's body template

## What not to change

- **Memory namespaces** — the setup and scan flows depend on `oss_guard_config` and `oss_guard_stats`
- **Runner scripts** — these contain the osv-scanner logic. Understand the scan pipeline before modifying.

## Common issues

**Scanner fails on private repos:**
Add a `GITHUB_TOKEN` secret to the runner nodes. The token needs Contents:Read permission on the target repository.

**No project license detected:**
The scanner reads the license from `LICENSE`, `package.json`, or `pyproject.toml`. Make sure one of these exists in the repository.

**Setup not run:**
The console shows no data if the "Run Setup" and "Scan Main" manual triggers haven't been executed yet. Run Setup first (enter `owner/repo`), then Scan Main.
