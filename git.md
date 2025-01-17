- Rebase on first commit: `git rebase -i --root`
- Show all files tracked by git: `git ls-files`
- Show stash stats: `git stash show <stash id>`
- Show stash contents: `git stash show -p <stash id>`
- Push your branch to the remote and make sure that the local branch tracks the newly created remote
branch: `git push -u origin <branch-name>`
- Make your local branch track a remote branch:
`git branch --set-upstream-to=origin/<remote-branch-name> <local-branch-name>`
- Show the current branch name with no other information: `git branch --show-current`
