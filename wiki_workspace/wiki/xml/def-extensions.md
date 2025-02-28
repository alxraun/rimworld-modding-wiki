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
