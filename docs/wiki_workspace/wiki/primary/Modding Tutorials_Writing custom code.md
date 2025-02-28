```markdown
## Writing Custom Code - Cheat Sheet

**Purpose:** Extend RimWorld functionality beyond XML via C# code (classes, Harmony patches).

**Requirements:**
- Set up Mod Solution.

**Creating a Class:**

- **Template Structure:**
  ```csharp
  using System; // Common namespaces
  using RimWorld;
  using Verse;

  namespace YourNamespace // Unique mod namespace
  {
      public class YourClassName // Class name
      {
          public YourClassName() // Constructor (optional)
          {
              // Initialization code
          }
      }
  }
  ```
- **`using` Statements:** Import namespaces for easier code access (e.g., `using System;`, `using RimWorld;`, `using Verse;`).
- **`namespace YourNamespace`:**  Unique identifier for your mod's code; prevents naming conflicts.
    - Project Root Namespace: Set in project properties (e.g., `YourModName`).
    - Folder-Based Namespaces: Subfolders create sub-namespaces (e.g., `YourNamespace.SubfolderName`).
- **`public class YourClassName`:**  Class definition; entry point for custom code.
- **`public YourClassName()`:** Constructor; initializes class instances.
    - Parameterless Constructor: `public YourClassName() {}` - allows `new YourClassName()` calls.
    - Parameterized Constructor: `public YourClassName(DataType param1, DataType param2) {}` - requires parameters on instantiation.
    - Default Parameter Constructor: `public YourClassName(DataType param1 = defaultValue) {}` - allows optional parameters.

**Useful Namespaces:**

- **General:**
    - `System.Collections.Generic`: `List<>`, `Dictionary<>`, etc.
    - `System.Linq`: LINQ operations (`.Where()`, etc.).
    - `System.Text.RegularExpressions`: RegEx for string manipulation.
    - `System.Collections`: Base collections interfaces.
    - `System.Text`: `StringBuilder` for efficient string building.
- **RimWorld Specific:**
    - `RimWorld`: Core RimWorld classes.
    - `Verse`: Base game classes, general functionality.
    - `RimWorld.BaseGen`: Settlement generation.
    - `RimWorld.Planet`: World-related classes.
    - `Verse.AI`: Pawn AI, Jobs.
    - `Verse.AI.Group`: Squad AI.
    - `Verse.Grammar`: Text generation, localization.
    - `UnityEngine`: GUI, Rect, Color, etc. (Unity engine).

**Writing Code Workflow:**

1. **Decompile Source Code:** Use a decompiler to study existing RimWorld code.
2. **Compile to DLL:** Build your C# project in IDE (e.g., Visual Studio) as a Class Library (.NET Framework).
    - Output Path: `YourMod/Assemblies/YourMod.dll`.
    - Ensure "Class Library" output type.
3. **Reference in XML (Optional):** Link C# classes to XML Defs.
    - `thingClass`:  Specify custom `Thing` class in `ThingDef`.
    - `verbClass`:  Specify custom `Verb` class for weapons.
    - DefModExtensions, etc.
4. **Test in RimWorld:** Enable mod and run game.

**Examples:**

- Hello World Mod: Basic C# mod for testing setup.
- Assembly Modding Example: Small example project.
- Plague Gun Tutorial: Step-by-step guide for a weapon mod.
```
