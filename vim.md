# Macros

## Editing a macro:

- Use `:new` to open a new throwaway buffer
- Use `:put a` to put the contents of the register (`a` in this example) into the buffer
- Edit the macro however you want
- Use `"ayy` to yank the line back into the buffer (`a` in this example)
- Use `:q!` to close the throwaway buffer

## Using a macro from :cdo

- `:cdo normal @a`

# Git

## Resolving Conflicts With vim fugitive

- Open the file in which you wish to resolve conflicts
- Run `:Gvdiffsplit!`
- You will see files open either side of your file, one from each involved branch
- Put the cursor in the middle file (the "working copy"), on the conflict that you want to resolve.
- Type `:diffget` (without hitting `Enter`) and look at the autocomplete options:
  - One will be from a temporary file in the `left` buffer whose name containts a `2`; it represents
    the file as it exists in the `HEAD` commit - for which commit this really means, see
    [[git#Rebasing|git rebase]]
  - The other will be from a temporary file in the `right` buffer whose name contains a `3`; it
    represents the file as it exists the "other" branch, namely the branch that `HEAD` is not
    referring to - for which commit this really means see [[git#Rebasing|git rebase]]
  - Choose whichever you want and notice that the conflict hunk where the cursor sits in your
    current copy has been replaced by the hunk that you wanted
