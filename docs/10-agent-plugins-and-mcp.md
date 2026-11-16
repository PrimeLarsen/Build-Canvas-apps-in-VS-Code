# Module 10 — Agent plugins & MCP servers for the best Copilot experience 🤖🔌

Everything in modules 0–9 works with just Copilot. But you can make Copilot much more effective on
Power Platform work with two add-ons you were prompted to install:

1. **Agent plugins** (what `@agentPlugins` in the VS Code Extensions view shows) — ready-made agents,
   skills, and instructions for a domain, e.g. canvas apps and model-driven apps
2. **MCP servers** — Model Context Protocol servers that give Copilot live tools: current Microsoft
   docs, your Dataverse environment, and the PAC CLI itself

**Are they still needed?** Not strictly — the workshop runs without them — but they are the recommended
setup for the best experience (and several are official Microsoft offerings). Caveats below.

## 1. Agent plugins (`@agentPlugins`)

- In **VS Code**: open the Extensions search view and type `@agentPlugins` to browse installable
  plugins, or use the Command Palette → `Chat: Plugins`. [awesome-copilot](https://github.com/github/awesome-copilot)
  is a default marketplace, so its plugins are discoverable with no configuration.
- In **Copilot CLI / Claude Code**: `copilot plugin marketplace add github/awesome-copilot` then
  `copilot plugin install <name>@awesome-copilot` (same pattern as `/plugin install` in Claude Code).

Plugins recommended for this workshop:

| Plugin | Marketplace | What it adds |
|---|---|---|
| `canvas-apps` | `microsoft/power-platform-skills` (official) | Skills/agents for building canvas apps with AI tools |
| `model-apps` | `microsoft/power-platform-skills` (official) | Build & edit model-driven apps from natural language (incl. generative pages) |
| `power-platform-architect` | `github/awesome-copilot` | Senior solution-architect skill across canvas, model-driven, code apps, Dataverse, connectors |
| `dataverse` | `github/awesome-copilot` | Dataverse skills + a `/dataverse:mcp-configure` skill that sets up the Dataverse MCP server for you |
| `power-apps-code-apps` | `github/awesome-copilot` | Code Apps toolkit (code-first apps) |

Install example (Copilot CLI):

```bash
copilot plugin marketplace add microsoft/power-platform-skills
copilot plugin install canvas-apps@power-platform-skills
copilot plugin install model-apps@power-platform-skills
```

In VS Code chat, invoke a plugin's agent/skill by name (e.g. `@power-platform-architect design a
solution for …` or the plugin's `/skill-name` command).

**Safety:** plugin content is third-party — inspect an agent's docs before installing (the
awesome-copilot README says so explicitly). Plugins can bundle tools and MCP servers, so expect
approval prompts; grant auto-approval only in trusted/sandboxed environments.

## 2. MCP servers

| Server | What Copilot can do with it | Setup | Notes |
|---|---|---|---|
| **Microsoft Learn MCP** | Search & fetch current official Microsoft docs (`microsoft_docs_search`, `microsoft_docs_fetch`) — kills hallucinated `pac` flags | URL `https://learn.microsoft.com/api/mcp`, no auth | Already configured in this repo's `.vscode/mcp.json` |
| **Dataverse MCP** | Query live schema & records in your environment ("how many open `wsp_task` rows?") | URL = instance URL + `/api/mcp`, e.g. `https://yourorg.crm4.dynamics.com/api/mcp` | Admin must allow the *Microsoft GitHub Copilot* MCP client for the environment (PPAC); since 2025-12-15 non-Copilot-Studio clients are billed in Copilot Credits (Dynamics 365 Premium / M365 Copilot USL exempt) |
| **PAC CLI built-in MCP** | Run `pac` commands via natural language ("list the canvas apps in the current environment") | `pac copilot mcp --run` (PAC CLI 1.44+), registered as a stdio server | No extra install — ships with the CLI |
| Power Apps MCP | App-level tools for agents in model-driven apps | — | **Not needed here**: it targets Copilot Studio agents and is model-driven only, not VS Code canvas source work |

### Add a server in VS Code (guided — recommended)

Command Palette (`Ctrl+Shift+P`) → **MCP: Add Server** → *HTTP or Server Sent Events* → paste the URL
(e.g. `https://learn.microsoft.com/api/mcp`, or your instance URL + `/api/mcp`) → pick a name →
choose Global or workspace. For the PAC CLI server choose *Command (stdio)*: `pac copilot mcp --run`.

### Or configure via file

VS Code workspace file `.vscode/mcp.json` (key: `servers`; the portable `.mcp.json` /
`~/.copilot/mcp-config.json` use `mcpServers` instead):

```json
{
  "servers": {
    "microsoft-learn": { "type": "http", "url": "https://learn.microsoft.com/api/mcp" },
    "dataverse": { "type": "http", "url": "https://yourorg.crm4.dynamics.com/api/mcp" },
    "pac-cli": { "type": "stdio", "command": "pac", "args": ["copilot", "mcp", "--run"] }
  }
}
```

Only enable the servers you need — each adds tools to every chat session (context rot). VS Code prompts
you to trust/start each server; in agent mode you approve each tool call. Orgs can restrict MCP via
GitHub Copilot policy — check with your admin if servers don't appear.

References: [Get started with the Microsoft Learn MCP Server](https://learn.microsoft.com/en-us/training/support/mcp-get-started),
[Connect to Dataverse with MCP](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/data-platform-mcp),
[Connect Dataverse MCP with GitHub Copilot in VS Code](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/data-platform-mcp-vscode),
[Use PAC CLI with the built-in MCP server](https://learn.microsoft.com/en-us/power-platform/developer/howto/use-mcp),
[Add and use MCP servers in VS Code](https://code.visualstudio.com/docs/agent-customization/mcp-servers).

## 3. Using them well

- **Ground every command in current docs**: "Using the Learn MCP, what is the current status and
  recommended alternative of `pac canvas pack/unpack`?" — answers come from Microsoft Learn, not training data
- **Let Copilot inspect the real environment**: with Dataverse MCP or PAC MCP connected — "list the
  tables in my current environment whose names start with wsp_"
- **Combine with plugins**: "@power-platform-architect propose the components for a workshop task
  tracker, then use the canvas-apps skill to scaffold the canvas app source"
- **Keep the guardrails from modules 8–9**: review AI output, validate with `pac canvas pack` /
  `pac solution pack` + `pac solution check`, never paste secrets into chat, everything still goes
  through branch → PR → CI

## 4. What this repo ships

- `.vscode/mcp.json` — Microsoft Learn MCP preconfigured (workspace-scoped, shared with everyone who clones)
- `.vscode/extensions.json` — Power Platform Tools + Copilot extensions
- `.github/copilot-instructions.md` — repo rules Copilot applies automatically

✅ **Checkpoint**

- [ ] You can see plugins via `@agentPlugins` in the Extensions view and installed at least one
      (e.g. `canvas-apps` + `model-apps` from `microsoft/power-platform-skills`)
- [ ] Microsoft Learn MCP is connected and you asked Copilot a docs question through it
- [ ] You know how to add the Dataverse MCP (`<instance-url>/api/mcp`) and PAC CLI MCP
      (`pac copilot mcp --run`), and what the admin/billing prerequisites are
- [ ] You understand Power Apps MCP is for Copilot Studio agents, not needed for this workshop
