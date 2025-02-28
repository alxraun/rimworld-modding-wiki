# Modding Tutorials

|  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| |  |  |  |  |  |  |  |  |  |  | | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | | |  |  |  |  |  |  |  |  |  | | --- | --- | --- | --- | --- | --- | --- | --- | --- | | [Basics](https://rimworldwiki.com/wiki/Basics "Basics") | [Menus](https://rimworldwiki.com/wiki/Menus "Menus") | [Game Creation](https://rimworldwiki.com/wiki/Game_Creation "Game Creation") | [Gameplay](https://rimworldwiki.com/wiki/Gameplay "Gameplay") | [Pawns](https://rimworldwiki.com/wiki/Pawns "Pawns") | [Plants](https://rimworldwiki.com/wiki/Plants "Plants") | [Resources](https://rimworldwiki.com/wiki/Resources "Resources") | [Gear](https://rimworldwiki.com/wiki/Gear "Gear") | **Mods** | |  |  |  | | --- | --- | | [Modding](https://rimworldwiki.com/wiki/Modding "Modding") | Modding Tutorials | |

---

This is the hub page for tutorials, guides, and reference materials for creating mods for RimWorld. If you are looking for instructions on how to use RimWorld, please check out the general [Modding](https://rimworldwiki.com/wiki/Modding "Modding") hub.

As RimWorld does not have a formal modding API, nearly all of the information here has been gathered and maintained by the modding community.

**NEW: [RimWorld 1.5 Mod\_Updates](https://rimworldwiki.com/wiki/Modding_Tutorials/RimWorld_1.5_Mod_Updates "Modding Tutorials/RimWorld 1.5 Mod Updates")** - A work-in-progress list of changes datamined by the modding community in the current unstable version of RimWorld 1.5. **THERE MAY BE ANOMALY SPOILERS, YOU HAVE BEEN WARNED.**

---

## Contents

* [1 About RimWorld](#About_RimWorld)
  + [1.1 XML Tutorials](#XML_Tutorials)
  + [1.2 C# Guides](#C.23_Guides)
  + [1.3 Slightly Outdated](#Slightly_Outdated)
  + [1.4 Uploading to Steam Workshop](#Uploading_to_Steam_Workshop)
* [2 Outdated / Under Review](#Outdated_.2F_Under_Review)
  + [2.1 XML tutorials](#XML_tutorials_2)
  + [2.2 C# tutorials](#C.23_tutorials)
  + [2.3 Art Tutorials](#Art_Tutorials)
  + [2.4 Under Construction](#Under_Construction)
  + [2.5 Dangerously Outdated](#Dangerously_Outdated)
* [3 External Links](#External_Links)

## About RimWorld[[edit](/index.php?title=Modding_Tutorials&action=edit&section=1 "Edit section: About RimWorld")]

RimWorld is a multi-platform game written on Unity 2019.4. However, the Unity Editor is not used for creating mods unless you are creating new shaders.

* [Recommended Software](https://rimworldwiki.com/wiki/Modding_Tutorials/Recommended_software "Modding Tutorials/Recommended software") - Editors and other useful software for mod development
* [Mod Folder Structure](https://rimworldwiki.com/wiki/Modding_Tutorials/Mod_Folder_Structure "Modding Tutorials/Mod Folder Structure") - Explore the basic folder structure of a mod
  + [About.xml](https://rimworldwiki.com/wiki/Modding_Tutorials/About.xml "Modding Tutorials/About.xml") - About.xml identifies and describes your mod to RimWorld so that it can be loaded properly
* [Defs](https://rimworldwiki.com/wiki/Modding_Tutorials/Defs "Modding Tutorials/Defs") - XML Definitions are used to define and configure content in a way that does not require compiling code
  + [MayRequire](https://rimworldwiki.com/wiki/Modding_Tutorials/MayRequire "Modding Tutorials/MayRequire") - MayRequire and MayRequireAnyOf are used to conditionally load Defs and list entries based on whether a DLC or other mod is loaded
* [Localization](https://rimworldwiki.com/wiki/Modding_Tutorials/Localization "Modding Tutorials/Localization") - Define text strings used for translations and word lists used in name and text generation
* [PatchOperations](https://rimworldwiki.com/wiki/Modding_Tutorials/PatchOperations "Modding Tutorials/PatchOperations") - PatchOperations are used to modify XML Defs without overwriting them directly
* [Sounds](https://rimworldwiki.com/wiki/Modding_Tutorials/Sounds "Modding Tutorials/Sounds") - (Needs Rewriting) Adding sound files for mods
* [Textures](https://rimworldwiki.com/wiki/Modding_Tutorials/Textures "Modding Tutorials/Textures") - How to create and add textures to mods

### XML Tutorials[[edit](/index.php?title=Modding_Tutorials&action=edit&section=2 "Edit section: XML Tutorials")]

The following are step-by-step tutorials for creating basic content mods.

Basic Tutorials:

* [Basic Melee Weapon](https://rimworldwiki.com/wiki/Modding_Tutorials/Basic_Melee_Weapon "Modding Tutorials/Basic Melee Weapon") - How to create a basic melee weapon with a texture mask
* [Basic Ranged Weapon](https://rimworldwiki.com/wiki/Modding_Tutorials/Basic_Ranged_Weapon "Modding Tutorials/Basic Ranged Weapon") - How to create a basic ranged weapon with custom sound effects
* [Basic Plant](https://rimworldwiki.com/wiki/Modding_Tutorials/Basic_Plant "Modding Tutorials/Basic Plant") - How to create a custom plant with both a cultivated and wild variant
* Custom Animal (Upcoming)
* Simple Building (Upcoming)
* Custom Workbench (Upcoming)
* Custom Drug (Upcoming)

Advanced Tutorials:

* Custom Faction (Upcoming)
* Custom Culture (Upcoming)
* Custom Trader Type (Upcoming)

### C# Guides[[edit](/index.php?title=Modding_Tutorials&action=edit&section=3 "Edit section: C# Guides")]

C# is used to create and define custom game behaviors

* [Decompiling Source Code](https://rimworldwiki.com/wiki/Modding_Tutorials/Decompiling_source_code "Modding Tutorials/Decompiling source code") - How to set up and use a decompiler to read vanilla game code
* [Setting up a Solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution") - How to set up a solution for compiling a custom mod assembly
* [Application Startup](https://rimworldwiki.com/wiki/Modding_Tutorials/Application_Startup "Modding Tutorials/Application Startup") - Describes the application startup process and the order in which game data is loaded
* Custom Consumable (Upcoming)
* Custom Overlays (Upcoming)

### Slightly Outdated[[edit](/index.php?title=Modding_Tutorials&action=edit&section=4 "Edit section: Slightly Outdated")]

* [Plague Gun](https://rimworldwiki.com/wiki/Modding_Tutorials/Plague_Gun_(1.1) "Modding Tutorials/Plague Gun (1.1)") - This tutorial was created for RimWorld 1.0 but updated for 1.4. While the exact content is obsolete as you can now accomplish the same result with purely vanilla XML, it is still useful as a crash course for end-to-end mod creation and is here until newer tutorials can replace it.

### Uploading to Steam Workshop[[edit](/index.php?title=Modding_Tutorials&action=edit&section=5 "Edit section: Uploading to Steam Workshop")]

* You can upload your mod to Steam Workshop by enabling Development Mode from your game Options and then using the Upload option under the Advanced button in the vanilla mod manager.
* Note that in order to upload to Steam Workshop, you must own the game on Steam Workshop. Owning RimWorld on GOG or Epic will not work.
* Your Preview.png should be a 640x360 or 1280x720 PNG and **must** be under 1MB. If it is too large, then your upload will be rejected with `Error : Limit Exceeded`
* If you get a `OnItemSubmitted Fail` error, make sure you close any programs that are targeting items in your mods folder. This can also mean that Steam Workshop is having some technical issues at the moment. If it keeps occurring, then the only thing to do is to wait a few hours for it to clear up.
* Steam mod descriptions don't use markdown, they use a variant of BBCode. Please check out the [Steam text formatting guide](https://steamcommunity.com/comment/Guide/formattinghelp).

---

**Note:** All of the above tutorials have been cleaned up and reviewed by the #mod-development team on the [RimWorld Discord](https://discord.gg/rimworld) in cooperation with RimWorld Wiki staff editors. Please let us know before creating, adding, or making any major edits to the vetted tutorials and guides section!

## Outdated / Under Review[[edit](/index.php?title=Modding_Tutorials&action=edit&section=6 "Edit section: Outdated / Under Review")]

The following tutorials are either out of date or in need of a rewrite. The information in them might be useful but may not be up to standard; please be aware of any potential inaccuracies until they can be addressed.

* [First Steps and Some Links](https://rimworldwiki.com/wiki/Modding_Tutorials/First_Steps "Modding Tutorials/First Steps")
* [Essence of Modding](https://rimworldwiki.com/wiki/Modding_Tutorials/Essence "Modding Tutorials/Essence")
* [Modding Troubleshooting Tips and Guides](https://rimworldwiki.com/wiki/Modding_Troubleshooting_Tips_and_Guides "Modding Troubleshooting Tips and Guides")
* [Testing Mods](https://rimworldwiki.com/wiki/Modding_Tutorials/Testing_mods "Modding Tutorials/Testing mods")
  + [Development Mode](https://rimworldwiki.com/wiki/Modding_Tutorials/Testing_mods#Development_mode "Modding Tutorials/Testing mods")
* [Adding and Testing Sounds](https://rimworldwiki.com/wiki/Modding_Tutorials/Sounds "Modding Tutorials/Sounds")
* [Decompiling Texture/Sound Assets](https://rimworldwiki.com/wiki/Modding_Tutorials/Assets "Modding Tutorials/Assets")
* [Compatibility](https://rimworldwiki.com/wiki/Modding_Tutorials/Compatibility "Modding Tutorials/Compatibility")
* [Distribution](https://rimworldwiki.com/wiki/Modding_Tutorials/Distribution "Modding Tutorials/Distribution")
* [Modifying Defs](https://rimworldwiki.com/wiki/Modding_Tutorials/Modifying_defs "Modding Tutorials/Modifying defs")
* [Troubleshooting mods](https://rimworldwiki.com/wiki/Modding_Tutorials/Troubleshooting "Modding Tutorials/Troubleshooting")

(Redundant, need to be reviewed and consolidated or removed)

* [Getting Started With XML](https://rimworldwiki.com/wiki/Modding_Tutorials/Getting_started_with_mods "Modding Tutorials/Getting started with mods")
* [Adding a New Weapon, Trait and Research to the game](https://rimworldwiki.com/wiki/Modding_Tutorials/Xml_Adding_Weapons_Traits_Research "Modding Tutorials/Xml Adding Weapons Traits Research")
* [Making Patches using Xml](https://rimworldwiki.com/wiki/Modding_Tutorials/Xml_Patches "Modding Tutorials/Xml Patches")

### XML tutorials[[edit](/index.php?title=Modding_Tutorials&action=edit&section=7 "Edit section: XML tutorials")]

* [XML File Structure](https://rimworldwiki.com/wiki/Modding_Tutorials/XML_file_structure "Modding Tutorials/XML file structure")
* [Introduction to XML Defs](https://rimworldwiki.com/wiki/Modding_Tutorials/XML_Defs "Modding Tutorials/XML Defs")
  + [XML Def Compatibility](https://rimworldwiki.com/wiki/Modding_Tutorials/Compatibility_with_defs "Modding Tutorials/Compatibility with defs")
  + [ThingDef explained](https://rimworldwiki.com/wiki/Modding_Tutorials/ThingDef "Modding Tutorials/ThingDef")
  + [Weapons\_Guns.xml explained](https://rimworldwiki.com/wiki/Modding_Tutorials/Weapons_Guns "Modding Tutorials/Weapons Guns"). Slightly dated.

### C# tutorials[[edit](/index.php?title=Modding_Tutorials&action=edit&section=8 "Edit section: C# tutorials")]

* [Hello World](https://rimworldwiki.com/wiki/Modding_Tutorials/Hello_World "Modding Tutorials/Hello World")
* [Writing Custom Code](https://rimworldwiki.com/wiki/Modding_Tutorials/Writing_custom_code "Modding Tutorials/Writing custom code")
* [Linking XML and C#](https://rimworldwiki.com/wiki/Modding_Tutorials/Linking_XML_and_C "Modding Tutorials/Linking XML and C")
* [Alter Code at Runtime with Harmony](https://rimworldwiki.com/wiki/Modding_Tutorials/Harmony "Modding Tutorials/Harmony") - this is a best practice for modifying game code, replacing C# code injection to reduce Mod Conflicts
* [Adding fields and methods to classes](https://rimworldwiki.com/wiki/Modding_Tutorials/Modifying_classes "Modding Tutorials/Modifying classes")
* [Mod settings](https://rimworldwiki.com/wiki/Modding_Tutorials/ModSettings "Modding Tutorials/ModSettings") - Add settings to your mod
* [Def mod extensions](https://rimworldwiki.com/wiki/Modding_Tutorials/DefModExtension "Modding Tutorials/DefModExtension") - Add (custom) fields to Defs
* [Custom Comp Classes](https://rimworldwiki.com/wiki/Modding_Tutorials/Custom_Comp_Classes "Modding Tutorials/Custom Comp Classes") - A quick overview of what types of Comps there are, and what they're suited for.
* [ThingComp](https://rimworldwiki.com/wiki/Modding_Tutorials/ThingComp "Modding Tutorials/ThingComp") - Learn all there is to know about ThingComps.
* [Components](https://rimworldwiki.com/wiki/Modding_Tutorials/GameComponent "Modding Tutorials/GameComponent") - GameComponents, WorldComponents, and MapComponents
* [Introduction to Def Classes](https://rimworldwiki.com/wiki/Modding_Tutorials/Def_classes "Modding Tutorials/Def classes")
* [Using Harmony to optionally patch other mods for the sake of compatibility](https://rimworldwiki.com/wiki/Modding_Tutorials/Compatibility_with_DLLs "Modding Tutorials/Compatibility with DLLs")
* [TweakValues](https://rimworldwiki.com/wiki/Modding_Tutorials/TweakValue "Modding Tutorials/TweakValue") - Change values on the fly (handy for quick iteration!)
* [ExposeData](https://rimworldwiki.com/wiki/Modding_Tutorials/ExposeData "Modding Tutorials/ExposeData") - Save stuff
* [The big ass list of useful classes](https://rimworldwiki.com/wiki/Modding_Tutorials/BigAssListOfUsefulClasses "Modding Tutorials/BigAssListOfUsefulClasses") - A non-exhaustive list of classes you'll use most
* [Grammar Resolver](https://rimworldwiki.com/wiki/Modding_Tutorials/GrammarResolver "Modding Tutorials/GrammarResolver") - PAWN\_objective, PAWN\_possessive? Find out what it all means here.
* [ExampleJob](https://github.com/Mehni/ExampleJob/wiki) - Mehni's top to bottom breakdown of Jobs.
* [Config Errors](https://rimworldwiki.com/wiki/Modding_Tutorials/ConfigErrors "Modding Tutorials/ConfigErrors") - Provide configuration issues to the user on startup.
* [Debug Actions](https://rimworldwiki.com/wiki/Modding_Tutorials/DebugActions "Modding Tutorials/DebugActions") - Call methods from the debug menu
* [Getting started with RimWorld modding on Linux](https://www.arp242.net/rimworld-mod-linux.html)

### Art Tutorials[[edit](/index.php?title=Modding_Tutorials&action=edit&section=9 "Edit section: Art Tutorials")]

* [Artstyle](https://spdskatr.github.io/RWModdingResources/artstyle) - Officially unofficial guide to RimWorld's Artstyle
* [Ekksu's guide to creating RimWorld animals](https://www.reddit.com/r/RimWorld/comments/5tn1pi/rimworldstyle_sprite_tutorials/)
* [ChickenPlucker's guide to creating apparel](https://steamcommunity.com/sharedfiles/filedetails/?id=1114369188)
* [Seraphile's guide to masks](https://github.com/seraphile/rimshare/wiki/Colouring-in-Images)

### Under Construction[[edit](/index.php?title=Modding_Tutorials&action=edit&section=10 "Edit section: Under Construction")]

These are currently unfinished and need to be cleaned up or removed

* [Modding Tutorials/Quests](https://rimworldwiki.com/wiki/Modding_Tutorials/Quests "Modding Tutorials/Quests")
* [Modding Tutorials/Troubleshooting/Finding Exceptions](https://rimworldwiki.com/wiki/Modding_Tutorials/Troubleshooting/Finding_Exceptions "Modding Tutorials/Troubleshooting/Finding Exceptions")

### Dangerously Outdated[[edit](/index.php?title=Modding_Tutorials&action=edit&section=11 "Edit section: Dangerously Outdated")]

* [RimWorld 1.3: XML Mod Creation](https://rimworldwiki.com/wiki/RimWorld_1.3:_XML_Mod_Creation "RimWorld 1.3: XML Mod Creation")
* [Assembly Modding](https://rimworldwiki.com/wiki/Modding_Tutorials/Assembly_Modding_Example "Modding Tutorials/Assembly Modding Example")
* [The Plague Gun](https://rimworldwiki.com/wiki/Plague_Gun_(1.1) "Plague Gun (1.1)") tutorial originally by Jecrell, updated to 1.1+.
* [Modding Tutorials/Xenotype template](https://rimworldwiki.com/wiki/Modding_Tutorials/Xenotype_template "Modding Tutorials/Xenotype template") originally by Ryflamer

* [Modding Tutorials/Smelter](https://rimworldwiki.com/wiki/Modding_Tutorials/Smelter "Modding Tutorials/Smelter")
* [Modding Tutorials/Items](https://rimworldwiki.com/wiki/Modding_Tutorials/Items "Modding Tutorials/Items")
* [Modding Tutorials/Furniture](https://rimworldwiki.com/wiki/Modding_Tutorials/Furniture "Modding Tutorials/Furniture")

## External Links[[edit](/index.php?title=Modding_Tutorials&action=edit&section=12 "Edit section: External Links")]

* [Roxxploxx's set of modding tutorials](https://github.com/roxxploxx/RimWorldModGuide/wiki)
* [RimWorld Modding Resources - A hub for guides, modders, practical tips](https://spdskatr.github.io/RWModdingResources/)

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:08 -->
