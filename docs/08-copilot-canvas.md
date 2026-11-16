# Module 8 — GitHub Copilot for canvas apps (Power Fx) 🤖

Canvas app source is YAML + Power Fx — exactly the kind of text GitHub Copilot is good at. In VS
Code, Copilot works directly on the unpacked `.pa.yaml` files from module 3.

## Setup (2 minutes)

1. Install **GitHub Copilot** + **GitHub Copilot Chat** extensions, sign in (needs a Copilot license)
2. Inline suggestions appear as you type — `Tab` accepts, `Esc` dismisses
3. Open **Copilot Chat** (`Ctrl+Alt+I`) for conversation-style help; reference files with `#file:Screen1.pa.yaml`

## What Copilot is great at here

- **Explaining formulas** — mark a long `OnSelect` and ask "explain this step by step"
- **Writing Power Fx** — "add validation to `btnSubmit.OnSelect`: keep the existing `Set()` call, only navigate when `txtTitle.Text <> ""`"
- **Refactoring** — "extract the gallery item template in `Screen1.pa.yaml` into a component under `Src/Components/`"
- **Dataverse CRUD** — "generate a `Patch()` call for table `wsp_task` with columns `wsp_title`, `wsp_duedate`, `wsp_status` in `Screen2.pa.yaml`"
- **Renaming safely** — "list every reference to `galTasks` under `Src/` so I can rename it"

## Example prompt

```text
In #file:Screen1.pa.yaml, the gallery galTasks shows all wsp_task rows. Add a text input
txtSearch above it and change the gallery Items formula to filter wsp_title contains txtSearch.Text.
Keep the existing sort order.
```

Then **review the diff** (Source Control view), pack (`pac canvas pack`), import, publish, and play-test.

## Guardrails

- **Review every suggestion before committing** — Copilot can invent column names that don't exist
  in your table or reference controls that don't exist; `pac canvas pack` and the player catch these
- **Control names must stay unique** after AI edits — check with a repo-wide search
- **Never paste secrets, connection strings, or auth profiles** into prompts
- AI-generated changes go through the same **branch → PR → CI** flow (modules 6–7) — Copilot doesn't skip ALM
- Inside Power Apps Studio itself, the built-in **Copilot pane** can design/explain while you work;
  with Git integration enabled, publishing syncs the `.pa.yaml` source to your repo automatically

## ✅ Checkpoint

- [ ] Copilot extensions installed and signed in
- [ ] You used Copilot Chat on a `.pa.yaml` file with a `#file:` reference
- [ ] You reviewed, adjusted, and pack-tested at least one AI-generated formula before committing
