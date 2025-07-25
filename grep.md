# Excluding Dirs

Use the `--exclude-dir` option. E.g. `grep -r --exclude-dir=.git 'foo'` . If you provide several
`--exclude-dir` options, they will be anded together:
`grep -r --exclude-dir=.git --exclude-dir=build 'foo'` ; you can also use [[bash#brace expansion]]
to get the same thing: `grep -r --exclude-dir={.git,build} 'foo'`
