# Modding Tutorials/Hello World

< [Modding Tutorials](https://rimworldwiki.com/wiki/Modding_Tutorials "Modding Tutorials")

In this tutorial you will learn how to make a mod that runs on startup and writes "Hello World" to the dev console.

## Contents

* [1 Requirements](#Requirements)
* [2 Bootstrapping](#Bootstrapping)
* [3 Hello World](#Hello_World)
* [4 Common pitfalls](#Common_pitfalls)
* [5 StaticConstructorOnStartup vs inheriting from Mod](#StaticConstructorOnStartup_vs_inheriting_from_Mod)
  + [5.1 But why can't I make my StaticConstructorOnStartup inherit from mod?](#But_why_can.27t_I_make_my_StaticConstructorOnStartup_inherit_from_mod.3F)

# Requirements[[edit](/index.php?title=Modding_Tutorials/Hello_World&action=edit&section=1 "Edit section: Requirements")]

* This tutorial requires you to have [set up a solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution").
* This tutorial requires a Mod folder (here called: MyMod) and the mod folder will need an About folder containing a valid About.xml and an Assemblies folder where we will put our HelloWorld.dll file. For more info, see  [Mod folder structure](https://rimworldwiki.com/wiki/Modding_Tutorials/Mod_folder_structure "Modding Tutorials/Mod folder structure")

# Bootstrapping[[edit](/index.php?title=Modding_Tutorials/Hello_World&action=edit&section=2 "Edit section: Bootstrapping")]

There are two acceptable ways of making RimWorld load your mod. One is by using the StaticConstructorOnStartup annotation, the other is by inheriting from the Mod class. We will be using the StaticConstructorOnStartup annotation, for reasons explained below.

# Hello World[[edit](/index.php?title=Modding_Tutorials/Hello_World&action=edit&section=3 "Edit section: Hello World")]

```
using RimWorld;
using Verse;

namespace TestMod
{
    [StaticConstructorOnStartup]
    public static class MyMod
    {
        static MyMod() //our constructor
        {
            Log.Message("Hello World!"); //Outputs "Hello World!" to the dev console.
        }
    }
}
```

* The [StaticConstructorOnStartup] annotation does what it says on the tin. At startup, it runs the Static Constructor of any class with that annotation. For a thorough explanation of what a static constructor is, refer to [official C# documentation.](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-constructors) What you need to know: It's a method without a return type, has the same name as the class and doesn't take any arguments.

# Common pitfalls[[edit](/index.php?title=Modding_Tutorials/Hello_World&action=edit&section=4 "Edit section: Common pitfalls")]

* Your constructor isn't static.
* Your constructor isn't a constructor. Refer once more to the [official C# documentation.](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-constructors)
* There are red squiggles under [StaticConstructorOnStartup] (or anywhere else): you either typo'd, are missing *using* statements or don't have a reference to *Assembly-Csharp.dll*. Read how to [set up a solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution") again.
* You did not compile, and if you did compile, your IDE placed the resulting assembly in some far away folder. Set the output path of your build to something more sane. Read how to [set up a solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution") again.
* There are lot of yellow errors along the line of "x already has short hash": You did not set "Copy local" to false. Read how to [set up a solution](https://rimworldwiki.com/wiki/Modding_Tutorials/Setting_up_a_solution "Modding Tutorials/Setting up a solution") again.

# StaticConstructorOnStartup vs inheriting from Mod[[edit](/index.php?title=Modding_Tutorials/Hello_World&action=edit&section=5 "Edit section: StaticConstructorOnStartup vs inheriting from Mod")]

Subclasses of the Mod class are loaded extremely early during launch. As such, it is less suitable for most purposes (most notably: using Defs or anything that uses Defs). Classes with the StaticConstructorOnStartup are initialised just before the Main Menu is shown. Classes with this annotation are loaded in the main thread: a requirement for loading any Texture, Material, Shader, Graphic, GameObject or MaterialPropertyBlock.

Inheriting from Mod is necessary if you want [Mod Settings](https://rimworldwiki.com/wiki/Modding_Tutorials/ModSettings "Modding Tutorials/ModSettings"), or if you need to run really early in the loading process.

For the exact order of initialisation, look up Verse.PlayDataLoader.DoPlayLoad.

## But why can't I make my StaticConstructorOnStartup inherit from mod?[[edit](/index.php?title=Modding_Tutorials/Hello_World&action=edit&section=6 "Edit section: But why can't I make my StaticConstructorOnStartup inherit from mod?")]

When C# tries to instantiate a type (like your Mod class) for the very first time, it first runs the static constructor and then an instance constructor. The static constructor in this case is the StaticConstructorOnStartup: meaning it'll try to load your textures outside the main thread and run your Harmony patches using Defs the game doesn't know exist yet. Subsequent new instances of your Mod class will not have their static constructor called again: they run once and only once. Likewise, if the StaticConstructorOnStartup then tries to call the static constructor again, nothing will happen as the static constructor already ran once.

Sometimes you need both a StaticConstructorOnStartup and a class that inherits from Mod. The correct way of doing that is by making two separate classes; one that inherits from Mod and the other with a StaticConstructorOnStartup attribute.

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/Hello_World

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:25 -->
