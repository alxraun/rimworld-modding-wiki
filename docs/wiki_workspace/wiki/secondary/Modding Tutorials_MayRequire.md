```markdown
## MayRequire - Cheat Sheet

**Purpose:**

- Conditional XML loading based on DLCs or mods.
- Enables optional mod dependencies.

**XML Attributes:**

- `MayRequire="packageId1,packageId2"`: Node loads only if *all* listed DLCs/mods are active.
- `MayRequireAnyOf="packageId1,packageId2"`: Node loads if *any* listed DLCs/mods are active.

**DLC Package IDs:**

- Royalty: `Ludeon.RimWorld.Royalty`
- Ideology: `Ludeon.RimWorld.Ideology`
- Biotech: `Ludeon.RimWorld.Biotech`
- Anomaly: `Ludeon.RimWorld.Anomaly`
- Mod Package IDs: Found in `About.xml` of the mod.

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

**Best Practices:**

- Use `MayRequire` for *optional* mod compatibility.
- For *required* dependencies, use `modDependencies` in `About.xml`.
- Test patches individually before using in `PatchOperationSequence`.
- Be aware of `MayRequire` limitations, especially with top-level nodes.
```
