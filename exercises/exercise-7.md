# Exercise 7 — Install agent plugins & MCP, then use them (30 min)

**Goal:** Set up the recommended AI tooling from module 10 and use it on a real workshop task.

1. In VS Code, open the Extensions search view and type `@agentPlugins`; find and install
   `canvas-apps` and `model-apps` from the `microsoft/power-platform-skills` marketplace
   (or, in Copilot CLI: `copilot plugin marketplace add microsoft/power-platform-skills` +
   `copilot plugin install canvas-apps@power-platform-skills model-apps@power-platform-skills`)
2. Optionally also install `power-platform-architect@awesome-copilot` — read its description first
   (third-party content: inspect before trusting)
3. Confirm the Microsoft Learn MCP server from this repo's `.vscode/mcp.json` is started
   (VS Code prompts to trust it; tools appear in the chat tool picker). If not, add it via
   Command Palette → **MCP: Add Server** → HTTP → `https://learn.microsoft.com/api/mcp`
4. In Copilot Chat (agent mode), ask: *"Using the Learn MCP, what is the current status of
   `pac canvas unpack` and what does Microsoft recommend instead?"* — note the tool call and approval
5. If your admin has enabled the GitHub Copilot MCP client for your environment, add Dataverse MCP:
   **MCP: Add Server** → HTTP → your instance URL + `/api/mcp` (instance URL: make.powerapps.com →
   Settings → Session details), then ask: *"List the tables in this environment starting with wsp_"*
6. Add the PAC CLI MCP as a stdio server (`pac copilot mcp --run`) and ask: *"List the canvas apps in
   my current environment"* — watch Copilot call `pac canvas list` for you
7. Use one plugin agent/skill on a real task (e.g. `@power-platform-architect` on your exercise-3 app,
   or the canvas-apps skill on your exercise-2 change) and record in the PR description what it did well
   and what you changed

**Definition of done:** at least one plugin installed, Learn MCP answered a docs question via a real
tool call, and you can explain the Dataverse/PAC MCP prerequisites (admin enablement, billing) to a colleague.

---

## Bonus

- Compare an answer with and without the Learn MCP for a recently changed command — note the difference
- If MCP is blocked by your org's Copilot policy, find the policy name and document it for your team
