## Basic Melee Weapon Modding - Cheat Sheet

**Goal:** Create a simple machete melee weapon mod with custom texture and stat bonus.

**Required Files & Folders:**

```
Mods
└ MyModFolder
  ├ About
  │ └ About.xml
  ├ Defs
  │ └ ThingDefs
  │   └ ExampleWeapons_Melee.xml
  └ Textures
    └ ExampleMod
      ├ ExampleWeapon_Machete.png
      └ ExampleWeapon_Machete_m.png (mask)
```

**About Folder:**

- `About.xml`: Mod metadata.
  - Required tags: `<packageId>`, `<name>`, `<author>`, `<description>`, `<supportedVersions>`.
  - Example:
    ```xml
    <ModMetaData>
        <packageId>AuthorName.ExampleMeleeWeapon</packageId>
        <name>Example Melee Weapon</name>
        <author>AuthorName</author>
        <supportedVersions><li>1.5</li></supportedVersions>
        <description>Example melee weapon mod.</description>
    </ModMetaData>
    ```

**Defs Folder:**

- `ThingDefs/ExampleWeapons_Melee.xml`: ThingDef for the machete.
  - Start by copying vanilla `Gladius` ThingDef from `Data/Core/Defs/ThingDefs_Misc/Weapons/MeleeMedieval.xml`.
  - Modify copied ThingDef:
    - `<defName>`: Unique ID (e.g., `ExampleMeleeWeapon_Machete`).
    - `<label>`: In-game name (e.g., `machete`).
    - `<description>`: Item description.
    - `<graphicData>`:
      - `<texPath>`: Texture path (e.g., `ExampleMod/ExampleWeapon_Machete`).
      - `<graphicClass>`: `Graphic_Single`.
      - `<shaderType>`: `CutoutComplex` (for texture mask).
    - `<costStuffCount>`: Material cost (e.g., `40`).
    - `<statBases>`:
      - `<WorkToMake>`: Work units to craft (e.g., `10000`).
      - `<Mass>`: Weight (e.g., `0.75`).
    - `<equippedStatOffsets>`: Stat bonuses when equipped.
      - `<PlantWorkSpeed>`: Plant cutting speed bonus (e.g., `0.10`).
    - `<relicChance>`: Relic chance weight (e.g., `1`).
    - `<tools>`: Melee attack definitions.
      - `<li>`: Tool entry.
        - `<label>`: Tool name (e.g., `edge`, `point`, `handle`).
        - `<capacities>`: Damage capacities (e.g., `<li>Cut</li>`, `<li>Stab</li>`, `<li>Blunt</li>`).
        - `<power>`: Damage amount (e.g., `14`, `9`).
        - `<cooldownTime>`: Attack cooldown (e.g., `2`).
    - `<recipeMaker>`: Crafting recipe settings.
      - `<researchPrerequisite>`: Research needed to unlock recipe (e.g., `Smithing`).
      - `<skillRequirements>`: Skill requirements.
        - `<Crafting>`: Crafting skill level (e.g., `2`).
      - `<displayPriority>`: Recipe display order (e.g., `409`).

**Textures Folder:**

- `ExampleMod/ExampleWeapon_Machete.png`: Weapon texture.
- `ExampleMod/ExampleWeapon_Machete_m.png`: Texture mask (optional).

**Instructions:**

1. **Create Folders:** `Mods/MyModFolder/About`, `Mods/MyModFolder/Defs`, `Mods/MyModFolder/Defs/ThingDefs`, `Mods/MyModFolder/Textures`, `Mods/MyModFolder/Textures/ExampleMod`.
2. **Create `About.xml`:**  In `About` folder, fill in metadata.
3. **Create `ExampleWeapons_Melee.xml`:** In `Defs/ThingDefs` folder.
4. **Copy Gladius ThingDef:** Paste Gladius XML into `ExampleWeapons_Melee.xml`.
5. **Modify ThingDef:** Edit `defName`, `label`, `description`, `graphicData`, `costStuffCount`, `statBases`, `equippedStatOffsets`, `relicChance`, `tools`, `recipeMaker` to machete stats.
6. **Add Textures:** Place `ExampleWeapon_Machete.png` and `ExampleWeapon_Machete_m.png` in `Textures/ExampleMod`.
7. **Test:** Enable mod in RimWorld, spawn machete using dev mode, or craft at smithy.

**Key Points:**

- `defName` must be unique.
- `texPath` in `graphicData` must be unique.
- Use `CutoutComplex` shader for texture masks.
- `packageId` in `About.xml` must be unique.
- Check troubleshooting guide and RimWorld Discord for errors.
