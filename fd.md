[[regular_expressions]]

# From the help page

```
Usage: fd [OPTIONS] [pattern] [path]...
```

# Search patterns

By default, the pattern will be matched as a regex: `fd "bren.*nolan" my_dir` . If instead you want
it to be matched as a glob, use the `-g` option: `fd -g "bren*nolan" my_dir` .

# The optionalness of pattern and path

If you provide no arguments (please not that we are talking about arguments here, not options), then
`pattern` will get a default value that matches everything and `path` will get a default value which
is the current dir. BUT if you provide only one argument, then it will be interpreted as the
`pattern` argument, with the `path` argument again defaulting to the current dir. This is pretty
standard behaviour for any unix-style commandline utility. See [[shell_arguments_and_options]] for
more details.
