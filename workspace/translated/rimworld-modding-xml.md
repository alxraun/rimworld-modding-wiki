---
description: "xml" module Rimworld modding documentation
globs: 
alwaysApply: true
---
`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\config-errors.md`:

```````md
## ConfigErrors - Developer Cheat Sheet

**Purpose:**
- Report XML configuration errors during RimWorld startup.
- Inform modder/user of XML issues (missing fields, invalid values).

**Usage:**
- Implement `ConfigErrors()` method in custom Def classes and related classes.
- Displayed as red errors on game launch.

**Implementation (Def Subclass):**

```csharp
public class MyOwnDef : Def
{
    float someValue = 4f;
    const float minSomeValue = 0f;
    const float maxSomeValue = 10f;

    public override IEnumerable<string> ConfigErrors()
    {
        foreach(string error in base.ConfigErrors()) // Include base Def errors
            yield return error;

        if(someValue < minSomeValue || someValue > maxSomeValue) // Value range check
            yield return $"{nameof(someValue)} is {someValue} , out of range: {minSomeValue}-{maxSomeValue}!";
    }
}
```
- Override `ConfigErrors()` in custom `Def` subclass.
- Call `base.ConfigErrors()` to include base validations.
- Use `yield return "error message"` for each error condition.

**Implementation (Custom Classes):**

```csharp
public class MyOwnDef : Def
{
    List<MyOwnSpecialType> specialStuffList;

    public override IEnumerable<string> ConfigErrors()
    {
        foreach(string error in base.ConfigErrors())
            yield return error;

        if(specialStuffList == null) // Null list check
            yield return $"Required list {nameof(specialStuffList)} is empty";
        else
            foreach(MyOwnSpecialType specialStuff in specialStuffList) // Nested class errors
                foreach(string error in specialStuff.ConfigErrors())
                    yield return error;
    }
}

public class MyOwnSpecialType
{
    float someValue;
    const float minSomeValue = 0;
    const float maxSomeValue = 10;

    public IEnumerable<string> ConfigErrors()
    {
        if(someValue < minSomeValue || someValue > maxSomeValue) // Value range check in nested class
            yield return $"{nameof(someValue)} is {someValue} , out of range: {minSomeValue}-{maxSomeValue}!";
    }
}
```
- Implement `ConfigErrors()` method in custom classes even if not subclassing `Def`.
- Call nested class `ConfigErrors()` within parent `ConfigErrors()`.

**Fallback (No ConfigErrors Method):**

```csharp
// Example for manual validation
public static void ValidateMyTypes()
{
    foreach(var myTypeInstance in MyTypeInstances) // Iterate instances of custom type
    {
        if(myTypeInstance.someValue < 0)
            Log.Error($"MyType: {myTypeInstance.name} has invalid someValue: {myTypeInstance.someValue}"); // Log error message
    }
}
```
- Iterate instances of custom types.
- Use `Log.Error("error message")` to report issues.
- Call validation method after startup.

**Key Code Elements:**

- `override`:  Modify base `ConfigErrors()` behavior.
- `yield return`: Return single error message in `IEnumerable<string>`.
- String Interpolation (`$"{}"`): Embed variables in error messages.
- `nameof(variable)`: Get variable name as string for error messages.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\def-compatibility.md`:

```````md
## Def Compatibility - Cheat Sheet

**Overwriting Defs:**

- **Core Defs:**
    - **DON'T OVERWRITE.** Use XPath patches instead.
- **Mod Defs:**
    - **DON'T OVERWRITE.** Use XPath patches instead.
    - Use `PatchOperationFindMod` or `PatchOperationConditional` for targeted changes.

**Referencing Defs:**

- **RecipeDef:**
    - Link recipes to buildings via `<recipeUsers>` in `RecipeDef`.
    - Link buildings to recipes via `<recipes>` in `ThingDef`.
    - Link recipes to buildings via `<recipeMaker><recipeUsers>` in `ThingDef`.
    - XPath patching: `<PatchOperationAdd>` to `Defs/ThingDef[defName="BuildingDef"]/recipes`.

- **Facilities:**
    - Buildings to Facilities (Facility Links):
        - Building `ThingDef` with `CompAffectedByFacilities`:
            - `<linkableFacilities>` for facilities attaching to building.
    - Facilities to Buildings (Building Links to Facilities):
        - Facility `ThingDef` with `CompFacility`:
            - `<linkableBuildings>` for buildings facility attaches to.

- **Animals:**
    - Add animals to biomes in animal `ThingDef`:
        - `<wildBiomes>` tag with biome defNames and weights.

**Conditional Loading (DLC/Mods):**

- `MayRequire="packageId1,packageId2"`: Node loads only if *all* listed DLCs/mods are active.
- `MayRequireAnyOf="packageId1,packageId2"`: Node loads if *any* listed DLCs/mods are active.

**DLC Package IDs:**
- Royalty: `Ludeon.RimWorld.Royalty`
- Ideology: `Ludeon.RimWorld.Ideology`
- Biotech: `Ludeon.RimWorld.Biotech`
- Anomaly: `Ludeon.RimWorld.Anomaly`
- Mod Package IDs: Found in `About.xml` of the mod.

**Core Game Content:**
- Some content that was previously DLC-exclusive is now available in the base game:
  - **Skulls**: Skull objects are in Core, although extraction still requires Ideology precepts or Anomaly research.
  - **Robes**: Apparel is available and craftable without DLCs.
  - **Combat Stats**: Stats like `MeleeDamageFactor`, `RangedCooldownFactor`, and `StaggerDurationFactor` are available without DLCs.
  - **Mech Stats**: Some stats like `MechStatBase` (Abstract) and `EMPResistance` can work with either Biotech or Anomaly DLC.

**Usage Contexts:**
- **List Entries (`<li>`):**
    - Conditional loading of list items based on dependencies.
    - Example: `<li MayRequire="Ludeon.RimWorld.Biotech">...</li>`

- **Def References:**
    - Suppress cross-reference errors if dependency not loaded.
    - Does NOT prevent loading if Def is found, even if dependency is missing.
    - Example: `<thinkTreeMainOverride MayRequire="Ludeon.RimWorld.Biotech">...</thinkTreeMainOverride>`

- **Optional Defs:**
    - Conditionally load entire Defs.
    - Example: `<ThingDef MayRequire="Ludeon.RimWorld.Ideology" ParentName="Brazier">`

**Exceptions & Limitations:**
- Top-Level Non-Def Nodes: `MayRequire` does NOT work on top-level XML nodes that are not Defs (e.g., `<Operation>`).
- `PatchOperationSequence`: `MayRequire` can be used on `<li>` within `<operations>` of `PatchOperationSequence`.
- Mod Name vs PackageId: `PatchOperationFindMod` uses Mod Names, `MayRequire` uses `packageId`
- Steam Mod Suffix Bug: `MayRequire` may not recognize Steam mods if local version exists. Use `MayRequireAnyOf="MyName.MyMod,MyName.MyMod_steam"` as workaround.

**Example - MayRequire:**
```xml
<Tribal_Child MayRequire="Ludeon.RimWorld.Biotech">10</Tribal_Child>
```

**Example - MayRequireAnyOf:**
```xml
<li MayRequireAnyOf="Ludeon.RimWorld.Royalty,Ludeon.RimWorld.Biotech">
```

**Defensive Patching Techniques:**

Using `PatchOperationFindMod` for mod compatibility:
```xml
<Operation Class="PatchOperationFindMod">
  <mods>
    <li>Royalty</li>
  </mods>
  <match>
    <!-- Operations if mod is present -->
  </match>
  <nomatch>
    <!-- Operations if mod is not present -->
  </nomatch>
</Operation>
```

**Key Recommendations:**

- **Prioritize XPath Patches:** For modifying existing Defs.
- **Avoid Overwriting:** Prevents mod conflicts.
- **Use `recipeUsers`/`recipes`/`recipeMaker`:** For linking recipes and buildings.
- **Use `wildBiomes`:** For adding animals to biomes.
- **Use `MayRequire`/`MayRequireAnyOf`:** For conditional content loading based on DLCs/mods.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\def-extensions.md`:

```````md
## Def Extensions - Cheat Sheet

**Methods for Modifying Defs:**

#### 1. Overwriting Defs (XML)
- **Pros**: Easiest method
- **Cons**: Incompatible with other mods
- **When to Use**: Never, except for personal use

#### 2. XPath Changes (XML Patches)
- **Pros**: Highly specific, very compatible, non-destructive
- **Cons**: Limited to XML-defined Defs, complex operations can be tricky
- **When to Use**: For most XML value changes, general compatibility

#### 3. Adding a (Self-Made) Comp (C#)
- **Pros**: Flexible, well-supported, highly compatible, adds functionality
- **Cons**: Not applicable to all Def types
- **When to Use**: Adding new behaviors, non-static data, or functionality to ThingWithComps

#### 4. DefModExtensions (C#)
- **Pros**: Simple, lightweight, highly compatible, adds data fields
- **Cons**: For static data only
- **When to Use**: Adding custom data fields to existing Defs

#### 5. Subclassing Defs (C#)
- **Pros**: Powerful, extends Def functionality
- **Cons**: Compatibility issues, less flexible than Comps, casting required
- **When to Use**: Complex Def modifications where Comps/DefModExtensions are insufficient

#### 6. Custom Defs (C#)
- **Pros**: Full control, no compatibility issues within your mod
- **Cons**: Implementation from scratch, more work
- **When to Use**: Creating entirely new Def types unique to your mod

#### 7. Checking for Tags (XML/C#)
- **Pros**: Lightweight, easy for simple features and compatibility
- **Cons**: Hacky, risk of side effects, less robust
- **When to Use**: Simple, lightweight feature checks or cross-mod compatibility flags

#### 8. Changing Def Class (XML Patches)
- **Pros**: Easy, retains most original Def values
- **Cons**: Compatibility issues with Harmony patches targeting original class
- **When to Use**: Specific class behavior replacement, consider compatibility impacts

#### 9. Harmony Patching (C#)
- **Pros**: Highly flexible for code-level changes
- **Cons**: Overuse can be complex, alternatives often better suited
- **When to Use**: Modifying game code execution, consider alternatives first

## DefModExtension in Detail

**Concept:**

- Add fields to existing Defs without modifying base classes.
- Implemented as `public List<DefModExtension>` in `Def` class.
- Extends Def functionality via XML and C#.

**Benefits:**

- Simple and lightweight.
- Works with any Def.
- Functionality exposed to XML.
- Savegame compatible.
- Mod compatible.
- Avoids issues of custom Def classes.
- Lighter overhead than ThingComps.
- Broader application than ThingComps (works on all Defs).

**Downsides:**

- Static, global data.
- Cannot save data directly within DefModExtension.
- Limited to Defs.
- DefModExtension is unaware of its associated Def (can be worked around).

**C# Code Example:**

```csharp
using Verse;

namespace YourNamespace
{
    public class YourModExtension : DefModExtension
    {
        public bool yourBoolField = true; // Example field
        // Add other fields as needed (C# Types, ThingFilters, Lists, etc.)
    }
}
```

**Using DefModExtension in C#:**

```csharp
// Accessing field from a Def instance:
bool value = def.GetModExtension<YourModExtension>().yourBoolField;
```

**Adding DefModExtension to Def in XML:**

```xml
<Defs>
  <YourDefType>
    <defName>YourDefName</defName>
    <modExtensions>
      <li Class="YourNamespace.YourModExtension">
        <yourBoolField>false</yourBoolField>
        <!-- Set other fields here, matching C# field names -->
      </li>
    </modExtensions>
  </YourDefType>
</Defs>
```

**Patching Def to Add DefModExtension (XPath):**

```xml
<Patch>
  <Operation Class="PatchOperationAddModExtension">
    <xpath>Defs/YourDefType[defName="YourDefName"]</xpath>
    <value>
      <li Class="YourNamespace.YourModExtension">
        <yourBoolField>false</yourBoolField>
        <!-- Set other fields here, matching C# field names -->
      </li>
    </value>
  </Operation>
</Patch>
```

## ThingComp vs DefModExtension

**When to Use ThingComp:**
- Need instance-specific data (varies per Thing instance)
- Need to hook into tick/update events
- Need to add functionality to Things (items, pawns, buildings)
- Need to add UI elements (gizmos, inspectors)
- Need to save/load data with individual Things

**When to Use DefModExtension:**
- Need global settings for all Things of a Def type
- Need to add data to non-Thing Defs (RecipeDef, ResearchDef, etc.)
- Need lightweight data storage without behavior
- Need to avoid overhead of Comp system

**Key Considerations:**

- **Compatibility:** Prioritize XPath, Comps, and DefModExtensions for best mod compatibility.
- **Functionality:** Comps for behavior, DefModExtensions for data.
- **Complexity:** Subclassing and Custom Defs for advanced, unique modifications.
- **Performance:** DefOf for optimized Def access in C#.
- **Error Handling:** XML errors cascade; fix the first error first.
- **Key Syntax:**
  - C# Class: Inherit from `DefModExtension`.
  - XML: Use `<modExtensions><li>` with `Class` attribute to specify your extension class.
  - Access in Code: `def.GetModExtension<YourModExtension>().fieldName`.
  - Patching: `PatchOperationAddModExtension` for adding via patches.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\def-fundamentals.md`:

```````md
## Def Fundamentals - Cheat Sheet

**What are Defs?**

- XML Definitions used to add content to RimWorld mods.
- Define game elements: weapons, apparel, animals, plants, factions, etc.
- Stored in XML files, human-readable, located in the `Defs` folder of a mod.
- Act as blueprints: define properties and configurations without code compilation.
- C# code provides behavior, Defs configure data.
- Turn generic entities into specific game elements (e.g., generic plant → potato plant).

**XML File Structure:**

- **XML Declaration:** `<?xml version="1.0" encoding="utf-8"?>` - Start of every XML file.
- **Root Node:** `<Defs>` - Encloses all Defs in the file.
- **Def Tags:** `<DefType>` - Container for a specific Def (e.g., `<ThingDef>`, `<RecipeDef>`).
- **Example Structure:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<Defs>
    <ThingDef>
        <defName>SomeName</defName>
        <ingestible>
            <ingestSound>Slurp</ingestSound>
        </ingestible>
    </ThingDef>
</Defs>
```

**Folder Location:**

- Placed in `Defs` folder of mod's structure.
- XML files within `Defs` can be organized into subfolders (e.g., `ThingDefs_Items`, `RecipeDefs`).
- Filenames within `Defs` are for organization, except when using versioned LoadFolders.xml.
- Vanilla Defs location: `[RimWorld Install Folder]\Data\Core\Defs`.
- Use vanilla Defs as templates and examples.

**C# & XML Connection:**

- **Structure:** C# class (`Def` inheritance) defines XML structure.
- **Tags:** Public C# fields (e.g., `public SomeType someTagName;`) become XML tags (`<someTagName>`).
- **Subtags:** Nested classes (e.g., `StuffProperties`) create subtags within XML.
- **Lists:** `List<T>` in C# creates XML lists with `<li>` items.
    - `StatModifier` in lists: Special case, uses tag name as `StatDef`, tag value as `float`.
- **C# Correspondence:** Each Def Type matches a C# class inheriting from `Def`.
- **XML Tags:** Represent fields of the corresponding C# Def class.
    - Simple Values: `<defName>value</defName>`, `<label>value</label>` (strings, integers).
    - Complex Values: Nested tags for classes like `IngestibleProperties` (`<ingestible>`).
    - Hierarchy: `<Defs> > <Def Type> > <fieldTag> > <childNode>`.

**XML Inheritance:**

- **Abstract="True"**: Used in `<Def Name="DefName" Abstract="True">`.
    - Prevents Def from being loaded into the game.
    - Serves as a template for inheritance.
- **Name="DefName"**: Defines a template/parent Def.
    - Used by other Defs for inheritance via `ParentName`.
- **ParentName="ParentDefName"**: Used in `<Def ParentName="ParentDefName">`.
    - Inherits content from `<Def Name="ParentDefName">`.
- **Inheritance Mechanism:**
    1. **Parent Def:** `<Def Name="ParentDefName" Abstract="True">` - Defines base properties.
    2. **Child Def:** `<Def ParentName="ParentDefName">` - Inherits properties from ParentDefName.
    3. **Game Load:**
       - Parent Defs (`Name` & `Abstract="True"`) are templates, not loaded directly.
       - Child Defs (`ParentName`) copy & apply parent's content.
       - Child Def content overwrites parent content if conflicts.
- **Stopping Inheritance - `Inherit="False"`:**
    - Use `Inherit="False"` in a child tag to prevent inheriting a specific tag's content from the parent.
    - Useful for overriding specific tags, especially list tags (`<li>`).
    - Example:

```xml
<ThingDef Name="ThingOne" Abstract="True">
  <comps>
    <li Class="CompOne">
      <valueA>1</valueA>
    </li>
  </comps>
</ThingDef>

<ThingDef ParentName="ThingOne">
  <comps Inherit="False">
    <li Class="CompOne">
      <valueA>2</valueA>
    </li>
  </comps>
</ThingDef>
```

**Custom Def Classes:**

- **Purpose:** Create new Def types with custom fields and behavior.
- **XML Requirement:**
    - `<ThingDef Class="MyNamespace.MyCustomDefName">` in XML to link to custom C# class.
- **Code Requirement:**
    - `class MyCustomDefName : ThingDef` (or other Def type) in C#.
- **Accessing Custom Fields (C#):**
    - Casting: `var def = thing.def as MyCustomDefName;`
- **Example (Custom Weapon Def):**
  - **XML (`ThingDefs.xml`):**
  ```xml
  <ThingDef ParentName="BaseHumanGun" Class="MyNamespace.MyCustomDef_ThingDef">
    <defName>MyCoolNewGun</defName>
    <thingClass>MyNamespace.MyCoolNewGun</thingClass>
    <myNewFloat>1.0</myNewFloat>
    <myNewBool>true</myNewBool>
    <myNewThingDefList>
      <li>Steel</li>
    </myNewThingDefList>
  </ThingDef>
  ```
  - **C# (`MyCustomDef_ThingDef.cs`):**
  ```csharp
  namespace MyNamespace
  {
      public class MyCustomDef_ThingDef : ThingDef
      {
          public float myNewFloat;
          public bool myNewBool;
          public List<ThingDef> myNewThingDefList = new List<ThingDef>();
      }
  }
  ```
  - **Accessing Custom Def in Code:**
  ```csharp
  MyCustomDef_ThingDef def = weapon.def as MyNamespace.MyCustomDef_ThingDef;
  float floatValue = def.myNewFloat;
  ```

**Working with ThingDefs:**

- **Purpose:** Define properties of almost all in-game "things" (tangible & intangible).
- **Learning Tag Meanings:**
    - **Decompiler (Recommended):** Use a decompiler (ILSpy, dnSpy) to inspect `ThingDef` C# class.
    - **Search Tag Names:** Search for XML tag names in decompiled code.
    - **Analyze Code Usage:** Understand tag effects from how they're used in code.
- **Finding Tag Meaning Example (`<intricate>`):**
    1. **Identify Tag:** `<intricate>`.
    2. **Decompile & Search:** Search decompiler for "intricate".
    3. **Analyze Code Usage:**
       - `PlayerItemAccessibilityUtility.CacheAccessibleThings`
       - `Thing.SmeltProducts` → determines smelting products.
    4. **Deduce Meaning:** `<intricate>true</intricate>` for `ThingDef` = item is lost upon smelting (e.g., Components).
- **General Approach for Unknown Tags:**
    1. **Identify XML Tag** (e.g., `<DeteriorationRate>`).
    2. **Text Editor (Find in Files):** Search for tag name in RimWorld `Data` and mod folders.
    3. **Decompiler (Analyze):** Search and analyze tag name in decompiled C# code.

**Common XML Errors:**

- **Syntax Errors:** Formatting, unclosed tags, case sensitivity.
- **Cross-reference Errors:** Incorrect or missing `defName` references. "Could not resolve cross-reference: No DefType named DefName found".
- **Cascading Errors:** One XML error can cause a chain of subsequent errors.
- **Solving XML Errors:**
    - Fix Errors Incrementally: Start with the first error reported in logs.
    - Validate XML: Use XML editor features or online validators to check for syntax.
    - Check Player.log: Consult the log file for detailed error messages and locations.

**Key Considerations:**

- `Class` attribute links XML to C# class.
- `defName` must be unique.
- XML is case-sensitive.
- Root element is always `<Defs>`.
- `Name` for parent Defs, `ParentName` for child Defs.
- `Abstract="True"` for template Defs.
- `Inherit="False"` for selective inheritance control.
- Use namespaces to organize code and XML.
- Be mindful of compatibility when modifying core Defs.
- When NOT to use Custom Def Classes: Compatibility issues, limited scope, consider alternatives like `ThingComp` and `DefModExtension`. 

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\grammar-resolver-guide.md`:

```````md
## GrammarResolver - Cheat Sheet

**Purpose:**
- Customize text strings dynamically based on game objects (pawns, things, factions).
- Used for descriptions, tooltips, messages.
- Employs symbols within strings for dynamic content.

**Symbols:**
- Format: `{KEY_subsymbol}`.
    - `KEY`: Object identifier (e.g., `PAWN`, `FACTION`, `THING`).
    - `subsymbol`: (Optional) Specifies object property and formatting.

**Thing Subsymbols:**
- `{THING}`: Indefinite label (e.g., "a chair").
- `{THING_label}`: `def.label` (e.g., "chair").
- `{THING_labelShort}`: Short label.
- `{THING_definite}`: "the" + label (e.g., "the chair").
- `{THING_indefinite}`: "a" or "an" + label (e.g., "a chair").
- `{THING_labelPlural}`: Plural label (e.g., "chairs").
- `{THING_labelPluralDef}`: "the" + plural label (e.g., "the chairs").
- `{THING_labelPluralIndef}`: Indefinite plural label (e.g., "chairs").
- `{THING_pronoun}`: Pronoun (e.g., "it").
- `{THING_possessive}`: Possessive pronoun (e.g., "its").
- `{THING_objective}`: Objective pronoun (e.g., "it").
- `{THING_factionName}`: Faction name (for Things with factions).
- `{THING_gender}`: Gender symbol.

**Pawn Subsymbols (Extends Thing Subsymbols):**
- `{PAWN_labelShort}`: Pawn's short name (e.g., "Alice").
- `{PAWN_nameFull}`: Full name, defaults to `kindIndef` (e.g., "Alison 'Alice' Abbey").
- `{PAWN_nameFullDef}`: Definite full name, defaults to `kindDef`.
- `{PAWN_nameDef}`: Definite name (e.g., "Alice").
- `{PAWN_nameIndef}`: Indefinite name (e.g., "Alice").
- `{PAWN_kind}`: Pawn kind based on pawnkind, gender, lifestage (e.g., "colonist").
- `{PAWN_kindDef}`: Definite kind (e.g., "the colonist").
- `{PAWN_kindIndef}`: Indefinite kind (e.g., "a colonist").
- `{PAWN_kindPlural}`: Plural kind (e.g., "colonists").
- `{PAWN_kindPluralDef}`: Definite plural kind (e.g., "the colonists").
- `{PAWN_kindPluralIndef}`: Indefinite plural kind (e.g., "colonists").
- `{PAWN_kindBase}`: PawnKindDef label (e.g., "colonist").
- `{PAWN_lifestage}`: Lifestage noun (e.g., "adult").
- `{PAWN_lifestageDef}`: Definite lifestage noun (e.g., "the adult").
- `{PAWN_lifestageIndef}`: Indefinite lifestage noun (e.g., "an adult").
- `{PAWN_lifestageAdjective}`: Lifestage adjective (e.g., "adult").
- `{PAWN_title}`: Backstory title (e.g., "chef").
- `{PAWN_titleDef}`: Definite title (e.g., "the chef").
- `{PAWN_titleIndef}`: Indefinite title (e.g., "a chef").
- `{PAWN_humanlike}`: "Humanlike" or blank.

**Faction Subsymbols:**
- `{FACTION}`: Faction name (e.g., "Alicetopia").
- `{FACTION_name}`: Faction name (e.g., "Alicetopia").
- `{FACTION_pawnSingular}`: Faction pawn singular label (e.g., "colonist").
- `{FACTION_pawnSingularDef}`: Definite singular pawn label (e.g., "the colonist").
- `{FACTION_pawnSingularIndef}`: Indefinite singular pawn label (e.g., "a colonist").
- `{FACTION_pawnsPlural}`: Faction pawn plural label (e.g., "colonists").
- `{FACTION_pawnsPluralDef}`: Definite plural pawn label (e.g., "the colonists").
- `{FACTION_pawnsPluralIndef}`: Indefinite plural pawn label (e.g., "colonists").

**Misc Subsymbols (Strings & Integers):**

- **Strings:**
    - No subsymbol: Printed as-is.
    - `plural`, `pluralDef`, `pluralIndef`, `definite`, `indefinite`, `pronoun`, `possessive`, `objective`, `gender`: Resolver's best guess for noun subsymbols.
- **Integers:**
    - No subsymbol: Printed as integer.
    - `ordinal`: Integer as ordinal (e.g., "1st").
- **Other Objects:**
    - No subsymbol: `ToString()` method invoked.

**Invoking Resolver (C#):**

- `.Formatted(object.Named("KEY"))`
    - `string`: Text with symbols.
    - `object`: Object to resolve symbols against.
    - `KEY`: Symbol key (e.g., "PAWN", "FACTION").

**AdjustedFor Resolver (C# - Battle Log):**

- `.AdjustedFor(pawn, "KEY")`
    - `string`: Text with symbols.
    - `pawn`: Pawn to resolve symbols against.
    - `KEY`: Symbol key.
- Supported keywords: `nameFull`, `nameShort`, `nameShortDef`, `definite`, `nameShortIndef`, `indefinite`, `pronoun`, `possessive`, `objective`, `factionName`, `kind`, `title`.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\thingdef-guide.md`:

```````md
## ThingDef - Cheat Sheet

**Purpose of ThingDef:**

- Defines properties and behavior of in-game "things" (items, buildings, pawns, projectiles, etc.).
- Backed by `ThingDef` C# class.
- Configured via XML in Def files.

**Learning Tag Meanings:**

- **Challenge:** 200+ tags, limited official documentation.
- **Method:** Decompilation + Code Analysis.
    1. **Identify Tag:** Find tag in XML (e.g., `<intricate>`).
    2. **Decompile:** Use a decompiler (ILSpy, dnSpy).
    3. **Search:** Search decompiler for tag name (e.g., "intricate").
    4. **Analyze Usage:** Analyze code using tag (e.g., `Thing.SmeltProducts`).
    5. **Infer Meaning:** Deduce tag's function from code context (e.g., `intricate` affects smelting products).

**Example: `intricate` Tag**

- **XML Usage:**
  ```xml
  <ThingDef>
      <defName>ComponentIndustrial</defName>
      <intricate>true</intricate>
  </ThingDef>
  ```
- **Analysis:**
    - Used in `ComponentIndustrial` and `AdvancedComponent` ThingDefs.
    - Read by `PlayerItemAccessibilityUtility.CacheAccessibleThings` and `Thing.SmeltProducts`.
    - `SmeltProducts` checks `intricate` to determine smelting output.
- **Meaning:** Indicates item is lost upon smelting (not recoverable).

**Adding More Tags (Custom Fields):**

- **Method:** Use `DefModExtension` (C#).
- **Benefits:** Compatibility, modularity.
- **Alternative to:** Directly modifying `ThingDef` class (complex, compatibility issues).

**Key Takeaway:**

- `ThingDef` defines core properties of in-game objects.
- Decompilation and code analysis are essential for understanding tag behavior.
- `DefModExtension` is recommended for adding custom data/functionality to ThingDefs.
- No comprehensive tag documentation exists - rely on code analysis and community resources.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\xml-csharp-integration.md`:

```````md
## XML and C# Integration - Cheat Sheet

### Using C# from XML

**A. Classy Design Pattern:**
- RimWorld exposes Def classes in XML.
- Look for tags like `workerClass`, `thingClass`.
- Example:
  ```xml
  <BiomeDef>
    <workerClass>BiomeWorker_BorealForest</workerClass>
  </BiomeDef>
  ```

**B. "Classy" Pattern (Class Attribute Override):**
- Overwrite default Def class or field class using `Class` attribute.
- Useful for Components, GenSteps, etc.
- Example:
  ```xml
  <comps>
    <li Class="CompProperties_Forbiddable"/>
  </comps>
  ```
  ```xml
  <GenStepDef Class="MyNamespace.MyCustomGenStepDefClass">
  ```
- **Note:** Use DefModExtensions for adding fields to Defs instead of overwriting Def classes.

**C. Defining Custom Types (Self-Defined):**
- Supply custom C# type directly in XML, e.g., AlienRace.
- Example:
  ```xml
  <AlienRace.ThingDef_AlienRace ParentName="BasePawn">
    <alienRace>
      </alienRace>
  </AlienRace.ThingDef_AlienRace>
  ```

**D. Serializing Custom Classes:**
- `LoadDataFromXmlCustom` method in C# class.
- RimWorld uses this method to parse XML for your custom type.
- Limitations: Doesn't work for structs (no constructor).
- Example:
  ```csharp
  public class RotR_cost
  {
      public void LoadDataFromXmlCustom(XmlNode xmlRoot) { ... }
  }
  ```

**E. Common Issues:**
- Namespace.Class must be exact.
- Assembly containing the type must be supplied.

### Using XML from C#

**A. DefOf:**
- Annotated class `[DefOf]` auto-fills static fields with Defs.
- Access Defs directly: `MyDefOf.JustOneExampleDefName`.
- **Pros:**
    - Runtime fast.
    - Easy to use.
    - Startup error detection (early warnings).
- **Cons:**
    - Direct XML referencing (less flexible to XML changes).
    - DefOf fields are `null` until initialized (startup issues).
    - Minor negative startup time impact.
- Example:
  ```csharp
  [DefOf]
  public static class MyDefOf
  {
      public static SomeDef JustOneExampleDefName;
      static MyDefOf() { DefOfHelper.EnsureInitializedInCtor(typeof(MyDefOf)); }
  }
  ```

**B. DefDatabase:**
- Look up Defs directly using `DefDatabase<DefType>.GetNamed("defName")`.
- Example:
  ```csharp
  SomeDef def = DefDatabase<SomeDef>.GetNamed("JustOneExampleDefName");
  ```
- **Pros:**
    - Null return allowed (silent failure).
    - No need for extra class.
    - String-based flexibility for dynamic Def lookup.
- **Cons:**
    - Null return allowed (potential runtime errors if not handled).
    - Slower than DefOf (negligible if cached in static variable).
    - Typo-prone (string-based defName).

### Modifying Classes

**Changing Methods:**
- Use Harmony for method modification.
- For details on Harmony usage, see the Harmony Guide.

**Adding Fields/Methods to Classes:**
- Not directly possible (even with Harmony).
- Alternatives:
    - Subclassing.

**Adding Fields to Defs:**
- Use `DefModExtension`.
- Allows adding custom fields to any Def via XML.
- Access via `def.GetModExtension<ExtensionType>()`.

**Adding Behavior to Thing Class:**
- For `ThingWithComps`, use `ThingComp`.
- `ThingComp`: Modular, for data storage and functionality.

**Adding Behavior to Hediff Class:**
- For `HediffWithComps`, use `HediffComp`.
- Similar to `ThingComp` but for Hediffs.

**Overriding Non-Overridden Methods:**
- Not possible directly with Harmony.
- Harmony patches require an existing method to patch.
- Alternatives:
    - Patch Base Class: Patch parent class and use `__instance is SubClass` check.
    - Subclass & Replace: Subclass target class, replace instantiation with Harmony.

**Patching Base Class of Subclass:**
- Patch parent class method.
- Use `__instance is SubClass` to apply patch logic only to specific subclasses.
- Example:
  ```csharp
  if (!(__instance is SubClass))
      return;
  ```

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\xml\xml-patching.md`:

```````md
## XML Patching - Cheat Sheet

**Purpose:** 
- Modify existing game values (Defs) via XML without overwriting
- Non-destructive modding for compatibility
- Avoid breaking other mods or the base game

**Location:** 
- XML files in `Patches` folder within mod

**Useful Tools:**
- RimWorld Dev Mode: Debug Inspector for XPath discovery
- XML Code Editor: VSCode, Notepad++ with XML support
- RimWorld Dotnet Template (optional): For mod setup
- Code Snippets (optional): For faster XML creation

**Basic Structure:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<Patch>
  <!-- PatchOperations go here -->
</Patch>
```

## XPath Basics

- Target XML nodes for PatchOperations
- Syntax: `Defs/DefType[predicate]/nodeToTarget`
- `Defs/`: Root for all Def XML
- `DefType`: Type of Def (e.g., `ThingDef`, `RecipeDef`)
- `[predicate]`: Conditional matching (e.g., `[defName="Wall"]`)
- `nodeToTarget`: XML tag to modify (e.g., `statBases`)
- Attribute Targeting: `Defs/ThingDef[@Name="ShelfBase"]/stuffCategories`
- Parent Targeting: `Defs/ThingDef[@ParentName="ApparelBase"]`
- Multiple Targets: `[defName="A" or defName="B"]`
- Text Content: `/text()` in XPath to target tag text content

## PatchOperation Types

### XML Node Operations:

- **PatchOperationAdd**: 
  - Adds child node (`<value>`) to XPath target
  - `<order>`: `Prepend` or `Append` (default)
  - Example:
  ```xml
  <Operation Class="PatchOperationAdd">
    <xpath>Defs/ThingDef[defName="Wall"]/statBases</xpath>
    <value>
      <MaxHitPoints>400</MaxHitPoints>
    </value>
  </Operation>
  ```

- **PatchOperationInsert**: 
  - Inserts sibling node (`<value>`) relative to XPath target
  - `<order>`: `Prepend` (default) or `Append`
  - Example:
  ```xml
  <Operation Class="PatchOperationInsert">
    <xpath>Defs/ThingDef[defName="Wall"]/statBases/MaxHitPoints</xpath>
    <value>
      <Beauty>-5</Beauty>
    </value>
  </Operation>
  ```

- **PatchOperationRemove**: 
  - Deletes XPath target node
  - Example:
  ```xml
  <Operation Class="PatchOperationRemove">
    <xpath>Defs/ThingDef[defName="Wall"]/statBases/MaxHitPoints</xpath>
  </Operation>
  ```

- **PatchOperationReplace**: 
  - Replaces XPath target node with `<value>`
  - Example:
  ```xml
  <Operation Class="PatchOperationReplace">
    <xpath>Defs/ThingDef[defName="Bullet_Revolver"]/projectile/damageAmountBase</xpath>
    <value>
      <damageAmountBase>100</damageAmountBase>
    </value>
  </Operation>
  ```

### XML Attribute Operations:

- **PatchOperationAttributeAdd**: Adds attribute (`<attribute>`, `<value>`) if not present
- **PatchOperationAttributeSet**: Sets/overwrites attribute (`<attribute>`, `<value>`)
- **PatchOperationAttributeRemove**: Removes attribute (`<attribute>`)

### Special Operations:

- **PatchOperationSequence**: 
  - Executes list of operations; aborts on failure
  - Example:
  ```xml
  <Operation Class="PatchOperationSequence">
    <operations>
      <li Class="PatchOperationReplace">
        <!-- First operation -->
      </li>
      <li Class="PatchOperationAdd">
        <!-- Second operation -->
      </li>
    </operations>
  </Operation>
  ```

- **PatchOperationAddModExtension**: Adds `DefModExtension` to Def
- **PatchOperationSetName**: Renames XML node (`<name>`)

### Conditional Operations:

- **PatchOperationFindMod**: 
  - Conditional operations based on mod/DLC presence
  - Uses mod `name`, not `packageId`
  - Include DLC names for DLC-specific patches
  - Example:
  ```xml
  <Operation Class="PatchOperationFindMod">
    <mods>
      <li>Royalty</li>
      <li>Ideology</li>
      <li>Biotech</li>
      <li>Anomaly</li>
    </mods>
    <match>
      <!-- Operations if mod is present -->
    </match>
    <nomatch>
      <!-- Operations if mod is not present -->
    </nomatch>
  </Operation>
  ```

- **PatchOperationConditional**: 
  - Conditional operations based on XPath test
  - Example:
  ```xml
  <Operation Class="PatchOperationConditional">
    <xpath>Defs/ThingDef[defName="Wall"]/statBases/MaxHitPoints</xpath>
    <match>
      <!-- Operations if XPath exists -->
    </match>
    <nomatch>
      <!-- Operations if XPath doesn't exist -->
    </nomatch>
  </Operation>
  ```

- **PatchOperationTest**: Test XPath validity within `PatchOperationSequence` (Obsolete - use `PatchOperationConditional`)

## Complete Example (Explosive Assault Rifle):

```xml
<Patch>
  <Operation Class="PatchOperationSequence">
    <operations>
      <li Class="PatchOperationReplace">
        <xpath>Defs/ThingDef[defName="Gun_AssaultRifle"]/verbs/li/defaultProjectile</xpath>
        <value>
          <defaultProjectile>Bullet_Explosive</defaultProjectile>
        </value>
      </li>
      <li Class="PatchOperationAdd">
        <xpath>Defs/ThingDef[defName="Gun_AssaultRifle"]/verbs/li</xpath>
        <value>
          <forcedMissRadius>2</forcedMissRadius>
          <targetParams>
            <canTargetLocations>true</canTargetLocations>
          </targetParams>
        </value>
      </li>
    </operations>
  </Operation>
</Patch>
```

## Tips and Tricks:

- **Patch Order:** Run after Def load, in mod load order
- **Patching Scope:** Before Def inheritance
- **`Inherit="False"`:** Prevent child from inheriting parent tag value
- **Deep Inspection Mode:** Find XPath easily in Debug Mode
- **Reference Vanilla XML:** Locate values for patching in game files (`Data` folder)
- **Test Patches Incrementally:** Start with simple patches and build complexity
- **Check Player.log:** For patch errors and debugging
- **Class Name Changes:** Some classes have been renamed in recent versions:
  - `ManhunterPackIncidentUtility` → `AggressiveAnimalIncidentUtility`
  - `IncidentWorker_ManhunterPack` → `IncidentWorker_AggressiveAnimals`
  - If patching incident workers, use the new class names.

## Common Issues / Troubleshooting:

- **Case Sensitivity:** XPath, XML tags are case-sensitive
- **XML Syntax:** Ensure valid XML structure; use validators
- **`Insert` vs `Add`:** `Insert` for siblings, `Add` for children
- **XPath Targets:** XML data structure, not file paths
- **Unique Nodes:** Non-`li` XML nodes must be unique at each level
- **Cascading Errors:** Fix the first error first, others may resolve automatically

## Obsolete Notes:

- PatchOperationTest & `<success>` tag in `PatchOperationSequence` are obsolete; use `PatchOperationConditional`
- `<success>` options: `Always` (suppress errors - generally avoid), `Normal`, `Invert`, `Never`
- `MayRequire` in `PatchOperationSequence` child operations for conditional loading
- PatchOperationFindMod uses mod `name`, not `packageId`

```````
