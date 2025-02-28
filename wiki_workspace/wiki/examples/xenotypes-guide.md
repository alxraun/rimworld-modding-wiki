
## Xenotypes - Cheat Sheet

**XenotypeDef Template:**

```xml
<XenotypeDef>
  <defName>xenotypedefname</defName>
  <label>ingame name</label>
  <description>Long description</description>
  <descriptionShort>Short description</descriptionShort>
  <iconPath>UI/Icons/Xenotypes/Hussar</iconPath>
  <inheritable>true</inheritable> 
  <nameMaker>NamerPersonDirtmole_Male</nameMaker> 
  <nameMakerFemale>NamerPersonDirtmole_Female</nameMakerFemale>
  <chanceToUseNameMaker>1</chanceToUseNameMaker>
  <combatPowerFactor>1.5</combatPowerFactor>
  <genes>
    <!-- Gene List -->
  </genes>
</XenotypeDef>
```

**Key XenotypeDef Tags:**

- `<defName>`: Unique identifier.
- `<label>`: In-game name.
- `<description>`: Full description.
- `<descriptionShort>`: Short description.
- `<iconPath>`: Icon texture path (`UI/Icons/Xenotypes/`).
- `<inheritable>`: `true` if inheritable, omit for non-inheritable.
- `<nameMaker>`: Name generator (male, or default if `nameMakerFemale` omitted).
- `<nameMakerFemale>`: Name generator (female).
- `<chanceToUseNameMaker>`: Chance to use custom name generator (0-1).
- `<combatPowerFactor>`: Combat power multiplier (omit for 1x).
- `<genes>`: List of `<li>` gene defNames.

**Genes - Categories (within `<genes>` list):**

- **Visual Genes:** Beard, Bodies, Brows, Ears, Eyes, Facial Overlay, Hands, Hair, Head, Horns, Jaw, Skin, Nose, Tail.
- **Stat Genes:** Abilities, Aggression, Aptitude, Beauty, Chemical Dependency, Fertility, Immunity, Learning, Libido, Melee Damage, Mood, Movement Speed, Pain, Psychic Ability, Sleep, Temperature, Toughness, Toxic Resistance, UV Sensitivity, Vision, Voice, Wound Healing.
- **Unique Genes:** Vampire (Hemogenic, etc.), Misc (FireResistant, Inbred, etc.).

**Adding Xenotypes to Pawns (FactionDef):**

```xml
<FactionDef>
  <xenotypeSet>
    <xenotypeChances>
      <XenotypeDefName MayRequire="DLC.PackageId">Chance/Weight</XenotypeDefName>
      </xenotypeChances>
    </xenotypeSet>
</FactionDef>
```

- `<xenotypeSet>`: Defines xenotype distribution for the faction.
- `<xenotypeChances>`: List of `<xenotypeDefName>Chance/Weight</xenotypeChances>`.
    - `<XenotypeDefName>`: defName of the XenotypeDef.
    - `MayRequire`: Optional DLC/Mod dependency.
    - `Chance/Weight`: Value interpreted as probability or weight.

**Interpreting `xenotypeChances`:**

1. **Probabilities (sum < 1.0):**
   - Values are percentage chances for each xenotype.
   - Remainder (1.0 - sum) is chance for Baseliner.

2. **Weights (sum >= 1.0):**
   - Values are weights.
   - Chance = (Xenotype Weight / Total Weights).
   - Baseliner excluded if not explicitly listed.

**Xenotype Chance Sources (Pawn Generation):**

- FactionDefs (`<xenotypeSet>`).
- PawnKindDefs (`<xenotypeSet>`).
- Memes (`<xenotypeSet>`).
- Summed during pawn generation.
- PawnKindDef Flag: `<useFactionXenotypes>false</useFactionXenotypes>` to ignore faction's xenotypeSet.
```
