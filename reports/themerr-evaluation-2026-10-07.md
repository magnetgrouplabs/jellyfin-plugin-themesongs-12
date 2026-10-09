# Evaluation: LizardByte Themerr as a replacement for the Theme Songs fork

Researched 2026-10-07. All sources read on 2026-10-07 unless stated.

## Executive summary

- Themerr is now a standalone app (Docker image) that talks to Jellyfin by API key and installs its own small "Themerr Connector" plugin into Jellyfin. The old Jellyfin-only plugin (themerr-jellyfin) was archived on 2026-10-06 and replaced by it.
- Jellyfin support in Themerr is brand new: it first shipped in a pre-release built on 2026-10-07. The last non-pre-release (2026-10-04) is Plex only. The Docker "latest" tag moved several times today.
- It can replace the fork in function: it covers TV series (1,501 shows in ThemerrDB vs 4,654 movies) and writes theme files next to the media that Jellyfin plays natively.
- Main catch 1: the connector requires Jellyfin 12.1.0 or newer (compiled against 12.1.0, range 12.1.0 to below 13.0.0). Anthony runs 12.0.0, so Jellyfin would have to be updated first.
- Main catch 2: very young, one main developer, pre-release only, and audio comes from YouTube through yt-dlp, which can break when YouTube changes.
- Files are written as theme.m4a or theme.opus, not theme.mp3. Existing theme.mp3 files are protected by default, not overwritten.
- Decision for Anthony: wait for a stable Themerr release and move Jellyfin to 12.1+ first, or trial the pre-release on a copy now. Keep the fork running until then.

## 1. Architecture

- Themerr is a standalone application (Python web app, web UI on port 9494) that runs "alongside Plex Media Server or Jellyfin". Source: https://github.com/LizardByte/Themerr/blob/master/docs/source/about/installation.rst (rendered at https://docs.lizardbyte.dev/projects/themerr/latest/about/installation.html), read 2026-10-07.
- It is both: standalone app plus a Jellyfin plugin called "Themerr Connector". The README says "Plex does not require a plug-in; Themerr installs its matching connector on Jellyfin." Source: https://github.com/LizardByte/Themerr/blob/master/README.md, 2026-10-07.
- Install of the connector is automatic. In the web UI, Servers > Jellyfin tab: enter the Jellyfin URL and an API key (Jellyfin Dashboard > Advanced > API Keys). Themerr then "automatically installs and updates its Themerr Connector", waits for playback to finish and restarts Jellyfin (a "Force restart" button exists). Source: https://github.com/LizardByte/Themerr/blob/master/docs/source/about/usage.rst, 2026-10-07.
- Themerr serves the connector as a Jellyfin plugin repository itself. Port 9495 "serves only the Jellyfin connector repository over HTTP"; the address saved on the server card must be reachable by the Jellyfin server (http://localhost:9495 locally, otherwise Themerr's host/IP). Source: https://github.com/LizardByte/Themerr/blob/master/DOCKER_README.md and usage.rst, 2026-10-07. Connector manifest path is /jellyfin/connector/manifest-12.json, archive connector-12.zip (https://github.com/LizardByte/Themerr/blob/master/src/jellyfin/compatibility.props).
- Jellyfin versions targeted (compatibility.props, read 2026-10-07): series 10.11 (min 10.11.0, max 10.12.0, net9.0) and series 12 (JellyfinMinimumVersion 12.1.0, JellyfinMaximumVersion 13.0.0, net10.0, latest validated 12.2). The csproj compiles against the minimum version (https://github.com/LizardByte/Themerr/blob/master/connectors/jellyfin/Themerr.Connector.csproj). usage.rst states: "Themerr supports Jellyfin 10.11.x and stable Jellyfin 12.x releases starting at 12.1, including 12.2." There is no 12.0 target, so Anthony's 12.0.0 is below the supported floor.
- The release on 2026-10-06 of themerr-jellyfin (v2026.1006.1228.21) is neither the connector nor a new feature. It is the final release of the old standalone plugin: "This is the final release of Themerr-jellyfin. This project is archived and has been replaced by LizardByte/Themerr." Its changes are dependency bumps plus the archival notice. Source: https://github.com/LizardByte/Themerr-jellyfin/releases/tag/v2026.1006.1228.21 (gh api, 2026-10-07). Repo is archived: https://github.com/LizardByte/Themerr-jellyfin.
- The old plugin's repo manifest https://app.lizardbyte.dev/jellyfin-plugin-repo/manifest.json still returns HTTP 200 (checked 2026-10-07). It is not needed for the new route, since Themerr hosts the connector itself.
- How the connector works (code read): Themerr downloads audio, then uploads it to the connector API, which writes a fixed-name theme file into the Jellyfin item folder (https://github.com/LizardByte/Themerr/blob/master/connectors/jellyfin/ThemeFiles.cs), 100 MB cap, with SHA-256 ownership records in Jellyfin's data dir under themerr-connector/ownership.db (ThemeOwnership.cs).

## 2. Deployment

- Official image: lizardbyte/themerr on Docker Hub (https://hub.docker.com/r/lizardbyte/themerr), also GitHub Container Registry. Tags seen 2026-10-07: latest, master, and per-build tags like v2026.1007.172328 (several per day), amd64 and arm64. Docker Hub repo registered 2026-10-06, 2,146 pulls, last pushed 2026-10-07 (Docker Hub API, read 2026-10-07).
- The image bundles Python 3.14, Deno (for yt-dlp YouTube challenge solving) and the Jellyfin connector (https://github.com/LizardByte/Themerr/blob/master/Dockerfile).
- Required parameters (https://github.com/LizardByte/Themerr/blob/master/DOCKER_README.md, 2026-10-07):
  - Port 9494 web UI (required); port 9495 connector repository (needed for Jellyfin).
  - Volume /config (required).
  - A private Fernet key file mounted read-only at /run/secrets/themerr_token_key, with THEMERR_TOKEN_KEY_FILE=/run/secrets/themerr_token_key (required; the README gives a one-line python command to generate it; Docker sign-in requires it).
  - TZ (required), PUID/PGID (optional).
  - Optional TMDB_API_READ_ACCESS_TOKEN (ID lookups, mainly Plex; usage.rst).
- First start prints a one-time admin setup link in the container log (replace 127.0.0.1 with the reachable host); the single admin account needs a 12+ character password; the UI uses a self-signed certificate on 9494 (usage.rst).
- Unraid Community Applications template: none found. GitHub repository search for "themerr unraid template" and a code search in selfhosters/unRAID-CA-templates on 2026-10-07 returned zero hits. Unconfirmed: I did not search the CA app feed directly. It would otherwise be a manual "Add Container" with the parameters above.
- Media file access: Themerr does not need a mount of the TV library. It talks to Jellyfin through the API and connector. But "Jellyfin must be able to write to your media folders, and each movie needs its own folder" (usage.rst), so the Jellyfin container needs read-write on the TV library path (the fork already needs this).

## 3. Data source

- ThemerrDB (https://github.com/LizardByte/ThemerrDB, BSD-3-Clause, "Theme song database for movies, tv shows, and video games"). Counts from the published pages.json files, read 2026-10-07:
  - Movies: 4,654 (https://app.lizardbyte.dev/ThemerrDB/movies/pages.json)
  - TV shows: 1,501 (https://app.lizardbyte.dev/ThemerrDB/tv_shows/pages.json)
  - Movie collections: 144 (https://app.lizardbyte.dev/ThemerrDB/movie_collections/pages.json)
  So TV is covered, at about a third of movie coverage. Jellyfin TV libraries using TMDB or TheTVDB metadata are supported (usage.rst).
- Submissions are community requests via GitHub issues with YouTube URLs ("codeless contributions", https://github.com/LizardByte/ThemerrDB/blob/master/README.md). ThemerrDB has 659 open issues, mostly theme requests under review.
- Audio source is YouTube. Themerr resolves the audio-only stream with yt-dlp and downloads it, then stores it as a file. It does not stream from YouTube at play time. For Jellyfin the files are theme.m4a (MP4 AAC) or theme.opus (WebM Opus remuxed to Ogg, no re-encode). The name list in ThemeFiles.cs also includes theme.mp3 and others for detection. Sources: https://github.com/LizardByte/Themerr/blob/master/src/jellyfin/audio.py and ThemeFiles.cs.
- ffmpeg: not required locally. troubleshooting.rst: "Themerr selects an audio-only stream URL and does not require local FFmpeg for that extraction path" (it uses PyAV in process). yt-dlp plus Deno is required and bundled.
- YouTube fragility is documented. YouTube may rate limit or require sign-in; the fix is pasting exported YouTube cookies (JSON) in Settings, and updating the locked yt-dlp version (https://github.com/LizardByte/Themerr/blob/master/docs/source/about/troubleshooting.rst). Expect occasional breakage until a new image ships.
- Plex-only features: Plex sign-in, GDM discovery, SSH/data-directory cleanup of old uploads. None apply to Jellyfin. Jellyfin-specific: collections need the TMDb Box Sets plugin; each browser/app needs "Theme songs" enabled under user Display settings.

## 4. Maturity

All from gh api on 2026-10-07 unless noted.
- Repo: https://github.com/LizardByte/Themerr (renamed from Themerr-plex; created 2022-09-12; licence AGPL-3.0; last push 2026-10-07).
- Last 5 releases: v2026.1007.172328 (2026-10-07, pre-release), v2026.1007.161959 (2026-10-07, pre-release), v2026.1004.152244 (2026-10-04, stable, Plex only), v2024.813.13709 (2024-08-13), v2024.717.231002 (2024-07-17). Note the 22-month gap before the rewrite.
- Jellyfin support first appears in the 2026-10-07 pre-release ("feat: support Jellyfin and rename repo to Themerr", PR #600), followed the same day by "fix(jellyfin): prevent connector replacement restart loops" (#606) and "support compatibility series and runtime validation" (#613). No stable release contains Jellyfin support yet.
- Open issues: 2, both bot dashboards ("Dependency Dashboard", "Top Issues Dashboard"); 0 open pull requests. Recently closed: #611 "Movie themes not playing on Apple TV" (2026-10-06), #586 "Crash in the last plex media server" (2026-10-03). No open Jellyfin bug reports, but there is almost no Jellyfin user history yet.
- Contributors: 5 (gh api contributors). Release notes show one human (ReenigneArcher) doing nearly all merges, plus renovate, dependabot and LizardByte-bot. Effectively one maintainer, inside the LizardByte organisation.
- Predecessor themerr-jellyfin: AGPL-3.0, 6 contributors, releases 2026-04-28, 2026-06-01, 2026-09-15, 2026-10-06 (final), now archived.

## 5. Migration

- Existing theme.mp3 files: protected, not overwritten by default. The connector treats a theme as Themerr-owned only if its SHA-256 matches its own database record; other files are "user themes". "Overwrite user themes" (Settings > Jellyfin) is off by default; "Back up replaced user themes" controls whether replaced files are kept (usage.rst; ThemeFiles.cs). So the fork's theme.mp3 files would be kept and ignored, and those shows would get no Themerr theme unless overwrite is enabled.
- If overwrite is enabled, Themerr writes theme.m4a or theme.opus and backs up the old file. Having both theme.mp3 and a Themerr file in one folder looks ambiguous: ThemeFiles.State reports "not owned" when more than one theme file exists. This is inference from code, not tested.
- Legacy recognition covers only the official Themerr-jellyfin plugin (LegacyTheme.cs reads its ThemeHash/ThemeProvider records); Themerr removes that old plugin and its repository on connect by default. The danieladov-based fork is not recognised, and its scheduled task would keep running if left installed.
- Coexistence: both can write theme files into the same folders and would fight over them. If trialled, disable the fork's scheduled task first. Not tested.

## 6. Other maintained options for Jellyfin 12

- Themerr-jellyfin (old LizardByte plugin): archived 2026-10-06, superseded by Themerr. https://github.com/LizardByte/Themerr-jellyfin
- No other concrete maintained Jellyfin 12 theme-song plugin was found. Search scope was limited to LizardByte repos; I did not run a broad web search for alternatives.

## Comparison

| Item | Theme Songs fork (current) | Themerr (new) |
|-|-|-|
| Jellyfin 12 support | Works on 12.0.0 (self-built) | Connector requires 12.1.0 or newer (up to below 13); 12.0.0 unsupported |
| Install model | Plugin built and installed by Anthony | Separate Docker container (ports 9494 and 9495, /config, key file) that auto-installs a connector plugin and restarts Jellyfin |
| Data source | themes.moe, theme.mp3 downloaded | ThemerrDB (community, YouTube links), downloaded via yt-dlp to theme.m4a or theme.opus |
| TV coverage | Whatever themes.moe has (not measured here) | 1,501 TV shows in ThemerrDB (4,654 movies), measured 2026-10-07 |
| Maintenance status | Upstream dormant; self-maintained | Active, one main maintainer; Jellyfin support only in same-day pre-releases; stable release has none |
| Failure modes | themes.moe availability, Jellyfin API changes breaking the plugin | YouTube and yt-dlp breakage (cookies, rate limits), young code (restart-loop fix same day), extra container, Jellyfin restart on connector update, needs Jellyfin 12.1+ |

## Conclusion

Themerr can replace the fork functionally (TV supported, native theme files, no media mount), but not today: Jellyfin support exists only in pre-releases from 2026-10-07 and needs Jellyfin 12.1 or newer, which a 12.0.0 server is not. Recommended: keep the fork, revisit when Themerr has a stable release with Jellyfin support and the server is on 12.1+.
