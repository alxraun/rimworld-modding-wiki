## Component Pattern - Cheat Sheet

**Comp Design Pattern:** Modular functionality for RimWorld objects.

### Types of Components (Most Specific to Most Generic):

- **HediffComp:**
    - Behavior specific to Hediffs (health).
    - Simple to use.
    - **When to use:** Pawn's health functionality, Hediff-specific behavior.

- **ThingComp:**
    - Behavior for specific Things.
    - Powerful; data storage, special functionality.
    - Building block of RimWorld modding.
    - Fewer compatibility issues than custom classes.
    - **When to use:** Thing-level functionality, item behavior, data storage for Things.

- **WorldObjectComp:**
    - Behavior for WorldObjects.
    - Like ThingComp, but for WorldObjects.
    - **When to use:** WorldObject-level functionality.

- **MapComponent:**
    - Behavior at the Map level.
    - Tracks multiple things, data storage, managing entity.
    - Map-specific functionality.
    - **When to use:** Map-level tracking, managing multiple entities on a map.

- **WorldComponent:**
    - Behavior at the World level.
    - World-wide scope.
    - **When to use:** World-level tracking, game-world behavior.

- **GameComponent:**
    - Behavior at the Game level.
    - Instantiated when a new game starts (tutorial, scenario config, load from main menu).
    - Game-level scope.
    - **When to use:** Game-level tracking, game-session behavior.

- **StorytellerComp:**
    - Behavior for Storytellers.
    - Storyteller logic.
    - **When to use:** Storyteller behavior customization.

### Adding Components

**Adding Behavior to Thing Class:**
- For `ThingWithComps`, use `ThingComp`.
- `ThingComp`: Modular approach for data storage and functionality.
- **XML integration:**
  ```xml
  <comps>
    <li Class="YourNamespace.YourCompProperties"/>
  </comps>
  ```

**Adding Behavior to Hediff Class:**
- For `HediffWithComps`, use `HediffComp`.
- Similar to `ThingComp` but for Hediffs.
- **XML integration:**
  ```xml
  <comps>
    <li Class="YourNamespace.YourHediffCompProperties"/>
  </comps>
  ```

### Component Structure (ThingComp as example)

**C# classes:**
1. **CompProperties class** - defines static data and XML structure
   ```csharp
   public class YourCompProperties : CompProperties
   {
       public bool yourProperty = true;
       
       public YourCompProperties()
       {
           compClass = typeof(YourComp);
       }
   }
   ```

2. **Comp class** - contains logic and instance data
   ```csharp
   public class YourComp : ThingComp
   {
       public YourCompProperties Props => (YourCompProperties)props;
       
       // Override methods
       public override void CompTick() { /* your code */ }
       public override void PostSpawnSetup(bool respawningAfterLoad) { /* your code */ }
       public override void PostExposeData() { /* save data */ }
       public override IEnumerable<Gizmo> CompGetGizmosExtra() { /* UI elements */ }
   }
   ```

**Important methods available for override:**
- `PostSpawnSetup(bool)`: Called when created or loaded.
- `CompTick()`: Called every tick (1/60 sec) for active components.
- `CompTickRare()`: Called every 250 ticks for less frequent updates.
- `PostExposeData()`: For saving/loading data.
- `PostDrawExtraSelectionOverlays()`: For drawing overlays.
- `PostDestroy(DestroyMode, Map)`: Called when Thing is destroyed.
- `CompGetGizmosExtra()`: Adds buttons/icons to selected object.
- `CompInspectStringExtra()`: Adds information to object inspector.

### Comparison with Other Approaches

**ThingComp vs DefModExtension:**

**When to use ThingComp:**
- Need instance-specific data (varies for each Thing instance)
- Need to hook into tick/update events
- Need to add functionality to Things (items, pawns, buildings)
- Need to add UI elements (gizmos, inspectors)
- Need to save/load data with individual Things

**When to use DefModExtension:**
- Need global settings for all Things of one Def type
- Need to add data to non-Thing Defs (RecipeDef, ResearchDef, etc.)
- Need lightweight data storage without behavior
- Need to avoid Comp system overhead

**ThingComp vs Harmony:**

**When to use ThingComp:**
- Adding new functionality to existing Things
- Storing data for specific instances
- Modular extension of functionality
- Prioritizing compatibility with other mods

**When to use Harmony:**
- Changing existing game behavior
- Patching specific methods
- Global behavior changes not tied to specific objects
- Modifying game-wide algorithms

For detailed information on Harmony, refer to the Harmony Guide.

### Best Practices

- **Performance:** Use `CompTickRare()` instead of `CompTick()` when possible.
- **Code Clarity:** Follow Single Responsibility Principle, each Comp should do one specific thing.
- **Compatibility:** Check for other Comps when interacting, don't rely on specific load order.
- **SaveData:** Always override PostExposeData() to save instance variables.
- **Cleanup:** Release resources in PostDestroy() if necessary. 

