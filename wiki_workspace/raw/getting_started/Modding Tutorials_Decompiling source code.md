# Modding Tutorials/Decompiling source code

< [Modding Tutorials](https://rimworldwiki.com/wiki/Modding_Tutorials "Modding Tutorials")

The base game provides a bunch of code snippets in *../Source/*, relative to your Rimworld installation. Since this isn't a lot, one might want to take a look at the game's full source code. RimWorld's [EULA](https://rimworldgame.com/eula/) allows you to decompile the game for personal use. It's recommended to read it.

The following programs are used and recommended by modders:

## Contents

* [1 Decompiling source code](#Decompiling_source_code)
  + [1.1 ILSpy](#ILSpy)
  + [1.2 dnSpy](#dnSpy)
  + [1.3 Rider / dotPeek](#Rider_.2F_dotPeek)
  + [1.4 MonoDevelop](#MonoDevelop)
  + [1.5 MacOS Directories](#MacOS_Directories)
* [2 How to make use of a decompiler](#How_to_make_use_of_a_decompiler)
* [3 Tips when decompiling](#Tips_when_decompiling)
* [4 See also](#See_also)

# Decompiling source code[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=1 "Edit section: Decompiling source code")]

### ILSpy[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=2 "Edit section: ILSpy")]

ILSpy is generally recommended as it is the best-maintained and most reliable decompiler. The core project only has binaries for Windows, but there is an [Avalonia-based port](https://github.com/icsharpcode/AvaloniaILSpy) as well as a [CLI application for Mono framework](https://github.com/icsharpcode/ILSpy#unix--mac) for OS X and Linux.

1. Download [ILSpy](http://ilspy.net/) ([Download latest release](https://github.com/icsharpcode/ILSpy/releases)) and extract it to a directory of your choosing. Optionally create a desktop shortcut;
2. **Either**: associate the .dll extension with ILSpy:
   1. Navigate to *Assembly-CSharp.dll* in *../Rimworld\*\*\*\_Data/Managed/*, relative to your Rimworld installation and with \*\*\* being a version number (See Note on MacOS below);
   2. Right-click "Open with" and select a standard program. Navigate to your ILSpy installation and double-click *ILSpy.exe*, tick the checkbox and accept;
   3. Double-click *Assembly-CSharp.dll*,
3. **Or**: open ILSpy and open a .dll:
   1. Open ILSpy;
   2. Go to File -> Open or press Ctrl+O, navigate to *../Rimworld\*\*\*\_Data/Managed/*, relative to your Rimworld installation and with \*\*\* being a version number (See Note on MacOS below);
   3. Select *Assembly-CSharp.dll* and confirm,
4. Click the "+" next to *Assembly-CSharp (\*\*\*)*, you will now see a list including the items *Rimworld* and *Verse*;
5. Take your time to look through the source code, to make yourself familiar. If you ever need the source code, open ILSpy again:
   1. Ctrl+Shift+F or Ctrl+E opens the search bar which can be used to search through all loaded assemblies;
   2. Ctrl+F opens a search bar for the currently opened decompiled class.

If using CLI application:

1. Despite the instructions saying .NET 5.0 SDK is needed, you may also need [.NET Core 3.1](https://dotnet.microsoft.com/download/dotnet/3.1);
2. It seems that it's possible to simply build from a [release tarball](https://github.com/icsharpcode/ILSpy/releases), even if the instructions suggest to use a git checkout;
3. After you've built ILSpy, find the *ilspycmd* tool (should be located at *ICSharpCode.Decompiler.Console/bin/Release/netcoreapp3.1*);
4. Run a command like *ilspycmd RimWorld/RimWorld\*\_Data/Managed/Assembly-CSharp.dll -p -o <output directory>* (use proper paths to RimWorld and for the output directory);
5. The given output directory now contains decompiled sources.

### dnSpy[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=3 "Edit section: dnSpy")]

dnSpy is an alternative with a Visual Studio editor feel. At the time of this writing, however, the original project has been archived for more than two years and none of its forks have reached a similar level of adoption. Decompilation glitches can occur.

1. Download [dnSpy](https://github.com/dnSpyEx/dnSpy/releases) and extract it somewhere.
2. Open dnSpy.exe. Once it's open, click "open" on the top ribbon (or press Ctrl+O).
3. Navigate to *../Rimworld\*\*\*\_Data/Managed/*, relative to your Rimworld installation and with \*\*\* being a version number.
4. Ctrl+Shift+K to open the search bar.
5. Explore the assembly and look through the source code to your heart's desire.

### Rider / dotPeek[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=4 "Edit section: Rider / dotPeek")]

Rider is a cross-platform IDE with a built-in decompiler. If you're using Rider as your IDE ([Setup Instructions](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution#Rider_.28good_for_Mac.29 "Modding Tutorials/Setting up a solution")), you can view the source of any RimWorld class or method by right-clicking its name and clicking Go To > Definition.

The developer of Rider also offers a free standalone decompiler in the form of [dotPeek](https://www.jetbrains.com/decompiler/). dotPeek is preferred by some for reading IL Code for the purpose of Harmony transpilers, but suffers similar glitches and inconsistency as dnSpy when decompiling back to C# code.

### MonoDevelop[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=5 "Edit section: MonoDevelop")]

MonoDevelop is capable of decompiling DLLs, albeit using clumsy initial settings. It is Linux only, otherwise you have to download Xamarin Studio which doesn't have a decompiler.

1. Download [MonoDevelop](http://www.monodevelop.com/download/) and install it;
2. **Either**: associate the .dll extension with MonoDevelop:
   1. Navigate to *Assembly-CSharp.dll* in *../Rimworld\*\*\*\_Data/Managed/*, relative to your Rimworld installation and with \*\*\* being a version number (See Note on MacOS below);
   2. Right-click "Open with" and select MonoDevelop as standard program;
   3. Double-click *Assembly-CSharp.dll*,
3. **Or**: open MonoDevelop and open a .dll:
   1. Open MonoDevelop;
   2. Go to File -> Open, navigate to *../Rimworld\*\*\*\_Data/Managed/*, relative to your Rimworld installation and with \*\*\* being a version number (See Note on MacOS below);
   3. Select *Assembly-CSharp.dll* and confirm,
4. **Very important**: search for a dropdown called "Visibility" and change it from "Only public members" to "All members";
5. **Very important**: search for a dropdown called "Language" and change it from "Summary" to "C#";
6. Click the "+" next to *Assembly-CSharp (\*\*\*)*, you will now see a list including the items *Rimworld* and *Verse*;
7. Take your time to look through the source code, to make yourself familiar. If you ever need the source code, open *Assembly-CSharp.dll* again.

### MacOS Directories[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=6 "Edit section: MacOS Directories")]

For Macs, directories are similar but in: ../RimWorldMac.app/Contents/Resources/Data/Managed.

For Steam installed RimWorld, find your app here: ~/Library/Application Support/Steam/steamapps/common/RimWorld/RimWorldMac.app.

# How to make use of a decompiler[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=7 "Edit section: How to make use of a decompiler")]

Main article: [Modding\_Tutorials/ThingDef](https://rimworldwiki.com/wiki/Modding_Tutorials/ThingDef "Modding Tutorials/ThingDef")

# Tips when decompiling[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=8 "Edit section: Tips when decompiling")]

1. Right-click any Type or Method and hit "analyse" to obtain more context on that item. 'Used by' and 'Uses' provide a lot of contextual clues which is required to know how things work.
2. RimWorld often uses reflection to instantiate Workers and MakeThings. This means no decompiler will cleanly find what/where an instance of a class is created. Hint: if you find yourself using 'new Pawn()', you're doing it wrong.
3. If you're going in circles trying to find things like "where is X assigned", odds are you'll need to look at the XML for it. The XML contains the data, C# does things with it.

# See also[[edit](/index.php?title=Modding_Tutorials/Decompiling_source_code&action=edit&section=9 "Edit section: See also")]

* [Writing custom code](https://rimworldwiki.com/wiki/Modding_Tutorials/Writing_custom_code "Modding Tutorials/Writing custom code") shows a broad overview of C# coding.

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/Decompiling_source_code

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:37 -->
