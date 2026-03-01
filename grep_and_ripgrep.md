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
