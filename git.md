# Brief Descriptions of Useful Commands

- Rebase on first commit: `git rebase -i --root`
- Delete remote branch: `git push origin --delete <branch-name>`
- Show all files tracked by git: `git ls-files` (add the `-z` switch to separate them with a null
  byte)
- Stash

  - show stash stats: `git stash show <stash id>`
  - show stash contents: `git stash show -p <stash id>`
  - stash the current changes without changing the files back to their HEAD state:

    ```bash
    git stash store -m "<stash message>" $(git stash create)
    ```

- Push your branch to the remote and make sure that the local branch tracks the newly created remote
  branch: `git push -u origin <branch-name>`
- Make your local branch track a remote branch:
  `git branch --set-upstream-to=origin/<remote-branch-name> <local-branch-name>`
- Show the current branch name with no other information: `git branch --show-current`
- Show stats (changed lines in changed files):
  - for a range of commits: `git diff --stat <commit_a> <commit_b>`
  - for a commit `git show --stat <commit>`
- Make an empty commit: `git commit --allow-empty -m "commit message"`

# Rebasing

While you are rebasing, you are successively applying commits from `feature` on top of `master`.
Thus, during the rebase, `HEAD` will refer to the last commit from `feature` that you successfully
rebased on top of `master` (possibly after resolving conflicts) or, if you have not yet successfully
rebased a commit from `feature` on top of `master`, `HEAD` will refer to the top commit of `master`
itself. Keep this in mind when resolving conflicts, e.g. with
[[vim## Resolving Conflicts With vim fugitive|vim fugitive]].

# Stop Tracking A File

```bash
git rm --cached my_file.txt # Stop tracking the file
echo 'my_file.txt' >> .gitignore
```

# Hooks

Hooks are essentially custom scripts that run automatically after a certain `git` command runs. For
example, if you want to run a script after a commit, put that script in the file
`.git/hooks/post-commit` (you may need to create this file) and make sure that
`.git/hooks/post-commit` is executable. (For an example, see my `dotfiles` repo, for a post-commit
hook that reruns the GNU `stow` utility to make sure that all of my dotfile symlinks are in order).

# git diff --diff-filter

The `--diff-filter=<character>` switch will filter git changes according to the following rules:

| Character | Meaning      |
| --------- | ------------ |
| A         | Added        |
| C         | Copied       |
| D         | Deleted      |
| M         | Modified     |
| R         | Renamed      |
| T         | Type Changed |
| U         | Unmerged     |
| X         | Unknown      |
| B         | Broken       |

# Merging

- If you want the merge to go through if and only if it is a fast-forward (i.e. the current HEAD of
  `master` is an ancestor of the current HEAD of `feature`), then run

```bash
git checkout master
git merge --ff-only feature
```

# Gitignore

## Gitignore Files

If a pattern is specified in any of the following files, git will ignore matching files:

- Repository `.gitignore` - applies to whole repository
- `.gitignore` in subdirectories - patterns apply to that directory and below
- Global ignore (`~/.config/git/ignore`) - applies to all your repos

## Gitignore Syntax

### Basic Patterns

| Pattern     | Meaning                                                   |
| ----------- | --------------------------------------------------------- |
| `file.txt`  | Ignore `file.txt` in any directory                        |
| `/file.txt` | Ignore `file.txt` only in the root directory              |
| `dir/`      | Ignore any directory `dir` (and all its contents)         |
| `/dir/`     | Ignore the directory `dir` (and all its contents) in root |
| `*.log`     | Ignore all files ending in `.log`                         |

### Wildcards

| Wildcard | Meaning                                                       |
| -------- | ------------------------------------------------------------- |
| `*`      | Matches any characters but does not span directory boundaries |
| `**`     | Matches any characters and does span directory boundaries     |
| `?`      | Matches exactly one character                                 |
| `[abc]`  | Matches any character in the brackets                         |
| `[0-9]`  | Matches any character in the range                            |

### Examples

```gitignore
# Comments start with #

# Ignore all .log files
*.log

# Ignore node_modules anywhere
node_modules/

# Ignore build directory in root only
/build/

# Ignore all .txt files in doc/ directory (not subdirectories)
doc/*.txt

# Ignore all .pdf files in doc/ and its subdirectories
doc/**/*.pdf

# Ignore files named temp with any single-char extension
temp.?

# Negation: track this file even if ignored above
!important.log
```

### Key Rules

1. **Trailing slash** (`dir/`) matches only directories
2. **Leading slash** (`/file`) anchors to the repository root
3. **Negation** (`!pattern`) re-includes previously ignored files
4. **Later rules override earlier ones** in the same file
5. **Blank lines** are ignored
6. **`#`** starts a comment (use `\#` for literal `#`)

### Double-star patterns

- `**/foo` -- matches `foo` anywhere
- `foo/**` -- matches everything inside `foo/`
- `a/**/b` -- matches `a/b`, `a/x/b`, `a/x/y/b`, etc.
