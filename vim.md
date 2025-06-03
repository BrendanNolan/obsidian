# Repeating Inserts

- In normal mode, just type the number, followed by what you wish to insert, followed by escape. For
  example, to insert `10` `space` characters, you can do the following in `normal` mode:
  `10i<Space><Esc>` .

# Incrementing/Decrementing En-Masse

- Hilight the range of numbers you want to increment (e.g. you have ten lines that all just contain
  `0`) and then type `g<Ctrl-a>`.

# Marks

- To make a mark, just type `m` followed by a capital letter naming the mark (you can type a small
  letter but the mark will then be local to the file). To go to the mark, type a backtick followed
  by the mark name.

# Macros

## Editing a macro:

- Use `:new` to open a new throwaway buffer
- Use `"ap` to put the contents of the register (`a` in this example) into the buffer
- Edit the macro however you want. In particular, if you wish to add keysequences like `Enter` or
  `Ctrl-x` etc., you can type `Ctrl-v` (in insert mode) and then type key sequence you want and vim
  will add the appropriate escape sequence.
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
    the file as it exists in the `HEAD` commit - to understand which commit this really means, see
    [[git#Rebasing|git rebase]]
  - The other will be from a temporary file in the `right` buffer whose name contains a `3`; it
    represents the file as it exists the "other" branch, namely the branch that `HEAD` is not
    referring to - to understand which commit this really means see [[git#Rebasing|git rebase]]
- From these `:diffget` autocomplete options, choose whichever one you want to resolve the conflict
  and hit `Enter`; notice that the conflict hunk where the cursor sits in your current copy has been
  replaced by the hunk that you wanted

# Useful General Commands

- Try this handy little repo: [actaneon/VimCommands.txt](https://gist.github.com/actaneon/366070)
