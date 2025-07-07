# Shortcuts

- Open spotlight: `Cmd + Space`
- Fullscreen an app: `Fn + F`

# Opening Applications

Some applications (like those in `/Applications`) are discovered via the `Launch Services` API. If
you want to launch an appliction with this API (e.g. window managers seem to prefer this to just
running a binary), use `open -a <application name>` . You can provide arguments to the applications
e.g. `open -a Zathura my_file.pdf`
