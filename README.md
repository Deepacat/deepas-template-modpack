<h1 align="center">Deepa's modpack development template</h1>
<p align="center"><b><i>Deepa's preferred modpack development environment, focused on forge 1.20.1, with preset mods and github actions</i></b></p>
<h1 align="center">
    <a href="https://github.com/ThePansmith/PanPack/blob/main/LICENSE.md"><img src="https://img.shields.io/github/license/ThePansmith/PanPack?style=for-the-badge&logo=github" alt="License"></a>
</h1>

## Features
- [Pakku](https://juraj-hrivnak.github.io/Pakku/about.html):
    - Easily control mods and resourcepacks, handling dependendices for you.
    - Can sync your existing installed mods and resourcepacks from your modpack to be automatically manifested for exports
    - Can easily fetch mods from the pakku manifest file that others have synced to, for contributors to pull the latest mods and resource packs
    - Mods and folders can be easily marked as client or serverside only, or even to not export at all. Useful for dev environment specific mods like ProbeJS or worldedit.
- [Github workflow/Actions](https://github.com/Deepacat/deepas-template-modpack/tree/dev/.github/workflows):
    - Autolinting
      - By default, will **automatically fix** any lint issues in the KubeJS folder, keeping your code organized hassle free.
        - This behavior can be easily changed as applicable, if you prefer it to check pull requests.
    - Build and Release
        - Automatically builds on commit, and will push to curseforge on a version change (Serverpacks included)
        - Automatically Diffs Mods and Pull Requests, and attaches your changelog
        - Can replace text with the update number, useful for the main menu or other files that should have the modpack version.
        - Can publish a truncated changelog to discord if a webhook is provided.
- Other:
    - Setup that allows contributors to pull and make commits directly from their minecraft instance, no need to drag files between them
    - Issue and pull request templates
    - A default `.gitignore` Including many common mods files, and client configs that should be used with the "configured defaults" mod
    - A client config default options setup, preventing users having their client configs overwritten on modpack updates

### Requirements
* This template was written with [Prism Launcher](https://prismlauncher.org/) in mind. Those using other launchers will need to adjust setup instructions as needed to allow their launcher to recognize the template as an instance.
* Expects understanding git and some basics like commits, pull requests, merging, pushing/pulling

## Setup
### As a template
1. Clone your copy of this template into an empty [`(instancename)\minecraft`](https://github.com/user-attachments/assets/f9de6554-925d-4827-b51c-c7159e6f915f) folder
2. Copy the contents of `(instancename)\minecraft\.pakku\prism-overrides`[^2] into your `(instancename)` folder to have a working [Prism Instance](https://prismlauncher.org/).[^3]

By default, the pack comes with a set of mods most packdevs find useful (optimization mods, KubeJS, Jade, etc); To add your mods and resourcepacks, open the project's /minecraft/ folder in a terminal (using a code editor such as VSC is recommended), and run [`java -jar pakku.jar add [<options>] [<projects>]`](https://juraj-hrivnak.github.io/Pakku/managing-projects.html#adding-projects). Pakku will handle dependencies for you.

### Importing into an existing repository
1. In your existing minecraft instance's `/minecraft/` folder, ensure that you have one of the following available: `manifest.json` `modrinth.index.json` `.mrpack`, or a curseforge `.zip` file. [(You can generate one with Prism)](https://github.com/user-attachments/assets/88f3518d-604f-46d9-a319-775c6daa05cb)
2. Clone the panpack template somewhere, copy over everything but `pakku-lock.json` (and `.gitattributes` and .git folder, of course)
3. Open up your terminal, [change directory](https://www.wikihow.com/images/thumb/0/08/Change-Directories-in-Command-Prompt-Step-7-Version-2.jpg/v4-460px-Change-Directories-in-Command-Prompt-Step-7-Version-2.jpg.webp) to your instance's `/minecraft/` folder, and run [`java -jar pakku.jar import <file from step 1>`](https://juraj-hrivnak.github.io/Pakku/managing-projects.html#adding-projects)
4. Edit `minecraft/pakku.json`, and `minecraft/.pakku/prism-overrides/` as applicable, and add `java -jar pakku.jar fetch` to your instance's [prelaunch commands](https://github.com/user-attachments/assets/494a632d-1af4-453d-9329-5454ac3d22da)

Don't forget to link to this page in your README so contributors will know how to set up their own instance!

### Contributing to an existing repository that uses this template
1. Clone your fork of the repository into an empty [`(instancename)\minecraft`](https://github.com/user-attachments/assets/f9de6554-925d-4827-b51c-c7159e6f915f) folder, and copy the contents of `(instancename)\minecraft\.pakku\prism-overrides` into your `(instancename)` folder to have a working Prism Instance. From there, you can start your newly created instances and the mods will be downloaded for you.[^3]

### Building and releasing
[Read here for build/release actions setup](https://github.com/Deepacat/deepas-template-modpack/blob/dev/.github/workflows/README.md)

## Usage
* To initate a release, update `CHANGELOG.MD` with a new version, [Unreleased] can be used as a staging ground for changes.
  * [Unreleased] changes are included in the changelog for builds created from the dev branch.
* Release type, overrides, and otherwise can be set in pakku.json
* Give the workflow read/write permissions

## Notes
* This template supports automatically posting changelogs to discord: add a [discord webhook](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) to your secrets with the name `discord-webhook` if you wish to enable that.
* Some of the linting rules are disabled by default, you can enable them (or even add your own!) by editing `custom-plugin.mjs`.
* By default, the changes on the `main` and `dev` branches are built, branch names and behavior can be adjusted in the buildscript.

<!-- #### I need a Curseforge Key?
Accessing CurseForge requires the CurseForge API key.

The API key can be generated in the CurseForge for Studios(https://console.curseforge.com/) developer console.

 1. Login to the developer console
 2. Go to the "API keys" tab
 3. Copy your API key
 4. Run `java -jar pakku.jar credentials set --cf-api-key '(API Key)'` in your `minecraft` folder via your terminal (to get to the folder, use the [cd](https://en.wikipedia.org/wiki/Cd_(command)) command). -->


## Credits
- Buildscript modified from [Terrafirmagreg](https://www.curseforge.com/minecraft/modpacks/terrafirmagreg-modern)

[^1]: Modrinth buildscripts are disabled by default, as most pack developers do not plan on releasing to modrinth due to important mods not being present, but can be easily uncommented if you do. If so, also add a `MODRINTH_TOKEN` and `MODRINTH_ID` secret and variable.
[^2]: The included `mmc-pack` is for forge 1.20.1, edit/replace `mmc-pack` with your own if on another version when initially setting up, just remember to add `PreLaunchCommand=java -jar pakku.jar fetch` to it. The repository will automatically update the `mmc-pack` in `prism-overrides` based on your pakku.lock when you push.
[^3]: In the event that the prelaunch fetch command isn't automatically applied, simply just add `java -jar pakku.jar fetch` (or one of it's varients) to your instance's [prelaunch commands](https://github.com/user-attachments/assets/494a632d-1af4-453d-9329-5454ac3d22da) manually.