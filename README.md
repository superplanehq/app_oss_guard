# OSS Guard

[![Launch in SuperPlane](https://superplane.com/badges/launch-in-superplane.svg)](https://app.superplane.com/install?repo=github.com/superplanehq/app_oss_guard)

Check **open-source license compatibility** on every pull request — run [osv-scanner](https://google.github.io/osv-scanner/) against the PR head, publish commit statuses, comment on failures, and track dependency license statistics in the console.

Built with [SuperPlane](https://superplane.com).

## How it works

### PR check flow

1. **On pull request** — listen for `opened`, `synchronize`, and `reopened` events on a selected repository
2. **Run license check** — clone the PR head, detect the project license from `LICENSE`, `package.json`, or `pyproject.toml`, then run `osv-scanner --licenses` recursively across lockfiles
3. **Enforce** — publish an **OSS Guard** commit status (success or failure) and comment on the PR when dependencies use unapproved licenses

### Setup and main scan flow

1. **Setup** — save the repository (`owner/repo`) to canvas memory
2. **Scan main** — read the saved repository, clone `main`, and run the same license scan as the PR flow
3. **Record stats** — populate the console dashboard from `main`, including a list of unapproved dependencies (no commit statuses or PR comments)

### Console

- **Latest scan** and **Licenses in use** reflect the latest **Scan main** run on `main`, not individual PR branches
- **Unapproved dependencies** lists packages on `main` that fail the license check

## Prerequisites

- [SuperPlane](https://superplane.com) account
- GitHub integration connected to the target repository
- A project license declared in the repository (`LICENSE`, `package.json`, or `pyproject.toml`)

## Setup

1. **Add a LICENSE file** (or set `license` in `package.json` / `pyproject.toml`) in your repository — OSS Guard reads the project license from source code.
2. **Connect GitHub** — bind a GitHub integration on the canvas nodes, then select the repository on **On Pull Request**.
3. **Run Setup** — enter the repository as `owner/repo` (for example, `superplanehq/superplane`).
4. **Run Scan main** — scan `main` and populate the console.
5. **Optional:** add a `GITHUB_TOKEN` secret on **Run setup scan** and **Run license check** for private repositories. Add dependency exceptions in `osv-scanner.toml` at the repository root (applies repo-wide).

## `GITHUB_TOKEN` secret

Add a secret named `GITHUB_TOKEN` on the **Run setup scan** and **Run license check** nodes. It is used to clone private repositories.

- **Private repositories:** required
- **Public repositories:** optional

### Fine-grained personal access token (recommended)

- **Repository access:** Only select repositories → choose the target repository
- **Repository permissions:**
  - **Contents:** Read
  - **Metadata:** Read

## License

MIT
