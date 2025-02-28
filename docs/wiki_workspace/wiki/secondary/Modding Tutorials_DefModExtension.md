```markdown
## DefModExtension - Cheat Sheet

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

**Key Syntax Points:**

- C# Class: Inherit from `DefModExtension`.
- XML: Use `<modExtensions><li>` with `Class` attribute to specify your extension class.
- Access in Code: `def.GetModExtension<YourModExtension>().fieldName`.
- Patching: `PatchOperationAddModExtension` for adding via patches.
```
