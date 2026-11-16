# Exercise 4 — Team flow with pull requests (45 min)

**Goal:** Simulate how a team collaborates on the same app source.

1. Make sure your app source lives in a shared GitHub repository (your fork of this repo, or the workshop repo)
2. Create a feature branch: `git switch -c feature/<your-name>-footer`
3. Make a small change in unpacked source (canvas `.pa.yaml` label, or solution XML comment/description)
4. Commit and push the branch
5. Open a PR and fill in `.github/PULL_REQUEST_TEMPLATE.md` completely
6. **Reviewer swap:** review your pair's PR in the Files changed view — leave at least one comment on the actual diff
7. Merge after approval (protect `main` with required reviews if your repo allows it)

**Definition of done:** at least one merged PR per person, approved by someone else.

---

## Bonus (for the fast ones)

- Create two branches that touch the same line of the same `.pa.yaml` file — merge one, then resolve the conflict in the other (module 6, step 4)
- Ask GitHub Copilot to review the PR diff and summarize what changed ("explain this diff"), then verify its summary yourself
