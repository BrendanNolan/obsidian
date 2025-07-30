# Suppressing warnings

## Add A Comment In A Script

Ensure there is no code (there may be comments) between the shebang and the `shellcheck disable`
directive

```bash
#!/usr/bin/env bash
# shellcheck disable=SC2015,SC2016
```

## Disable Within The shellcheck Command

```bash
shellcheck -e SC2015,SC2016
```
