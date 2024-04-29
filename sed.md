[[regular_expressions]] [[shell_arguments_and_options]]

# Using Extended Regular Expression Syntax

Use the `-E` or `-r` options.

# Delete All Lines That Match A Regular Expression

Use the `/<match_text>/d` command to delete matching lines and the `/<match_text>/Id` command to
delete case-insensitively matching lines.

For example, to delete all commented lines in a python file, run `sed -i "/^\s*#/d" my_file.py` .

As another example, `printf "abc\nac\naxc\nZABCD" | sed "/b/d"` will print

```
ac
axc
ZABCD
```

while `printf "abc\nac\naxc\nZABCD" | sed "/b/Id"` will print

```
ac
axc
```

# Special Match Characters

If you use `&` in the replace string, it refers to the entire matched text. For example,
`printf "abc" | sed 's/ab/&X/g` will print `abXc`.

If you use `\1`, ... , `\9` in the replace string, it will refer to the corresponding capture group
match from the regex. For example, `printf "abc" | sed 's#\(a\)\(b\)\(c\)#X\1Y\2Z\3#g` will print
`XaYbZc` . See [[regular_expressions#Capture Groups]] for more details on capture groups.

You can clip the end off every line by having the first capture group match everything except the
last character: `sed 's#\(.*\).#\1#g` .
