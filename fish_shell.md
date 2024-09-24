[[shell_arguments_and_options]]

# Variables

For setting a variable in a script, I only ever seem to need to set local variables. For example, do
the following to create a local variable whose value is the string hello.

`set -l abc hello`

To check if a variable is set, do this:

```
if set -q abc
      echo "set"
end
```

Local variable scoping seems to be quite strict and sensible to someone coming from `C`. For
example, if you set local variable inside an `if` block, then that variable will be unset when you
leave the `if` block.

Every variable in fish is a list. To get the count of items in the list, run `count $my_var`, you
can use this to check if a variable is empty.

# Conditionals

You can use the `test` keyword and some other specifiers to check for various conditions; here are
some examples from the fish docs (more details here
[Fish Docs On Conditionals](https://fishshell.com/docs/current/cmds/test.html)).

```sh
# If the /tmp directory exists, copy the /etc/motd file to it:
if test -d /tmp
    cp /etc/motd /tmp/motd
end

# If the variable MANPATH is defined and not empty, print the contents.
# (If MANPATH is not defined, then it will expand to zero arguments, unless quoted.)
if test -n "$MANPATH"
    echo $MANPATH
end

# Parentheses and the -o and -a operators can be combined to produce more complicated expressions.
# In this example, success is printed if there is a /foo or /bar file as well as a /baz or /bat
# file.

if test \( -f /foo -o -f /bar \) -a \( -f /baz -o -f /bat \)
    echo Success.
end

# Numerical comparisons will simply fail if one of the operands is not a number:

if test 42 -eq "The answer to life, the universe and everything"
    echo So long and thanks for all the fish # will not be executed
end

# A common comparison is with status:

if test $status -eq 0
    echo "Previous command succeeded"
end

# The previous test can likewise be inverted:

if test ! $status -eq 0
    echo "Previous command failed"
end

# which is logically equivalent to the following:

if test $status -ne 0
    echo "Previous command failed"
end
```

# String Handling

## String Matching

The `string match` function will exit with 0 if it finds at least one match and with 1 otherwise.

Search for a glob pattern:

`string match 'a*b' axxb`

will match and exit with code 0.

Search for a regex:

`string match -r 'a.*b' axxb`

will match and exit with code 0.

## String Escaping

You can escape the regex characters in a string with `string escape --style=regex`:

`string escape --style=regex 'a\b*c'`

will return the string `a\\b\*c`.

# Autoloading functions

If you have a function that you want to be autoloaded so that it is always available to the shell,
you should put that function in a `.fish` file in the `~/.config/fish/functions` directory, where
the `.fish` file has the same name as the function. For example, if you have a function `ll` which
perhaps does some extra processing on top of the `ls` command, you should create the file
`~/.config/fish/functions/ll.fish`, which will look something like this:

```sh
function ll
    ls -lh $argv
end
```
