- Rebase on first commit: `git rebase -i --root`
- Delete remote branch: `git push origin --delete <branch-name>`
- Show all files tracked by git: `git ls-files`
- Show stash stats: `git stash show <stash id>`
- Show stash contents: `git stash show -p <stash id>`
- Push your branch to the remote and make sure that the local branch tracks the newly created remote
  branch: `git push -u origin <branch-name>`
- Make your local branch track a remote branch:
  `git branch --set-upstream-to=origin/<remote-branch-name> <local-branch-name>`
- Show the current branch name with no other information: `git branch --show-current`
- Show stats (changed lines in changed files):
  - for a range of commits: `git diff --stat <commit_a> <commit_b>`
  - for a commit `git show --stat <commit>`

# Rebasing

While you are rebasing, you are successively applying commits from `feature` on top of `master`.
Thus, during the rebase, `HEAD` will refer to the last commit from `feature` that you successfully
rebased on top of `master` (possibly after resolving conflicts) or, if you have not yet successfully
rebased a commit from `feature` on top of `master`, `HEAD` will refer to the top commit of `master`
itself. Keep this in mind when resolving conflicts, e.g. with
[[vim## Resolving Conflicts With vim fugitive|vim fugitive]].
