```markdown
## Custom Comp Classes - Cheat Sheet

**Comp Design Pattern:**
- Modular approach to add functionality to RimWorld objects.
- Not a formal code concept, but a design pattern.

**Types of Components:**

- **HediffComp:**
    - For Hediff-specific behavior.
    - Relatively simple.

- **ThingComp:**
    - For Thing-specific functionality.
    - Powerful, versatile.
    - Data storage, Thing functionality.
    - Building block of RimWorld modding.
    - Fewer compatibility issues than custom classes.

- **WorldObjectComp:**
    - ThingComp equivalent for WorldObjects.

- **MapComponent:**
    - Map-level functionality.
    - Tracks multiple things on a map.
    - Data storage at map level.
    - Map-level management or coordination.

- **WorldComponent:**
    - World-level functionality.
    - Similar to MapComponent but for the World.

- **GameComponent:**
    - Game-level functionality.
    - Instantiated when a new game starts (tutorial, scenario config, load from main menu).
    - Game-level data storage and management.

- **StorytellerComp:**
    - Storyteller behavior customization.
    - Specific to storyteller logic.

**Which Comp to Use:**

- Pawn's health-related? `HediffComp`.
- Thing-level functionality? `ThingComp`.
- Map-level functionality (multiple things, map management)? `MapComponent`.
- World-level functionality? `WorldComponent`.
- Game-level functionality (new game scope)? `GameComponent`.
- Storyteller behavior? `StorytellerComp`.

**Key Considerations:**

- Choose the most specific Comp type for functionality scope.
- Comps offer modularity and reduce compatibility issues compared to full custom classes.
- GameComponents instantiate at game start; WorldComponents at world level; MapComponents at map level; ThingComps at Thing level; HediffComps at Hediff level.
```
