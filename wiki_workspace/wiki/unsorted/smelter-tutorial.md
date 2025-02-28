```markdown
## Smelter Modding - Cheat Sheet

**Concept:** Create a Smelter building to process slag debris into metal.

**Overview:** Define 3 key elements:
- Building Definition (`ThingDef`)
- Recipe Definition (`RecipeDef`)
- Work Giver Definition (`WorkGiverDef`)

**1. Building Definition (`ThingDef`):**
   - File: `Defs/ThingDefs/Buildings_Smelter.xml`
   - Root Tag: `<Buildings>`
   - Basic Structure:
     ```xml
     <Buildings>
       <ThingDef ParentName="BuildingBase">
         <DefName>BuildingSmelter</DefName>
         <EType>Building_WorkTable</EType> <ThingClass>Building_WorkTable</ThingClass>
         <Label>Smelter</Label>
         <Description>...</Description>
         <TexturePath>...</TexturePath>
         <CostList>...</CostList>
         <AltitudeLayer>BuildingTall</AltitudeLayer>
         <WorkToBuild>...</WorkToBuild>
         <maxHealth>...</maxHealth>
         <Size>(3,2)</Size>
         <surfaceNeeded>Heavy</surfaceNeeded>
         <DesignationCategory>Production</DesignationCategory>
         <Passability>Impassable</Passability>
         <hasInteractionSquare>true</hasInteractionSquare>
         <interactionSquareOffset>(0,0,2)</interactionSquareOffset>
         <inspectorTabs><li>ITab_Bills</li></inspectorTabs>
         <recipes><li>SmeltDebrisSlag</li></recipes>
       </ThingDef>
     </Buildings>
     ```
   - Key Tags:
     - `ParentName="BuildingBase"`: Inherits base building properties.
     - `DefName`: Unique identifier (`BuildingSmelter`).
     - `EType`, `ThingClass`: Set to `Building_WorkTable` for workbench functionality.
     - `Label`, `Description`: User-facing info.
     - `TexturePath`: Graphic path.
     - `CostList`: Build resources.
     - `Size`: Building dimensions.
     - `surfaceNeeded`: Heavy.
     - `DesignationCategory`: Production.
     - `Passability`: Impassable.
     - `hasInteractionSquare`, `interactionSquareOffset`: Enables and positions interaction spot.
     - `inspectorTabs`: `<li>ITab_Bills</li>` for bill management.
     - `recipes`: `<li>SmeltDebrisSlag</li>` (recipe defName).

**2. Recipe Definition (`RecipeDef`):**
   - File: `Defs/RecipeDefs/Recipe_Smelter.xml`
   - Root Tag: `<RecipeDefs>`
   - Basic Structure:
     ```xml
     <RecipeDefs>
       <RecipeDef>
         <defName>SmeltDebrisSlag</defName>
         <label>Smelt slag debris</label>
         <description>...</description>
         <jobString>Smelting slag.</jobString>
         <workAmount>...</workAmount>
         <products>
           <li>
             <thingDef>Metal</thingDef>
             <count>5</count>
           </li>
         </products>
         <ingredients>
           <li>
             <filter>
               <thingDefs>
                 <li>DebrisSlag</li>
               </thingDefs>
             </filter>
             <count>1</count>
           </li>
         </ingredients>
       </RecipeDef>
     </RecipeDefs>
     ```
   - Key Tags:
     - `defName`: Unique identifier (`SmeltDebrisSlag`).
     - `label`, `description`, `jobString`: User-facing info.
     - `workAmount`: Work units for recipe completion.
     - `<products>`: Defines output items.
       - `<li>`: Product entry.
         - `<thingDef>`: Product `ThingDef` (`Metal`).
         - `<count>`: Product quantity.
     - `<ingredients>`: Defines input items.
       - `<li>`: Ingredient entry.
         - `<filter>`: Ingredient filter.
           - `<thingDefs>`: Allowed `ThingDef` list.
             - `<li>`: Allowed ingredient `ThingDef` (`DebrisSlag`).
         - `<count>`: Ingredient quantity.

**3. Work Giver Definition (`WorkGiverDef`):**
   - File: `Defs/WorkgiverDefs/Workgiver_Smelter.xml`
   - Root Tag: `<WorkGivers>`
   - Basic Structure:
     ```xml
     <WorkGivers>
       <WorkGiverDef>
         <DefName>DoBillsSmelter</DefName>
         <giverClass>AI.WorkGiver_DoBill</giverClass>
         <workType>Crafting</workType>
         <priorityInType>10</priorityInType>
         <WorkTableDef>BuildingSmelter</WorkTableDef>
         <verb>smelt</verb>
         <gerund>smelting</gerund>
       </WorkGiverDef>
     </WorkGivers>
     ```
   - Key Tags:
     - `DefName`: Unique identifier (`DoBillsSmelter`).
     - `giverClass`: `AI.WorkGiver_DoBill` for bill-based work.
     - `workType`: Skill type (`Crafting`).
     - `priorityInType`: Job priority.
     - `WorkTableDef`: Building `DefName` (`BuildingSmelter`).
     - `verb`, `gerund`: Text for job descriptions.

**Testing:**
- Enable Development mode.
- Build Smelter.
- Spawn Slag Debris via Dev tools.
- Create "Smelt slag debris" bill at Smelter.
- Colonists should haul slag and smelt metal.

**Troubleshooting:**
- Verify XML syntax and tag spelling.
- Check for errors in `Player.log`.
- Ensure DefNames are unique.
- Reboot game and mod list to refresh data.
```
