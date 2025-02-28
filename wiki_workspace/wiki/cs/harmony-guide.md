## Harmony Cheat Sheet

**Harmony:** Runtime patching library for .NET/Mono methods in RimWorld. Best practice for code modification.

**Integration:**
- Add Harmony as reference to C# project.
- Do NOT include `0Harmony.dll` in mod's Assemblies.
- Add Harmony as Steam dependency.
- Avoid HugsLib for basic Harmony; use only when needing HugsLib features.

**Patch Types:**

- **Prefix:**
    - Runs **before** original method.
    - Return type: `void` or `bool`.
    - `bool` return `false`: Skips original method (use with caution - compatibility risk).
    - May prevent other prefixes from running.

- **Postfix:**
    - Runs **after** original method.
    - Guaranteed execution.
    - Can alter original method's `__result` (pass by `ref`).
    - Recommended for compatibility.

- **Transpiler:**
    - Alters method's IL code (low-level).
    - Uses `CodeInstructions`, `System.Reflection`, `System.Reflection.Emit`.
    - Complex, difficult to debug/maintain.
    - Use `Transpiler Hints` guide if necessary.

**Altering Results:**

- Modify original method's return value:
    - Postfix method parameter `ref __result`.

**Pitfalls:**

- **Overuse:** Consider alternatives (subclassing, Comps) before Harmony.
- **Result Not Changed:** Missing `ref` keyword when altering `__result`.
- **Returning Value from Void/Prefix:** Harmony return type != original method return type. Use `__result` for prefixes to alter outcome.
- **Incorrect Method Patch:** Use `AccessTools.Method` or `System.Reflection.MethodInfo` for accurate method specification, especially for overloaded methods.
- **Patch Not Applying:**
    - Check `HarmonyInstance.DEBUG = true` & Harmony log file.
    - Bootstrap order: `StaticConstructorOnStartup` for early patches.
    - Inlining: Patched method might be inlined; harder to patch.

**Bootstrapping:**

- Initialize Harmony in `[StaticConstructorOnStartup]` class.
- Use `Harmony.PatchAll()` or manual patching within static constructor.

**Links:**

- [Harmony Mod (Steam)](https://steamcommunity.com/workshop/filedetails/?id=2009463077)
- [Harmony 2 Releases (GitHub)](https://github.com/pardeike/Harmony/releases)
- [Harmony 2 NuGet](https://www.nuget.org/packages/Lib.Harmony/)
- [Harmony 2 Docs](https://harmony.pardeike.net/)
- [Alien Races HarmonyPatches Example](https://github.com/RimWorld-CCL-Reborn/AlienRaces/blob/master/Source/AlienRace/AlienRace/HarmonyPatches.cs)
