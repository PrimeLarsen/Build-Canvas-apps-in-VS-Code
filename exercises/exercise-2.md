# Exercise 2 — Edit a canvas app from source & republish (45 min)

**Goal:** Make a real change in `.pa.yaml`, pack, import, publish — the daily loop from modules 2–3.

1. Start from your unpacked app from exercise 1 (branch `feature/<your-name>-edit-app`)
2. In `Src/<Screen>.pa.yaml`, change one visible label's `Text` property
3. Change one Power Fx formula — e.g. a button's `OnSelect` to also `Set(varLastAction, "clicked")`
4. Verify control-name uniqueness if you renamed anything: Ctrl+Shift+F the old name across `Src/`
5. `pac canvas pack --msapp ./artifacts/App.msapp --sources ./src/CanvasApps/App --layout SourceCode --overwrite`
6. Import in make.powerapps.com → Apps → **Import canvas app** into your dev environment
7. `pac canvas publish`, then play the app and verify both changes
8. Commit + push the branch

**Definition of done:** a colleague can pull your branch, pack, import, and see your change in the player.

---

## Bonus

- Break something on purpose (reference a non-existent control) and confirm `pac canvas pack` fails — that's your safety net
- Try a merge conflict: two branches editing different lines of the same screen file, then resolve and repack
