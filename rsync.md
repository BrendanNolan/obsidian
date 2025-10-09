# Purpose

The `rsync` command is for transferring dirs/files between machines over `ssh` (or even within the
same machine, obviously not over `ssh`).

# Example Usage

`rsync -raPvh <local_dir_or_file> <target_user>@<target_ip_or_machine_name>:<target_dir_or_file>`

If you want to use a machine name rather than an IP address, then the machine name needs to be
understood by `ssh` (usually this means that it is listed as a host in your `~/.ssh` directory).

# Warning!

Do not place trailing slashes on directory names unless you want rsync to dump the contents of the
dir directly into the target directory without creating an equivalently named directory on the
target machine.

# Flags

- Usually you want to use `rsyc -raPvh`
  - `-r` recursively copy stuff
  - `-a` preserve file permissions, metadata etc.
  - `-P` combines the `--partial` and `--progress` options, which respectively allow partial file
    transfer and show a progress indicator
  - `-v` verbose mode
  - `-h` show human-readable file sizes
