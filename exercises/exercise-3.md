# Exercise 3 — Model-driven app change via solution XML (60 min)

**Goal:** Change a model-driven app by editing unpacked solution XML in VS Code — no portal clicks.

1. Make sure solution `WorkshopCore` exists in your dev environment (module 1) and contains a
   model-driven app — create one with
   `pac model create --name "Workshop Hub" --solution WorkshopCore --publish` if needed
2. `pac solution export --name WorkshopCore --path ./artifacts --managed false`
3. `pac solution unpack --zipfile ./artifacts/WorkshopCore.zip --folder ./src/WorkshopCore`
4. Open `src/WorkshopCore/AppModuleSiteMaps/` and add one **SubArea** for your `wsp_*` table
   (or an existing table) inside an existing Area — copy the XML pattern of a neighboring SubArea
5. Optionally edit `src/WorkshopCore/AppModules/AppModule_*.xml` (e.g. the app description)
6. Validate: `pac solution pack --folder ./src/WorkshopCore --zipfile ./artifacts/WorkshopCore_managed.zip --managed true`
7. `pac solution check --path ./artifacts/WorkshopCore_managed.zip` — no errors
8. Import back into your dev environment:
   `pac solution import --path ./artifacts/WorkshopCore.zip --force-overwrite --activate-plugins`
   (import the unmanaged zip into dev; keep the managed one for downstream)
9. Open the app in make.powerapps.com and verify the new navigation entry
10. Commit + push on a feature branch

**Definition of done:** the sitemap change is visible in the running app, and the diff in Git is a clean, reviewable XML change.

---

## Bonus

- Use Ctrl+Shift+H to rename a label across **all** forms of a table at once
- Add a column to a view's FetchXml in `Entities/<table>/...`
