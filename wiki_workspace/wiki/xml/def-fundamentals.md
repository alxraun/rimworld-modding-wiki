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
