**Changes**

This adds Theme Songs 12 to the third-party plugin list and its repository to the third-party repository list.

Some background on why this exists. The Theme Songs plugin has been the go-to way to get TV show theme songs into Jellyfin for years. It downloads a theme.mp3 into each show's folder and Jellyfin picks it up natively, so it works on every client with nothing extra to run. The problem is that it stopped getting updates. The last commit was December 2025, there is no build for Jellyfin 12, and a pull request adding Jellyfin 12 support has been sitting in the repo since early September with no response. Issues have gone unanswered for most of this year.

The other option people point to, Themerr, dropped its Jellyfin plugin in October and moved to a standalone Docker container that installs its own connector. That is fine if you want it, but a lot of us just want the simple plugin back.

So I forked it. Theme Songs 12 is the same plugin built against Jellyfin 12, MIT licensed like the original, with the original copyright kept. It uses the same plugin ID, so anyone who already has Theme Songs installed just swaps the repository URL and it updates in place with their settings and existing theme files intact. I have been running it on my own Jellyfin 12 server since September 9 with no problems.

Repo: https://github.com/magnetgrouplabs/jellyfin-plugin-themesongs-12
Manifest: https://raw.githubusercontent.com/magnetgrouplabs/jellyfin-plugin-themesongs-12/master/manifest.json

The original entry in the repository list is left alone since that is the original author's repo, and the fork is listed separately so people on Jellyfin 12 can find a working build.

**Copyediting**

- [x] I have checked the spelling
- [x] I have checked the formatting

**Issues**

None.
