# Macros

## Editing a macro:

- Use `:new` to open a new throwaway buffer
- Use `:put a` to put the contents of the register (`a` in this example) into the buffer
- Edit the macro however you want
- Use `"ayy` to yank the line back into the buffer (`a` in this example)
- Use `:q!` to close the throwaway buffer

## Using a macro from :cdo

- `:cdo normal @a`
