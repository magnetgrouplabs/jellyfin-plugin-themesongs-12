# Theme Songs 12

Theme Songs downloads the theme song for each TV show in your library as `theme.mp3` in the show's folder, so Jellyfin plays it natively when you open the show.

This is a maintained hard fork of [danieladov's Theme Songs plugin](https://github.com/danieladov/jellyfin-plugin-themesongs) for Jellyfin 12 and later. Credit to danieladov, the original author.

## Requirements

- Jellyfin 12.0 or newer.
- A theme source URL template. The plugin does not ship a default source, so downloads do nothing until you set one:
  1. Go to Dashboard, Plugins, Theme Songs.
  2. Enter a valid source URL in the settings field.
  3. Click Save.

## Install

1. In Jellyfin, go to Dashboard, Plugins, Repositories, Add, and paste:
   `https://raw.githubusercontent.com/magnetgrouplabs/jellyfin-plugin-themesongs-12/master/manifest.json`
2. Go to Catalog, find Theme Songs, and install it.
3. Restart Jellyfin.
4. Configure the source URL (see Requirements).

To install by hand, download the zip from the Releases page, extract the .dll into a folder called `plugins/Theme Songs` under the Jellyfin program data directory, and restart.

## Using it

Download theme songs from the scheduled task, or directly from the plugin's settings page. Enable the "Theme Songs" option under Display in your user settings so Jellyfin plays them.

## Migrating from the original plugin

This build keeps the original plugin ID (`afe1de9c-63e4-4692-8d8c-7c964df19eb2`), so it upgrades in place. Add the repository above, remove the old repository, and update the plugin. Your configuration and existing `theme.mp3` files are kept.

## Compatibility

| Plugin version | Jellyfin target ABI | Jellyfin version | Where |
|---|---|---|---|
| 12.0.0.0 | 12.0.0.0 | 12.0 and newer | This repository |
| 10.11.0.2 | 10.11 | 10.11 | [danieladov's repository](https://github.com/danieladov/jellyfin-plugin-themesongs/releases) |

Jellyfin 10.11 users should stay on the original plugin.

## Building

1. Clone this repository.
2. Install the .NET 10 SDK.
3. Run:
   ```sh
   dotnet publish --configuration Release --output bin
   ```
4. Put the resulting `Jellyfin.Plugin.ThemeSongs.dll` in a folder called `plugins/Theme Songs` under the Jellyfin program data directory.

## Status and support

Report problems in this repository's issues. The upstream plugin has been unmaintained since December 2025, and pull request 56 has had no response since 2026-09-02 (checked 2026-10-07).

## License

MIT. The original copyright notice (Copyright (c) 2019 Claus Vium) is preserved in [LICENSE](LICENSE).
