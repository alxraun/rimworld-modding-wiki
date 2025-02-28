# Modding Tutorials/RimWorld 1.5 Mod Updates

< [Modding Tutorials](https://rimworldwiki.com/wiki/Modding_Tutorials "Modding Tutorials")

**NOTICE**: This is a repository of information for mod developers updating their mods to RimWorld 1.5. **THERE MAY BE ANOMALY SPOILERS HERE.**

For questions and clarifications, please come visit us at the #mod-development channel on the [RimWorld Discord server](https://discord.gg/rimworld).

## Contents

* [1 General Tips](#General_Tips)
* [2 Furniture and Buildings](#Furniture_and_Buildings)
* [3 Things](#Things)
* [4 Pawns](#Pawns)
* [5 MapDrawer](#MapDrawer)
* [6 Manhunters](#Manhunters)
* [7 Misc](#Misc)
* [8 Class migrations](#Class_migrations)
* [9 About.xml](#About.xml)
* [10 DLC Dependency changes](#DLC_Dependency_changes)

## General Tips[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=1 "Edit section: General Tips")]

If you are updating a mod between RimWorld versions for the first time:

1. Update your supportedVersions in your About.xml
2. Try to recompile your assemblies. Address any issues that come up.
3. Try to load your mod. Work the error list top to bottom.
4. Check your mod functionality. This is a great time to create a functionality checklist for future updates if you haven't made one already!
5. Check back after further patches. Updates and additional content may be released in post-release patches for months to come, don't assume that mods never break once updated!

## Furniture and Buildings[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=2 "Edit section: Furniture and Buildings")]

* There is a wall lamp in vanilla now. Where were you when the RimWorld changed forever? BuildingProperties has an isAttachment flag to support wall mounted items
* Vanilla has a hidden conduit now as well.
* Building\_MultiTileDoor - used for ornate doors
* Dialog\_Rename has been changed to an abstract generic that works against IRenamable, look at Dialog\_RenameArea for example
* ShelfBase has been separated into StorageShelfBase and BookcaseBase
* New haulable interface: IHaulEnroute -> allows for multiple pawns to haul to the same destination. Vanilla now allows multiple pawns to haul materials to a building blueprint/frame for example

## Things[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=3 "Edit section: Things")]

* Thing.Draw is gone, now Thing.DynamicDrawPhase. Projectiles now using DrawAt
* MapMeshFlags is now a Def
* RimWorld.HiddenItemsManager for undiscovered items along with <hiddenWhileUndiscovered> tag. Undiscovered items won't show up in storage settings

## Pawns[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=4 "Edit section: Pawns")]

* PawnRenderer has been completely overhauled, more details pending.
  + PawnRenderTree
  + RenderNodes
  + RenderNodeWorkers
  + [Official documentation from Ludeon is linked here.](https://docs.google.com/document/d/e/2PACX-1vRpvIu49_CpHFMCk-8vLuzIsLqbFFDnf0DNFlYRpwAYXjNlYH6ZudPoox7Gj6K8wuCLPDCWkWEGARrO/pub)
* Dynamically rendered items are now handled in PawnRenderUtility.DrawEquipmentAndApparelExtras instead of PawnRenderer itself
* ApparelProperties have been dramatically changed
  + new: <renderSkipFlags>, <drawData>, <parentTagDef>
  + removed: <hatRenderedFrontOfFace>, <hatRenderedAboveBody>, <hatRenderedBehindHead>, <shellRenderedBehindHead>, <forceRenderUnderHair>, <coversHeadMiddle>, <shellCoversHead>, <countsAsMaskForBreathing>
* Human BodyDef has been altered
  + Tongue coverage changed from 0 to 0.001
  + New tags added for eyes
  + flipGraphic tag added to left eye, left ear, left shoulder, clavicle, left arm, left hand
* BodyPartDefs now have an <executionPartPriority> value to determine destruction order when a pawn is executed.
  + ExecutionUtility.ExecuteCutPart has been modified to use this value instead of a hardcoded list of parts
* "Mirrored" tag has been added to many body parts
* Traits have a possessions field under <degreeDatas> list items now, which allows colonists with the specified traits to grant extra items during new game startup
* Backstories have had <baseDesc> removed and now use <description>.
* Skills now use custom XML parsing
  + Useful search and replace regexes:
  + `\s*<key>(.*)</key>\s*<value>(.*)</value>\s*`
  → `<$1>$2</$1>`
  - Make sure to remove the li elements themselves
* Corpse Defs now have the <race> props copied over from their source race. This can cause false positives if you previously only used def.race != null to check if a ThingDef was a race; you should also check !def.IsCorpse now
* FleshTypeDef now has a <isOrganic> flag. This is now used by RaceProperties.IsFlesh instead of hardcoding against mechs.
* Backstory-Possessions are now populated as "PossessionThingDefCountClass" instead of "BackstoryThingDefCountClass" during pawn generation
* RaceProperties changes
  + deathActionWorkerClass is now deathAction, which is a DeathActionProperties object for better caching
  + <doesntMove>
  + <renderTree> - new PawnRenderTreeDef
  + <startingAnimation> new AnimationDef
  + <linkedCorpseKind> - Allows a race to use the corpse from another race
  + <canOpenFactionlessDoors>
  + <alwaysAwake>
  + <alwaysViolent>
  + <isImmuneToInfection>
  + <bleedRateFactor> - default 1.0
  + <canBecomeShambler> - defaults to false
  + <disableIgniteVerb> - disallows this race from setting buildings on fire when hostile
  + <detritusLeavings> - list of possible things to spawn when a member of this race dies
  + <overrideShouldHaveAbilityTracker> - if set to true, creates an abilitytracker for his race even if it isn't humanlike or mechanoid
  + ShouldHaveAbilityTracker property - reads the above field in addition to Humanlike and IsMechanoid
  + <hasMeat> - if set to false, no meat def is generated for this race. (no more tank meat? heresy~)
  + <hasCorpse> - if set to false, then no corpse is generated for this race
  + <corpseHiddenWhileUndiscovered> - if set to true, then the corpse does not come up in lists until you see one. possibly used by many anomalies?
  + <soundMoving> - new field
  + Animal property now checks !IsAnomalyEntity (in addition to checking !ToolUser && IsFlesh)
  + <bloodDef> will no longer default to Filth\_Blood if left blank. Pawns with no bloodDef will not show bleeding on the health tab, cannot bleed to death, and generate no filth when injured.
  + <bloodSmearDef> - new field, defines the filth Def used for blood smears when the pawn is crawling while bleeding
* Pathfinding changes
  + Pawn.TicksPerMoveCardinal, Pawn.TicksPerMoveDiagonal, and Pawn\_PathFollower.CostToMoveIntoCell have been refactored from int to float.
* SerializablePawnList is no more, replaced with List<Pawn>.

## MapDrawer[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=5 "Edit section: MapDrawer")]

* MapMeshFlag enum has been removed in favor of MapMeshFlagDef, which provides an `index` value which represents the replaced enum value. All enum values have been moved to the MapMeshFlagDefOf for ease of use.

## Manhunters[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=6 "Edit section: Manhunters")]

* Manhunter classes have been renamed to "AggressiveAnimals":
  + ManhunterPackIncidentUtility -> AggressiveAnimalIncidentUtility
  + IncidentWorker\_ManhunterPack -> IncidentWorker\_AggressiveAnimals

## Misc[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=7 "Edit section: Misc")]

* StatDef new fields:
  + <offsetLabel>
  + <showOnEntities> (default true)
  + <showOnNonPowerPlants> (default true)
  + <overridesHideStats> (default false)
  + <displayMaxWhenAboveOrEqual> (default false)
  + OffsetLabel (property)
  + OffsetLabelCap (property) -> currently only used in CompFacility.CompInspectStringExtra
* Apparel stat changes
  + StatPart\_ApparelStatOffset renamed to StatPart\_GearStatOffset
  + Added StatPart\_GearStatFactor
* ResearchTabDef new fields:
  + <generalTitle> (default "") - Sets the yellow text at the top of the tab's tooltip.
  + <generalDescription> (default "") - Sets the white text in the body of the tab's tooltip.
  + <visibleByDefault> (default true) - Appears to hide info about the tab until minMonolithLevelVisible is met.
  + <minMonolithLevelVisible> (default -1) - Something to do with Anomaly.
  + <tutorTag> (default null) - Used to highlight research tabs during learning helper lessons.
* CompUseEffect
  + Virtual method CanBeUsedBy has changed its signature. Old: `bool CanBeUsedBy(Pawn, out string)` New: `AcceptanceReport CanBeUsedBy(Pawn)`

## Class migrations[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=8 "Edit section: Class migrations")]

* Verse -> RimWorld
  + IngredientValueGetter
* Verse -> LudeonTK
  + DebugActionAttribute AKA [DebugAction]
  + DebugActionNode
  + DebugActionType
  + A lot of other debug-related classes.

## About.xml[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=9 "Edit section: About.xml")]

* ~~<modIconPath IgnoreIfNoMatchingField="True">UI/YourModIconpath</modIconPath> for icons on the save loading screen. Rendered as 32x32. Path points to the Textures folder.~~
* In the latest build you can now just put a ModIcon.png in your About folder without having to configure any XML. Still renders at 32x32, the vanilla puzzle icon is 64x64 but be aware that Unity image compression can make your icons very crunchy at this size. Avoiding too much detail and using either flat colors or very subtle gradients is recommended.

## DLC Dependency changes[[edit](/index.php?title=Modding_Tutorials/RimWorld_1.5_Mod_Updates&action=edit&section=10 "Edit section: DLC Dependency changes")]

* Skulls
  + The skull ThingDef has been migrated from Ideology to Core.
  + The "extract skull" DesignationDef, WorkGiverDef, and JobDef have been migrated to Core. It still requires the correct Ideology precept or a new Anomaly research to be unlocked.
* StatDef migrations
  + The following stats have been migrated from the data files for Biotech, to the files for Core. They no longer require any DLC to be usable.
    - Stats\_Pawns\_Combat
      * MeleeDamageFactor
      * RangedCooldownFactor
      * StaggerDurationFactor
    - Stats\_Pawns\_General
      * **These changes were apparently regressed at some point. These stats now require either Biotech or Anomaly.**
      * ~~MechStatBase (Abstract)~~
      * ~~EMPResistance~~
* Apparel
  + Robes have been migrated from Ideology to Core. They can now be crafted without any DLCs active.

---
Источник: https://rimworldwiki.com/wiki/Modding_Tutorials/RimWorld_1.5_Mod_Updates

<!-- Метаданные -->
<!-- Категория: basics -->
<!-- Дата загрузки: 2025-02-27 21:50:05 -->
