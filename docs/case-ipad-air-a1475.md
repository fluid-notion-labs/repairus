# Case: iPad Air 1 (A1475) — reset/restore

Model A1475 on the back = iPad Air 1st gen, Wi-Fi + Cellular, identifier **iPad4,2**.
Lightning connector, Home button, max OS **iOS 12.5.8** (build **16H88**, verified
signed by Apple 2026-10; older builds unsigned — restores must target 12.5.8).
Full context: contexture `cx.contexts.ipad-reset`.

## Reset paths, in order

1. **Settings erase** (works + unlocked): Settings → General → Reset → **Erase All
   Content and Settings** (iOS 12 wording; no "Transfer or Reset" until iOS 15).
2. **Remote erase**: icloud.com/find → select iPad → Erase (device must be online).
3. **Force restart** (frozen): hold **Top + Home** ~10 s to the Apple logo. No data loss.
4. **Recovery mode** (locked out): power off → connect via Lightning to computer →
   hold **Home** while plugging in → recovery screen → Restore (erases) / Update (keeps
   data, try first).
5. **DFU** (recovery restore failed): Top + Home 8 s → release Top, keep Home ~10 s more
   until screen stays black; host detects and offers Restore.

## Manual restore from Linux (this machine has the toolchain already)

```bash
idevice_id -l
idevicerestore --latest              # pulls signed 12.5.8, restores (erases)
idevicerestore --latest --keep-data  # update-style restore, attempts to keep data
```

`idevicerestore` can drive the device into recovery/DFU itself.

## If restore still fails

- Try another known-good Lightning **data** cable and USB port first.
- **Error 9 / 4013 / 4014** ≈ NAND/hardware fault → Genius Bar or board-level repair.
- **Activation Lock** after restore needs the original owner's Apple ID (see gotchas
  in `install-tools.md`).
