# Repeating Inserts

- In normal mode, just type the number, followed by what you wish to insert, followed by escape. For
  example, to insert `10` `space` characters, you can do the following in `normal` mode:
  `10i<Space><Esc>` .

# Incrementing/Decrementing En-Masse

- Hilight the range of numbers you want to increment (e.g. you have ten lines that all just contain
  `0`) and then type `g<Ctrl-a>`. (Use `g<Ctrl-x>` for decrementing).

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
- Use `0"ay$` to yank the line back into the buffer (`a` in this example)
- Use `:q!` to close the throwaway buffer

## Using a macro from :cdo

- `:cdo normal @a`

# Reading Shell Command Output Into A Vim Buffer

- To place the output of a shell command into the current buffer on the line below the cursor, run
  `:r!<shell_command>` .
- If you want to do the same thing but replace the current line (rather than inserting below it),
  use `:.!<shell_command>`

# Git

## Resolving Conflicts With vim fugitive

- Open the file in which you wish to resolve conflicts
- Run `:Gvdiffsplit!`
- You will see files open either side of your file, one from each involved branch
- Put the cursor in the middle file (the "working copy"), on the conflict that you want to resolve.
- Hit `:diffget //2` to choose the `ours` option and type `:diffget //3` to choose the `theirs`
  option (for the meanings of these options, see [[git## Ours and Theirs| git ours and theirs]])
- After `:diffget ...`, notice that the conflict hunk where the cursor sits in your current copy has
  been replaced by the hunk that you wanted

# Useful General Commands

- Try this handy little repo: [actaneon/VimCommands.txt](https://gist.github.com/actaneon/366070)

# Quickfix

- `:cdo` will apply an action to every **entry** in the quickfix list.
- `:cfdo` will apply an action to every **file** in the quickfix list.

# Make Command

Use

`:set makeprg=<your build command>`

in order that you can use the `:make` command in vim to run your command and see the output in the
quickfix list. If your command has spaces, remember to escape them:

`:set makeprg=cargo\ build`
