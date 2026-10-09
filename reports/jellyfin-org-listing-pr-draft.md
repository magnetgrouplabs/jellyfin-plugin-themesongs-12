# Listing PR draft: Theme Songs 12

NOTE: Jellyfin's LLM policy forbids LLM-written PR bodies. The body below is a rough draft of facts only. Anthony rewrites it in his own words before it is posted.

## Diff (git diff HEAD~1, local commit "Add Theme Songs 12 to third-party plugins")

```diff
diff --git a/docs/general/server/plugins/index.mdx b/docs/general/server/plugins/index.mdx
index 104f73c..cf13b37 100644
--- a/docs/general/server/plugins/index.mdx
+++ b/docs/general/server/plugins/index.mdx
@@ -345,6 +345,14 @@ Local AI-powered subtitle generation using whisper.cpp. Generates SRT subtitles
 
 - [GitHub](https://github.com/GeiserX/whisper-subs)
 
+#### Theme Songs 12
+
+Downloads TV show theme songs as theme.mp3 into each show's folder so Jellyfin plays them natively. Maintained fork of the original Theme Songs plugin, built for Jellyfin 12 and later.
+
+**Links:**
+
+- [GitHub](https://github.com/magnetgrouplabs/jellyfin-plugin-themesongs-12)
+
 ## Repositories
 
 import { OfficialPluginRepositories, ThirdPartyRepositories } from '../../../../src/data/pluginRepositories';
diff --git a/src/data/pluginRepositories.ts b/src/data/pluginRepositories.ts
index f2c42c5..242cbf5 100644
--- a/src/data/pluginRepositories.ts
+++ b/src/data/pluginRepositories.ts
@@ -112,5 +112,13 @@ export const ThirdPartyRepositories: Array<PluginRepository> = [
     includes: {
       WhisperSubs: 'https://github.com/GeiserX/whisper-subs'
     }
+  },
+  {
+    id: 'gh:magnetgrouplabs/jellyfin-plugin-themesongs-12',
+    name: "magnetgrouplabs's Theme Songs 12 Repo",
+    url: 'https://raw.githubusercontent.com/magnetgrouplabs/jellyfin-plugin-themesongs-12/master/manifest.json',
+    includes: {
+      'Theme Songs 12': 'https://github.com/magnetgrouplabs/jellyfin-plugin-themesongs-12'
+    }
   }
 ];
```

## PR template found at .github/pull_request_template.md (quoted verbatim)

```markdown
<!--
Thank you for contributing to our documentation. We receive a lot of pull requests here, so we want to streamline this process.
-->

**Changes**
<!-- Describe a little about what you've changed and why. -->

**Copyediting**

To avoid "nitpicky" reviews, please ensure all of the following have been done for any non-trivial changes.

- [ ] I have run this PR [through a spellchecker](https://jellyfin.org/docs/general/contributing/documentation#please-self-review) (e.g. `aspell`).
- [ ] I have re-read my PR at least twice and fixed any obvious mistakes I see.
- [ ] I have received [out-of-band peer copyediting](https://jellyfin.org/docs/general/contributing/documentation#peer-copyediting) from someone in [#jellyfin-documentation](https://matrix.to/#/#jellyfin-documentation:matrix.org).

While you're waiting for someone to look at your pull request, How about looking at another one? You do not have to do this, but it will help ensure your PR is reviewed quickly in turn.

- [ ] I have provided [a *substantive* review of another documentation PR](https://jellyfin.org/docs/general/contributing/documentation#peer-reviews).

**Issues**

<!-- If applicable, please list any open issues that this PR addresses -->
<!-- e.g. -->
<!-- - closes #1234 -->
```

## PR title

Add Theme Songs 12 to third-party plugins

## PR body (draft facts, rewrite in own words)

**Changes**

I added Theme Songs 12 to the 3rd-party plugin list and its manifest to the 3rd-party repository list, the same way WhisperSubs and SmartCovers were added.

The plugin downloads TV show theme songs as theme.mp3 into each show's folder so Jellyfin plays them natively. It is a maintained, MIT-licensed fork of danieladov's Theme Songs plugin, built for Jellyfin 12 and later. The original has no Jellyfin 12 release (its last commit is from 2025-12-21). The fork keeps the same plugin id, so existing installs upgrade in place.

Manifest: https://raw.githubusercontent.com/magnetgrouplabs/jellyfin-plugin-themesongs-12/master/manifest.json

Release 12.0.0.0 was published on 2026-09-09 and I have run it in production since then.

Repo: https://github.com/magnetgrouplabs/jellyfin-plugin-themesongs-12

**Issues**

None.

(Template checklist items under Copyediting are for Anthony to tick honestly.)

## Commands for the coordinator (run from the clone, only on Anthony's go)

```
cd "C:/Users/anthony/AppData/Local/Temp/claude/C--Users-anthony-claude-projects/635a1624-a566-4e82-a877-9dfbc6c27252/scratchpad/jellyfin.org"
git push -u origin add-theme-songs-12
gh pr create -R jellyfin/jellyfin.org --head magnetgrouplabs:add-theme-songs-12 --title "Add Theme Songs 12 to third-party plugins" --body-file <path to Anthony's rewritten body>
```

Note: `git push -u origin` changes the branch upstream from upstream/master to origin; harmless.
