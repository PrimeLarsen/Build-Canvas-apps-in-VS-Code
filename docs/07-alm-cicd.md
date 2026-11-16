# Module 7 — ALM & CI/CD with GitHub Actions

Manual export/pack/import works for one person. For a team you automate it: **DEV → TEST → PROD**,
unmanaged in dev, managed downstream, with a quality gate in between.

Microsoft ships official actions: [microsoft/powerplatform-actions](https://github.com/microsoft/powerplatform-actions).
Docs: [GitHub Actions for Power Platform](https://learn.microsoft.com/en-us/power-platform/alm/devops-github-actions)
and [available actions & inputs](https://learn.microsoft.com/en-us/power-platform/alm/devops-github-available-actions).

## 1. The pipeline shape

| Stage | What happens |
|---|---|
| PR opened | Solution Checker (`check-solution`) runs on the packed solution; markdown lint runs on docs |
| Push to `main` | Pack `src/WorkshopCore` → managed zip → import into TEST; canvas apps packed from source |
| Release | Import the same managed zip into PROD (or use deployment pipelines in Power Platform) |

## 2. Authentication: service principal (recommended)

Create an app registration in Microsoft Entra, grant it the Power Platform admin/service role, and
store three secrets in the repo (Settings → Secrets → Actions):

- `POWERPLATFORM_APP_ID` (application/client ID)
- `POWERPLATFORM_TENANT_ID`
- `POWERPLATFORM_CLIENT_SECRET`
- plus `POWERPLATFORM_URL` / `POWERPLATFORM_TEST_URL` for the environment URLs

(Username + `password-secret` also works but is less safe — see the Learn tutorial for both variants.)

## 3. Sample workflow

```yaml
name: ALM pipeline

on:
  pull_request:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Power Platform Tools
        uses: microsoft/powerplatform-actions/actions-install@v1

      - name: Who am I (auth check)
        uses: microsoft/powerplatform-actions/whoami@v1
        with:
          environment-url: ${{ secrets.POWERPLATFORM_TEST_URL }}
          app-id: ${{ secrets.POWERPLATFORM_APP_ID }}
          tenant-id: ${{ secrets.POWERPLATFORM_TENANT_ID }}
          client-secret: ${{ secrets.POWERPLATFORM_CLIENT_SECRET }}

      - name: Pack solution (managed)
        uses: microsoft/powerplatform-actions/pack-solution@v1
        with:
          solution-source-folder: src/WorkshopCore
          solution-output-file: artifacts/WorkshopCore_managed.zip
          managed: true

      - name: Solution Checker
        uses: microsoft/powerplatform-actions/check-solution@v1
        with:
          environment-url: ${{ secrets.POWERPLATFORM_TEST_URL }}
          app-id: ${{ secrets.POWERPLATFORM_APP_ID }}
          tenant-id: ${{ secrets.POWERPLATFORM_TENANT_ID }}
          client-secret: ${{ secrets.POWERPLATFORM_CLIENT_SECRET }}
          solution-input-file: artifacts/WorkshopCore_managed.zip

      - name: Import solution into TEST
        if: github.ref == 'refs/heads/main'
        uses: microsoft/powerplatform-actions/import-solution@v1
        with:
          environment-url: ${{ secrets.POWERPLATFORM_TEST_URL }}
          app-id: ${{ secrets.POWERPLATFORM_APP_ID }}
          tenant-id: ${{ secrets.POWERPLATFORM_TENANT_ID }}
          client-secret: ${{ secrets.POWERPLATFORM_CLIENT_SECRET }}
          solution-input-file: artifacts/WorkshopCore_managed.zip
          force-overwrite: true
```

> Input names drift between action versions — always check the action's `action.yml` and the
> [available actions](https://learn.microsoft.com/en-us/power-platform/alm/devops-github-available-actions) page.
> Related actions: `export-solution`, `unpack-solution` (environment → source sync), `pack-solution`.

## 4. Canvas apps in CI

Pack from source instead of importing a binary:

```yaml
- name: Pack canvas app
  run: pac canvas pack --msapp ./artifacts/MyApp.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite
```

(With Git integration enabled, source sync is handled by the platform and you only pack/validate.)

## 5. Deployment pipelines

For PROD, many teams use Power Platform **deployment pipelines** (in the Power Platform admin center)
instead of raw GitHub Actions imports — they add approvals, checklists, and connection remapping.
GitHub Actions remains the build/check step feeding them.

✅ **Checkpoint:** a PR runs the Solution Checker green, and a push to `main` imports the managed solution into your TEST environment.
