
## Writing Custom Code - Cheat Sheet

**Purpose:**

- Extend RimWorld functionality beyond XML.
- Create standalone C# classes or Harmony patches.

**Creating a Class:**

- IDE Setup: Use Visual Studio, Rider or other IDEs.
- Project Type: Class Library (.NET Framework 4.7.2).
- Output Path: `(RimWorldInstallFolder)/Mods/(YourModName)/Assemblies`.
- References: Add `Assembly-CSharp.dll`, `UnityEngine.CoreModule.dll`.
- `using` Statements: Import namespaces (e.g., `using System;`, `using RimWorld;`, `using Verse;`).
- `namespace`: Organize code, prevent naming conflicts. Use unique namespace (e.g., `YourName.YourModName`).
- Class Definition: `public class MyClassName { ... }`.
- Constructor: `public MyClassName() { ... }` - Initialize class instances.

**Namespaces (Useful):**

- **General:**
    - `System.Collections.Generic`: `List<>`, `Dictionary<>`, `IEnumerable<>`.
    - `System.Linq`: LINQ operations (`.Where()`, etc.).
    - `System.Text.RegularExpressions`: RegEx for string manipulation.
    - `System.Collections`: `IEnumerable` interface.
    - `System.Text`: `StringBuilder` for efficient string building.

- **RimWorld Specific:**
    - `RimWorld`: Core RimWorld classes.
    - `Verse`: Base game classes, general game functionality.
    - `RimWorld.BaseGen`: Settlement generation.
    - `RimWorld.Planet`: World-related classes.
    - `Verse.AI`: Pawn AI, Jobs.
    - `Verse.AI.Group`: Squad AI.
    - `Verse.Grammar`: Text generation, localization.
    - `UnityEngine`: GUI, Rect, Color.

**Writing Code Workflow:**

1. **Decompile Source Code:** Examine existing game code for reference.
2. **Compile to DLL:** Use IDE (Build/Compile) to create `.dll` in `Assemblies` folder.
3. **Reference in XML (Optional):** Link C# code to XML Defs using `<thingClass>`, `<compClass>`, etc.
    - Example: `ThingDef` with custom `thingClass` pointing to C# class.
4. **Test in Game:** Run RimWorld; custom classes should load.

**Examples (Quick Start):**

- Hello World Mod: Basic C# mod for testing setup.
- Assembly Modding Example: Small modding project example.
- Plague Gun Tutorial: Step-by-step guide for a weapon mod (more complex).

**Key Points:**

- Namespaces: Crucial for code organization and preventing conflicts.
- `using` Directives: Simplify code by importing namespaces.
- Constructors: Initialize class instances.
- C# & XML Linking: Bridge between code and content definition.
- Debugging: Use decompiled code for reference; ask for help in modding communities.
- Compilation: Ensure correct .NET Framework and output settings.
```
