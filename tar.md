# Arguments

- If you are compressing, the arguments are the files/directories that you want to compress
- If you are extracting, the arguments refer to files/directories inside the archive (you will
  probably never need this)

# Switches

| Switch/Option | Meaning | Description                                                              |
| ------------- | ------- | ------------------------------------------------------------------------ |
| `x`           | extract | Extract files from an archive                                            |
| `c`           | create  | Create a new archive                                                     |
| `v`           | verbose | Show progress / list files as they are processed                         |
| `f`           | file    | Specify the archive file name (must be followed by filename)             |
| `t`           | list    | Show the contents of the archive (use with `f`, so `tar -tf myfile.tar`) |
| `z`           | gzip    | Filter archive through `gzip` (`.tar.gz`)                                |
| `j`           | bzip2   | Filter archive through `bzip2` (`.tar.bz2`)                              |
| `J`           | xz      | Filter archive through `xz` (`.tar.xz`)                                  |
