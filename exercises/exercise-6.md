# Exercise 6 — Copilot-accelerated change with review discipline (30 min)

**Goal:** Use GitHub Copilot as a pair programmer on real app source — and still own the result.

1. Open Copilot Chat (`Ctrl+Alt+I`); confirm the repo's `.github/copilot-instructions.md` is picked up (ask: "which naming prefix does this repo use?" — it should answer `wsp`)
2. **Canvas:** in a `#file:<Screen>.pa.yaml`, ask Copilot to add input validation to a submit button (module 8 example). Review the diff, adjust, `pac canvas pack` to validate
3. **Model-driven:** in `#file:AppModuleSiteMap.xml` (or a view's FetchXml), ask Copilot for a sitemap SubArea or an extra column (module 9 example). Validate with `pac solution pack` + `pac solution check`
4. In the PR description, write down: which suggestions you accepted, which you changed, and why — "I can explain every line Copilot generated"
5. Get a human review as usual (exercise 4)

**Definition of done:** one merged PR where Copilot generated the first draft and a human reviewed every changed line.

---

## Bonus

- Ask Copilot to explain an existing complex Power Fx formula or FetchXml, and paste the explanation as a YAML/XML comment for the next developer
- Try a deliberately vague prompt ("make the app better") and compare with a specific one — note the difference in your PR description
