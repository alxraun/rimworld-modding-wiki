# Modding Tutorials/Distribution

< [Modding Tutorials](https://rimworldwiki.com/wiki/Modding_Tutorials "Modding Tutorials")

This tutorial suggests steps to take when releasing your mod.

## Contents

* [1 Requirements](#Requirements)
* [2 Preparation](#Preparation)
* [3 Distribution](#Distribution)
  + [3.1 Steam](#Steam)
    - [3.1.1 Common issues](#Common_issues)
  + [3.2 GitHub](#GitHub)
  + [3.3 General](#General)

# Requirements[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=1 "Edit section: Requirements")]

* (Optional): This tutorial requires you to have [set up a solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution").
* (Optional): You will have to [have written code](https://rimworldwiki.com/wiki/Modding_Tutorials/Writing_custom_code "Modding Tutorials/Writing custom code") (there's no use in an empty project).

# Preparation[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=2 "Edit section: Preparation")]

1. Compile all your DLLs and make sure they're in the Assemblies folder.
2. Activate your mod. Test it. Your mod should not cause any errors, warnings or other logspam on startup.
3. **Recommended**: Consider a license, preferably one that will let other modders update and expand upon your mod.
4. **Optional**: Make a copy of your mod folder for release. This folder shouldn't contain files that aren't of interest to most users.
   1. Backups of older versions of textures and such can be removed from this folder.
   2. Your .git folder and the .vs and bin/obj folders made by your IDE/Visual Studio can be removed.
   * [Publisher Plus](https://steamcommunity.com/sharedfiles/filedetails/?id=1510554297) is a mod that gives you the option to exclude certain files and folders for uploading.
5. Update your About folder, that thing was made in a hurry anyways. Add a nice Preview.png to showcase your mod:
   1. A good preview will help garner interest in your mod. For Steam, your Preview.png can't be bigger than 1 MB. A resolution of 640x360 works best.
   2. Make sure you've updated the supportedVersions tag.
   3. Write a good description. A good description will have a sentence at the top stating what the mod does.

# Distribution[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=3 "Edit section: Distribution")]

## Steam[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=4 "Edit section: Steam")]

1. Put your prepared folder in the **Mods** subfolder of your RimWorld installation.
2. Enable dev mode (You're a mod developer now, you should already have it enabled).
3. Press the Mods button. If you have dev-mode enabled, there will be a button labeled "Upload on Steam".
   1. Once you've successfully uploaded the mod on Steam, there will be a new file called PublishedFileId.txt. It contains a long number like 1454024362 which is the unique ID for your workshop submission. This file is required to update your mod later. Only your Steam account can update the workshop submission with this number.
4. Set the visibility of your item. If you keep it hidden nobody will see it!

### Common issues[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=5 "Edit section: Common issues")]

* Your Preview.png can't be larger than 1MB in size.
* Your mod isn't a local folder.
* Dev mode not active.
* You are not logged in as the original author of the mod. This can happen when you're a contributor, or when the template mod you used had a pre-existing PublishedFileId.txt.
  + Make sure you are the original author and you're not re-uploading someone else's mod without permission.
* Steam not running or initialised. Make sure you're logged in, there are no errors regarding Steam or Steamworks in the output\_log (dev console doesn't always show these erros!) and that you can access the workshop.
* Steam (workshop) is down. Verify [here](https://downdetector.com/status/steam/map/).
* You have a [Limited User Account](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663). *Limited user accounts are prevented from accessing several features on Steam, including but not limited to Submitting content on the Steam Workshop.* Which is what uploading a mod is.

## GitHub[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=6 "Edit section: GitHub")]

1. If your mod has a GitHub repository, you can visit its <https://github.com/YOURUSERNAME/YOURREPOSITORY/releases> section to make a new release.
   1. <https://github.com/YOURUSERNAME/YOURREPOSITORY/releases/latest> will always link to your latest release.

## General[[edit](/index.php?title=Modding_Tutorials/Distribution&action=edit&section=7 "Edit section: General")]

1. Some mods include a Dropbox download link to their source code in the mod's topic.
2. Take a look at the "Mod release rules" [Thread](http://ludeon.com/forums/index.php?topic=10561);
3. Along with that, this [Thread](http://ludeon.com/forums/index.php?topic=7037) has tips on host sites and further naming conventions.
4. While possible to add your mod as an attachment to your Ludeon thread, these attachments are severely limited in size (max. 600kB) and are regularly deleted. They are also only visible to logged in users.

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/Distribution

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:47 -->
