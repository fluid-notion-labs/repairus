# repairus

iPhone/iPad repair & forensics toolkit: install guides for USB/device toolchains,
backup–restore–acquisition procedures, and case notes for specific devices.

Lives in `explorations/nursery/` — research docs live in contexture
(`cx.contexts.ipad-reset` holds the current iPad case).

## Contents

- `docs/install-tools.md` — install the iPhone/iPad toolchain (Linux, macOS, Windows) plus
  forensics extras (backups, MVT, iLEAPP), with quick-start commands.
- `docs/case-ipad-air-a1475.md` — current case: iPad Air 1 (A1475) reset/restore, signing
  status, DFU/recovery, error triage.

## One-line quick start (Linux)

```bash
sudo apt install libimobiledevice-utils usbmuxd ifuse idevicerestore libirecovery-common
idevice_id -l   # confirm the device shows up over USB
```
