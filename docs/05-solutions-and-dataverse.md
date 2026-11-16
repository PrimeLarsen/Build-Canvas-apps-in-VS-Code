# Module 5 — Dataverse schema, modelbuilder & data in source

Tables (entities), columns, choices, forms, and views are all solution components — so the schema
lives in `src/WorkshopCore/Entities/<table>/` as XML and is versioned like everything else.

## 1. Schema in source control

- `Entities/wsp_task/Entity.xml` — the table definition
- Columns, choices, relationships — attribute/relationship XML in the same folder
- Forms, views, ribbon diffs — also in the entity folder

Editing schema XML by hand is precise but verbose; for bigger changes use the maker portal, then
export → unpack again so Git sees the result.

## 2. Early-bound classes with `pac modelbuilder`

When you write plug-ins or custom code against Dataverse, generate strongly typed classes from the
schema instead of using `Entity` with string attribute names:

```bash
pac modelbuilder build \
  --outdirectory ./src/Workshop.Model \
  --namespace Workshop.Model \
  --serviceContextName OrgContext \
  --entitynamesfilter "wsp_*" \
  --generatesdkmessages
```

Generated code is a build artifact — keep it out of Git (`.gitignore` excludes `src/**/Model/`) and
regenerate in CI or on build. References:
[pac modelbuilder reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/modelbuilder) and
[generate early-bound classes](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/org-service/generate-early-bound-classes).
Check the reference for the current status of this command in your CLI version.

## 3. Seed data

`pac data export` / `pac data import` move data between environments (schema file + data zip):

```bash
pac data export --schemaFile ./artifacts/schema.xml --dataFile ./artifacts/seed.zip --environment https://yourorg.crm4.dynamics.com
pac data import --dataFile ./artifacts/seed.zip --environment https://yourorg-test.crm4.dynamics.com
```

Use this for small, checked-in seed data in the workshop. See the
[pac data reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/data).

## 4. Configuration without hardcoding

Never hardcode environment-specific values in app source or XML:

- **Environment variables** (`EnvironmentVariableDefinitions/`) for URLs, flags, IDs — read them in
  canvas apps with `EnvironmentVariableDefinition` values, in flows with the Environment Variable connector
- **Connection references** (`ConnectionReferences/`) for all connector connections — the actual
  connection is chosen per environment at deploy time

This is what makes one solution zip deploy cleanly to dev, test, and prod.

✅ **Checkpoint:** your solution source contains a `wsp_*` table, you generated early-bound classes into an ignored folder, and you know where environment variables and connection references live in the unpacked solution.
