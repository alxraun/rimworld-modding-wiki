
## Modding Tutorials/Flooring - Cheat Sheet

**Goal:** Create custom flooring using XML.

**Prerequisites:**
- Completed "Items" Tutorial.
- Familiar with RimWorld mod structure.
- Basic understanding of `TerrainDef`.

**Steps:**

1.  **Create Folders:**
    - In your mod folder (`Mods/YourModFolder`):
        - Create `Defs` folder (if not existing).
        - Create `Defs/TerrainDefs` folder.

2.  **Create TerrainDef XML:**
    - In `Defs/TerrainDefs`, create `YourFlooringFileName.xml` (e.g., `ExampleFlooring.xml`).
    - Add XML header:
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    ```
    - Add root `TerrainDefs` tag:
    ```xml
    <TerrainDefs>
    </TerrainDefs>
    ```
    - Within `TerrainDefs` tags, define your `TerrainDef`:
    ```xml
    <TerrainDef>
        <defName>YourFlooringDefName</defName>
        <label>In-game Flooring Label</label>
        <RenderPrecedence>210</RenderPrecedence>
        <Description>Flooring description.</Description>
        <TexturePath>Path/to/Your/Texture</TexturePath>
        <Beauty>BeautyLevel</Beauty>
        <SurfacesSupported>
            <li>Light</li>
            <li>Heavy</li>
            <li>SmoothHard</li>
        </SurfacesSupported>
        <WorkToBuild>WorkAmount</WorkToBuild>
        <DesignationCategory>Structure</DesignationCategory>
        <Fertility>0</Fertility>
        <CostList>
            <li>
                <thingDef>StuffDefName</thingDef>
                <count>CostCount</count>
            </li>
        </CostList>
        <ConstructionEffect>ConstructEffect</ConstructionEffect>
        <AcceptTerrainSourceFilth>True</AcceptTerrainSourceFilth>
    </TerrainDef>
    ```
    - Replace placeholder values with desired properties.
    - Refer to `TerrainDef` documentation for tag options.

3.  **Testing:**
    - Enable Development mode in RimWorld options.
    - Activate your mod in Mods menu.
    - Check for errors on game startup (tilde key `~`).

4.  **Key `TerrainDef` Tags:**
    - `<defName>`: Unique identifier.
    - `<label>`: In-game name.
    - `<description>`: Item description.
    - `<texturePath>`: Path to texture in `Textures` folder.
    - `<costList>`: Resources required for building.
    - `<WorkToBuild>`: Work units to build.
    - `<DesignationCategory>`: Architect menu category.
    - `<RenderPrecedence>`: Rendering order.
    - `<Beauty>`: Beauty stat.
    - `<SurfacesSupported>`: Surfaces flooring can be built on.
    - `<ConstructionEffect>`: Construction visual effect.
    - `<AcceptTerrainSourceFilth>`: Whether flooring accepts terrain filth.

5.  **Run RimWorld:**
    - Enable dev mode and your mod.
    - Check for errors.
    - Load game and find your flooring in the Structure menu (or specified `DesignationCategory`).
```
