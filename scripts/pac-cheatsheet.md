# PAC CLI cheatsheet — commands used in this workshop

Full references: [PAC CLI command groups — Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/).
`pac <group> <command> --help` is always authoritative for your installed version.

## Auth & environments

```bash
pac auth create --url https://yourorg.crm4.dynamics.com   # sign in (browser)
pac auth list                                              # list local auth profiles
pac env list                                               # list environments
pac env select --environment <id-or-url>                   # pick active environment
pac org whoami                                             # verify connection
```

## Canvas apps (.msapp ↔ source)

```bash
pac canvas list
pac canvas download --name "My App" --file-name ./artifacts/MyApp.msapp --overwrite
pac canvas unpack --msapp ./artifacts/MyApp.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite
pac canvas pack   --msapp ./artifacts/MyApp.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite
pac canvas publish
```

> `pack`/`unpack` is marked deprecated in favor of Power Platform Git integration — see
> [pac canvas reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/canvas) and
> [canvas app source control](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/canvas-apps-git-integration).

## Solutions (model-driven apps, tables, flows, canvas apps in ALM)

```bash
pac solution init --publisher-name Workshop --publisher-prefix wsp   # local project (.cdsproj)
pac solution list
pac solution export --name WorkshopCore --path ./artifacts --managed false
pac solution unpack --zipfile ./artifacts/WorkshopCore.zip --folder ./src/WorkshopCore
pac solution pack   --folder ./src/WorkshopCore --zipfile ./artifacts/WorkshopCore_managed.zip --managed true
pac solution import --path ./artifacts/WorkshopCore.zip --force-overwrite --activate-plugins
pac solution check  --path ./artifacts/WorkshopCore_managed.zip      # Solution Checker quality gate
```

## Model-driven apps

```bash
pac model list
pac model create --name "Workshop Hub" --description "..." --solution WorkshopCore --publish
```

## Dataverse: early-bound classes & data

```bash
pac modelbuilder build --outdirectory ./src/Workshop.Model --namespace Workshop.Model --serviceContextName OrgContext --entitynamesfilter "wsp_*" --generatesdkmessages
pac data export --schemaFile ./artifacts/schema.xml --dataFile ./artifacts/seed.zip
pac data import --dataFile ./artifacts/seed.zip
```

## CI/CD (GitHub Actions)

Use the official actions from [microsoft/powerplatform-actions](https://github.com/microsoft/powerplatform-actions):
`actions-install`, `whoami`, `export-solution`, `unpack-solution`, `pack-solution`, `check-solution`,
`import-solution` — see [docs/07-alm-cicd.md](../docs/07-alm-cicd.md) for a full workflow.
Auth via repo secrets: `POWERPLATFORM_APP_ID`, `POWERPLATFORM_TENANT_ID`, `POWERPLATFORM_CLIENT_SECRET`, `POWERPLATFORM_TEST_URL`.
