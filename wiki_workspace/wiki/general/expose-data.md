## RimWorld Saving (ExposeData) - Cheat Sheet

**Core Concept:**
- `ExposeData()` method (override virtual) in classes like `Thing`, `ThingComp`, `HediffComp`, `LordJob`, `JobDriver`.
- `Scribe` methods within `ExposeData()` for saving individual values.

**General Scribe Method Parameters:**
- `ref item`: Reference to the variable to save (passed by `ref`).
- `"label"`: Unique save key (string, appears in save file).
- Optional parameters: Default value, force save, constructor arguments, etc.

**Saving Data Types:**

- **Simple Values (Scribe_Values.Look):**
    - Types: `int`, `float`, `bool`, `long`, `double`, `sbyte`, Enums, `string`, `System.Type`, Vector Types, `Rect`, `Color`, `ColorInt`, `Rot4`, `CellRect`, `CurvePoint`, `FloatRange`, `IntRange`, `QualityRange`, `PublishedFileId_t`.
    - Parameters: `ref value`, `"label"`, `defaultValue` (optional, for loading defaults), `forceSave` (optional, boolean).
    - Example: `Scribe_Values.Look(ref foo, "foo", 10);` // int foo, default 10 if no save.

- **Defs (Scribe_Defs.Look):**
    - For: Saving references to Defs.
    - Parameters: `ref defField`, `"label"`.
    - Example: `Scribe_Defs.Look(ref myDef, "myDef");`

- **Things (Scribe_Deep.Look, Scribe_References.Look):**
    - Base class: `Thing` (Pawn, Building, Mote, Plant, etc.).
    - Two Save Methods:
        - `Scribe_References.Look`: For `ILoadReferenceable` Things (spawned, saved elsewhere).
        - `Scribe_Deep.Look`: For `IExposable` Things (not spawned, not saved elsewhere).

    - **Saving IExposable Things (Scribe_Deep.Look):**
        - For: `IExposable` implementing classes.
        - Parameters: `ref exposableItem`, `saveDestroyedThings` (optional, boolean), `"label"`, `ctorArgs` (optional, constructor arguments for instantiation).
        - Example: `Scribe_Deep.Look(ref myExposable, "myExposable");`

    - **Saving ILoadReferenceable Things (Scribe_References.Look):**
        - For: `ILoadReferenceable` implementing classes (existing in world).
        - Parameters: `ref referenceableItem`, `"label"`, `saveDestroyedThings` (optional, boolean).
        - Example: `Scribe_References.Look(ref myReference, "myReference");`

- **TargetInfo (Scribe_TargetInfo.Look):**
    - For: `TargetInfo`, `LocalTargetInfo`, `GlobalTargetInfo`.
    - Parameters: `ref targetInfo`, `"label"`, `saveDestroyedThings` (optional, boolean), `defaultValue` (optional, TargetInfo).
    - Example: `Scribe_TargetInfo.Look(ref myLocation, "target");`

- **Collections (Scribe_Collections.Look):**
    - For: `List`, `Dictionary`, `HashSet`, `Stack`.
    - Required: Specify `LookMode` for collection elements.
    - Dictionaries: If key or value LookMode is `Reference`, provide `ref` lists for keys/values.
    - Parameters: `ref collection`, `"label"`, `LookMode elementLookMode`, (optional parameters for dictionaries).
    - Examples:
        - `Scribe_Collections.Look(ref list1, "list1", LookMode.Value);` // List<string>
        - `Scribe_Collections.Look(ref dict1, "dict1", LookMode.Value, LookMode.TargetInfo);` // Dictionary<string, LocalTargetInfo>
        - `Scribe_Collections.Look(ref dict2, "dict2", LookMode.Reference, LookMode.Value, ref list2, ref list3);` // Dictionary<Faction, int> with ref lists for keys and values
        - `Scribe_Collections.Look(ref stack1, "stack1", LookMode.Reference);` // Stack<Thing>

**Important Notes:**
-  `label` parameter must be unique within the `ExposeData()` method.
-  For `Scribe_Values.Look`, default value is set during loading if save key is missing.
-  For `Scribe_Deep.Look`, constructor arguments can be passed using `ctorArgs`.
-  `Scribe_References.Look` requires the object to be saved elsewhere.
-  Collections require specifying `LookMode` for elements. Dictionaries might need additional lists for saving.
