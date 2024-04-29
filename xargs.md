[[shell_arguments_and_options]]

# Xargs

Let us use the terms "source command" and "target command" in the obvious way.

## Provide input one line at a time

`xargs` will by default provide the whole output of the source command to the target command all at
once. This is sometimes fine but is a problem if the target command cannot accept several lines of
input at once; in this case, the `-n1` flag will tell `xargs` to provide one line at a time from the
source command to the target command. For example, if we want to run `host -t A` on every IP address
coming from the `cat hostnames` command, we can do this:

`cat hostnames | xargs -n1 host -t A`

## Passing input text in the middle of the target command

Now, what if we want to provide the output of the host command to the target command _but_ we do not
want it to appear at the end of the command? Sticking with the above example, let's say we want to
pass a second optional argument (a custom DNS server) to the `host` command. Well, we can do this
with the `-I` option:

`cat hostnames | xargs -I{} host -t A {} 8.8.8.8`

The `-I` option allows you to put a placeholder in the target command which will be filled with the
output of the source command. In this case, you do not need the `-n1` option, because the `-I`
option already implies that you want to pass input to the target command one line at a time. You can
use other separators (not just the `{}` as above) - just choose something that is not going to
appear in the target command.

## Running target commands in parallel

We can tell `xargs` to parallelise its execution with the `-P` flag, e.g. for running on 4 cores:

`cat hostnames | xargs -I{} -P4 host -t A {} 8.8.8.8`

## Using pipes in the target command

The command

`cat hostnames | xargs -I{} -P4 host -t A {} 8.8.8.8`

will print a lot of output that we do not really care about. Suppose we want only the last line of
every invokation of the `host` command. Well, we could try this

`cat hostnames | xargs -I{} -P4 host -t A {} 8.8.8.8 | tail -n1`

but this will just print the last line of the last invokation of the `host` command, because the
shell just runs the piped commands one after another. To get over this, let us run the target
command in its own shell:

`cat hostnames | xargs -I{} -P4 sh -c "host -t A {} 8.8.8.8 | tail -n1"`

N.B. the `sh -c` command is not particular to `xargs`; it is just a way to run a command in a new
shell, so you can even (tautologically) do something like `sh -c "ls"` instead of `ls` .
