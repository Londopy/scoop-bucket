# scoop-bucket

A [Scoop](https://scoop.sh) bucket for my apps.

## Install

```powershell
scoop bucket add londopy https://github.com/Londopy/scoop-bucket
scoop install londopy/hidedesktopapps
```

Updating works the usual way:

```powershell
scoop update
scoop update hidedesktopapps
```

## Apps

| App | Description |
|---|---|
| [hidedesktopapps](https://github.com/Londopy/HideDesktopApps) | System tray app to hide/show desktop icons, taskbar, and all windows via configurable hotkeys. Rust-native single binary. |
| [nexium](https://github.com/Londopy/nexium) | The Nexium language: `nx`, a compiler that emits C and ships libraries, packages and tools. Needs a C compiler (`scoop install zig`); `nx doctor` says what it found. |

## Notes

Manifests carry `checkver` and `autoupdate`, so new upstream releases are picked
up automatically and hashes are read from each release's `SHA256SUMS.txt`.

If you maintain a bucket too and want any of these in it, go ahead — the
manifests are MIT-licensed like the apps.
