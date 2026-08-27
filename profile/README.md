# Xero-Team

**A small R&D collective building three things.** Most other repos in this org are research / reference forks we study — the active work is focused on the three pillars below.

> This `Xero-Team/.github` repo is the organization's shared foundation: the **org profile** you're reading, **org-wide default community health files** (issue/PR templates, contributing guide, security policy, code of conduct), and a **reusable CI/CD workflow library** any repo here can opt into.

---

## 🔧 zerolang — a programming language & compiler

A from-scratch programming language, "zerolang", with a compiler, shared testcases, and editor tooling.

| Repo | What it is |
|---|---|
| [CZC](https://github.com/Xero-Team/CZC) | The zerolang compiler, implemented in C/C++. |
| [Zero-Lang-Testcases](https://github.com/Xero-Team/Zero-Lang-Testcases) | Shared, text-based testcases for zerolang. |
| [Zero-Lang-Highlight-Vsix](https://github.com/Xero-Team/Zero-Lang-Highlight-Vsix) | `.zero` syntax-highlighting extension for VS Code. |

<details><summary>Historical / archived</summary>

- [Zero-Compiler-OLD](https://github.com/Xero-Team/Zero-Compiler-OLD) 🗄️ — earlier zerolang compiler in Rust.
- [Zero-Lang-Spec-Old](https://github.com/Xero-Team/Zero-Lang-Spec-Old) 🗄️ — earlier language spec.
- [CZC-old](https://github.com/Xero-Team/CZC-old) 🗄️ — earlier C/C++ compiler iteration.
- [Zero-Lang-Error-Msg-Old](https://github.com/Xero-Team/Zero-Lang-Error-Msg-Old) 🗄️ — earlier error-message work.

</details>

## 🤖 AstrBot & AI — agent frameworks and tooling

| Repo | What it is |
|---|---|
| [AstrBot](https://github.com/Xero-Team/AstrBot) | Core AI agent assistant & dev framework — integrates IM platforms, LLMs, plugins & AI features. |
| [astrbot_plugin_image_generation](https://github.com/Xero-Team/astrbot_plugin_image_generation) | AstrBot plugin — 通用图像生成. |
| [astrbot_plugin_oneroll](https://github.com/Xero-Team/astrbot_plugin_oneroll) | AstrBot plugin. |
| [codexmcp](https://github.com/Xero-Team/codexmcp) | Enable Claude Code ↔ Codex collaboration — single agent → multi-agent. |
| [hermes-agent](https://github.com/Xero-Team/hermes-agent) | "The agent that grows with you." |

> [Zephirum](https://github.com/Xero-Team/Zephirum) holds an AstrBot-derived framework — a research/reference fork, not a separate product line.

## 📄 zpdf — PDF parsing in Rust

| Repo | What it is |
|---|---|
| [zpdf](https://github.com/Xero-Team/zpdf) | A PDF parsing library written in pure Rust. |
| [zpdf-skill](https://github.com/Xero-Team/zpdf-skill) | zpdf skill bindings. |

---

### About the other repos

The organization also hosts a number of **research / reference** repositories — music & audio tooling, ephemeris & astronomy libraries (e.g. swisseph, anise), calendar & web apps, and others. These are studied or forked for learning, not active product lines, and aren't enumerated here.

---

## CI/CD — shared workflow library

This repo ships **reusable GitHub Actions** that any Xero-Team repo can call. Opt a repo into CI by dropping this into `<repo>/.github/workflows/ci.yml`:

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

The reusable `ci.yml` **auto-detects the repo's stack** (Python / Rust / Node / Go / C++ / Docker) and runs build + test + lint for each detected language. Also available: `codeql.yml` (security scanning) and `release.yml` (tag-driven releases).

➡️ **Full usage guide:** [`docs/ci-cd.md`](https://github.com/Xero-Team/.github/blob/main/docs/ci-cd.md)

> ⚠ Mind the doubled `.github` path: `Xero-Team/.github` is the *repo name*, `.github/workflows/ci.yml` is the *directory inside it*.

New repos can pick a starter template (**Xero CI / Xero CodeQL / Xero Release**) at repository-creation time.

---

## Contributing & community

Org-wide **default community health files** (in this repo) are used as fallback by every Xero-Team repo that lacks its own:

- [Issue templates](https://github.com/Xero-Team/.github/tree/main/ISSUE_TEMPLATE) — bug report, feature request
- [CONTRIBUTING.md](https://github.com/Xero-Team/.github/blob/main/CONTRIBUTING.md)
- [CODE_OF_CONDUCT.md](https://github.com/Xero-Team/.github/blob/main/CODE_OF_CONDUCT.md) — Contributor Covenant 2.1
- [PR template](https://github.com/Xero-Team/.github/blob/main/PULL_REQUEST_TEMPLATE.md)

## Security

Found a vulnerability? **Do not open a public issue.** See [`SECURITY.md`](https://github.com/Xero-Team/.github/blob/main/SECURITY.md) — report privately via GitHub Security Advisory or `thefoolyuzhe@gmail.com`.

---

*This organization profile is maintained in the [`Xero-Team/.github`](https://github.com/Xero-Team/.github) repository.*
