# Why use it in preference to xargs?

- GNU parallel has more extensive features and, very handily, has a `--dry-run` option to show you
  what it would run.
- GNU parallel reads input line by line, so that it is not confused by file names with spaces etc.
- GNU parallel runs commands in parallel (on as many cores as are available) by default.

# Basic Syntax

The basic syntax is very similar to `xargs`:

`fd -t f | parallel --dry-run sed -i "s/foo/bar/g" {}`
