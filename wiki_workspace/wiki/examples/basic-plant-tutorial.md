
## Basic Plant Modding - Cheat Sheet

**Goal:** Create a custom plant (theragold), raw food item, and herbal medicine recipe using XML.

**Folder Structure:**

- Mods/MyModFolder/
    - About/About.xml
    - Defs/
        - RecipeDefs/ExampleRecipe_Theragold.xml
        - ThingDefs_Items/ExampleItem_Theragold.xml
        - ThingDefs_Plants/ExamplePlant_Theragold.xml
    - Textures/ExampleMod/
        - ImmatureTheragold/ImmatureTheragold_a.png
        - Theragold/Theragold_a.png
        - RawTheragold.png

**About.xml (Required):**

```xml
<ModMetaData>
  <packageId>AuthorName.ExamplePlant</packageId>
  <name>Example Plant</name>
  <author>AuthorName</author>
  <supportedVersions><li>1.5</li></supportedVersions>
  <description>Example plant mod.</description>
</ModMetaData>
```

**1. Create Plant ThingDefs (ExamplePlant_Theragold.xml):**

- Copy & modify vanilla `Healroot` ThingDefs as starting point.
- Use XML inheritance for shared properties (`TheragoldBase`).

```xml
<Defs>
  <ThingDef ParentName="PlantBase" Name="TheragoldBase" Abstract="True">
    <graphicData>...</graphicData>
    <selectable>true</selectable>
    <plant>...</plant>
  </ThingDef>

  <ThingDef ParentName="TheragoldBase"> <!-- Cultivated -->
    <defName>ExamplePlant_Theragold</defName>
    <label>theragold</label>
    <description>...</description>
    <plant>...</plant>
  </ThingDef>

  <ThingDef ParentName="TheragoldBase"> <!-- Wild -->
    <defName>ExamplePlant_TheragoldWild</defName>
    <label>wild theragold</label>
    <description>...</description>
    <neverMultiSelect>false</neverMultiSelect>
    <plant>
      <wildBiomes>...</wildBiomes> <!-- Add biomes -->
    </plant>
  </ThingDef>
</Defs>
```

**TheragoldBase (Abstract):**
- `Abstract="True"`: Base for inheritance, not loaded directly.
- `<graphicData>`: Mature plant texture folder (`Graphic_Random`).
- `<selectable>true</selectable>`: Plant is selectable.
- `<plant>`: Plant properties.
    - `<immatureGraphicPath>`: Immature texture folder.
    - `<growDays>`: Days to maturity (wild variant).
    - `<harvestedThingDef>`: Raw food item DefName.
    - `<harvestYield>`: Harvested amount.
    - `<wildOrder>` & other plant properties.

**ExamplePlant_Theragold (Cultivated):**
- `ParentName="TheragoldBase"`: Inherits from base.
- `<defName>`, `<label>`, `<description>`: Unique name, label, description.
- `<plant>`: Cultivated plant properties.
    - `<growDays>`: Faster grow time (cultivated variant).
    - `<sowWork>`, `<sowMinSkill>`, `<sowTags>`, `<purpose>Food</purpose>`.

**ExamplePlant_TheragoldWild (Wild):**
- `ParentName="TheragoldBase"`: Inherits from base.
- `<defName>`, `<label>`, `<description>`: Unique name, label, description.
- `<neverMultiSelect>false</neverMultiSelect>`: Selectable in bandboxes.
- `<plant>`: Wild plant properties.
    - `<wildBiomes>`: Biome spawning weights (vanilla & modded examples with `MayRequire`).

**2. Create Raw Food ThingDef (ExampleItem_Theragold.xml):**

- Copy & modify vanilla `RawRice` ThingDef.

```xml
<Defs>
  <ThingDef ParentName="PlantFoodRawBase">
    <defName>ExamplePlant_RawTheragold</defName>
    <label>theragold flowers</label>
    <description>...</description>
    <graphicData>
      <texPath>ExampleMod/RawTheragold</texPath>
    </graphicData>
    <comps>
      <li Class="CompProperties_Rottable">
        <daysToRotStart>30</daysToRotStart>
        <rotDestroys>true</rotDestroys>
      </li>
    </comps>
  </ThingDef>
</Defs>
```

- `<defName>`, `<label>`, `<description>`: Unique name, label, description.
- `<graphicData>`: Raw food texture (`Graphic_Single`).
- `<comps>`: `CompProperties_Rottable` for spoilage.
    - `<daysToRotStart>`: Rot start days (30).

**3. Create Medicine RecipeDef (ExampleRecipe_Theragold.xml):**

- Create RecipeDef from scratch.

```xml
<Defs>
  <RecipeDef>
    <defName>ExamplePlant_Make_MedicineHerbal</defName>
    <label>make herbal medicine from theragold</label>
    <description>...</description>
    <jobString>Making herbal medicine from theragold flowers.</jobString>
    <effectWorking>Cook</effectWorking>
    <soundWorking>Recipe_CookMeal</soundWorking>
    <workSkill>Cooking</workSkill>
    <workAmount>300</workAmount>
    <workSpeedStat>GeneralLaborSpeed</workSpeedStat>
    <recipeUsers>
      <li>CraftingSpot</li>
      <li>DrugLab</li>
    </recipeUsers>
    <ingredients>
      <li>
        <filter>
          <thingDefs><li>ExamplePlant_RawTheragold</li></thingDefs>
        </filter>
        <count>12</count>
      </li>
    </ingredients>
    <fixedIngredientFilter><thingDefs><li>ExamplePlant_RawTheragold</li></thingDefs></fixedIngredientFilter>
    <products><MedicineHerbal>1</MedicineHerbal></products>
    <skillRequirements><Crafting>2</Crafting></skillRequirements>
  </RecipeDef>
</Defs>
```

- `<defName>`: Recipe DefName (e.g., `ExamplePlant_Make_MedicineHerbal`).
- `<label>`, `<description>`, `<jobString>`: UI strings.
- `<effectWorking>`, `<soundWorking>`: Visual/audio effects during work.
- `<workSkill>`, `<workAmount>`, `<workSpeedStat>`: Work skill, amount, speed stat.
- `<recipeUsers>`: Workbenches for recipe (e.g., `CraftingSpot`, `DrugLab`).
- `<ingredients>`: Input ingredients.
    - `<filter><thingDefs><li>...</li></thingDefs></filter>`: Ingredient ThingDef filter.
    - `<count>`: Ingredient count (12 theragold flowers).
- `<fixedIngredientFilter>`: Allowed ingredient filter for UI.
- `<products>`: Output products.
    - `<MedicineHerbal>1</MedicineHerbal>`: Produces 1 herbal medicine.
- `<skillRequirements>`: Required skills (Crafting 2).

**5. Testing:**

- Enable mod in Mod Manager.
- Use Dev Mode to spawn plant/raw food, check recipes at workbenches.
- Verify plant spawning in specified biomes (if applicable).
```
