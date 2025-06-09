# Making The Behaviour Of A Script Sane

Most scripts should have `set -euo pipefail` on the line after the `#!`.

- The `-u` will make sure that the script fails if it tries to use an unset variable
- The `-e` will make sure that the shell running the script will exit whenever a command in the
  script fails
- The `-o pipefail` will make sure that a pipe expression fails if any of the commands along the
  pipe fail

# Variables

- Be aware that a variable that contains a number is actually treated as a string, so `var=147` is
  treated as the string `"147"` .
- To prevent the shell from expanding a variable in all sorts of weird ways, get its value with
  `"$var"` rather than just `$var` . For example,

```bash
var=*
echo $var
```

will print a list all of the files and dirs in the current dir, whereas

```bash
var=*
echo "$var"
```

will just print `*` . To be even more sure and avoid ambiguities, use `"${var}"` ; this way, you can
even do something like

```bash
var=*
echo "${var}yeah"
```

which will print `*yeah` .

## Parameter Expansion

### `${MY_VAR:-backup}`

**Meaning**:

- If `MY_VAR` is **unset or empty**, use `"backup"` as a **temporary value**.
- **Does not modify** `MY_VAR`.

**Example**:

```bash
MY_VAR=""
echo "${MY_VAR:-backup}"   # prints "backup"
echo "$MY_VAR"          # still prints an empty string
```

### `${MY_VAR:=backup}`

**Meaning**:

- If MY_VAR is unset or empty, assign "backup" to MY_VAR and use it.
- Modifies MY_VAR if it was empty or unset.

```bash
unset MY_VAR
echo "${MY_VAR:=backup}"   # prints "backup"
echo "$MY_VAR"          # now prints "backup"
```

### `${MY_VAR+x}`

Expands to `x` if `MY_VAR` is set (even if empty) and to nothing if `MY_VAR` is not set.

# Conditionals

## Basic Syntax

Bash does support `[]` conditional syntax but it often behaves strangely - always use `[[]]`
conditional syntax.

```bash
i=10

if [[ "${i}" -eq 1 ]]; then
    echo "one"
elif [[ "${i}" -eq 2 ]]; then
    echo "two"
else
    echo "other"
fi

```

## Checking If A Var Exists

Use [[#Parameter Expansion]]

```bash
if [[ ${var+x} ]]; then
    echo "var is defined"
fi
```

This is a bit hacky but is the standard, reliable way to check the existence of a variable in
`bash`.

## Switches For Checking Various Things

You will see the condition written as `[[ <modifier> <value> ]]` (if there is no `<modifier>`, then
it just checks that the `<value>` string is nonempty).

Here are some of the basic modifiers:

| Modifier | Meaning                                                                          |
| :------- | :------------------------------------------------------------------------------- |
| `-n`     | Not Empty                                                                        |
| `-z`     | Empty                                                                            |
| `-f`     | File Exists                                                                      |
| `-d`     | Dir Exists                                                                       |
| `-e`     | Path Exists                                                                      |
| `==`     | Equal (supports wildcards)                                                       |
| `=~`     | Regex Match (if the regex is a literal, do not quote it e.g. use a.*b not "a.*b" |

## Wildcards And Regular Expressions

For wildcards and regular expressions, there are some subtleties to the matching behaviour. In
particular, only the right hand side of the `==` resp. `=~` will be treated as a wildcard resp.
regex. Moreover, any wildcard or regex special character that appears inside quotes will be treated
as escaped as far as the matching is concerned; this goes for quoted literals like `"hello.*world"`
and quoted variable expansions like `"$my_var"`, `"${my_var}"`.

# Getting the lengths of variables

To get the length of a variable, use `${#var}` , remembering that numbers will just be treated as
strings, to that

```bash
var=147
echo "${#var}"
```

will print `3` . If the variable is an array and you want its length, you need to use the `[@]`
syntax to refer to the whole array : `"${#array[@]}` .

# Arrays

- Declare an array like this:

```bash
my_empty_array=()
my_filled_array=("foo" "bar" "baz")
```

- Push to an array like this:

```bash
my_array+=("yolo")
```

- Access array elements with the familiar ($0$-based) `[]` syntax: `echo "${my_array[0]}"`

- Loop over an array like this:

```bash
for i in "${my_arr[@]}"; do
    echo "$i"
done
```
