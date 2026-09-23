---
name: finish-task
description: Finish the current task on this project — commit, open a PR, merge it into main. Use only when the user explicitly asks to finish/wrap up the task ("заверши задачу", "коммит, пр и мерж").
---

Each task lives on its own `feature/*` branch in its own git worktree. Finishing means shipping that branch into `main` on GitHub. The task is done when the PR is `MERGED`.

1. **Review the diff.** `git status` and `git diff`: every changed file belongs to the task. Leave scratch output (`.playwright-mcp/`, screenshots, local servers) out of the commit and delete it.
2. **Commit.** Compose the message with the `commit-message-composer` skill. No `Co-Authored-By` or other attribution trailers.
3. **Push.** `git push -u origin HEAD`.
4. **Open the PR.** `gh pr create --base main`. Title: the commit subject (for several commits, the one describing the user-visible change). Body: one sentence on what changed for the user, then short bullets with notable details. No "Generated with Claude Code" footer.
5. **Merge.** `gh pr merge <number> --merge` — a merge commit, matching the existing history. Skip `--delete-branch`: the branch is checked out in this worktree, so gh fails trying to delete it locally. Remote feature branches are kept.
6. **Sync the main checkout.** Find the worktree on `main` via `git worktree list`. If its `git status --porcelain` is empty, `git -C <path> pull --ff-only`; otherwise leave it and tell the user it has local changes.
7. **Report.** `gh pr view <number> --json state,url,mergeCommit` shows `MERGED`; give the user the PR URL and merge commit.
