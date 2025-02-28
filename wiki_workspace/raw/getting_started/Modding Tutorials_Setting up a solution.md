# Modding Tutorials/Setting up a solution

< [Modding Tutorials](https://rimworldwiki.com/wiki/Modding_Tutorials "Modding Tutorials")

*This page was originally created by [Alistaire](https://rimworldwiki.com/wiki/User:Alistaire "User:Alistaire").*

In this tutorial you will learn how to set up a solution, along with instructions on setting the output directory and files for more convenient building right into the Assemblies folder.

## Contents

* [1 Requirements](#Requirements)
* [2 Setting up a solution](#Setting_up_a_solution)
  + [2.1 Visual Studio Community 2022](#Visual_Studio_Community_2022)
    - [2.1.1 Option 1 (Manual Method):](#Option_1_.28Manual_Method.29:)
    - [2.1.2 Option 2 (Using Nuget):](#Option_2_.28Using_Nuget.29:)
    - [2.1.3 Option 3 (Using Rimworld Dotnet Template):](#Option_3_.28Using_Rimworld_Dotnet_Template.29:)
    - [2.1.4 Option 4 (Using Cookiecutter):](#Option_4_.28Using_Cookiecutter.29:)
  + [2.2 Sharpdevelop](#Sharpdevelop)
  + [2.3 Linux](#Linux)
  + [2.4 Xamarin/MonoDevelop](#Xamarin.2FMonoDevelop)
  + [2.5 Rider (good for Mac)](#Rider_.28good_for_Mac.29)
  + [2.6 .NET Framework via Commmandline](#.NET_Framework_via_Commmandline)
  + [2.7 Visual Studio Code via Dev Container](#Visual_Studio_Code_via_Dev_Container)
* [3 Common issues](#Common_issues)
* [4 See also](#See_also)

# Requirements[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=1 "Edit section: Requirements")]

* The manual option in this tutorial requires you to have [set up a Source and Assemblies folder](https://rimworldwiki.com/wiki/Modding_Tutorials/Mod_folder_structure#The_Source_and_Assemblies_folders "Modding Tutorials/Mod folder structure") (the Visual Studio automatic option sets this up for you).
* You will want to have an IDE installed: [Recommended IDE's](https://rimworldwiki.com/wiki/Modding_Tutorials/Recommended_software#IDE.27s "Modding Tutorials/Recommended software").

# Setting up a solution[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=2 "Edit section: Setting up a solution")]

Setting up can be different for different IDE's. Feel free to add ***complete*** instructions for your IDE of choice.

### Visual Studio Community 2022[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=3 "Edit section: Visual Studio Community 2022")]

*NOTE: Visual Studio 2022 is a rather heavy application (2-3 GB for basic functionality) but works out of the box. It is strongly recommended if you are on Windows and have a PC that can handle it. This tutorial is similar for other versions of Visual Studio.*

#### Option 1 (Manual Method):[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=4 "Edit section: Option 1 (Manual Method):")]

1. Create a new class library project
   1. Once loaded, go to File -> New -> Project...
   2. Type "Class Library (.NET Framework)" in the search bar, and select the C# option.

      [![](/images/thumb/f/f3/ClassLibrary.png/200px-ClassLibrary.png)](https://rimworldwiki.com/wiki/File:ClassLibrary.png)

      Installing the .NET framework
   3. Enter your project name.
   4. Choose a location, preferably:  

      ```
      (RimWorldInstallFolder)/Mods/(YourModName)/Source
      ```
   5. Enter a solution name (optionally, tick "Place solution and project in the same directory")
   6. Make sure Framework is ".NET Framework 4.7.2"
2. In your project, set target framework and various other properties
   1. In your Solution Explorer (the panel usually located on the right), right click your project -> Properties (or expand your project and double click "Properties" with the wrench icon)
   2. *Optional*: Under Application, change your Assembly and Namespace names to anything of your choice
   3. Go to Build -> Advanced... and set "Debugging information" to none
   4. Leave Advanced..., and set the Output Path to "..\..\Assemblies\" (Or wherever the Assemblies folder is)
3. Add references to RimWorld code
   1. Expand your project in Solution Explorer. Then right click "References" -> Add Reference...
   2. Click Browse...
   3. Navigate towards

      ```
      RimWorldInstallPath/RimWorld******_Data/Managed
      ```

      and select files:   

      ```
      Assembly-CSharp.dll
      UnityEngine.CoreModule.dll
      ```
   4. Click "Add"
   5. Click "OK" to close the Reference Manager.
   6. Right-click on both Assembly-CSharp.dll and UnityEngine.dll and set Copy Local to False (Properties pane).

#### Option 2 (Using Nuget):[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=5 "Edit section: Option 2 (Using Nuget):")]

This option uses [Krafs Rimworld Reference Package](https://www.nuget.org/packages/Krafs.Rimworld.Ref) it is less involved than using reference assemblies and is the recommended method.

1. Follow the above up till *Add references to RimWorld code*
2. From the Tools menu, select NuGet Package Manager -> Package Manager Settings.
   1. In the Settings dialog, under Package Management, change the *Default package management format* to **PackageReference**.
   2. Click OK to close the dialog.
3. Open Nuget Manager and type *Rimworld*
4. Add Krafs Rimworld Reference

You can now continue as if you added the assemblies
Doing this makes your project portable, because RimRef can be downloaded by anyone and used from anywhere, unlike Rimworld's assemblies which can't be distributed.

#### Option 3 (Using Rimworld Dotnet Template):[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=6 "Edit section: Option 3 (Using Rimworld Dotnet Template):")]

This option uses [Rimworld Dotnet Template](https://github.com/Zeta-of-the-rim/Rimwold-Dotnet-Template) it allows faster creation of mod files including Xml folders

After installing the template.

1. Open your mod folder
2. create a new folder for your mod (It is best to use the name you want for your mod)
3. Open a command prompt in that folder
4. type *dotnet new RimMod* (This will create a new mod with the name you specified)
5. Open the About folder and edit the About.xml file
6. Open the .sln file in your preferred IDE
7. Add the rimworld assemblies using your preferred method

While this method is faster, it is still good to know how to do it manually.

#### Option 4 (Using Cookiecutter):[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=7 "Edit section: Option 4 (Using Cookiecutter):")]

This option uses the [Rimworld Mod Development Cookiecutter](https://ludeon.com/forums/index.php?topic=39038.0) tool.  
**Note: despite being automatic and potentially taking away some of the tedium away, the environment it sets up is very particular and this tool is currently not recommended for newcomers.**  
*As of Jan 2019, the cookiecutter is set up for Windows development. Linux/Mac people can still use it, but they will have a few errors to clean up.*

1. Open Visual Studio
2. Once loaded, go to File -> New -> From Cookiecutter...
3. Search for *rimworld*
4. Double-click *cookiecutter-rimworld-mod-development*
5. Change the Template Options:
   1. *Create To* => *Your/Rimworld/Mod/Directory*
   2. *mod\_name*
   3. *namespace\_name* (don't change if unsure)
   4. *author* => *your steam username*
   5. *target\_version* => current RW version (can leave blank for most up-to-date)
   6. *in\_game\_description* (not required, can change later in About-Release.xml)
   7. *url* (can leave blank for link to your Steam Workshop profile)
6. Click "Create and Open Folder"

### Sharpdevelop[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=8 "Edit section: Sharpdevelop")]

**Caution:** Sharpdevelop (or #develop) does NOT CURRENTLY allow for C# 6.0+ syntax without plugins and does NOT ALLOW for C# 7.0+ syntax at all. For your average project this does not matter, however some existing projects are already built entirely upon C# 6.0+ syntax which can not be compiled anymore in Sharpdevelop. Visual Studio does not have these issues and should be your go-to for compiling large projects such as Combat Extended.

1. Create a new class library project in your [IDE of choice](https://rimworldwiki.com/wiki/Modding_Tutorials/Recommended_software#IDE.27s "Modding Tutorials/Recommended software");
   1. Go to File -> New -> Solution;
   2. Go to C# or .NET -> Library or Class Library (NOT portable);
   3. Enter a project name (solution name automatically updated);
   4. Choose a location, preferably:  

      ```
      (RimWorldInstallFolder)/Mods/(YourModName)/Source
      ```
   5. *Optional*: Untick "Create a directory for solution"/"Create a project within the solution directory",
2. In your project, add references to Assembly-CSharp.dll and UnityEngine.dll:
   1. In your IDE project file browser, right-click the "References" folder and "Add reference";
   2. Choose the ".NET Assembly Browser" tab and "Browse...";
   3. Navigate towards

      ```
      RimWorld******/RimWorld******_Data/Managed
      ```

      and select files:   

      ```
      Assembly-CSharp.dll
      UnityEngine.CoreModule.dll
      ```
   4. Click "Open" then "OK";
   5. In the References folder, right-click Assembly-CSharp -> Properties and change "Local copy" to False. Do the same for UnityEngine,
3. In your project properties, change the target framework to .NET 4.7.2:
   1. In your IDE project file browser, right-click "(YourSolutionName)";
   2. Choose Properties;
   3. Go to the "Compiling" tab, "Output", "Target framework", "Change" and choose ".NET Framework 4.7.2",
4. In your project properties, change the build events so only a single file is built:
   1. Go to the "Compiling" tab, "Output", "Debug info" and choose "No debug information";
   2. Right-click your .cs files -> Properties and change "Copy to output" (If you haven't resized the properties bar, this will be truncated to "Copy to out") to Never,
5. In your project properties, fix the output location to put the DLL in the Assemblies folder:
   1. Go to the "Compiling" tab, "Output", "Output path" and change the output path to "..\..\Assemblies\".

### Linux[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=9 "Edit section: Linux")]

On Linux it is possible to develop using .NET packages provided by Microsoft:

1. [Install .NET by following the provided instructions.](https://dotnet.microsoft.com/download)
2. If you use IDE, use it to build (no further details, not tested).
3. It is possible to build from CLI using a .csproj file. Find a mod that has description pointing to its GitHub sources and copy from there ([example here](https://github.com/llunak/rimworld-dietfiltersinstorage/blob/master/Source/DietFiltersInStorage.csproj)), or use another way to create a .csproj file.
4. For building use a command like the following (substitute the proper .csproj file, change Release to Debug for a debug build):  

   ```
   dotnet build <project file>.csproj /property:Configuration=Release
   ```
5. If you get errors, possibly the .csproj is Windows-specific and needs to be edited:
   1. You may need to fix paths (point them to .dll files in RimWorld/RimWorld\*\_Data/Managed, but it is generally better to use NuGet as described above in Option 2, as that is platform-independent).
   2. See other setups above for other settings (those IDEs write those settings to the .csproj files).

Note that .NET Framework from Microsoft is Windows-only, and some .csproj files do not build without it. Generally it seems those using Windows-specific paths are affected, while those platform-independent work fine. Either use Mono (see below), or use one that works. The example .csproj file linked above can usually be used as a drop-in replacement, with only properties in the first PropertyGroup needing adjustment. If you really want or need it, you can get .NET Framework from Mono, by additionally doing these steps:

1. Install Mono (refer to your distribution's instructions);
2. Change the build command to the following (4.7.2 is the .NET version from the .csproj file):  

   ```
   FrameworkPathOverride=$(dirname $(which mono))/../lib/mono/4.7.2-api/ dotnet build <project file>.csproj /property:Configuration=Release
   ```

### Xamarin/MonoDevelop[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=10 "Edit section: Xamarin/MonoDevelop")]

The setup is similar as the one above. A few special points to address:

1. Mono 4.X isn't backward compatible so you may need to install an older 3.X version of Mono in order to compile against .NET4.7.2 dlls.
2. Make sure you uncheck "Use MSBuild build engine (recommended for this project type)" under project > options > build > general (You might find this by right-clicking on your project - not solution - name and selecting options)
3. Changing the framework to 4.7.2 can be found (for Linux anyway) in the same place.

More detailed installation instructions for Linux can be found [here](https://blog.rubenwardy.com/2016/07/21/rimworld-setup-monodevelop/) and [here](https://spdskatr.github.io/RWModdingResources/mono-arch). Note that as of now (2021) these may be outdated, so if it doesn't work, you can try the steps described in the Mono section.

### Rider (good for Mac)[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=11 "Edit section: Rider (good for Mac)")]

JetBrains Rider is a great cross-platform C# IDE. Previously a paid product (with exceptions), it now offers a community license, making it free for non-commercial use.

1. Open Rider and click New Solution in the welcome dialog.
   1. Click Class Library under .NET on the left. The option may a second to show up.
   2. Under Solution Name (and Project Name), enter the name of your mod.
   3. Set the Solution Directory to [your mod folder]/Source.
   4. Optionally check "put solution and project in the same directory." This is probably a good idea.
   5. Change Framework to .Net Framework 4.7.2. If you're on Windows and 4.7.2 does not show up as an option, you will have to install the "4.7.2 Dev Pack" from [Microsoft.](https://dotnet.microsoft.com/en-us/download/visual-studio-sdks)
   6. Click Create.
2. In the left side bar, expand your solution, right click your project (mod name with "C#" icon) and click Properties.
   1. In the Properties window, select Configurations > Debug on the left and uncheck Debug Symbols.
   2. For both configurations, change the Output Path to ../../Assemblies.
   3. Click OK.
3. Expand your project, right click References and click Add Reference.
   1. Click Add From.
   2. Browse to the folder with the RimWorld DLLs
      1. DLL Location for Mac: "/Users/[username]/Library/Application Support/Steam/steamapps/common/RimWorld/RimWorldMac.app/Contents/Resources/Data/Managed
      2. For Win10: "\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64\_Data\Managed"
   3. Select both Assembly-CSharp.dll and UnityEngine.dll and click OK.
   4. Expand Assemblies under References. For both of the assemblies that you just added:
      1. Right click the assembly and click Properties.
      2. Uncheck "Copy Local" (you may need to scroll down) and click OK.

You're done! Note that Rider has a built-in decompiler—to view the source of a RimWorld class or method, just right-click its name and click Go To > Definition.

### .NET Framework via Commmandline[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=12 "Edit section: .NET Framework via Commmandline")]

If you are on Windows and for some reason you don't want to use Visual Studio, you can also use the C# compiler that comes with the .NET Framework from within the terminal.

1. Install the [.NET Framework](https://dotnet.microsoft.com).
2. (Optional) Adding the compiler to PATH.
   1. Search for "env" -> Edit system environment variables -> click on "Environment Variables" -> Edit the Path of the user variables -> Add the folder of csc.exe (Usually C:\Windows\Microsoft.NET\Framework\vX.X.XXXXX).
3. Find   

   ```
   Assembly-CSharp.dll
   UnityEngine.CoreModule.dll
   ```

   in your game files(Usualy at: C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64\_Data\Managed\).
4. Open a terminal and cd to the location of your .cs file.
5. Compile your .cs file (If you skipped adding csc.exe to path, replace it with its full filepath in the following command).
   1. It should look something like this:

      ```
      csc.exe /t:library /r:"C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\Assembly-CSharp.dll","C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\UnityEngine.CoreModule.dll" MyMod.cs
      ```
   2. The command is structured where:

      ```
      csc.exe
      ```

      is the compiler(or the whole filepath), and:

      ```
      /r:"...","..."
      ```

      is the filepath of both gamefiles from step 3, and:

      ```
      MyMod.cs
      ```

      is your C# source code.
6. Explanation:
   1. the /t flag sets it to be a .dll file.
   2. the /r flag references the game files(Not the decompiled source code, the game files).

### Visual Studio Code via Dev Container[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=13 "Edit section: Visual Studio Code via Dev Container")]

|  |  |
| --- | --- |
| [AncientTerminal east.png](https://rimworldwiki.com/wiki/Category:Pages_to_be_recoded "Category:Pages to be recoded") | This section has been suggested for [recoding](https://rimworldwiki.com/wiki/Category:Pages_to_be_recoded "Category:Pages to be recoded"). **Reason:** *Section commented out due to unknown, unvalidated source. Ideally a full guide for VCS needs to be written*. You can help RimWorld Wiki by **[improving](https://rimworldwiki.com/index.php?title=Modding_Tutorials&action=edit)** it. |

# Common issues[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=14 "Edit section: Common issues")]

* Can't find the option to target .NET Framework 4.7.2? It may require additional installation steps. In Visual Studio, Tools => Get Tools and features => Individual Components => Select *.NET Framework 4.7.2 development tools* (or google installation instructions). Also make sure your project is a *Class Library (.NET **Framework**)*. Not .NET Core or .NET Standard.

# See also[[edit](/index.php?title=Modding_Tutorials/Setting_up_a_solution&action=edit&section=15 "Edit section: See also")]

* [Writing custom code](https://rimworldwiki.com/wiki/Modding_Tutorials/Writing_custom_code "Modding Tutorials/Writing custom code") continues on setting up your solution.

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:08 -->
