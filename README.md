# Build Canvas & Model-Driven Apps in VS Code — Workshop 🚀

A step-by-step repository that teaches Power Platform developers to build, version, and ship **canvas apps** and **model-driven apps** from **Visual Studio Code** — with the Power Platform CLI (PAC CLI), Git, and **GitHub Copilot** — instead of living in the maker portal.

> **What does "developing Power Apps in VS Code" mean?** Canvas apps are unpacked from their binary `.msapp` package into readable `.pa.yaml` source files (screens, controls, **Power Fx**), so you edit them as code, review diffs in pull requests, and pack them back. Model-driven apps live inside **solutions**, which are unpacked into XML/YAML (app module, sitemap, forms, views, tables), so you edit those as code too. Everything lands in Git, and GitHub Copilot works directly on the source.
>
> **Who is this for?** Makers and Power Platform developers who are used to Power Apps Studio and the maker portal and want a pro workflow: source control, code review, CI/CD, and an AI pair programmer. No prior .NET or XML experience needed — the modules walk through the file formats.

## 🎯 Learning goals

By the end of the workshop, participants can:

1. Set up VS Code with the **Power Platform Tools** extension, **PAC CLI**, Git, and GitHub Copilot
2. Sign in to a Power Platform environment and select it (`pac auth` / `pac env`)
3. Take a canvas app from the environment to editable source in VS Code (`pac canvas download` → `unpack`) and back (`pack` → import → `publish`)
4. Read and edit unpacked canvas source (`.pa.yaml`, Power Fx formulas) with meaningful Git diffs
5. Work with **model-driven apps** as unpacked solution source in VS Code (app module, sitemap, forms, views)
6. Keep Dataverse schema in source and generate early-bound classes with `pac modelbuilder`
7. Use Git branching and pull requests as part of the app lifecycle
8. Automate export → pack → check → import with **GitHub Actions** (`microsoft/powerplatform-actions`)
9. Use **GitHub Copilot** as a pair programmer on Power Fx and solution XML — with review discipline

## 🗓️ Agenda (full day, ~7½ hours)

| Module | Topic | Time |
|---|---|---|
| 0 | Prerequisites & VS Code setup | 30 min |
| 1 | Auth & environments with PAC CLI | 30 min |
| 2 | Canvas app source control in VS Code (unpack/pack loop) | 45 min |
| 3 | Editing canvas apps: `.pa.yaml` & Power Fx | 45 min |
| 4 | Model-driven apps & solutions in VS Code | 60 min |
| 5 | Dataverse schema, `modelbuilder` & data | 30 min |
| 6 | Git workflow: branches, PRs, review | 45 min |
| 7 | ALM & CI/CD with GitHub Actions | 45 min |
| 8 | Copilot for canvas apps (Power Fx) | 30 min |
| 9 | Copilot for solutions & model-driven XML | 30 min |
| — | Wrap-up & Q&A | 15 min |

## 📁 Repository structure

```text
Build-Canvas-apps-in-VS-Code/
├── README.md                          ← you are here
├── AGENTS.md                          ← operating manual for AI agents
├── .github/
│   ├── copilot-instructions.md        ← rules GitHub Copilot follows automatically
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── workflows/
│       └── docs-validation.yml        ← CI: markdown lint on every push/PR
├── .vscode/
│   └── extensions.json                ← recommended VS Code extensions
├── docs/
│   ├── 00-prerequisites.md            ← tools, extensions, licensing, GitHub
│   ├── 01-environment-and-auth.md     ← pac auth / pac env / whoami
│   ├── 02-canvas-source-control.md    ← download → unpack → commit → pack → publish
│   ├── 03-editing-canvas-apps.md      ← .pa.yaml structure & Power Fx editing
│   ├── 04-model-driven-in-vscode.md   ← solutions, app module, sitemap, forms XML
│   ├── 05-solutions-and-dataverse.md  ← schema in source, modelbuilder, data
│   ├── 06-git-workflow.md             ← branching, PRs, merge-conflict rules
│   ├── 07-alm-cicd.md                 ← GitHub Actions ALM pipeline
│   ├── 08-copilot-canvas.md           ← Copilot on Power Fx / .pa.yaml
│   └── 09-copilot-solutions.md        ← Copilot on solution XML + instructions file
├── exercises/
│   ├── exercise-1.md                  ← first canvas unpack/pack loop
│   ├── exercise-2.md                  ← edit a canvas app from source & republish
│   ├── exercise-3.md                  ← model-driven app change via solution XML
│   ├── exercise-4.md                  ← team flow with pull requests
│   ├── exercise-5.md                  ← CI pipeline with Solution Checker
│   └── exercise-6.md                  ← Copilot-accelerated change with review
├── scripts/
│   └── pac-cheatsheet.md              ← every PAC CLI command used in the workshop
└── .gitignore
```

## 🚦 Getting started

Start in [docs/00-prerequisites.md](docs/00-prerequisites.md) and follow the modules in order 0 → 9. Each module ends with a ✅ checkpoint.

## 📚 Sources

- [Power Platform CLI (PAC CLI) introduction — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction)
- [pac canvas command reference — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/canvas)
- [pac solution command reference — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/solution)
- [pac model command reference (model-driven apps) — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/model)
- [pac modelbuilder reference (early-bound classes) — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/modelbuilder)
- [Source code files for canvas apps (.pa.yaml) — Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/power-apps-yaml)
- [Source control for canvas apps (Power Platform Git integration) — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/canvas-apps-git-integration)
- [Dataverse Git integration overview — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/overview)
- [SolutionPackager tool — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/alm/solution-packager-tool)
- [GitHub Actions for Microsoft Power Platform — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/alm/devops-github-actions)
- [Available GitHub Actions for Power Platform — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/alm/devops-github-available-actions)
- [powerplatform-actions — official actions repo (GitHub)](https://github.com/microsoft/powerplatform-actions)
- [Power Platform Tools for VS Code — marketplace](https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.powerplatform-vscode)
- [Generate early-bound classes for the SDK for .NET — Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/org-service/generate-early-bound-classes)
- [GitHub Copilot in VS Code — documentation](https://code.visualstudio.com/docs/copilot/overview)
- [Customize Copilot with instructions files — VS Code docs](https://code.visualstudio.com/docs/copilot/copilot-customization)
