## Code Compatibility - Cheat Sheet

**Objective:** Create compatibility between your mod and another mod's DLL using Harmony (manual patching).

**Requirements:**

- [x] Your Mod
- [x] Target Mod ("OtherMod")
- [x] Harmony
- [x] "Hello World" setup
- [x] Willingness to create compatibility

**Manual Patching Snippet:**

```csharp
try
{
    ((Action)(() =>
    {
        if (LoadedModManager.RunningModsListForReading.Any(x=> x.Name == "OtherMod"))
        {
            harmony.Patch(AccessTools.Method(typeof(FullNameSpaceOfSomeOtherMod.SomeClass), nameof(FullNameSpaceOfSomeOtherMod.SomeClass.SomeOtherMethod)),
                postfix: new HarmonyMethod(typeof(HarmonyPatches), nameof(PatchOnSomeMethodFromSomeOtherMod_PostFix)));
        }
    }))();
}
catch (TypeLoadException ex) { }
```

**Explanation:**

- **Purpose:** Patches a method from "OtherMod" DLL conditionally, if "OtherMod" is loaded.
- **`try-catch` Block:** Handles `TypeLoadException` if "OtherMod" is not present, preventing errors.
- **Anonymous Delegate & Action:**  Workaround to avoid direct dependency on "OtherMod" at compile time.
- **`LoadedModManager.RunningModsListForReading.Any(...)`:** Checks if "OtherMod" is in the active mod list.
- **`harmony.Patch(...)`:** Applies Harmony patch if "OtherMod" is loaded.
    - `AccessTools.Method(...)`: Gets method info from "OtherMod" using reflection.
    - `postfix: new HarmonyMethod(...)`: Specifies postfix patch method in `HarmonyPatches` class.

**Constraints Addressed by Snippet:**

- **Optional Mod:** Handles cases where "OtherMod" may or may not be active/present.
- **No Direct Dependency:** Avoids compile-time dependency on "OtherMod" DLL.
- **Compiler Optimizations:** Circumvents compiler inlining and JIT behavior issues that can break conditional patching.

**Gotchas:**

- **AssemblyVersion vs. FileVersion (AssemblyFileVersion):**
    - Avoid referencing specific `AssemblyVersion` of "OtherMod.dll".
    - Patch against `FileVersion` (AssemblyFileVersion) if possible, or ideally no specific version.
    - `AssemblyVersion` changes indicate API breaking changes and may break patches.
- **Reference Updates:**  If "OtherMod" updates `AssemblyVersion`, your patch may break.

**Best Practice:**
- Use this manual patching approach for optional compatibility with other DLL mods.
- Prefer patching against `FileVersion` if versioning is needed.
- Consider not referencing specific versions at all for more robust compatibility.

