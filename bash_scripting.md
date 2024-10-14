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

# String matching

## Straightforward Matching

If you want to check a string for equality, just use the familiar `==` operator:

```bash
st="why hello world you wild thing"
if [[ $st == "hello world" ]]; then
    echo "match"  # will not print
fi
```

## Wildcards

The `==` operator even supports wildcards

```bash
st="why hello world you wild thing"
if [[ $st == *"hello world"* ]]; then
    echo "sub match"  # will print
fi
```

## Regular Expressions

For regexes, use the `=~` operator

- If your are writing your regex as a literal, do not quote it.

```bash
st="hello world"
if [[ $st =~ he.*wo.*d ]]; then
    echo "match"
fi
```

- If your regex comes from a variable, use the `$` syntax as normal.

```bash
st="hello world"
regex="he.*wo.*d"
if [[ $st =~ $regex ]]; then
    echo "match"
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
for i in "${my_arr[@]}"; do
    echo "$i"
done
```
