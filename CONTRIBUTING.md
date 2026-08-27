# Contributing to Xero-Team

Thanks for your interest in contributing! This guide applies to all repositories under the **Xero-Team** organization. A repository may override this file with its own `CONTRIBUTING.md`.

## Before you start

- Search existing issues and pull requests to avoid duplicates.
- For large changes, open an issue or discussion first to align on the approach.

## Development workflow

1. Fork the repository (or create a branch if you have write access).
2. Create a feature branch off `main`: `git switch -c my-change`.
3. Make your changes, keeping commits focused.
4. **Enable CI** — drop the org reusable CI into the repo if it isn't there yet (see the [CI/CD guide](https://github.com/Xero-Team/.github/blob/main/docs/ci-cd.md)):
   ```yaml
   # .github/workflows/ci.yml
   name: CI
   on: [push, pull_request]
   permissions:
     contents: read
   jobs:
     ci:
       uses: Xero-Team/.github/.github/workflows/ci.yml@main
       secrets: inherit
   ```
5. Ensure CI is green locally and in the Actions tab.
6. Open a pull request against `main` and fill in the PR template.

## Code style

- Follow the existing style in the repository (each repo may pin a formatter / linter).
- Keep diffs minimal and reviewable.
- Don't commit secrets or credentials.

## Commit messages

- Use a short, imperative summary line (≤ ~72 chars).
- Reference issues where relevant (`Closes #N`).

## Reporting security issues

Do **not** open a public issue. See [SECURITY.md](./SECURITY.md).

## Code of conduct

Participation in this project is governed by the [Code of Conduct](./CODE_OF_CONDUCT.md). By contributing you agree to uphold it.
