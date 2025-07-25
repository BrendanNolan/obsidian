# Excluding Dirs

Use the `--exclude-dir` option. E.g.

- `grep -r --exclude-dir=.git 'foo'`
- `grep -r --exclude-dir={.git,build} 'foo'`
- `grep -r --exclude-dir=.git --exclude-dir=build 'foo'`
