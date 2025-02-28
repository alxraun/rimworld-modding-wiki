```markdown
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

- **Conditional Loading (DLC/Mods):**
    - `MayRequire="packageId"`: Load if **all** listed DLCs/mods are active.
    - `MayRequireAnyOf="packageId1,packageId2"`: Load if **any** listed DLCs/mods are active.

    - **Example - MayRequire:**
        ```xml
        <Tribal_Child MayRequire="Ludeon.RimWorld.Biotech">10</Tribal_Child>
        ```
    - **Example - MayRequireAnyOf:**
        ```xml
        <li MayRequireAnyOf="Ludeon.RimWorld.Royalty,Ludeon.RimWorld.Biotech">
        ```

**Key Recommendations:**

- **Prioritize XPath Patches:** For modifying existing Defs.
- **Avoid Overwriting:** Prevents mod conflicts.
- **Use `recipeUsers`/`recipes`/`recipeMaker`:** For linking recipes and buildings.
- **Use `wildBiomes`:** For adding animals to biomes.
- **Use `MayRequire`/`MayRequireAnyOf`:** For conditional content loading based on DLCs/mods.
```
