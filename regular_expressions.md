# Signifying The Start Or End Of A Line

- `^` signifies the start of a line
- `$` signifies the end of a line

# Matching One Or Other

- `abc|xyz` will match both `abc` and `xyz` .

# Single Character Matching

- `.` matches any character
- `\d` matches any digit character (seems not to work with `sed`)
- `\D` matches any NON-digit character (seems not to work with `sed`)
- `\s` matches any whitespace character
- `\S` matches any NON-whitespace character
- `\w` matches any alphanumeric character
- `\W` matches any NON-alphanumeric character
- `[abc]` matches any single character which IS one of `a`, `b`, or `c`
- `[^abc]` matches any single character which IS NOT one of `a`, `b`, or `c`
- `[A-C]` matches any single character in the range `A,B,C`
- `[^A-C]` matches any single character NOT in the range `A,B,C`
- You can concatenate ranges and append single matches to them. For example, `[A-Za-z0-9]` matches
  any alphanumeric character (exactly the same as `\w` does) and `[A-Za-z0-9@]` matches any
  character which is alphanumeric or is the @ symbol.

# Capture Groups

- Capture groups can be used to match text and capture part of the text for further processing. Use
  the parenthesis syntax for capture groups. For example, the following `sed` command will have
  `abc` as its first match group and will then use it in the replace string
  `printf "xxabcxx" | sed -E "s/(abc)/\1Z/"`, printing `xxabcZxx` .
- You can also have capture subgroups. Generally, the results of the captured groups are in the
  order in which they are defined (in order by open parenthesis). For example, the regular
  expression `(Jan (1987))(x)` will match "Jan 1987x" with the first capture group being `Jan 1987`,
  the second being `1987`, and the third being `x`.

# Repetitions

- `*` matches zero or more of the preceding character or group
- `+` matches one or more of the preceding character or group
- `?` matches zero or one of the preceding character or group (i.e. it makes the preceding character
  or group _optional_)
- `{3}` matches exactly 3 of the preceding character or group
- `{3,9}` matches between 3 and 9 of the preceding character or group
- `{3,}` matches 3 or more of the preceding character or group
