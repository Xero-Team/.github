# Xero-Team CI/CD Guide

This repository (`Xero-Team/.github`) hosts the organization's **reusable GitHub Actions workflows** and **starter workflow templates**. Any repo in the org can opt in by adding a thin caller workflow — no per-repo CI code to maintain.

## What's available

| Reusable workflow | Path (in this repo) | Purpose |
|---|---|---|
| `ci.yml` | `.github/workflows/ci.yml` | Polyglot CI: auto-detects Python/Rust/Node/Go/C++/Docker and runs build + test + lint |
| `codeql.yml` | `.github/workflows/codeql.yml` | GitHub CodeQL security analysis with auto language detection |
| `release.yml` | `.github/workflows/release.yml` | Tag-driven GitHub Release with auto-generated notes (+ optional artifacts) |

Starter templates (shown when creating a new repo in the org) live under `workflow-templates/`:
`xero-ci`, `xero-codeql`, `xero-release` — each a thin caller of the reusable workflow above.

## Opting a repo into CI

Drop this into `<repo>/.github/workflows/ci.yml`:

```yaml
name: CI
on: [push, pull_request]
permissions:
  contents: read
jobs:
  ci:
    uses: Xero-Team/.github/.github/workflows/ci.yml@main
    secrets: inherit
```

> **Mind the doubled `.github` path:** `Xero-Team/.github` is the *repository* name; `.github/workflows/ci.yml` is the *directory* inside it. The full `uses:` string is `Xero-Team/.github/.github/workflows/ci.yml@main`.

> **`secrets: inherit`** passes the calling repo's repository-level secrets down. This org does not configure org-level secrets, so each repo wires its own (e.g. `PYPI_TOKEN`) under *Settings → Secrets and variables → Actions*.

### Pinning

`@main` tracks the latest version. For reproducibility, pin to a tag (e.g. `@v1`) or a commit SHA after this repo starts tagging releases.

## How `ci.yml` detects languages

A single `detect` job runs `git ls-files` and greps for manifests, then sets boolean outputs. Each downstream language job runs only if its manifest is present:

| Language | Triggered by manifest(s) | What it runs |
|---|---|---|
| Python | `pyproject.toml` / `setup.py` / `setup.cfg` / `requirements*.txt` / `Pipfile` / `poetry.lock` | uv or pip install → `ruff check` (non-fatal) → `pytest` |
| Rust | `Cargo.toml` | `cargo fmt --check` (non-fatal) → `cargo clippy -D warnings` (non-fatal) → `cargo test` |
| Node | `package.json` | `npm ci` → `npm run build` → `npm test` → `npm run lint` (if present) |
| Go | `go.mod` | `go build ./...` → `go test ./...` |
| C/C++ | `CMakeLists.txt` / `Makefile` | cmake or make build → `ctest` (if cmake) |
| Docker | `Dockerfile` | hadolint (non-fatal) → `docker build` (no push) |

### `ci.yml` inputs

| Input | Type | Default | Meaning |
|---|---|---|---|
| `run-tests` | boolean | `true` | Set `false` for docs-only or build-only repos to skip test jobs |

Pass it from a caller:

```yaml
jobs:
  ci:
    uses: Xero-Team/.github/.github/workflows/ci.yml@main
    with:
      run-tests: false
    secrets: inherit
```

## CodeQL

```yaml
name: CodeQL
on:
  push: { branches: [main] }
  pull_request:
  schedule:
    - cron: "17 4 * * 1"
permissions:
  contents: read
  security-events: write
  actions: read
jobs:
  codeql:
    uses: Xero-Team/.github/.github/workflows/codeql.yml@main
    secrets: inherit
```

Override detected languages with the `languages` input (comma-separated, e.g. `python,javascript-typescript,go,rust,c-cpp`); default `auto` detects from manifests.

## Releases

Tag a commit `v*` (e.g. `v1.2.3`) and push the tag:

```bash
git tag v1.2.3 && git push --tags
```

Drop this caller to enable auto-release (the `workflow-templates/xero-release.yml` starter does this for new repos):

```yaml
name: Release
on:
  push:
    tags: ["v*"]
permissions:
  contents: write
jobs:
  release:
    uses: Xero-Team/.github/.github/workflows/release.yml@main
    with:
      generate_artifacts: false   # set true to build cargo/npm/go artifacts into the release
    secrets: inherit
```

The release uses `generate_release_notes: true`, so GitHub auto-generates notes from PRs/commits since the last release.

## FAQ

**My repo has multiple languages (e.g. Python + JS).** `ci.yml` runs every matching language job in parallel — nothing to configure.

**My repo is docs-only / has no manifests.** `detect` outputs all `false`; all downstream jobs are skipped; the run ends green. No CI noise.

**Archived repos.** Don't add CI to archived repositories. Archived repos don't run new workflow runs anyway.

**Lint steps are `continue-on-error: true`.** Lint (ruff, cargo fmt, clippy, hadolint) is advisory — it won't fail the run on existing repos, so opting in never breaks you on day one. Tighten later by forking the reusable workflow or by adding your own strict lint caller.

**CODEOWNERS.** Not supported as an org-wide default. Add per-repo `.github/CODEOWNERS` (e.g. `* @YUZHEthefool`) where you want required reviews.

**Org secrets / 2FA / sign-off.** Not configured at the org level in this setup. Configure per repository under its Settings, or contact an org admin.
