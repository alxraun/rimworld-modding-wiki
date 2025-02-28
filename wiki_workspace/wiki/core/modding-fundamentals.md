## Modding Fundamentals - Cheat Sheet

**What are Mods?**
- Player-made modifications.
- Alter game functionality: add, change, remove content.
- Items, textures, sounds, mechanics, etc.
- Enhance or change gameplay.

**Types of Mods:**
- **Cosmetic:** Visual changes only (textures, styles). No gameplay impact.
    - Examples: Hairstyles, clothing, color variations.
- **User Interface (UI):** Quality of life improvements. Simplify game interaction.
    - Examples: Expanded inventory, medical lists, planning overlays.
- **Game Changes:** Alter core gameplay, add complexity, new mechanics.
    - Examples: New items, weapons, animals, mechanics, overhauls.
    - May affect game balance.

**Acquiring Mods:**
- **Steam Workshop:** (Steam version) - Easy download, auto-updates.
- **Ludeon Forums:** Manual download.
- **Third-party websites (GitHub, Nexus Mods etc):** Manual download, use with caution.

**Installing Mods:**
- **Steam Workshop:** "Subscribe" - Steam handles installation & updates.
- **Manual Installation:**
    1. Download mod files (ZIP).
    2. Extract to `RimWorld/Mods/` directory.
        - Windows: `C:\Program Files (x86)\Steam\steamapps\common\RimWorld\Mods`
        - Mac: `~/Library/Application Support/Steam/steamapps/common/RimWorld/RimWorldMac.app/Mods`
        - Linux (standalone): `~/.steam/steam/steamapps/common/RimWorld/Mods`
        - Linux (GoG): `/home/<user>/GOG/Games/RimWorld/game/Mods/`
    3. Activate in-game: Main Menu -> Mods.
    4. Restart RimWorld.

**Selecting Mods:**
- Player choice: Based on preferences and play style.
- No "right" way to select mods.
- Mod Compatibility: Large mod collections may have conflicts.
- Mod Types: Cosmetic, UI, Game Changes (balance varies).

**Modding Goals & Approaches:**

- **Add a new Thing:**
    - XML `ThingDef` entry.
    - Start by duplicating & modifying existing `ThingDef` from Core XML.
    - For similar Things: Use abstract `ThingDef` as `ParentName` for inheritance.
    - For buildings/plants: Ensure `category` tag is correctly set.

- **Create New Functionality:**
    - C# code for new behaviors.
    - Apply C# to Things via XML.
    - New Functionality Only: C# class.
    - Thing with New Functionality:
        - C# class inheriting `Thing`.
        - `ThingDef` XML entry with `<thingClass>YourNamespace.YourNewClass</thingClass>`.
    - Thing with New XML Properties:
        - C# class inheriting `ThingDef` for new XML fields.
        - XML `ThingDef` with `Class="YourNamespace.YourThingDef"`.
        - OR: `ThingComp` for existing `ThingWithComps`, define `CompProperties`, XML with `Class="YourNameSpace.MyCompProperties"`.

- **Alter RimWorld Code:**
    - XML (Defs): `PatchOperations` to modify XML Defs (power consumption, plant locations).
    - C# (Methods): `Harmony` for code injection (prefixes, postfixes, transpilers).

**Key Considerations:**
- Mod Sources: Steam Workshop (easiest), Forums, Third-Party (manual).
- Manual Install Location: `RimWorld/Mods/`.
- Activation: In-game Mods menu, restart required.
- Compatibility: Potential conflicts with large mod lists or overlapping mods.
- Mod Types Impact: Cosmetic (visuals), UI (interface), Game Changes (mechanics). 
