# OSS Guard

[![Launch in SuperPlane](http://superplane.com/badges/launch-in-superplane.svg)](https://app.superplane.com/install?repo=github.com/superplanehq/app_oss_guard)

Check **open-source license compatibility** on every pull request — run [license_finder](https://github.com/pivotal/LicenseFinder) against the PR head, publish commit statuses, comment on failures, and track dependency license statistics in the console.

Built with [SuperPlane](https://superplane.com).

## How it works

1. **On pull request** — listen for `opened`, `synchronize`, and `reopened` events on a selected repository
2. **Read permitted licenses** — load any additional dependency licenses you have configured in the console
3. **Run license check** — clone the PR head, detect the project license from `LICENSE`, `package.json`, or `pyproject.toml`, then run `license_finder` against dependencies
4. **Record stats** — save per-license counts and the latest scan summary to the `ossGuardLicenseStats` and `ossGuardScanSummary` memory namespaces
5. **Enforce** — publish an **OSS Guard** commit status (success or failure) and comment on the PR when dependencies use unapproved licenses
6. **Console** — totals, a license breakdown chart, scan history, and panels to add or remove additional permitted dependency licenses

## Prerequisites

- [SuperPlane](https://superplane.com) account
- GitHub integration connected to the target repository
- A project license declared in the repository (`LICENSE`, `package.json`, or `pyproject.toml`)

## Setup

1. **Add a LICENSE file** (or set `license` in `package.json` / `pyproject.toml`) in your repository — OSS Guard reads the project license from source code.
2. **Connect GitHub** — bind a GitHub integration on the canvas nodes, then select the repository on **On Pull Request**.
3. **Optional:** add extra permitted dependency licenses in the console, or add a `GITHUB_TOKEN` secret on **Run license check** for private repositories.

## `GITHUB_TOKEN` secret

Add a secret named `GITHUB_TOKEN` on the **Run license check** node. It is used to clone private repositories at the PR head SHA.

- **Private repositories:** required
- **Public repositories:** optional

### Fine-grained personal access token (recommended)

- **Repository access:** Only select repositories → choose the target repository
- **Repository permissions:**
  - **Contents:** Read
  - **Metadata:** Read

## License

MIT
