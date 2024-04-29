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

# String Matching

The `string match` function will exit with 0 if it finds at least one match and with 1 otherwise.

Search for a glob pattern:

`string match 'a*b' axxb`

will match and exit with code 0.

Search for a regex:

`string match -r 'a.*b' axxb`

will match and exit with code 0.

# String Escaping

You can escape the regex characters in a string with `string escape --style=regex`:

`string escape --style=regex 'a\b*c'`

will return the string `a\\b\*c`.
