# Modding Tutorials/Recommended software

< [Modding Tutorials](https://rimworldwiki.com/wiki/Modding_Tutorials "Modding Tutorials")

This is a short list of programs that are useful for making mods. Most of the options listed here are free, but if you have development experience and have a favorite IDE, then by all means use what you're familiar and/or comfortable with!

## Contents

* [1 XML Editors](#XML_Editors)
* [2 IDEs/Editors](#IDEs.2FEditors)
* [3 C# Decompilers](#C.23_Decompilers)
* [4 Graphics Software](#Graphics_Software)
  + [4.1 Raster Graphics](#Raster_Graphics)
  + [4.2 Vector Graphics](#Vector_Graphics)
* [5 Audio Software](#Audio_Software)
* [6 Additional Links](#Additional_Links)

# XML Editors[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=1 "Edit section: XML Editors")]

While XML files are simple text files, basic Windows Notepad lacks any kind of syntax highlighting and rich text editors such as WordPad will insert invisible characters that are not recommended for machine-readable data files. The following are basic code editors that are recommended for writing or editing XML files:

| Software | OS | Description |
| --- | --- | --- |
| [Notepad++](http://notepad-plus-plus.org/download/) | Windows | Light-weight editor with extension support. Adding the "XML Tools" plugin is recommended for editing XML files, as it adds additional validation and error-checking. |
| [Visual Studio Code](https://code.visualstudio.com/) | Linux/Windows/Mac | A text editor with emphasis on productivity and extension support. Also has an excellent XML plugin. |
| [Sublime Text](http://www.sublimetext.com/) | Linux/Windows/Mac | Natively supports many programming languages and markup languages, and its functionality can be extended by users with plugins - A brief overview of its functionality can be found [here](http://webdesign.tutsplus.com/tutorials/useful-shortcuts-for-a-faster-workflow-in-sublime-text-3--cms-22185) and a full course [here](http://code.tutsplus.com/courses/perfect-workflow-in-sublime-text-2) |
| [XML Notepad](http://xmlnotepad.codeplex.com/releases/) | Windows | XML-oriented editor with a lot of features you will most likely never use in Rimworld modding |
| [Geany](http://www.geany.org/) | Linux/Windows/Mac | Extremely lightweight text editor with syntax highlighting and some other basic features oriented towards XML and code editing. |

# IDEs/Editors[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=2 "Edit section: IDEs/Editors")]

Writing C# code for custom assemblies requires not only code editing capability but also the ability to compile raw code into executable code. This is generally best done in an Integrated Development Environment that has a compiler built-in.

| Software | OS | Description |
| --- | --- | --- |
| [Visual Studio](http://www.visualstudio.com/) | Windows/Mac | Microsoft's flagship IDE and strongly recommended if you are running Windows. The [Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs) is free for individuals and small teams, and does everything you need to make a RimWorld mod assembly out of the box. |
| [Visual Studio Code](https://code.visualstudio.com/) | Linux/Windows/Mac | VS Code can technically be used for C# as well, but you will need to do a little more legwork to get it to actually build the assembly as it's not a true IDE. VS Code is generally recommended if you are using Linux, since there is no Visual Studio for Linux. |
| [SharpDevelop](http://www.icsharpcode.net/OpenSource/SD/Download/) | Windows | Free IDE specifically made for C# |
| [Rider](https://www.jetbrains.com/rider/) | Windows/Mac/Linux | Offers a lot of tools across all platforms. Free for non-commercial use since [October 24, 2024](https://blog.jetbrains.com/blog/2024/10/24/webstorm-and-rider-are-now-free-for-non-commercial-use/). |

Whichever IDE you choose, make sure it's capable of compiling for .NET Framework 4.7.2. If you for whatever reason don't have this framework installed, you can download it [here](https://dotnet.microsoft.com/download/dotnet-framework/net472).

# C# Decompilers[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=3 "Edit section: C# Decompilers")]

*See [Decompiling source code](https://rimworldwiki.com/wiki/Modding_Tutorials/Decompiling_source_code "Modding Tutorials/Decompiling source code")*

# Graphics Software[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=4 "Edit section: Graphics Software")]

For making textures, you will need a graphics program. The following are simply popular options, any software that can export transparent PNGs will work for RimWorld though.

## Raster Graphics[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=5 "Edit section: Raster Graphics")]

Raster graphics are graphics that are made of pixels. They can lose detail when resized, but may be more comfortable to use for those with traditional art experience.

| Software | OS | Description |
| --- | --- | --- |
| [Adobe Photoshop](http://www.photoshop.com/products) | Windows/Mac | RimWorld's original graphics are made in Photoshop and the official Art Source generally contains PSD files. Photoshop is the gold standard for graphics software, but also very expensive. There are a lot of free alternatives. |
| [GIMP](http://www.gimp.org/) | Linux/Windows/Mac | A free, open-source alternative to Photoshop that's been around since 1998. GIMP has a large user community, with great list of [tutorials](http://www.gimp.org/tutorials/) on the official sites |
| [Krita](https://krita.org/en/) | Linux/Windows/Mac | A free digital painting software. Better suited than GIMP for freehand painting and should feel like home pretty quick for Photoshop users. |
| [Paint.net](https://www.getpaint.net/) | Windows | A free photo editing program preferred by some modders. It is Windows-only. |

## Vector Graphics[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=6 "Edit section: Vector Graphics")]

Vector graphics are created using mathematical formulae for lines and shading. They may be easier to work with if you do not have an art tablet or prior art experience, and RimWorld's simplistic art style is easily emulated with vector graphics. Note however that as noted above, RimWorld's art source is in Photoshop files and not vectors as many people believe.

| Software | OS | Description |
| --- | --- | --- |
| [Adobe Illustrator](https://www.adobe.com/products/illustrator.html) | Windows/Mac | Adobe's primary vector drawing software package. Also the gold standard for vector graphics, but just like Photoshop is expensive. A few free options also exist. |
| [Inkscape](http://inkscape.org/en/download/) | Linux/Windows/Mac | Free vector graphic program with a steeper learning curve due to the vector based image editing. |

# Audio Software[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=7 "Edit section: Audio Software")]

Recording your own audio for mods might be too much to ask, but editing audio is always possible. This can be done with the following software or your own alternative:

| Software | OS | Description |
| --- | --- | --- |
| [Audacity](http://sourceforge.net/projects/audacity/) | Linux/Windows/Mac | Audio editing software with many tutorials online, lots of useful functionality and a relatively readable interface |

# Additional Links[[edit](/index.php?title=Modding_Tutorials/Recommended_software&action=edit&section=8 "Edit section: Additional Links")]

* [Mod Folder Structure](https://rimworldwiki.com/wiki/Modding_Tutorials/Mod_Folder_Structure "Modding Tutorials/Mod Folder Structure") - How to set up a mod folder.
* [Setting up a solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution") - How to set up a C# solution and project for building a RimWorld mod assembly.

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/Recommended_software

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:00 -->
