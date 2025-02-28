```markdown
## Custom Comp Classes - Cheat Sheet

**Comp Design Pattern:** Modular functionality for RimWorld objects.

**Types of Components (Most Specific to Most Generic):**

- **HediffComp:**
    - Behavior specific to Hediffs.
    - Simple.

- **ThingComp:**
    - Behavior for specific Things.
    - Powerful; data storage, special functionality.
    - Building block of RimWorld modding.
    - Fewer compatibility issues than custom classes.

- **WorldObjectComp:**
    - Behavior for WorldObjects.
    - Like ThingComp, but for WorldObjects.

- **MapComponent:**
    - Behavior at the Map level.
    - Tracks multiple things, data storage, managing entity.
    - Map-specific functionality.

- **WorldComponent:**
    - Behavior at the World level.
    - World-wide scope.

- **GameComponent:**
    - Behavior at the Game level.
    - Instantiated when a new game starts (tutorial, scenario config, load from main menu).
    - Game-level scope.

- **StorytellerComp:**
    - Behavior for Storytellers.
    - Storyteller logic.

**Which Comp to Use:**

- **HediffComp:** Pawn's health, Hediff-specific behavior.
- **ThingComp:** Thing-level functionality, item behavior, data storage for Things.
- **WorldObjectComp:** WorldObject-level functionality.
- **MapComponent:** Map-level tracking, managing multiple entities on a map.
- **WorldComponent:** World-level tracking, game-world behavior.
- **GameComponent:** Game-level tracking, game-session behavior.
- **StorytellerComp:** Storyteller behavior customization.
```
