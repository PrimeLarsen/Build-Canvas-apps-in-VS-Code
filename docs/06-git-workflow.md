# Module 6 — Git workflow for app source

With apps unpacked into text source, Git becomes the backbone of your ALM. This module sets the
team conventions.

## 1. Repository layout

```text
your-app-repo/
├── src/
│   ├── WorkshopCore/              ← unpacked solution (model-driven app, tables, flows)
│   └── CanvasApps/
│       └── MyApp/                 ← unpacked canvas app source (.pa.yaml)
├── artifacts/                     ← .msapp / .zip build output (gitignored)
├── docs/                          ← project docs (like this workshop repo)
└── .github/
    └── workflows/                 ← CI (module 7)
```

**Commit the source, never the binaries**: `*.msapp` and `*.zip` stay in `.gitignore`.

## 2. Branching

```bash
git switch -c feature/<your-name>-submit-validation
# ... edit src/, commit small steps ...
git push -u origin feature/<your-name>-submit-validation
```

- One feature per branch, one topic per commit
- Branch names: `feature/<name>-<short-change>`, `fix/<name>-<short-change>`
- Keep PRs small — YAML/XML diffs are readable, so reviewers can actually review them

## 3. Pull requests

- Open a PR per branch; fill in `.github/PULL_REQUEST_TEMPLATE.md`:
  what changed, how to test (e.g. "pack with `pac canvas pack ...`, import, play"), checklist
- Require **at least one review** before merging; protect `main` (Settings → Branches)
- Review the **diff**, not the description: does the `.pa.yaml` change match the intent? Does the
  sitemap XML add exactly one SubArea?

## 4. Merge conflicts

- `.pa.yaml` / solution XML: resolve as text. Screens and components are separate files, so most
  conflicts are small.
- `entropy.json` (canvas, older layouts): on conflict, delete it and repack — it regenerates.
- After **any** conflict resolution: `pac canvas pack` or `pac solution pack` to prove the result
  still builds, and `pac solution check` for solutions.

## 5. Commit messages

Conventional, short, imperative:

```text
feat: add due-date validation to task form (canvas)
fix: correct sitemap SubArea URL for wsp_task
chore: re-unpack WorkshopCore after portal change
```

✅ **Checkpoint:** every participant has one merged PR (branch → PR → review comment → merge) touching unpacked app source.
