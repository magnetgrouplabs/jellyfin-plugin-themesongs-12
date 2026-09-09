> This fork is the Jellyfin 12 build of danieladov's Theme Songs plugin, built from pull request 56.
> Add this repository in Jellyfin: https://raw.githubusercontent.com/magnetgrouplabs/jellyfin-plugin-themesongs/master/manifest.json

<h1 align="center">Jellyfin Theme Songs Plugin</h1>
<h3 align="center">Part of the <a href="https://jellyfin.org">Jellyfin Project</a></h3>

<p align="center">
Jellyfin Theme Songs plugin is a plugin that automatically downloads every theme song of your tv show library;

</p>

## Install Process


## From Repository
1. In jellyfin, go to dashboard -> plugins -> Repositories -> add and paste this link https://raw.githubusercontent.com/danieladov/JellyfinPluginManifest/master/manifest.json
2. Go to Catalog and search for Theme Songs
3. Click on it and install
4. Restart Jellyfin


## From .zip file
1. Download the .zip file from release page
2. Extract it and place the .dll file in a folder called ```plugins/Theme Songs``` under  the program data directory or inside the portable install directory
3. Restart Jellyfin
4. 
## Configuration ⚠️

**Important:** This plugin **does not provide a default source URL**. You must configure it manually for the downloads to work.

1. Go to **Dashboard** -> **Plugins** -> **Theme Songs**.
2. Enter a valid source URL in the settings field.
3. Click **Save**.
## User Guide
1. To download the theme songs you can do it from Schedule task or directly from the configuration of the plugin.
2. You need to have enabled the option "Theme Songs" under display





## Build Process
1. Clone or download this repository
2. Ensure you have .NET Core SDK setup and installed
3. Build plugin with following command.
```sh
dotnet publish --configuration Release --output bin
```
4. Place the resulting .dll file in a folder called ```plugins/Merge versions``` under  the program data directory or inside the portable install directory


