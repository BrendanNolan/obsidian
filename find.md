# Why Use Instead Of fd ?

`find` has more powerful query language than `fd`

## Query Language

### String Matching

- `-path` : match the whole path
- `-name` : match the file base name

### Logical Operators

- `!` : NOT
- `-o` : OR
- `-a` : AND

### Example

Find all the `.h` and `.c` files in the current dir, except those in the `build` and `.git` dirs.
`find . \( -name "*.c" -o -name "*.h" \) ! \( -path "./build/*" -o -path "./.git/*" \)`
