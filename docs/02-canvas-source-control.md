# Module 2 — Canvas app source control in VS Code

A canvas app in an environment is stored as a single binary **`.msapp`** package (it's a zip file).
Binaries are terrible in Git: no readable diffs, no merge, no review. So the first thing we do is
**unpack the app into text source** and commit *that*.

> **Modern alternative:** Power Platform **Git integration** (in make.powerapps.com) stores the
> `.pa.yaml` source in your repo automatically — no `.msapp` round-trip needed. See
> [Source control for canvas apps](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/canvas-apps-git-integration).
> This module teaches the CLI unpack/pack loop, which works with **any** existing `.msapp`. Note that
> Microsoft marks `pac canvas pack`/`unpack` as deprecated in favor of Git integration — check the
> [pac canvas reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/canvas)
> for the current status before using it in production.

## 1. List and download the app

```bash
pac canvas list
pac canvas download --name "My App" --file-name ./artifacts/MyApp.msapp --overwrite
```

(`artifacts/` and `*.msapp` are gitignored — the binary never enters source control.)

## 2. Unpack to source

```bash
pac canvas unpack --msapp ./artifacts/MyApp.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite
```

Use `--layout SourceCode` (the current supported layout; the older `Experimental` layout is deprecated).
The result is one readable YAML file per screen/control plus app metadata — see module 3.

## 3. Commit the source

```bash
git add src/CanvasApps/MyApp .gitignore
git commit -m "chore: unpack My App canvas source"
```

## 4. Prove the round-trip before editing anything

```bash
pac canvas pack --msapp ./artifacts/MyApp_roundtrip.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite
mkdir /tmp/roundtrip
pac canvas unpack --msapp ./artifacts/MyApp_roundtrip.msapp --sources /tmp/roundtrip --layout SourceCode --overwrite
diff -r src/CanvasApps/MyApp /tmp/roundtrip
```

If the diff is empty, your unpack → pack loop is trustworthy. Do this **once per app** before the first edit.

## 5. The edit loop (preview — details in module 3)

1. Edit files under `src/CanvasApps/MyApp/Src/` in VS Code
2. `pac canvas pack --msapp ./artifacts/MyApp.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite`
3. Import the `.msapp` into your dev environment: make.powerapps.com → Apps → **Import canvas app** —
   or ship the app inside a solution (module 4) for environment-to-environment moves
4. `pac canvas publish` to publish the imported version
5. Commit the source change; open a PR (module 6)

## 6. Why not just commit the .msapp?

- No readable diffs → no meaningful code review
- No merge — last writer wins, silently
- Binary churn in every commit inflates the repo

The unpacked `.pa.yaml` source is the source of truth; the `.msapp` is a build artifact.

✅ **Checkpoint:** you have unpacked an app into `src/CanvasApps/`, committed it, and verified pack → unpack produces an empty diff.
