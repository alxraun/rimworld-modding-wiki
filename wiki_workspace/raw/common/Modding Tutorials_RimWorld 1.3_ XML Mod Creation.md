# Modding Tutorials/RimWorld 1.3: XML Mod Creation

This tutorial is an XML only version of [Plague Gun (1.1)](https://rimworldwiki.com/wiki/Plague_Gun_(1.1) "Plague Gun (1.1)") updated to RimWorld v1.3

## Contents

* [1 Introduction](#Introduction)
* [2 Required Items](#Required_Items)
* [3 Tutorial](#Tutorial)
  + [3.1 Set Up](#Set_Up)
  + [3.2 About Folder](#About_Folder)
  + [3.3 Defs Folder](#Defs_Folder)
* [4 Uploading your Mod](#Uploading_your_Mod)

## Introduction[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=1 "Edit section: Introduction")]

Mods are an integral part of the RimWorld experience, mods allow the user to customize their individual experience in far more detail than the developer ever could. However, their is little RimWorld modding information available to the average person, and the few tutorials available are (for the most part) made using older versions of software. The aim of this tutorial is to provide an absolute beginner to coding with the information and tools to begin creating their own mods. After completing this tutorial you will know how to create new items and modify existing ones, as well as have a basic understanding of RimWorld XML file structure.

## Required Items[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=2 "Edit section: Required Items")]

This tutorial assumes you have the following software installed.

* Windows 10.
* [Notepad++ v8.3.3](https://notepad-plus-plus.org/downloads/). For help with install and set up, click [here](https://www.youtube.com/watch?v=EBvum8NiGx8).
* RimWorld v1.3 installed through Steam.

*Note: you may have trouble following this tutorial if you are not using the aforementioned software.*

## Tutorial[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=3 "Edit section: Tutorial")]

Now we will begin working on the mod.

### Set Up[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=4 "Edit section: Set Up")]

1. Locate your "RimWorld" folder.
   * Find your "RimWorld" folder by right-clicking the game in your Steam library, then hover your cursor over "Manage" and click "Browse local files".
     + By default, it will be located at `C:\Program Files (x86)\Steam\steamapps\common\RimWorld`
   * Inside `RimWorld`, Locate the `Mods` folder or create one if it doesn't already exist.
2. Inside the `Mods` folder, create a new folder and name it `MyFirstMod`
3. Inside the `MyFirstMod` folder, create 2 new folders, `About` and `Defs`

### `About` Folder[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=5 "Edit section: About Folder")]

The `About` folder tells both RimWorld and Steam important details about the mod, (ex. mod name, author, etc.). It contains 2 files, `About.xml` and `Preview.jpg`/`Preview.jpeg`/`Preview.png`

* Creating `About.xml`:

1. Open Notepad++, and navigate to the "Language" tab at the top of the window. Find "XML" in the list and select it.
2. Save the file in the `About` folder as `About.xml`. Make sure you have "File name extensions" enabled in the "View" tab of File Explorer.
3. Add the following code to `About.xml`,

```
<?xml version="1.0" encoding="utf-8"?>
```

This line of code goes at the top of every XML file you create. It gives RimWorld information about how to read the file.
  
*Note: the version tag should always be "1.0". It has no relation to what version of the game the mod is made for.*
  
 Then add:

```
<ModMetaData>
  <name>Mod</name>                          <!-- The name of your mod in the mod list -->
  <author>VibeCheque</author>               <!-- Usually set to your Steam name -->
  <packageId>VibeCheque.Mod</packageId>     <!-- a unique identifier for your mod. Must contain only a-z and periods, no spaces. -->
  <supportedVersions>                       <!-- the version(s) your mod supports in X.Y format. 1(X).3(Y) -->
    <li>1.3</li>
  </supportedVersions>
  <description>What the mod does.</description>   <!-- enter information describing purpose of mod -->
</ModMetaData>
```

Use the comments on the code above to enter your own information inside the tags to complete the file.

*Note: any text inside*

*```
<!-- -->
```*

*is not read by the program. It denotes a note left in the file. Notes are used to help organize/ identify parts of code whose function may not be apparent by looking at the code.*

Our `About.xml` file is now complete.

* `Preview.jpg`/`Preview.jpeg` The thumbnail image for your mod. Maximum resolution of 640x360 pixels and maximum file size of 1mb.

### `Defs` Folder[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=6 "Edit section: Defs Folder")]

`Defs`, short for definitions are like blueprints for in-game objects. Instead of using hidden C# code, RimWorld will look up an XML Def and copy it to the game world. For the purpose of this tutorial we only need 1 def file, `Gun_Revolver`

* Creating `Gun_Revolver`:

1. Inside `Defs` folder, create a new folder called `ThingDefs_Misc`
2. Inside `ThingDefs_Misc`, create a new folder called `Weapons`
3. Open Notepad++, and navigate to the "Language" tab at the top of the window. Find "XML" in the list and select it.
4. Save the file in the `Weapons` folder as `Gun_Revolver.xml`. Make sure you have "File name extensions" enabled in the "View" tab of File Explorer. `Gun_revolver.xml` will contain the blueprints (ThingDefs) for our new gun and bullets.
5. Add the following code to `Gun_Revolver.xml`,

*Note: The file is only called `Gun_Revolver.xml` for tutorial purposes, feel free to rename.*

```
<?xml version="1.0" encoding="utf-8"?>
<Defs>

</Defs>
```

Next we will copy the vanilla revolver and revolver bullet from `RimWorld\Data\Core\Defs\ThingDefs_Misc\Weapons\RangedIndustrial.xml` and paste it into `Gun_Revolver.xml`
  
 It should look like this:

```
<Defs>

  <ThingDef ParentName="BaseBullet">
    <defName>Bullet_Revolver</defName>
    <label>revolver bullet</label>
    <graphicData>
      <texPath>Things/Projectile/Bullet_Small</texPath>
      <graphicClass>Graphic_Single</graphicClass>
    </graphicData>
    <projectile>
      <damageDef>Bullet</damageDef>
      <damageAmountBase>12</damageAmountBase>
      <stoppingPower>1</stoppingPower>
      <speed>55</speed>
    </projectile>
  </ThingDef>
  
  <ThingDef ParentName="BaseHumanMakeableGun">
    <defName>Gun_Revolver</defName>
    <label>revolver</label>
    <description>An ancient pattern double-action revolver. It's not very powerful, but has a decent range for a pistol and is quick on the draw.</description>
    <graphicData>
      <texPath>Things/Item/Equipment/WeaponRanged/Revolver</texPath>
      <graphicClass>Graphic_Single</graphicClass>
    </graphicData>
    <uiIconScale>1.4</uiIconScale>
    <soundInteract>Interact_Revolver</soundInteract>
    <thingSetMakerTags><li>RewardStandardQualitySuper</li></thingSetMakerTags>
    <statBases>
      <WorkToMake>4000</WorkToMake>
      <Mass>1.4</Mass>
      <AccuracyTouch>0.80</AccuracyTouch>
      <AccuracyShort>0.75</AccuracyShort>
      <AccuracyMedium>0.45</AccuracyMedium>
      <AccuracyLong>0.35</AccuracyLong>
      <RangedWeapon_Cooldown>1.6</RangedWeapon_Cooldown>
    </statBases>
    <weaponTags>
      <li>SimpleGun</li>
      <li>Revolver</li>
    </weaponTags>
    <weaponClasses>
      <li>RangedLight</li>
    </weaponClasses>
    <costList>
      <Steel>30</Steel>
      <ComponentIndustrial>2</ComponentIndustrial>
    </costList>
    <recipeMaker>
      <skillRequirements>
        <Crafting>3</Crafting>
      </skillRequirements>
    </recipeMaker>
    <verbs>
      <li>
        <verbClass>Verb_Shoot</verbClass>
        <hasStandardCommand>true</hasStandardCommand>
        <defaultProjectile>Bullet_Revolver</defaultProjectile>
        <warmupTime>0.3</warmupTime>
        <range>25.9</range>
        <soundCast>Shot_Revolver</soundCast>
        <soundCastTail>GunTail_Light</soundCastTail>
        <muzzleFlashScale>9</muzzleFlashScale>
      </li>
    </verbs>
    <tools>
      <li>
        <label>grip</label>
        <capacities>
          <li>Blunt</li>
        </capacities>
        <power>9</power>
        <cooldownTime>2</cooldownTime>
      </li>
      <li>
        <label>barrel</label>
        <capacities>
          <li>Blunt</li>
          <li>Poke</li>
        </capacities>
        <power>9</power>
        <cooldownTime>2</cooldownTime>
      </li>
    </tools>
  </ThingDef>
  
</Defs>
```

Once you have successfully completed the previous step, save `Gun_Revolver.xml` and start experimenting with different values.

## Uploading your Mod[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation&action=edit&section=7 "Edit section: Uploading your Mod")]

Now that you have a completed mod, you may want to upload it to the Steam Workshop for others to enjoy. Simply make sure your `MyFirstMod` folder is in `RimWorld\Mods`. Then launch RimWorld. Click on "Mods", if you did everything right, your mod should show up under mods. On the same screen there is the option to "Upload to Steam Workshop".

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/RimWorld_1.3:_XML_Mod_Creation

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:02 -->
