# iPhone/iPad toolchain — install & quick start

Install the libimobiledevice family plus forensic extras for triage, backup, restore,
and acquisition of iPhone/iPad. All open source except where noted.

## Linux (Debian/Ubuntu)

```bash
sudo apt install \
  usbmuxd \
  libimobiledevice-glue-1.0-0 \
  libimobiledevice-utils \
  libusbmuxd-2.0-7 \
  ifuse \
  idevicerestore \
  libirecovery-common libirecovery-1.0-5
```

What you get:

| Tool | Purpose |
|---|---|
| `idevice_id -l` | list connected devices (serials/UDIDs) |
| `ideviceinfo` | dump device info: model, iOS version, battery, Activation state |
| `idevicepair pair` | pair/trust with the device (passcode must be on, confirm Trust dialog) |
| `idevicebackup2 backup` | full iTunes-style backup of a modern iOS device |
| `idevicebackup backup` | backup for pre-iOS-10 devices (iPad Air 1, iPhone 5s era) |
| `idevicecrashreport` | pull crash logs (forensic gold: usage, app state, timestamps) |
| `idevicesyslog` | live system log stream |
| `idevicescreenshot` | capture the device screen |
| `ideviceinstaller -l` | list installed apps |
| `idevicediagnostics` | battery/health diagnostics (supported devices) |
| `ifuse` | mount documents/media partitions as FUSE filesystem |
| `idevicerestore` | full firmware restore (downloads signed IPSW or uses a local one) |
| `irecovery` | talk to devices in recovery/DFU mode |

`usbmuxd` must be running (systemd does this automatically; debug with
`sudo usbmuxd --foreground -v`).

## Python tooling (cross-platform)

```bash
pip install pymobiledevice3   # modern Python alternative: info, syslog, apps, backup, developer services
pip install mvt               # Mobile Verification Toolkit — forensic backup/IPSW analysis
git clone https://github.com/abrignoni/iLEAPP   # iOS Logs, Events, And Parsers
```

- `pymobiledevice3` — the actively maintained Python suite; required for **iOS 17+**
  devices (they need its RSD/tunnel services, not just usbmuxd).
- `mvt` — the standard forensic toolkit:
  `mvt-ios check-backup`, `mvt-ios decrypt-backup`, `mvt-ios download-backups`, `mvt-ios check-ipsw`.
- `iLEAPP` — parses backups/crash logs into forensic timelines (HTML/TSV output).

## macOS

The Mac path needs no third-party tools for restore: **Finder** (Catalina+) or **iTunes**
(pre-Catalina) → select device → Restore/Update. **Apple Configurator 2** (free, App
Store) covers explicit DFU restores and device supervision.

For the CLI suite and Python tools on macOS:

```bash
brew install libimobiledevice usbmuxd ifuse idevicerestore libirecovery
pip install pymobiledevice3 mvt
```

## Windows

- **iTunes** (from Apple, not the Store version, if you need usbmuxd.dll exposure) —
  Finder-equivalent restore.
- 3uTools / iMazing are convenient but closed-source — fine for repair, not for
  forensics you intend to rely on.

## Firmware & identifiers

- Model number on the device back (`A1475`) ≠ internal identifier (`iPad4,2`).
  Mapping for the iPad Air 1: A1474 = iPad4,1, **A1475 = iPad4,2**, A1476 = iPad4,3.
  iPhones: A1457 = iPhone6,2 (iPhone 5s) — don't confuse the two families.
- Signing status & IPSW downloads: <https://ipsw.me> (API: `https://api.ipsw.me/v4/device/iPad4,2`).
  Only Apple-signed builds can be restored.
- Manual restore with a local IPSW:
  `idevicerestore --ipsw file.ipsw` (Linux/macOS) or Option/Shift-click **Restore** in Finder/iTunes.

## Forensic gotchas

- Use a **data** cable, not charge-only; a silent dead USB link wastes hours.
- First connection: unlock the device and tap **Trust**; pairing requires a passcode on the device.
- Full **file-system** acquisition needs a jailbreak (checkm8-class for A7–A11 devices).
  A normal backup (`idevicebackup2`/`idevicebackup`) is usually enough for triage.
- iOS 17+ devices: plain usbmuxd tools see nothing — use `pymobiledevice3` tunnels.
- Encrypted backups (set in device Settings) are the only ones containing
  health/keychain data; decrypt with `mvt-ios decrypt-backup`.
- **Activation Lock** is not bypassable by any tool here — owner's Apple ID or Apple + proof of purchase.
