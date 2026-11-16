# Exercise 1 — First canvas unpack/pack loop (45 min)

**Goal:** From a canvas app in your environment to committed source in VS Code — and back.

1. In VS Code's terminal: `pac auth create --url https://yourorg.crm4.dynamics.com`, then `pac env select --environment <id>` (module 1)
2. `pac canvas list` — pick one app (or create a blank canvas app in make.powerapps.com first)
3. `pac canvas download --name "<App>" --file-name ./artifacts/App.msapp --overwrite`
4. `pac canvas unpack --msapp ./artifacts/App.msapp --sources ./src/CanvasApps/App --layout SourceCode --overwrite`
5. Confirm `git status` does **not** show `*.msapp` or `artifacts/` (gitignore works)
6. Commit the source: `git add src/CanvasApps/App && git commit -m "chore: unpack App canvas source"`
7. Round-trip check: `pac canvas pack --msapp ./artifacts/App_rt.msapp --sources ./src/CanvasApps/App --layout SourceCode --overwrite`, unpack to a temp folder, `diff -r` — must be empty

**Definition of done:** committed unpacked source + empty round-trip diff.

---

## Bonus

- Browse `src/CanvasApps/App/Src/` — find the file for your main screen and one `OnSelect` formula
- Add a comment line at the top of the screen's `.pa.yaml` (YAML `#` comment), pack, and confirm the pack still succeeds
