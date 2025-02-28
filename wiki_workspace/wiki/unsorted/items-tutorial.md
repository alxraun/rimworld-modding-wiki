```markdown
## Modding Tutorials/Items - Cheat Sheet

**Goal:** Create a simple resource item mod (Titanium example).

**Prerequisites:**
- Read "Getting Started" tutorial.
- Understand mod folder structure and file locations.

**Folder Setup:**

- `Mods/YourModFolder/Defs/ThingDefs/`
    - Create `ThingDefs` folder in `Defs` if not existing.

**Creating Resource Item (Titanium Example):**

1. **Create XML file:** `Defs/ThingDefs/YourItemDefs.xml` (e.g., `ExampleItem_Titanium.xml`).

2. **XML Root Node:**
   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <Defs>
       <!-- ThingDefs go here -->
   </Defs>
   ```

3. **Abstract Base ThingDef (`ResourceBase`):**
   ```xml
   <ThingDef Name="ResourceBase" Abstract="True">
       <defName>base_Resource</defName>
       <thingClass>ThingResource</thingClass>
       <label>Unspecified resource</label>
       <category>Item</category>
       <eType>Item</eType>
       <resourceCountPriority>Middle</resourceCountPriority>
       <useStandardHealth>true</useStandardHealth>
       <selectable>true</selectable>
       <maxHealth>100</maxHealth>
       <altitudeLayer>Item</altitudeLayer>
       <stackLimit>75</stackLimit>
       <purchasable>true</purchasable>
       <comps>
           <li><compClass>CompForbiddable</compClass></li>
       </comps>
       <beauty>Ugly</beauty>
       <alwaysHaulable>true</alwaysHaulable>
       <drawGUIOverlay>true</drawGUIOverlay>
       <rotatable>false</rotatable>
       <pathCost>15</pathCost>
   </ThingDef>
   ```

4. **Concrete ThingDef (Titanium):**
   ```xml
   <ThingDef ParentName="ResourceBase">
       <defName>Titanium</defName>
       <label>Titanium</label>
       <description>A rare strong and useful metal.</description>
       <texturePath>Things/Item/Resource/Titanium</texturePath>
       <interactSound>MetalDrop</interactSound>
       <basePrice>3</basePrice>
       <useStandardHealth>false</useStandardHealth>
       <storeCategories>
           <li>ResourcesRaw</li>
       </storeCategories>
   </ThingDef>
   ```
   - `ParentName="ResourceBase"`: Inherits properties from abstract base.
   - `<defName>Titanium</defName>`: Unique item ID.
   - `<label>Titanium</label>`: In-game item name.
   - `<description>`: Item description.
   - `<texturePath>`: Texture path (e.g., `Things/Item/Resource/Titanium`).
   - `<interactSound>`: Sound when interacted with (e.g., `MetalDrop`).
   - `<basePrice>`: Base market price.
   - `<storeCategories>`: Category for storage (e.g., `ResourcesRaw`).

**Testing:**

1. Enable `Development mode` in game options.
2. Activate mod in Mods menu.
3. Start new game, enable `God mode`.
4. Use "Tool: Spawn thing" (debug menu).
5. Search & spawn "Titanium".

**Conclusion:**

- Create `ThingDef` in XML.
- Use `ParentName` for inheritance.
- Define key tags (`defName`, `label`, `description`, `texturePath`, etc.).
- Test using Development mode.
```
