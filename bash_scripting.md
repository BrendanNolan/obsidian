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

will just print `*` . To be even more sure and avoid ambiguities, use `"${var}` ; this way, you can
even do something like

```bash
var=*
echo "${var}yeah"
```

which will print `*yeah` .

# If statements

```bash
i=10

if [ "${i}" -eq 1 ]; then
    echo "one"
elif [ "${i}" -eq 2 ]; then
    echo "two"
else
    echo "other"
fi

```

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
for i in "${!my_arr[@]}"; do
    echo "${my_arr[$i]}"
done
```
