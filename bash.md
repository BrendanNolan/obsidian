# The Very Basics Of Quoting

If a string has no special characters (spaces, globs, etc.), then it makes no difference if you
quote it or not; the following are the same:

```bash
a="foo"
```

```bash
a=foo
```

# Making The Behaviour Of A Script Sane

Most scripts should have `set -euo pipefail` on the line after the `#!`.

- The `-u` will make sure that the script fails if it tries to use an unset variable
- The `-e` will make sure that the shell running the script will exit whenever a command in the
  script fails
- The `-o pipefail` will make sure that a pipe expression fails if any of the commands along the
  pipe fail

# Positional Arguments

- The special variable `#` holds the number of positional arguments. Example usage:
  `echo "received $# arguments`

# Quoting

Bash will treat text inside single quotes as literal text and will not e.g. expand variables. In
double quotes, bash will expand variables. I am sure there is more to this but this is the basic
picture.

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

if [[ "a" == "b" ]]; then
    echo "first"
elif [[ "a" == "a" ]]; then
    echo "second"
else
    echo "other"
fi

```

## Logical Operators

Use `||`, `&&` etc. for logical operators inside and outside `[[]]` ; the only exception seems to be
inside `[]`, but in bash there is no real reason to use `[]` .

### Precedence

## Grouping

### Grouping Inside `[[]]`

Use escaped parens:

```bash
[[ \( foo && bar \) || baz ]]
```

### Grouping Outside `[[]]`

Run the command in the current shell like this (note the trailing semicolon, you must close the
braces with this or a newline):

```bash
{ foo && bar ; } || baz
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
| `==`     | Strings Are Equal (supports wildcards)                                           |
| `=~`     | Regex Match (if the regex is a literal, do not quote it e.g. use a.*b not "a.*b" |
| `-gt`    | > (for numbers)                                                                  |
| `-ge`    | >= (for numbers)                                                                 |
| `-lt`    | < (for numbers)                                                                  |
| `-le`    | <= (for numbers)                                                                 |
| `-eq`    | == (for numbers)                                                                 |
| `-ne`    | != (for numbers)                                                                 |

## Wildcards And Regular Expressions

For wildcards and regular expressions, there are some subtleties to the matching behaviour. In
particular, only the right hand side of the `==` resp. `=~` will be treated as a wildcard resp.
regex. Moreover, any wildcard or regex special character that appears inside quotes will be treated
as escaped as far as the matching is concerned; this goes for quoted literals like `"hello.*world"`
and quoted variable expansions like `"$my_var"`, `"${my_var}"`.

## A sample script to tie the above together

```bash
#! /usr/bin/env bash

EMP=""
[[ -z ${EMP} ]] && echo "EMP is empty"
NONEMP="hello"
[[ -n ${NONEMP} ]] && echo "NONEMP is not empty"
[[ -f "$HOME/.tmux.conf" ]] && echo "File ~/.tmux.conf exists"
[[ -d "$HOME" ]] && echo "Dir ~ exists"
[[ -e "$HOME" ]] && echo "Path ~ exists"
[[ ${NONEMP} == he*o ]] && echo "Inline wildcard matches"
[[ ${NONEMP} =~ he.*o ]] && echo "Inline regex matches"
HELLO_WILDCARD="he*o"
HELLO_REGEX="he.*o"
[[ ${NONEMP} == ${HELLO_WILDCARD} ]] && echo "Expanded wildcard matches"
[[ ${NONEMP} =~ ${HELLO_REGEX} ]] && echo "Expanded regex matches"
[[ ${NONEMP} == "${HELLO_WILDCARD}" ]] || echo "Quoted/escaped wildcard does not match"
[[ ${NONEMP} =~ "${HELLO_REGEX}" ]] || echo "Quoted/escaped regex does not match"
[[ ${NONEMP} == "he*o" ]] || echo "Quoted/escaped wildcard does not match"
[[ ${NONEMP} =~ "he.*o" ]] || echo "Quoted/escaped regex does not match"


```

# Getting the lengths of variables

To get the length of a variable, use `${#var}` , remembering that numbers will just be treated as
strings, to that

```bash
var=147
echo "${#var}"
```

will print `3` . If the variable is an array and you want its length, you need to use the `[@]`
syntax to refer to the whole array : `"${#array[@]}` (see [[#Arrays]]) .

# Command Substitution

You can pass the output of one command to another command as follows:
`current_branch=$(git branch --show-current)`

# Process Substitution

You can treat the output of a command like a file and pass it to a command that expects a file as
follows: `cat <(find . -type f)`

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

- Append one bash array to another like this:

```bash
my_array+=("${my_other_array[@]}")
```

- Access array elements with the familiar ($0$-based) `[]` syntax: `echo "${my_array[0]}"`
- You will only get the first array element if you write `${my_array}` ; to refer to the whole
  array, you need `${my_array[@]}` .
- Loop over an array like this:

```bash
for i in "${my_arr[@]}"; do
    echo "$i"
done
```

## Creating Arrays

- Raw: `my_filled_array=("foo" "bar" "baz")`
- From a command: `my_git_files=($(git ls-files))`
- The `readarray` command, which reads lines from a file standard input into a bash array (you will
  usually want the `-t` switch here, to tell `readarray` to strip the trailing newline character
  from all incoming lines).
  - Example: You can use [[#Process Substitution]] to treat the output of `git ls-files` as a file
    and then pass it to `readarray`: `readarray -t my_git_files < <(git ls-files)` .
  - **Question**: Why not just pipe to `readarray`? **Answer**: In this case, `readarray` will run
    in a subshell and you won't actually have the array in the calling shell.

# Here String

The "here string" triple cheveron syntax is for passing strings directly to commands via std in e.g.
`command <<< "$my_var"` .

# String Manipulation

## Removing Substrings

Based on some simple globbing rules, you can remove substrings from strings (to create new strings,
rather than modify existing strings in place) using they following operators:

| Operator | Action                                         |
| -------- | ---------------------------------------------- |
| `#`      | Remove shortest match from beginning of string |
| `##`     | Remove longest match from beginning of string  |
| `%`      | Remove shortest match from end of string       |
| `%%`     | Remove longest match from end of string        |


```bash
#!/usr/bin/env bash

foo="abcdeabcde"
echo "${foo#a*}"    # prints bcdeabcde
echo "${foo##a*}"   # prints nothing
echo "${foo%e*}"    # prints abcdeabcd
echo "${foo%%e*}"   # prints abcda
```

## Splitting Strings On Characters

The `read` builtin (usually used with the `-r` switch which says "do not allow backslashes to escape
any characters") expects input from `stdin` and splits it on the `IFS` . See this example (which
uses the [[#Here String]] syntax):

```bash
stuff="hello,world"
IFS=',' read -ra hello_and_world <<< "$stuff"
# This creates the array hello_and_world to look like: ("hello" "world")
```

# Suppressing Failures

Suppose you want to run a command like this: `git ls-files | parallel chmod u-w {}` and you don't
want it to fail (return a nonzero exit code) even if some of the individual `chmod` calls failed.
You can force the whole thing to return a zero exit code just by adding `|| true`:
`git ls-files | parallel 'chmod u-w {} || true'` See also [[gnu_parallel#Suppressing Failures]]

# Exit Codes

If you want the exit status of the last command, it is stored in the special variable `?`, so you
can get it with `$?` .

# Streams

Here is a stream-agnostic list of all major Bash stream redirection syntaxes, using `n` as the file
descriptor  
(defaults: `0 == stdin`, `1 == stdout`, `2 == stderr`):

## Redirect to File

| Syntax     | Meaning                             |
| ---------- | ----------------------------------- |
| `n> file`  | Redirect output to file (overwrite) |
| `n>> file` | Redirect output to file (append)    |
| `n< file`  | Redirect input from file            |

## Redirect Between File Descriptors

| Syntax | Meaning                           |
| ------ | --------------------------------- |
| `n>&m` | Redirect `n` to wherever `m` goes |
| `n<&m` | Redirect `n` to read from `m`     |
| `n>&-` | Close output fd `n`               |
| `n<&-` | Close input fd `n`                |

## Here Docs

| Syntax   | Meaning                                                                      |
| -------- | ---------------------------------------------------------------------------- |
| `n<<EOF` | Pass multiline input until EOF. Format exactly as in [[###Here Doc Example]] |

### Here Doc Example:

```bash
cat <<EOF
This is a here doc.
EOF
```

### Here Doc Expansions

**Heredocs** act like **double-quoted strings** unless the **delimiter is quoted** (e.g. `'EOF'`).

- Unquoted delimiter -> expansions occur (`$var`, `$(...)`, etc.)
- Quoted delimiter -> no expansions; content is treated literally

## Here Strings

| Syntax          | Meaning                                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| `n<<< "string"` | Pass a single string as input (string need not be a literal, it can expand variables like `n<<< "${my_var}`) |

### Here String Expansions

**Herestrings** (`<<<`) are **always expanded**, as if the input were **double-quoted**, even if you
use `'text'`. This means variable and command substitution still occur

## Pipes

Pass stdout of one command to stdin of another (the piped-to command will run in a subshell).

## Process Substitution (Bash-specific)

| Syntax          | Meaning                              |
| --------------- | ------------------------------------ |
| `n> >(command)` | Redirect output to `command`'s input |
| `n< <(command)` | Use `command`'s output as input      |

# While Loops

## "while read" Loops

You can stream data into a `while read` loop in various ways (see [[#Streams]] for details on
streams).

```bash
while IFS=, read -r x y; do
    echo "$x and $y"
done <<< "a,A
b,B
c,C"
```

# Globbing

- `*` matches any string of non-`/` characters, so it will match within a single directory level.
- `**` matches any string of characters, but only if the shell option `globstar` is enabled
  (`shopt -s globstar`). Note that, in `zsh`, this behaviour is enabled by default and there is not
  shell option `globstar` .

Globbing will work with several different glob strings (with will be `OR`ed together). E.g.
`ls *md *txt` ([[#Brace Expansion]] is often useful here)

## Period Characters

The `.` character has no special meaning in globs

## Unmatched Globs

An unmatched glob will stay as the literal string. E.g. if there are no `.md` files, `*.md` will
expand to the literal string `*md`. If you set the `nullglob` shell option (`shopt -s nullglob`),
then an unmatched glob will expand to nothing.

# Brace expansion

The shell will expand something like `a{12}b` to `a1b a2b` . This is useful in globbing, since you
can list all `.md` and `.txt` file in a dir by running `ls *.{md,txt}` . Note that if you quote
around the `{}`, it will not expand.
