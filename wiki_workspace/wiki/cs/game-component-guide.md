## GameComponent, WorldComponent, MapComponent - Cheat Sheet

**General Component Info:**

- **Purpose:** Add custom code and functionality at different levels (Game, World, Map).
- **Implementation:** Inherit from `GameComponent`, `WorldComponent`, or `MapComponent`.
    - Implement constructor: `public MyComponent(TYPE type) : base(type) { }` (TYPE = Game, World, or Map).
- **Access:** Use `GetComponent<MyComponent>()` from:
    - `Current.Game` (GameComponent)
    - `Find.World` (WorldComponent)
    - `Find.CurrentMap` or `thing.Map` or `thing.MapHeld` (MapComponent)
- **Benefits:**
    - Well-supported by RimWorld.
    - Save-game compatible (generally).
    - Global level functionality.
    - Data saving (`ExposeData`).
    - RimWorld-driven method calls (e.g., `Update`, `Tick`).
    - Versatile functionality.
    - Always accessible.
- **Downsides:**
    - Removing mod: Harmless one-time error on load.
    - No XML-exposed functionality.

**Accessing Components:**

- **MapComponent:**
    ```csharp
    thing.Map.GetComponent<MyMapComponent>();         // From a Thing on map
    thing.MapHeld.GetComponent<MyMapComponent>();     // From ThingHolder
    Find.CurrentMap.GetComponent<MyMapComponent>();  // From visible map

    // Safe access method:
    public static MyMapComponent GetMapComponentFor(Map map) { /* null checks & instantiation */ }
    ```

- **WorldComponent:**
    ```csharp
    Find.World.GetComponent<MyWorldComponent>();
    ```

- **GameComponent:**
    ```csharp
    Current.Game.GetComponent<MyGameComponent>();
    ```
    - **Gotcha:** Game component constructor needs `Game` parameter.

**Working with Pawn Lists:**
- Use standard `List<Pawn>` for saving pawn references in components
  ```csharp
  public List<Pawn> savedPawns = new List<Pawn>();
  ```
- Pawn lists are automatically serialized when saved in ExposeData

**Overridable Methods:**

- **TYPEComponentUpdate:** Frame update, even when paused. Use for frame-rate dependent updates (visuals), not game logic.
- **TYPEComponentTick:** Game tick update. Game logic updates.
- **TYPEComponentOnGUI:** Frame update, when TYPE is visible. GUI rendering (not WorldComponent).
- **ExposeData:** Save/load component data.
- **FinalizeInit:** Called after TYPE instantiation, after constructor. Good for initialization, safer than constructor for error handling.
- **StartedNewGame:** GameComponent only. On new game start.
- **LoadedGame:** GameComponent only. On game load.
- **MapGenerated:** MapComponent only. On map generation.
- **MapRemoved:** MapComponent only. On map removal.

**Usage Notes:**

- Null checks for `Map` are crucial, especially for MapComponents.
- Choose component type based on scope (Game > World > Map > Thing).
- Override methods as needed for desired behavior.
- For data persistence, implement `ExposeData`.
