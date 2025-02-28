## Useful RimWorld Classes for Modders - Cheat Sheet

**Requirements:**
- Decompiler (ILSpy, dnSpy, Rider/dotPeek) for exploring game code.

**Class Categories:**

- **Gen:** (Classes starting with "Gen") - General utility, extension methods for:
    - `GenDate`, `GenLocalDate`: Time-related operations.
    - `GenDraw`: Drawing and graphics.
    - `GenAdj`: Grid and adjacency calculations.
    - `GenText`: Text manipulation (less recommended, use LINQ).

- **Utility:** (Classes ending with "Utility") - Task-specific utilities:
    - `FoodUtility`: Food consumption and handling.
    - `RestUtility`: Rest and sleep management.
    - `PawnUtility`: Pawn-related actions and checks.
    - `MassUtility`: Mass calculations.
    - `Area*`, `Roof*`, `Snow*`: Area, roof, and snow related utilities.
    - `AggressiveAnimalIncidentUtility`: Handles animal attacks (formerly ManhunterPackIncidentUtility).

- **Maker:** (Classes ending with "Maker") - Object creation and management:
    - Create: `Hediffs`, `Things`, `Lords`, `Zones`, `Sites`, `Filth`.

- **Tuning:** (Classes ending in "Tuning") - Game constants:
    - Reference: Game balance and tuning values (not for dynamic value access).

- **Find:** - Game state managers and singletons:
    - `LetterStack`: Letter UI management.
    - `Archive`: Game history and archiving.
    - `StoryWatcher`: Story event tracking.
    - `ResearchManager`: Research system management.
    - `World`: World state access.
    - `FactionManager`: Faction management.

- **PawnsFinder:** - Pawn lookup and filtering.

- **Map:** - Map-level data and managers:
    - Access: `ListerBuildings`, `ListerThings`, `LordManagers`, `ZoneManager`.
    - `MapMeshFlagDef`: Defines mesh flags for map rendering.
    - `MapMeshFlags`: Static properties for accessing mesh flags.

- **Pawn:** - Pawn data and components:
    - Access: `Health`, `Needs`, `Inventory`, `Jobs`, `Story`, `Equipment`.
    - **Pawn Rendering:**
        - `PawnRenderTree`: Node-based rendering system for pawns.
        - `PawnRenderNode`: Represents a node in the render tree.
        - `PawnRenderNodeWorker`: Handles rendering logic for nodes.
        - `PawnRenderNodeProperties`: Data defining how a node renders.
        - `PawnRenderTreeDef`: Defines a race-specific render tree structure.
        - `PawnRenderSubWorker`: Additive workers for mod compatibility.
        - `AnimationDef`: Defines pawn animations.

- **Thing:** - Base game object class.
    - Important methods:
        - `DynamicDrawPhaseAt(phase, drawLoc, flip)`: Entry point for parallel rendering.
        - `DrawAtNow(drawLoc, flip)`: Immediate rendering without parallelism.

- **DefDatabase:** - Def lookup and access:
    - Usage: `DefDatabase<DefType>.GetNamed("defName")`.

- **CellFinder:** - Map cell searching and filtering.

- **Command:** - UI command framework (like Gizmos).

- **Listing_Standard:** - Basic UI layout for settings and lists.

- **Gizmo:** - In-game UI elements (buttons, icons).

- **BoolUnknown:** - Nullable boolean type.

- **LoadedModManager:** - Mod loading and management:
    - Access: `LoadedModManager.RunningModsListForReading` (get loaded mods).

- **Pair<T1, T2>:** - Generic key-value pair (legacy, consider tuples).

- **Rand:** - Random number generation (use with care in multiplayer).

- **LudeonTK:** - Namespace for debug and utility classes (moved from Verse):
    - `DebugActionAttribute` (aka `[DebugAction]`)
    - `DebugActionNode`
    - `DebugActionType`
    - Various debug-related classes.

**Building and Item Classes:**
- `BuildingProperties`: Has `isAttachment` flag for wall-mounted items.
- `IHaulEnroute`: Interface for items that can be hauled by multiple pawns.
- `HiddenItemsManager`: Manages undiscovered items with the `<hiddenWhileUndiscovered>` tag.
- `Building_MultiTileDoor`: Class for ornate doors that span multiple tiles.

**Incident Worker Classes:**
- `IncidentWorker_AggressiveAnimals`: Handles animal attack incidents (formerly ManhunterPack).

**Important Namespace Locations:**
- `IngredientValueGetter`: Located in RimWorld namespace.
- Debug-related classes: Located in LudeonTK namespace.
