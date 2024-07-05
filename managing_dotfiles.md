# GNU Stow (Available on Linux and macOS)

- First, create a repo called `dotfiles` in your home directory and, inside it, create the directory
  structure and dotfiles that you want; later, you will use stow to create symlinks to these files
  from the places in your home dir where those files are expected to live. So, if you want to have
  your `.zshrc` file at `~/.zshrc`, then you should have a file at `~/dotfiles/.zshrc`; if you want
  to have your `init.vim` file at `~/.config/vim/init.vim`, then you should have a file at
  `~/dotfiles/.config/vim/init.vim`.
- Delete the dotfiles from your home directory, if they exist.
- From the `~/dotfiles` directory, run `stow --no-folding .` to create the desired symlinks (the
  `--no-folding` option tells `stow` not to create dir symlinks (you probably only want to symlink
  files, and only the files that you really care about). Now, `~/.config/vim/init.vim` will be a
  symlink to `~/dotfiles/.config/vim/init.vim`, and so on.
- You are ready to go. You can use your `~/dotfiles` as any other git repo.
- If you want to add or remove a dotfile, just add/remove it in the `~/dotfiles` directory and then,
  from the `~/dotfiles` directory, run `stow -D . && stow --no-folding .` . This will remove all the
  stow symlinks and then re-add the symlinks to whatever is still in your `~/dotfiles` repo; for me,
  this is the easiest way to make sure that everything stays in sync and nothing goes wrong.
