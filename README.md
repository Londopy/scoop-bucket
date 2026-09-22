# scoop-bucket

A [Scoop](https://scoop.sh) bucket for [Londopy](https://github.com/Londopy)'s
apps on Windows: add it once, and `scoop install` and `scoop update` handle
the rest. Every manifest is kept current by Scoop's own updater, and every
download is checked against the release's published SHA-256.

[![Excavator](https://img.shields.io/github/actions/workflow/status/Londopy/scoop-bucket/excavator.yml?label=excavator&logo=githubactions&logoColor=white)](https://github.com/Londopy/scoop-bucket/actions/workflows/excavator.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Scoop](https://img.shields.io/badge/scoop-bucket-1f6feb?logo=windows&logoColor=white)](https://scoop.sh)
[![nexium](https://img.shields.io/github/v/release/Londopy/nexium?label=nexium&logo=github&color=8b7cf6)](https://github.com/Londopy/nexium/releases/latest)
[![hidedesktopapps](https://img.shields.io/github/v/release/Londopy/HideDesktopApps?label=hidedesktopapps&logo=github&color=8b7cf6)](https://github.com/Londopy/HideDesktopApps/releases/latest)

## Install

Scoop itself, if you do not have it ([scoop.sh](https://scoop.sh) has the
long version):

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

Then the bucket, and whichever apps you want:

```powershell
scoop bucket add londopy https://github.com/Londopy/scoop-bucket
scoop install londopy/nexium
scoop install londopy/hidedesktopapps
```

The `londopy/` prefix is only needed when another bucket has an app of the
same name; `scoop install nexium` works once the bucket is added.

## Apps

| App | What it is | Platforms | Links |
| --- | --- | --- | --- |
| **nexium** | The Nexium language: `nx`, a compiler that emits C and ships libraries, packages and tools. Ownership without a borrow checker, a checked effect system, binary patterns, compile-time evaluation; one compiler that turns a source tree into a binary, a C library, a Python wheel, a Rust crate or an npm package. | x64, ARM64 | [site and docs](https://londopy.github.io/nexium/) · [the Topo tutorial](https://londopy.github.io/nexium/topo/01-base-camp.html) · [source](https://github.com/Londopy/nexium) · [releases](https://github.com/Londopy/nexium/releases) · [changelog](https://github.com/Londopy/nexium/blob/main/CHANGELOG.md) |
| **hidedesktopapps** | A lightweight system-tray app that hides and shows desktop icons, the taskbar and every window with configurable hotkeys. For ricing, streaming, Wallpaper Engine and focus. One native Rust binary. | x64, x86, ARM64 | [source](https://github.com/Londopy/HideDesktopApps) · [releases](https://github.com/Londopy/HideDesktopApps/releases) |

### nexium, after installing

`nx` builds programs with a C compiler. The portable build in this bucket
does not bundle one, so install Zig from Scoop's main bucket, or let `nx`
find gcc or clang if you have them:

```powershell
scoop install zig
nx doctor                   # which C compiler it found, and whether a build works
nx run "$(scoop prefix nexium)\examples\hello.nx"
nx topo                     # the tutorial's exercises, graded by the compiler
```

The examples, the standard library sources and the docs sit next to
`nx.exe` in the app's directory (`scoop prefix nexium`). The full Windows
installer on the [releases page](https://github.com/Londopy/nexium/releases/latest)
bundles Zig and the VS Code extension, if you would rather not manage a C
compiler; the two installs do not interfere, but keep only one of them on
the PATH.

Shell completions for PowerShell, from the compiler itself:

```powershell
Add-Content $PROFILE 'nx completions powershell | Out-String | Invoke-Expression'
```

## Updating

```powershell
scoop update                # refresh every bucket, this one included
scoop status                # what has a newer version
scoop update nexium         # one app
scoop update *              # everything
```

## Uninstalling

```powershell
scoop uninstall nexium
scoop bucket rm londopy     # the bucket itself, if you no longer want it
```

## How the bucket stays current

Every manifest carries a `checkver` section, which tells Scoop how to read
the latest version from the project's GitHub releases, and an `autoupdate`
section, which says how to build the download URLs and where the SHA-256
hashes come from (each release's `SHA256SUMS.txt`). The
[Excavator workflow](https://github.com/Londopy/scoop-bucket/actions/workflows/excavator.yml)
runs `checkver` every six hours and commits a bumped manifest when a
release has appeared, so a new version is installable within hours of its
tag with no hand involved.

`scoop install` verifies every download against the hash in the manifest
and refuses a file that does not match.

## Problems

Something about an app itself: its own repository's issues
([nexium](https://github.com/Londopy/nexium/issues),
[hidedesktopapps](https://github.com/Londopy/HideDesktopApps/issues)).
Something about installing or updating through Scoop, such as a hash
mismatch or a stale version: [this repository's issues](https://github.com/Londopy/scoop-bucket/issues).
`scoop checkup` and `scoop cache rm *` fix most local oddities.

## For other bucket maintainers

If you maintain a bucket and want any of these manifests in it, go ahead:
the manifests are MIT-licensed like the apps. `nexium` is also published
[in the language's own repository](https://github.com/Londopy/nexium/tree/main/bucket),
written by its release workflow, if you prefer to track that copy.
