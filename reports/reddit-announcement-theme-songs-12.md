Title: Theme Songs plugin working on Jellyfin 12

The existing Theme Songs plugin  has not been updated for Jellyfin 12. The repo has been dormant since December 2025 and pull requests have been sitting there unanswered. The original mWhy did you?aintainer does not seem interested in keeping it going anymore, so I forked it.

I looked at Themerr and it looks great but I don't want to run another Docker container for this.

I have been running the fork on my own Jellyfin 12 server since September 9 with no issues.

Install: Dashboard, Plugins, Repositories, add this URL, then install Theme Songs from the catalog and restart.

https://raw.githubusercontent.com/magnetgrouplabs/jellyfin-plugin-themesongs-12/master/manifest.json

It uses the same plugin ID as the original, so if you already have Theme Songs installed just swap the repository URL and update. Your settings and existing theme.mp3 files stay. Same as before, you still need to set a theme source URL in the plugin settings or it will not download anything.

I am in the process of getting this listed as an official plugin.

Repo and issues: https://github.com/magnetgrouplabs/jellyfin-plugin-themesongs-12
