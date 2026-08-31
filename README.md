# Logs Directory

Persistent Android logcat storage for debugging crashes and reboots.

## Service

- **Name:** `logcat-persist`
- **Manager:** runit (auto-starts on boot)
- **Status:** `sv status logcat-persist`

## Files

| File | Description |
|------|-------------|
| `logcat.txt` | Current active log (grows to 50MB) |
| `logcat.txt.1` | Previous rotated file |
| `logcat.txt.2` - `.5` | Older rotations (5 total) |

## Settings

- **Buffer:** All (`-b all`) — main, system, kernel, crash, events
- **Format:** Thread time (`-v threadtime`)
- **Rotation:** 50MB per file, 5 files max (250MB total)
- **Command:** `logcat -v threadtime -b all -f ~/logs/logcat.txt -r 52428800 -n 5`

## Usage

```bash
# View latest logs
tail -n 100 ~/logs/logcat.txt

# Search for crashes
grep -i "crash\|fatal\|panic\|oom" ~/logs/logcat.txt

# Check service status
sv status logcat-persist

# Restart service
sv restart logcat-persist
```

## Notes

- Logs survive Termux crashes but NOT full device reboots (Android wipes buffers).
- The rotated files (`logcat.txt.1`, etc.) contain pre-reboot logs if the crash triggered a rotation.
- If investigating a reboot, check `logcat.txt.1` first — it may have captured the event.
