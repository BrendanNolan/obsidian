# Uniquifying Arrays

If you want your array to be uniquified automatically, use `typeset -U my_array` .

# The PATH Env Var

`zsh` will automatically track updates to the `path` array (which is built in; you don't need to
create/declare the `path` array). So, in your config, you can just edit this array to edit your
path, then export the path. As in [[#Uniquifying Arrays]], you can use `typeset -U path` to make
sure the array elements are never duplicated.

```sh
typeset -U path
path+=($HOME/dev/scripts /usr/local/bin)
export PATH

```

# Config Files

Put your core env settings (path, other env vars) in `~/.zshenv` and your interactive settings in
`~/.zshrc` .
