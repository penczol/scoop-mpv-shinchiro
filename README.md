# Shinchiro mpv Git Scoop bucket

[![Excavator](https://github.com/penczol/scoop-mpv-shinchiro/actions/workflows/excavator.yml/badge.svg)](https://github.com/penczol/scoop-mpv-shinchiro/actions/workflows/excavator.yml)

A minimal [Scoop](https://scoop.sh/) bucket for the latest rolling/daily Windows build of [mpv](https://mpv.io/) published by [Shinchiro](https://github.com/shinchiro/mpv-winbuild-cmake).

The manifest tracks the latest GitHub Release and downloads the original `mpv-x86_64-v3-*` archive directly from Shinchiro. This repository does not rebuild, modify, mirror, or repack mpv.

## Why this bucket exists

The normal WinGet `shinchiro.mpv` package follows stable mpv releases. This bucket retains Shinchiro's daily Git release channel while letting Scoop, and therefore [UniGetUI](https://www.marticliment.com/unigetui/), manage installation and upgrades instead of running Shinchiro's local `updater.bat`.

The `x86_64-v3` build requires a compatible 64-bit processor (including AVX2 support).

## Install

```powershell
scoop bucket add mpv-shinchiro https://github.com/penczol/scoop-mpv-shinchiro
scoop install mpv-shinchiro/mpv-shinchiro-git
```

The `mpv` command and an **mpv** Start Menu shortcut are created through standard Scoop behavior.

## Update

```powershell
scoop update
scoop update mpv-shinchiro-git
```

Once Scoop and this bucket are configured, UniGetUI can manage the package's Scoop updates as well. The commands above remain the reliable fallback.

The `portable_config` directory is persisted across upgrades. It is placed next to `mpv.exe`, so mpv uses it for `mpv.conf`, `input.conf`, scripts, script options, and related state.

## Optional yt-dlp support

`yt-dlp` is intentionally a separate suggested package, so Scoop or UniGetUI can update it independently:

```powershell
scoop install yt-dlp
```

No separate FFmpeg package is installed. An external `ffmpeg.exe` is not mpv's playback backend; the FFmpeg libraries used for playback are part of the Shinchiro mpv build itself.

## File associations

The original upstream file-association scripts remain in the installed package, including `installer\mpv-install.bat`, `installer\mpv-uninstall.bat`, `mpv-register.bat`, and `mpv-unregister.bat`. They are not run automatically because they modify Windows registration and some require elevation. Shinchiro's `updater.bat` and `installer\updater.ps1` are removed during installation so Scoop remains the only mpv update mechanism.

## Jellyfin MPV Shim

[Jellyfin MPV Shim](https://github.com/jellyfin/jellyfin-mpv-shim) users can optionally enable its **External MPV** mode to use this Scoop-managed build instead of the bundled libmpv. Locate the Scoop command shim without hardcoding a user path:

```powershell
Get-Command mpv.exe | Select-Object -ExpandProperty Source
```

`yt-dlp` is not required for normal Jellyfin media playback.

## Maintenance

The Excavator workflow checks upstream approximately every four hours. When a new matching release appears, it updates the manifest version, URL, and SHA256 and commits the result using the repository's built-in `GITHUB_TOKEN`.

GitHub may disable scheduled workflows in a public repository after a long period without repository activity. If that happens, re-enable the workflow in the repository's Actions settings or run it manually.

## Upstream projects

- [mpv](https://github.com/mpv-player/mpv)
- [Shinchiro's mpv-winbuild-cmake](https://github.com/shinchiro/mpv-winbuild-cmake)
- [Scoop](https://github.com/ScoopInstaller/Scoop)
- [UniGetUI](https://github.com/marticliment/UniGetUI)
- [Jellyfin MPV Shim](https://github.com/jellyfin/jellyfin-mpv-shim)
