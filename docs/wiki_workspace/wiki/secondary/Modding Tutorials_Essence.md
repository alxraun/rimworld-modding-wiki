```markdown
## Modding Essence - Cheat Sheet

**Modding Difficulty:**

- **Easy:** XML editing for new Things (weapons, animals) using existing functionality.
- **Easy (Artist):** Create & integrate art/sound assets via XML.
- **Moderate:** C# for simple behaviors, integrated via XML.
- **Harder:** GUI programming.
- **Hard:** Complex inter-object behaviors.
- **Hard:** Custom AI behaviors.

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

**Essence of Modding:**

- Learning by doing and referencing existing mods/Core game.
- Modding = Connecting "RimWorld does X, can I do Y?" -> Searching & Implementing.
- Start modding to understand RimWorld deeply.
- Copy-paste & Reinventing the wheel are normal practices.
- No need to know everything upfront.
```
