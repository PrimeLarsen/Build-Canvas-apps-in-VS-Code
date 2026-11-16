# Module 3 — Editing canvas apps in VS Code (`.pa.yaml` & Power Fx)

Once unpacked, a canvas app is plain, diff-friendly YAML. This module shows the file layout and the
rules that keep your edits safe.

Full format reference: [Source code files for canvas apps (.pa.yaml)](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/power-apps-yaml).

## 1. Unpacked layout

```text
src/CanvasApps/MyApp/
├── Src/
│   ├── App.pa.yaml               ← app-level settings
│   ├── Screen1.pa.yaml           ← one file per screen: controls + properties
│   ├── Screen2.pa.yaml
│   └── Components/
│       └── MyHeader.pa.yaml      ← one file per component
├── DataSources/                  ← Dataverse tables / connectors used
├── Connections/                  ← connection references
├── Resources/                    ← images, media
└── Other/                        ← everything else from the package (incl. entropy.json in older layouts)
```

Only files under `Src/` (plus `DataSources/`, `Connections/`, `Resources/`) are meant for source
control. Never hand-edit the JSON blobs inside a raw `.msapp` — they aren't stable between
save/load cycles.

## 2. Power Fx lives in the YAML

Every formula property is a YAML entry whose expression starts with `=`:

```yaml
- btnSubmit:
    OnSelect: =Set(varSubmitted, true); Navigate(Screen2, ScreenTransition.Fade)
    Text: ="Submit " & txtTitle.Text
```

(Older unpack layouts split formulas into `*.fx.yaml` files; the grammar is the same — expressions
begin with `=`.)

## 3. Editing rules that save you

- **Control names must be unique.** Renaming a control means updating every formula that references it —
  use VS Code's search across `Src/` (Ctrl+Shift+F) before and after.
- **One file per screen/component** → small, reviewable diffs and fewer merge conflicts.
- **`entropy.json` (older layouts) is volatile** — timestamps and IDs that change on every save.
  On a conflict in it, delete the file and repack; it regenerates.
- **Validate by packing.** `pac canvas pack` fails on broken references — run it after every edit
  (see module 2, step 5).
- **Don't fight the format:** if a change is painful in YAML, make it in Power Apps Studio, publish,
  then re-download/re-unpack (or use Git integration, which keeps source in sync automatically).

## 4. The daily edit loop

```bash
# edit files under src/CanvasApps/MyApp/Src/ in VS Code, then:
pac canvas pack --msapp ./artifacts/MyApp.msapp --sources ./src/CanvasApps/MyApp --layout SourceCode --overwrite
# import ./artifacts/MyApp.msapp in make.powerapps.com → Apps → Import canvas app (dev environment)
pac canvas publish
# play the app, verify, then:
git add src/CanvasApps/MyApp && git commit -m "feat: submit button validates title"
```

## 5. Merge conflicts

- Resolve them in the YAML like any text file — screens and components are separate files, so most
  conflicts are small.
- After resolving, always `pac canvas pack` to prove the app still builds.
- Prefer small PRs (module 6) so conflicts stay rare.

✅ **Checkpoint:** you changed a label and one Power Fx formula in `Src/`, packed, imported into dev, published, and saw the change live in the player.
