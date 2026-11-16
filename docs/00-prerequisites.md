# Module 0 — Prerequisites

Every participant should have the following on their own machine **before** the workshop (ideally as homework).

## 1. Install the tools

| Tool | Version | Notes |
|---|---|---|
| [Visual Studio Code](https://code.visualstudio.com) | latest | the IDE for the whole workshop |
| [Power Platform Tools extension](https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.powerplatform-vscode) | latest | installs the PAC CLI and adds a Power Platform activity bar to VS Code |
| Power Platform CLI (`pac`) | latest | comes with the extension; standalone install via `winget install Microsoft.PowerAppsCLI` (Windows) or `dotnet tool install --global Microsoft.PowerAppsCLI.Tool` (macOS/Linux) |
| [Git](https://git-scm.com) | latest | source control |
| .NET SDK | 8+ | only needed if `pac` is installed as a .NET tool |

The repo's `.vscode/extensions.json` recommends the extensions — VS Code offers to install them when you open the repo.

## 2. VS Code extensions

Install from the Extensions marketplace (or accept the repo's recommendations):

- **Power Platform Tools** (`microsoft-IsvExpTools.powerplatform-vscode`) — PAC CLI + environment/solution browsing
- **GitHub Copilot** and **GitHub Copilot Chat** — modules 8 and 9

## 3. Verify your installation

Open the integrated terminal in VS Code (`Ctrl+`` `) and run:

```bash
git --version
pac --version   # Microsoft PowerPlatform CLI
code --version
```

If all three succeed, you're ready. If `pac` fails, check the [PAC CLI installation guide](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction).

## 4. Environment & licensing 🪪

You need:

- A **Power Platform developer environment** (Developer Program or a sandbox) with Dataverse — ask your admin if you don't have one
- A license that covers what you build: **canvas apps** need Power Apps per-app/per-user (or pay-as-you-go); **model-driven apps** need a Dataverse-backed license (a developer environment covers the workshop)
- **GitHub Copilot** access (individual trial/subscription, or assigned by your org) for modules 8–9

Check with your admin that the workshop environment is covered — otherwise publishing in module 2 will fail.

## 5. GitHub account

- Create a GitHub account (if you don't have one)
- Set up an SSH key or Personal Access Token so `git push` works
- Fork this workshop repository (or use the one your workshop lead shared)

✅ **Checkpoint:** all three version checks pass, `pac --version` works in the VS Code terminal, and you can push a commit to your fork.
