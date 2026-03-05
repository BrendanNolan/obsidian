# Finding the app ids

You may want app ids e.g. for listening for the opening of windows:

```toml
[[on-window-detected]]
if.app-id = 'com.github.wez.wezterm'
run = 'move-node-to-workspace 1'
```

To find these, run

```bash
aerospace list-windows --all --format '%{app-bundle-id} %{app-name}' | rg -i '<whatever>'
```
