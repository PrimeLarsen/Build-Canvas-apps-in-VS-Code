# Copilot Instructions — Build Canvas Apps in VS Code

This file is read automatically by GitHub Copilot when generating content and working on issues in this repo.

## What this repo is

A hands-on workshop repository that teaches Power Platform developers to build **canvas apps** and
**model-driven apps** in **Visual Studio Code** with the Power Platform CLI (PAC CLI), Git, GitHub
Actions, and GitHub Copilot. Content: `docs/` modules 00–10, `exercises/`, `scripts/pac-cheatsheet.md`,
`README.md`, plus `.github/` automation.

## Non-negotiable rules

- **Docs in English**, workshop tone (practical, direct). Module shape: `# Module N — Title`, numbered
  sections, fenced code blocks with a language tag, `## ✅ Checkpoint` at the end.
- **Never invent PAC CLI commands or flags.** `pac <group> <command> --help` is authoritative. When a
  flag is uncertain, write the command without it and link the Microsoft Learn reference page instead.
- **Always link Microsoft Learn references** for command groups used: `cli/reference/canvas`,
  `cli/reference/solution`, `cli/reference/model`, `cli/reference/modelbuilder`,
  `alm/devops-github-actions`, `alm/devops-github-available-actions`, `power-apps-yaml`,
  `alm/git-integration/canvas-apps-git-integration`.
- **State deprecations honestly**: `pac canvas pack`/`unpack` is marked deprecated in favor of Power
  Platform Git integration — say so, don't hide it.
- **Terminology**: canvas app source = `.pa.yaml` / `.fx.yaml` files under `Src/`; the binary package
  is `.msapp` and is never committed; model-driven apps are app module components inside solutions,
  unpacked to XML (or YAML in recent CLI versions).
- **No secrets**: never commit `.pac/`, `*.paccfg`, connection IDs, client secrets, or real
  credential-bearing URLs. Use placeholders like `https://yourorg.crm4.dynamics.com`. MCP config files
  must not contain tokens — only URLs and commands.
- **Naming in examples**: publisher prefix `wsp`, solution `WorkshopCore`, tables `wsp_*`, app
  "Workshop Hub", canvas app "My App".
- **Agent plugins & MCP** (module 10): reference only official marketplaces (`microsoft/power-platform-skills`,
  `github/awesome-copilot`) with their exact plugin names (`canvas-apps`, `model-apps`,
  `power-platform-architect`, `dataverse`, …), state that third-party plugin content must be inspected
  before installing, and give MCP setup via the guided `MCP: Add Server` flow plus the config-file
  snippets (`servers` in `.vscode/mcp.json`, `mcpServers` in portable `.mcp.json`). Note the Dataverse
  MCP prerequisites (admin must allow the GitHub Copilot client; Copilot Credits billing since 2025-12-15).

## How to work in this repo

1. Read `AGENTS.md` and the module you are editing before changing anything.
2. Keep changes small and reviewable: one module or exercise per PR.
3. Update `README.md` (agenda + repo structure) whenever a doc or exercise is added, renamed, or renumbered.
4. Keep markdownlint clean: blank lines around headings/lists/fences, language tag on every fence, one H1 per file.
5. Use `.github/PULL_REQUEST_TEMPLATE.md` for every PR.

## Tone of the repo

Practical, minimal, honest about previews and deprecations. When in doubt: accuracy and consistency beat cleverness.
