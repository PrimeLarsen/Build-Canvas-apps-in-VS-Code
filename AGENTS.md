# AGENTS.md — Build Canvas Apps in VS Code

Operating manual for AI coding agents (GitHub Copilot, Copilot Chat, coding agents) working in this repository.

## What this repo is

A training/workshop repository that teaches Power Platform developers to build **canvas apps** and
**model-driven apps** in **Visual Studio Code** using the Power Platform CLI (PAC CLI), Git, GitHub
Actions, and GitHub Copilot. Content is documentation: `docs/` modules 00–09, `exercises/`,
`scripts/pac-cheatsheet.md`, plus repo automation in `.github/`.

## Layout

| Path | Contents |
|---|---|
| `README.md` | Workshop overview, learning goals, agenda, repo structure, sources |
| `AGENTS.md` | This file |
| `docs/NN-*.md` | Workshop modules; each ends with a ✅ checkpoint |
| `exercises/exercise-N.md` | Hands-on exercises matching the modules |
| `scripts/pac-cheatsheet.md` | All PAC CLI commands used in the workshop |
| `.github/copilot-instructions.md` | Rules Copilot applies automatically in this repo |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR checklist |
| `.github/workflows/docs-validation.yml` | CI: markdownlint on every push/PR |

## Rules

- Docs are in **English**, workshop tone: practical, direct, example-driven.
- Every module follows the same shape: `# Module N — Title`, numbered sections, fenced code blocks
  **with a language tag**, and a `## ✅ Checkpoint` section at the end.
- **Never invent PAC CLI commands or flags.** `pac <group> <command> --help` and the Microsoft Learn
  reference pages are authoritative. When unsure, omit the flag and link the reference.
- Be honest about deprecations (e.g. `pac canvas pack/unpack` is deprecated in favor of Power
  Platform Git integration) — say so explicitly, don't hide it.
- No secrets, auth profiles (`.pac/`, `*.paccfg`), connection IDs, or credential-bearing URLs in
  docs or code. Use placeholders like `https://yourorg.crm4.dynamics.com`.
- The full rule set for generated content lives in `.github/copilot-instructions.md` — follow it too.

## Adding or changing a module

1. Create or edit `docs/NN-name.md` (keep numbering gap-free: 00–10).
2. Update the agenda table and repository-structure tree in `README.md`.
3. If the module needs a hands-on task, add `exercises/exercise-N.md` and reference it from the module.
4. Keep markdownlint clean (CI runs `.github/workflows/docs-validation.yml`): blank lines around
   headings, lists, and code fences; language tag on every fence; single H1 per file.
5. Open a PR using `.github/PULL_REQUEST_TEMPLATE.md` and fill in every section.
