# Module 4 — Model-driven apps in VS Code

A model-driven app is an **app module** component that lives inside a **solution** together with its
tables, forms, views, sitemap, and canvas apps. That's good news for VS Code: a solution can be
exported and **unpacked into XML/YAML source**, so the whole app becomes reviewable code in Git.

References: [pac solution command reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/solution),
[SolutionPackager tool](https://learn.microsoft.com/en-us/power-platform/alm/solution-packager-tool).

## 1. Create the app (CLI or portal)

```bash
pac model create --name "Workshop Hub" --description "Workshop model-driven app" --solution WorkshopCore --publish
pac model list
```

You can also build the app in the maker portal's app designer — the result is the same app module
component in the solution. See the [pac model reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/model).

## 2. Export & unpack the solution

```bash
pac solution export --name WorkshopCore --path ./artifacts --managed false
pac solution unpack --zipfile ./artifacts/WorkshopCore.zip --folder ./src/WorkshopCore
```

Unpacked layout (one folder per component type, XML by default; recent CLI versions also support a
YAML layout — see the SolutionPackager reference):

```text
src/WorkshopCore/
├── Other/
│   └── Solution.xml              ← solution metadata
├── AppModules/
│   └── AppModule_WorkshopHub.xml ← the model-driven app definition
├── AppModuleSiteMaps/            ← navigation (areas, groups, subareas)
├── Entities/
│   └── wsp_task/                 ← table: Entity.xml, forms, views, ribbon diffs
├── CanvasApps/                   ← canvas apps referenced by the solution
├── WebResources/
├── ConnectionReferences/
├── EnvironmentVariableDefinitions/
└── Workflows/
```

Commit `src/WorkshopCore/` — never the zip.

## 3. Edit as code in VS Code

This is where VS Code shines for model-driven apps:

- **Sitemap** (`AppModuleSiteMaps/*.xml`): add an Area/Group/SubArea in XML — far more precise than
  drag-drop, and diffable in PRs
- **App module** (`AppModules/*.xml`): URL, form factor, client settings
- **Forms, views, ribbons** (`Entities/<table>/...`): XML edits, multi-file search & replace
  (Ctrl+Shift+H) across all forms at once — e.g. renaming a column label everywhere
- **Web resources**: JavaScript/TypeScript files edited like any web project (Copilot helps — module 9)

What still belongs in the maker portal: drag-drop form layout and the app designer's visual UX.
Code-level changes belong in VS Code.

## 4. Pack & deploy

```bash
# quality gate first
pac solution check --path ./artifacts/WorkshopCore_managed.zip
# pack as managed for downstream environments
pac solution pack --folder ./src/WorkshopCore --zipfile ./artifacts/WorkshopCore_managed.zip --managed true
# deploy to another environment
pac solution import --path ./artifacts/WorkshopCore_managed.zip --environment https://yourorg-test.crm4.dynamics.com --force-overwrite --activate-plugins
```

Rule of thumb: **unmanaged in dev, managed in test/prod.**

## 5. Modern alternative: Dataverse Git integration

Power Platform now has native Git integration for solutions (make.powerapps.com → source control),
which syncs unpacked solution source to your repo automatically.
See [Dataverse Git integration overview](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/overview).
The export → unpack → pack loop above remains the CLI-native way and what CI pipelines use.

✅ **Checkpoint:** you added a sitemap SubArea in XML, packed the solution as managed, imported it into your dev environment (overwrite), and saw the new navigation entry in the app.
