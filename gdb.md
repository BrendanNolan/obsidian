# Dumping To File

Example: dumping the backtrace to a file:

```bash
(gdb) set logging file log.txt
(gdb) set logging enabled on
(gdb) backtrace
(gdb) set logging enabled off
```
