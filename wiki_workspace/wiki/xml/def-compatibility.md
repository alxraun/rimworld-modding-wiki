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
```
