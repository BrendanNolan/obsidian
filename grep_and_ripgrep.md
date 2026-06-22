# Context

To get surrounding lines of context:

- `rg -A 3 ...` gives the 3 lines after the match
- `rg -B 3 ...` gives the 3 lines before the match
- `rg -C 3 ...` gives the 3 lines before _and_ after the match

# Show Only Matching

- With colour: `rg -o foo bar.txt`
- Without colour: `rg -o --color=never foo bar.txt`

# Excluding Dirs

Use the `--exclude-dir` option. E.g. `grep -r --exclude-dir=.git 'foo'` . If you provide several
`--exclude-dir` options, they will be anded together:
`grep -r --exclude-dir=.git --exclude-dir=build 'foo'` ; you can also use [[bash#brace expansion]]
to get the same thing: `grep -r --exclude-dir={.git,build} 'foo'`

# Matching Several Lines

Use `grep -Pzo`

- `-P` for Perl regex matching (strict superset of extended regex matching)
- `-z` for treating the whole file as one big old null-separated string
- `-o` for reporting only the matches (needed to stop any match reporting the whole file, since it
  is being treated as one line thanks to the `-z` option)

E.g.

```bash
printf '\n\nhello\n\n' | grep -Pzo '\nhello\n'
```
