# Module 9 — GitHub Copilot for solutions & model-driven XML 🤖

Solution source is XML (app module, sitemap, forms, views, ribbons) and YAML. Copilot is very
effective on well-structured XML — and this repo ships a
[`.github/copilot-instructions.md`](../.github/copilot-instructions.md) that Copilot loads
automatically, so it already knows the naming conventions and rules of the project.
See [customize Copilot with instructions files](https://code.visualstudio.com/docs/copilot/copilot-customization).

## 1. What Copilot is great at here

- **Sitemap edits** — "in `#file:AppModuleSiteMap.xml`, add a new Area 'Workshop' with a SubArea for
  table `wsp_task`, matching the structure of the existing 'Sales' area"
- **Views (FetchXml)** — "add column `wsp_duedate` to the Active Tasks view FetchXml in
  `#file:Entities/wsp_task/SavedQueries/`, keeping the existing sort"
- **Ribbon / command bar** — "add an EnableRule to the `wsp_task` form command that disables the
  button when `wsp_status` equals 2"
- **Forms** — "add a read-only field bound to `wsp_duedate` in the main form XML, placed after `wsp_title`"
- **Explaining** — "what does this appmodule XML section control?" / "explain this PR diff"
- **modelbuilder** — "give me the `pac modelbuilder build` command for our `wsp_*` tables with namespace `Workshop.Model`"

## 2. Example prompt

```text
In #file:AppModuleSiteMaps/AppModuleSiteMap.xml add a SubArea with Id "sub_wsp_tasks",
Title "Tasks", pointing to entity wsp_task, inside the existing Area with Id "area_workshop".
Follow the exact XML pattern of the existing SubArea elements.
```

## 3. Validate AI edits like code

XML must stay well-formed and solution-valid:

```bash
pac solution pack --folder ./src/WorkshopCore --zipfile ./artifacts/check.zip --managed true
pac solution check --path ./artifacts/check.zip
```

Pack + Solution Checker after every AI-assisted change — same discipline as module 6's conflict rule.

## 4. Guardrails

- **Review before committing** — Copilot can hallucinate entity/attribute names or GUIDs; verify
  against `Entities/` and the maker portal
- **Never commit connection IDs, client secrets, or auth profiles**, even if a prompt mentions them
- Generated XML/YAML goes through the same **branch → PR → CI** flow (modules 6–7)
- The repo's `copilot-instructions.md` keeps suggestions consistent (naming `wsp_`, English, honest
  about deprecations) — improve that file when you find a rule worth adding

## ✅ Checkpoint

- [ ] You made one AI-assisted edit to solution XML (sitemap, view, ribbon, or form)
- [ ] You validated it with `pac solution pack` + `pac solution check`
- [ ] You reviewed the diff and can explain every change in the PR description
