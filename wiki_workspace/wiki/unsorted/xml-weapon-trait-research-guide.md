```markdown
## XML Modding - Adding Weapons, Traits, Research - Cheat Sheet

**Goal:** Add a new weapon, trait, and research project to RimWorld using XML.

**Useful Tools:**
- Rimworld dotnet template (for mod setup)
- VSCode Code Snippets (for XML structure)
- Debug Inspector (in-game XML inspection)

**1. Creating a New Weapon:**

   **a) Bullet Def (ThingDefs\_Weapons.xml):**
   ```xml
   <Defs>
       <ThingDef ParentName="BaseBullet">
           <defName>MM_Bullet</defName>
           <label>Tutorial Bullet</label>
           <graphicData>
               <texPath>Things/Projectile/Bullet_Small</texPath>
               <graphicClass>Graphic_Single</graphicClass>
               <drawSize>1.1</drawSize>
           </graphicData>
           <projectile>
               <damageDef>Bullet</damageDef>
               <damageAmountBase>10</damageAmountBase>
               <stoppingPower>1</stoppingPower>
               <speed>44</speed>
           </projectile>
       </ThingDef>
   </Defs>
   ```
   - `ParentName="BaseBullet"`: Inherits base bullet properties.
   - `<defName>`: Unique identifier (e.g., `MM_Bullet`).
   - `<label>`: User-friendly name.
   - `<graphicData><texPath>`, `<graphicClass>`: Texture path and class.
   - `<projectile><damageDef>`, `<damageAmountBase>`, `<speed>`: Projectile damage properties.

   **b) Weapon Def (ThingDefs\_Weapons.xml):**
   ```xml
   <ThingDef ParentName="BaseHumanMakeableGun">
       <defName>MM_Our_Gun</defName>
       <label>Tutorial Gun</label>
       <description>...</description>
       <graphicData>...</graphicData>
       <uiIconScale>1.4</uiIconScale>
       <soundInteract>Interact_Revolver</soundInteract>
       <statBases>
           <WorkToMake>2000</WorkToMake>
           <Mass>1.6</Mass>
           <Accuracy...></Accuracy...>
           <RangedWeapon_Cooldown>1.5</RangedWeapon_Cooldown>
       </statBases>
       <weaponTags><li>SimpleGun</li><li>Revolver</li></weaponTags>
       <costList><Steel>10</Steel><Plasteel>1</Plasteel><ComponentIndustrial>1</ComponentIndustrial></costList>
       <recipeMaker><skillRequirements><Crafting>5</Crafting></skillRequirements></recipeMaker>
       <verbs><li><verbClass>Verb_Shoot</verbClass><defaultProjectile>MM_Bullet</defaultProjectile><warmupTime>0.9</warmupTime><burstShotCount>3</burstShotCount><ticksBetweenBurstShots>22</ticksBetweenBurstShots><range>20</range><soundCast>Shot_Revolver</soundCast><soundCastTail>GunTail_Light</soundCastTail><muzzleFlashScale>8</muzzleFlashScale></li></verbs>
       <tools><li><label>handle</label><capacities><li>Blunt</li></capacities><power>7</power><cooldownTime>3.1</cooldownTime></li><li><label>barrel</label><capacities><li>Blunt</li><li>Poke</li></capacities><power>5</power><cooldownTime>2.1</cooldownTime></li></tools>
   </ThingDef>
   ```
   - `ParentName="BaseHumanMakeableGun"`: Inherits base gun properties, makeable at machining table.
   - `<defName>`, `<label>`, `<description>`, `<graphicData>`, `<soundInteract>`, `<uiIconScale>`: Basic info.
   - `<statBases>`: Weapon stats (WorkToMake, Mass, Accuracy, RangedWeapon_Cooldown).
   - `<weaponTags>`: Weapon categories (SimpleGun, Revolver).
   - `<costList>`: Ingredients for crafting.
   - `<recipeMaker><skillRequirements>`: Crafting skill requirement (Crafting: 5).
   - `<verbs><li><verbClass>Verb_Shoot</verbClass>...<defaultProjectile>MM_Bullet</defaultProjectile>...<range>`, `<soundCast>`, etc.: Shooting verb properties.
   - `<tools><li><label>handle</label><capacities><li>Blunt</li></capacities><power>`, `<cooldownTime>`: Melee attacks.

**2. Adding a Research Project (ResearchProjectDef.xml):**

   ```xml
   <ResearchProjectDef>
       <defName>tutorial_Research</defName>
       <label>My Mods Research</label>
       <description>Craft the mithical tutorial gun.</description>
       <baseCost>500</baseCost>
       <techLevel>Industrial</techLevel>
       <prerequisites>
           <li>Gunsmithing</li>
       </prerequisites>
       <researchViewX>9.00</researchViewX>
       <researchViewY>4.80</researchViewY>
   </ResearchProjectDef>
   ```
   - `<defName>`, `<label>`, `<description>`: Basic info.
   - `<baseCost>`: Research cost.
   - `<techLevel>`: Tech level (Industrial).
   - `<prerequisites><li>`: Required research projects (Gunsmithing).
   - `<researchViewX>`, `<researchViewY>`: Position in research tree.

   **Locking Weapon behind Research (ThingDefs\_Weapons.xml - recipeMaker):**
   ```xml
   <recipeMaker>
       ...
       <researchPrerequisite>tutorial_Research</researchPrerequisite>
   </recipeMaker>
   ```
   - `<researchPrerequisite>`: Links recipe to research project `defName`.

**3. Adding New Traits (TraitDef.xml):**

   ```xml
   <TraitDef>
       <defName>Epic_crafting</defName>
       <commonality>1</commonality>
       <disabledWorkTags>
           <li>Animals</li>
       </disabledWorkTags>
       <degreeDatas>
           <li>
               <label>Epic Crafting</label>
               <description>[PAWN_nameDef] excels at crafting. ...</description>
               <degree>1</degree>
               <statOffsets>
                   <CarryingCapacity>25</CarryingCapacity>
                   <WorkSpeedGlobal>0.20</WorkSpeedGlobal>
               </statOffsets>
               <skillGains>
                   <li><key>Crafting</key><value>4</value></li>
               </skillGains>
           </li>
       </degreeDatas>
   </TraitDef>
   ```
   - `<defName>`: Unique trait identifier.
   - `<commonality>`: Chance of trait appearing on pawns.
   - `<disabledWorkTags><li>`: Work types disabled by trait (Animals).
   - `<degreeDatas><li>`: Trait degree definition.
       - `<label>`, `<description>`: User-friendly name and description.
       - `<degree>`: Trait degree value.
       - `<statOffsets>`: Stat modifiers (CarryingCapacity, WorkSpeedGlobal).
       - `<skillGains><li><key>Skill</key><value>Amount</value></li></skillGains>`: Skill level increases (Crafting: 4).

**General Notes:**

- XML is case-sensitive.
- Use code snippets for faster XML creation (VSCode recommended).
- Experiment and reference Vanilla XML for further customization.
- Test mod in-game using Dev mode.
```
