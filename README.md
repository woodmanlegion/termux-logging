# termux-logging

Persistent logcat service for Termux. Captures all Android log buffers with rotation, managed by runit.

## Install

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/woodmanlegion/termux-logging/main/bin/termux-logging)" -- install
# or after claw-gh-install:
claw-gh-install woodmanlegion/termux-logging
termux-logging install
```

## Log location

`~/.termux/logs/logcat.txt` — rotated at 50MB, 5 files max (250MB total).

## Commands

```bash
termux-logging install           # install service + start logging
termux-logging status            # service status + file sizes
termux-logging tail              # live tail
termux-logging search <pattern>  # grep across all rotated files
termux-logging uninstall         # stop service (logs kept)
```

## Service

Runs as a runit service (`logcat-persist`) under termux-services. Starts automatically when Termux starts.

```bash
sv status logcat-persist
sv restart logcat-persist
```

## Buffers captured

All: main, system, kernel, crash, events (`-b all`), threadtime format.

## Notes

- Logs survive Termux crashes but not full device reboots (Android wipes buffers on reboot).
- Check `logcat.txt.1` after a reboot — rotation may have captured the pre-reboot event.
- Previous log location was `~/logs/` — `termux-logging install` migrates automatically.
