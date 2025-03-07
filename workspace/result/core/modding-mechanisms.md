## Modding Mechanisms Cheat Sheet

### 1. XML Defs

**Purpose:** Define game content (things, recipes, factions, etc.) declaratively in XML files.

**Structure:**
- Files located in `Defs/` folder within a mod.
- Root element: `<Defs>`.
- Each Def is defined by a `<DefType>` tag (e.g., `<ThingDef>`, `<RecipeDef>`).
- Tags within `<DefType>` correspond to public fields in the associated C# Def class.
- XML files must start with `<?xml version="1.0" encoding="utf-8"?>`.

**Inheritance:**
- `Abstract="True"` in `<Def Name="ParentName" Abstract="True">` defines a template Def, not loaded directly.
- `ParentName="ParentDefName"` in `<Def ParentName="ParentDefName">` inherits from a parent Def.
- Child Defs inherit and can override parent Def properties.
- `Inherit="False"` on a tag in a child Def prevents inheritance for that specific tag.

**Custom Defs:**
- Create new Def types by subclassing existing Def classes in C#.
- Link custom Def classes in XML using the `Class` attribute: `<ThingDef Class="YourNamespace.YourCustomDefClass">`.
- Access custom fields in C# by casting: `var customDef = thing.def as YourNamespace.YourCustomDefClass;`.

**Accessing Defs in C#:**
- **DefOf:** Static class with `[DefOf]` attribute, auto-populated static fields for direct Def access (e.g., `MyDefOf.ThingDefName`). Requires `DefOfHelper.EnsureInitializedInCtor`.
- **DefDatabase:** Use `DefDatabase<DefType>.GetNamed("defName")` for runtime Def lookup.

### 2. C# Components

**Purpose:** Add modular functionality to game objects (Things, Hediffs, Maps, Worlds, Games).

**Types:**
- **ThingComp:** For `ThingWithComps`, adds behavior and data to specific Things.
- **HediffComp:** For `HediffWithComps`, specific to Hediffs (health effects).
- **MapComponent:** Map-level behavior and data management.
- **WorldComponent:** World-level scope, game world behavior.
- **GameComponent:** Game-level scope, game session behavior.
- **StorytellerComp:** Customizes Storyteller behavior.

**Structure:**
- **CompProperties class:** Defines static data and XML structure, inherits from `CompProperties`, sets `compClass` to the Comp class type.
- **Comp class:** Contains logic and instance data, inherits from `ThingComp`, `HediffComp`, etc.
- Access `CompProperties` via `Props` property in Comp class: `YourCompProperties Props => (YourCompProperties)props;`.

**Adding Components in XML:**
- For `ThingWithComps` and `HediffWithComps`, use `<comps>` tag in Def XML.
- Add `<li>` with `Class="YourNamespace.YourCompProperties"` within `<comps>`.

**Key Methods (override in Comp class):**
- `PostSpawnSetup(bool respawningAfterLoad)`: Called when Thing/Hediff spawns or loads.
- `CompTick()`: Called every game tick (1/60 sec) for active components.
- `CompTickRare()`: Called every 250 ticks for less frequent updates.
- `PostExposeData()`: For saving and loading component data.
- `CompGetGizmosExtra()`: Adds gizmos (buttons) to the object's UI.

**Accessing Components in C#:**
- `thing.Map.GetComponent<MyMapComponent>()` (for MapComponent).
- `Find.World.GetComponent<MyWorldComponent>()` (for WorldComponent).
- `Current.Game.GetComponent<MyGameComponent>()` (for GameComponent).
- `thing.GetComp<YourComp>()` (for ThingComp).
- `hediff.TryGetComp<YourHediffComp>()` (for HediffComp).

### 3. Harmony Patching

**Purpose:** Modify existing game code at runtime without changing source code, for compatibility and behavior alteration.

**Patch Types:**
- **Prefix:** Executes code *before* the original method. Can skip original method execution by returning `false` (use with caution).
- **Postfix:** Executes code *after* the original method, guaranteed execution. Can modify the original method's return value via `ref __result`.
- **Transpiler:** Directly manipulates the method's IL code for complex modifications.

**Bootstrapping:**
- Initialize Harmony in a class with `[StaticConstructorOnStartup]` attribute for early patching.
- Use `Harmony.PatchAll()` to automatically patch all methods with Harmony attributes in the assembly, or use manual patching.

**Method Targeting:**
- Use `AccessTools.Method(typeof(ClassName), nameof(MethodName))` to get `MethodInfo` for patching, especially for overloaded methods.

**Patch Methods:**
- Static methods with specific parameters: `__instance`, `__params`, `__result`, `__exception`, `__originalMethod`.
- Prefix methods can return `bool` to skip original execution.
- Postfix methods can modify `ref __result`.

**Key Considerations:**
- Avoid overuse; consider alternatives like Comps or subclassing first.
- Check Harmony logs (`HarmonyInstance.DEBUG = true`) for patching issues.
- Be aware of potential inlining of patched methods.

### 4. Subclassing Defs

**Purpose:** Create specialized Def types with extended functionality and data, inheriting from existing Def classes.

**Implementation:**
- Create a new C# class that inherits from an existing Def class (e.g., `class MyCustomThingDef : ThingDef`).
- Add custom fields and properties to the subclass.
- In XML, use the `Class` attribute in the Def definition to link to the custom subclass: `<ThingDef Class="YourNamespace.MyCustomThingDef">`.

**Usage:**
- For complex Def modifications where adding Comps or DefModExtensions is insufficient.
- To introduce new XML tags corresponding to the custom fields in the subclass.
- Access custom fields in C# by casting the `def` to the custom subclass type.

**Considerations:**
- Can introduce compatibility issues if multiple mods subclass the same Def type.
- Less flexible than using Comps for adding behavior.
- Requires casting to access subclass-specific members.

### 5. XML Patching

**Purpose:** Modify existing XML Defs without overwriting them, ensuring mod compatibility.

**Mechanism:**
- Use XML patch files located in the `Patches/` folder of a mod.
- Patch files contain `<Patch>` root element and `<Operation>` tags for modifications.
- Target XML nodes using XPath expressions.

**XPath Basics:**
- `Defs/DefType[predicate]/nodeToTarget` to select nodes.
- `DefType`: Type of Def (e.g., `ThingDef`, `RecipeDef`).
- `[predicate]`: Conditions for selection (e.g., `[defName="Wall"]`).
- `nodeToTarget`: Tag to modify (e.g., `statBases`).

**PatchOperation Types:**
- **PatchOperationAdd:** Adds a child node.
- **PatchOperationInsert:** Inserts a sibling node.
- **PatchOperationRemove:** Removes a node.
- **PatchOperationReplace:** Replaces a node.
- **PatchOperationAttributeAdd/Set/Remove:** Modify XML attributes.
- **PatchOperationSequence:** Executes a list of operations sequentially.
- **PatchOperationConditional:** Executes operations conditionally based on XPath test.
- **PatchOperationFindMod:** Executes operations conditionally based on mod/DLC presence.
- **PatchOperationAddModExtension:** Adds a `DefModExtension` to a Def.

**Example Structure:**
```xml
<Patch>
  <Operation Class="PatchOperationReplace">
    <xpath>Defs/ThingDef[defName="Gun_AssaultRifle"]/verbs/li/defaultProjectile</xpath>
    <value>
      <defaultProjectile>Bullet_Explosive</defaultProjectile>
    </value>
  </Operation>
</Patch>
```

**Key Considerations:**
- XPath is case-sensitive.
- Use XML validators to ensure correct syntax.
- Test patches incrementally and check `Player.log` for errors.
- Prioritize patching over overwriting Defs for better mod compatibility.
