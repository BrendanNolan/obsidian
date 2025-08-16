# Shortcuts

- Open spotlight: `Cmd + Space`
- Fullscreen an app: `Fn + F`

# Opening Applications

Some applications (like those in `/Applications`) are discovered via the `Launch Services` API. If
you want to launch an appliction with this API (e.g. window managers seem to prefer this to just
running a binary), use `open -a <application name>` . You can provide arguments to the applications
e.g. `open -a Zathura my_file.pdf`

# Homebrew

To dump all of your installed stuff to a file which can be used to reinstall it:

```bash
brew bundle dump --all --file=<dump_file>
```

To ask brew to install everything from a dumped file:

```bash
brew bundle --file=<dump_file>
```
