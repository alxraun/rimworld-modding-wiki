
## Xenotype Template & Pawn Generation - Cheat Sheet

**Xenotype Template (`XenotypeDef`):**

- Used to assemble xenotypes from genes.
- XML Structure:
    ```xml
    <XenotypeDef>
        <defName>xenotypedefname</defName>
        <label>ingame name</label>
        <description>Long description</description>
        <descriptionShort>Short description</descriptionShort>
        <iconPath>UI/Icons/Xenotypes/Hussar</iconPath>
        <inheritable>true</inheritable> <!-- Optional: Inheritable xenotype -->
        <nameMaker>NamerPersonXenotype_Male</nameMaker> <!-- Optional: Custom name generator -->
        <nameMakerFemale>NamerPersonXenotype_Female</nameMakerFemale> <!-- Optional: Female name generator -->
        <chanceToUseNameMaker>1</chanceToUseNameMaker> <!-- Optional: Chance to use custom name generator -->
        <combatPowerFactor>1.0</combatPowerFactor>  <!-- Optional: Combat power factor adjustment -->
        <genes>
            <!-- Gene List (see below) -->
        </genes>
    </XenotypeDef>
```
- `<genes>` List: Contains `<li>geneDefName</li>` for each gene.
    - Comment out or delete `<li>` entries to exclude genes.
    - Genes categorized by effect (Visual, Stat, Unique).
    - Refer to template for gene lists under each category (Beard, Bodies, Brows, etc.).

**Adding Xenotypes to Pawns (Faction Def):**

- Faction Def XML: `<xenotypeSet>` tag defines xenotype distribution.
- `<xenotypeChances>` List: Defines xenotype probabilities/weights.

**Xenotype Distribution Interpretation:**

1. **Probabilities (Sum < 1.0):**
    - Values interpreted as percentage chances for each xenotype.
    - Remaining chance = Baseliner.
    - Example (Sum = 0.30):
        ```xml
        <xenotypeChances>
            <Neanderthal MayRequire="Ludeon.RimWorld.Biotech">0.05</Neanderthal>
            <Hussar MayRequire="Ludeon.RimWorld.Biotech">0.15</Hussar>
            <Genie MayRequire="Ludeon.RimWorld.Biotech">0.10</Genie>
        </xenotypeChances>
        ```
        Result: Neanderthal 5%, Hussar 15%, Genie 10%, Baseliner 70%.

2. **Weights (Sum > 1.0):**
    - Values interpreted as weights.
    - Chance = Xenotype Weight / Total Weights.
    - Baseliner excluded if not explicitly included.
    - Example (Weights):
        ```xml
        <xenotypeChances>
            <Neanderthal>999</Neanderthal>
        </xenotypeChances>
        ```
        Result: Neanderthal 100%.

**Xenotype Chance Accumulation:**

- Xenotype chances summed from:
    - FactionDef (`xenotypeSet`).
    - PawnKindDef (`xenotypeSet`).
    - Faction Memes (`xenotypeSet` in memes).
- Total chances interpreted as probabilities or weights.
- PawnKind Flag: `<useFactionXenotypes>false</useFactionXenotypes>` in PawnKindDef ignores faction xenotypeSet.

**Note:**

- Use `MayRequire="Ludeon.RimWorld.Biotech"` for Biotech DLC genes/xenotypes.
- Refer to template for gene lists (Visual, Stat, Unique).
- Choose probabilities or weights based on desired distribution and baseliner inclusion.
```
