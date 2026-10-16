# KefinTweaks for Jellyfin 12: releases

Release builds of a fork of [KefinTweaks](https://github.com/ranaldsgift/KefinTweaks) by ranaldsgift, for **Jellyfin 12** with the modern layout. KefinTweaks adds a Watchlist, breadcrumbs, episodes on the series page, playlist and collection sorting, subtitle search in the player and more to the Jellyfin web client. This fork adds four features: Request it (Seerr), Media Info Badges, Jump Bar and Kids Mode. It also fixes the modules that broke on Jellyfin 12.2.

This repository only holds the release files: a zip of the web files, a one-line installer and checksums. The fork's source is private. KefinTweaks is MIT-licensed (see [LICENSE](LICENSE), copyright ranaldsgift), and so is this fork.

## What you need

- Jellyfin 12
- The **JavaScript Injector** plugin (Dashboard > Plugins > Catalog)
- Optional: an API key (Dashboard > API Keys). With a key the installer writes the two injector entries for you.

## What the installer does

It prints each step as it goes:

1. Downloads the release zip and checks its sha256.
2. Backs up the JavaScript Injector configuration into the current folder.
3. Copies the files into Jellyfin's web folder, at `<web>/kefin/<version>/`. Jellyfin serves them at `/web/kefin/<version>/` (same origin, no CDN).
4. With `--api-key`, writes the two entries **KefinTweaks-Config** and **KefinTweaks-injector**. An existing KefinTweaks-Config keeps its settings; only its `kefinTweaksRoot` changes.
5. Checks that Jellyfin serves the files.

## Install: Jellyfin in Docker

Run this on the machine that runs the container. `--url` is how that machine reaches Jellyfin.

```
curl -fsSL https://github.com/scotthowson/kefintweaks-jf12-releases/releases/latest/download/install.sh | bash -s -- --container jellyfin --url http://localhost:8096 --api-key YOUR_API_KEY
```

A new Jellyfin image re-creates the container and loses the files. To keep them, do one of these:
- Before installing, bind-mount a folder at `/jellyfin/jellyfin-web/kefin` in the Jellyfin service, for example `./kefin:/jellyfin/jellyfin-web/kefin`.
- Or run the installer again after each image update.

## Install: Jellyfin from a package (Debian, Ubuntu, Fedora...)

The web folder is usually `/usr/share/jellyfin/web`.

```
curl -fsSL https://github.com/scotthowson/kefintweaks-jf12-releases/releases/latest/download/install.sh | sudo bash -s -- --web-dir /usr/share/jellyfin/web --url http://localhost:8096 --api-key YOUR_API_KEY
```

A package upgrade replaces that folder, so run the installer again after upgrading Jellyfin.

## Without an API key

The installer saves the two entries as files next to the backup. Add them in Dashboard > Plugins > JavaScript Injector:

1. **KefinTweaks-Config** first.
2. **KefinTweaks-injector** second.

Both must be Enabled, with "Requires authentication" off. Then reload the Jellyfin page. Administrators find the settings in the user menu > KefinTweaks.

## Options

- `--dry-run`: show what would happen and change nothing.
- `--version <v>`: install another release.
- `--zip <file>`: install from a zip you already have.

Games (117 MB of game pages) are not included in the release zip.

## Uninstall

```
curl -fsSL https://github.com/scotthowson/kefintweaks-jf12-releases/releases/latest/download/install.sh | bash -s -- --container jellyfin --version 0.5.0-jf12.1 --uninstall
```

This removes the files. Then delete the two JavaScript Injector entries yourself.

## Checksums

Each release carries `SHA256SUMS`. The installer checks the zip against the sha256 it was built with.
