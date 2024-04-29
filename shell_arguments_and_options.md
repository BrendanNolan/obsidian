# Arguments Vs Options

`Arguments` are positional and `options` (also called `flags` or `switches`) are passed with names
and perhaps values (if the option is boolean, it has no value, it is just either present or absent).

We will use the `fd` utility as our example here, but most of this stuff should apply in general.

# Examples of arguments and options to `fd`

The help page for `fd` says the following:

```
A program to find entries in your filesystem

Usage: fd [OPTIONS] [pattern] [path]...

Arguments:
  [pattern]  the search pattern (a regular expression, unless '--glob' is used; optional)
  [path]...  the root directories for the filesystem search (optional)

Options:
  -H, --hidden                     Search hidden files and directories
  -I, --no-ignore                  Do not respect .(git|fd)ignore files
  ...
```

So in the command `fd -t f -g "a*c" my_dir`, we have the `-t` option which has a value, the `-g`
option which has no value, and the positional arguments `pattern` and `path`.

# How the shell interprets arguments

Note that there is only one sensible way that the arguments can be interpreted: the $n^{th}$
argument passed on the commandline will be matched to the $n^{th}$ argument of the command, and all
further arguments will also be matched to the last argument of the command.

For example, let us take the example of the `pattern` and `path` arguments to the `fd` command. To
make it interesting, both arguments are optional, but again, there is still only one sensible way
that the shell can interpret arguments.

- If you provide no arguments (please not that we are talking about arguments here, not options),
  then `pattern` will get a default value that matches everything and `path` will get a default
  value which is the current dir.

- If you provide only one argument, then it will be interpreted as the `pattern`, with the `path`
  again defaulting to the current dir.

- If you provide two or more arguments, then the first will be interpreted as the `pattern` and the
  rest will be interpreted as `path`(s).
