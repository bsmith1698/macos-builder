<div align="center">

# mcpelauncher for Intel Macs

**Unofficial Intel (x86_64) macOS builds of [mcpelauncher](https://minecraft-linux.github.io), the open-source launcher for Minecraft: Bedrock Edition.**

[![CI](https://github.com/bsmith1698/mcpelauncher-intel-mac/actions/workflows/ci.yml/badge.svg)](https://github.com/bsmith1698/mcpelauncher-intel-mac/actions/workflows/ci.yml)
![Platform](https://img.shields.io/badge/platform-macOS%20Intel%20(x86__64)-lightgrey?logo=apple)
[![Release](https://img.shields.io/github/v/release/bsmith1698/mcpelauncher-intel-mac?include_prereleases&label=release)](https://github.com/bsmith1698/mcpelauncher-intel-mac/releases)
![Upstream](https://img.shields.io/badge/fork%20of-minecraft--linux%2Fmacos--builder-blue?logo=github)

</div>

---

## Why this fork exists

Official mcpelauncher releases after v1.7.6 ship for Apple Silicon only, and upstream stopped building the Intel DMG. The last Intel release (v1.7.6) predates the fix for the Google sign-in bug ("No Email returned"), so on an Intel Mac you can't log in to download the game.

This fork turns the Intel build back on and pins newer launcher sources so sign-in works again. Everything is built on GitHub's free `macos-15-intel` runners. No local toolchain is needed.

## What's changed from upstream

| Area | Change | Reason |
| --- | --- | --- |
| `ci.yml` | `build-m1: false` | The Apple Silicon ANGLE step fails on forks and cancels the Intel job with it |
| `ci.yml` | `publish: false` | Forks can't publish to upstream's release channel |
| `ci.yml` | Added `permissions: contents: read, id-token: write` to the `ci` job | Without it the reusable workflow fails with `startup_failure` on forks |
| `mcpelauncher-ui.commit` | Pinned to `2e9fdaf` | Includes the Google sign-in fix |
| `mcpelauncher.commit` | Pinned to `91220f0` | Newer launcher client |
| `main.yml` | Fallback SDK `MacOSX10.13.sdk` to `MacOSX10.14.sdk` | The 10.13 SDK's old libstdc++ headers break the libc++ symlink, which surfaces as a protobuf "C++11 required" error |

## Download

Grab the DMG for your macOS version from the [**Releases page**](https://github.com/bsmith1698/mcpelauncher-intel-mac/releases):

| File ends in | Minimum macOS |
| --- | --- |
| `_macOS_10.13.0.dmg` | 10.13 High Sierra or newer (pick this one on a current Mac) |
| `_macOS_10.12.0.dmg` | 10.12 Sierra |
| `_macOS_10.10.0.dmg` | 10.10 Yosemite |

1. Open the DMG and drag the launcher into **Applications**.
2. The build isn't notarized, so the first time you open it, right-click the app and choose **Open**.

Newer builds from the latest commits are also available as artifacts on [successful CI runs](https://github.com/bsmith1698/mcpelauncher-intel-mac/actions/workflows/ci.yml?query=is%3Asuccess). These need a GitHub login and expire after 90 days.

## Build it yourself

1. Fork this repo.
2. Edit the pins in `mcpelauncher.commit`, `mcpelauncher-ui.commit` or `msa.commit` to the upstream commits you want.
3. Push to `main`, or run **Actions → CI → Run workflow**. A full build takes roughly 30 to 60 minutes.

## Tested

| Mac | macOS | Launcher build | Minecraft version | Result |
| --- | --- | --- | --- | --- |
| MacBook Pro, Intel Core i9 | 26.5 | 0.2.405 | 1.21.60.10 | Google sign-in and gameplay work |

Newer Bedrock versions may need a newer `mcpelauncher.commit` pin and haven't been verified on Intel yet.

## Can I play with an APK?

No. This project follows upstream's policy: you need a valid Google Play license for Minecraft. Sideloading paid APKs without one enables piracy and is not supported. Minecraft Trial and Education Edition are the only exceptions.

See the [upstream FAQ](https://minecraft-linux.github.io/faq/index.html#can-i-play-with-an-apk) for the current wording of this rule.

## Credits

All of the launcher itself is the work of the [minecraft-linux](https://github.com/minecraft-linux) project and its contributors. This repo only changes the build configuration. Please report launcher bugs upstream, not here.

This project isn't affiliated with Mojang, Microsoft or Google. Minecraft is a trademark of Mojang Synergies AB.
