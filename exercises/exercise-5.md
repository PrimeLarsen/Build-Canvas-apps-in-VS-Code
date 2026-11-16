# Exercise 5 — CI pipeline with Solution Checker (45 min)

**Goal:** Automate pack → check → import so every PR is validated and `main` deploys to TEST.

1. Create an app registration in Microsoft Entra ID for CI, grant it access to your environments,
   and add repo secrets: `POWERPLATFORM_APP_ID`, `POWERPLATFORM_TENANT_ID`, `POWERPLATFORM_CLIENT_SECRET`, `POWERPLATFORM_TEST_URL` (module 7)
2. Add `.github/workflows/alm.yml` based on the sample in [docs/07-alm-cicd.md](../docs/07-alm-cicd.md):
   - `actions/checkout`, `microsoft/powerplatform-actions/actions-install@v1`
   - `pack-solution@v1` on `src/WorkshopCore` → managed zip in `artifacts/`
   - `check-solution@v1` on that zip (quality gate)
   - `import-solution@v1` into TEST, only on push to `main`
3. Verify input names against the action's `action.yml` and the
   [available actions doc](https://learn.microsoft.com/en-us/power-platform/alm/devops-github-available-actions)
4. Push a branch with a small solution change → open a PR → watch the workflow run green
5. Merge → watch the import into TEST succeed

**Definition of done:** PR checks pass automatically and a merge deploys the managed solution to TEST without anyone running `pac` locally.

---

## Bonus

- Add a canvas pack step (`pac canvas pack ...`) to the same workflow for `src/CanvasApps/*`
- Add the docs-validation markdownlint job from this repo to your app repo
- Break the solution on purpose in a PR (invalid XML) and confirm `check-solution` / `pack-solution` fails the PR
