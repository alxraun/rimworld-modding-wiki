## Node-Based Pawn Rendering System

**Overview:**
RimWorld uses a node-based pawn rendering system that provides a flexible framework for drawing pawns and their apparel, weapons, and other visual elements. This guide explains the core concepts and components for modders who need to work with the pawn rendering system.

### Core Components:

1. **Render Tree Def (`PawnRenderTreeDef`):**
   - Data-only object that defines how a specific race of pawns should be drawn
   - Contains a "root" node with properties and its children in a recursive structure
   - Set in RaceProperties via the `<renderTree>` tag

2. **Render Node Properties (`PawnRenderNodeProperties`):**
   - Data-only objects used in XML to describe rendering details
   - Can contain extra details specific to certain node types
   - Can be created dynamically in code

3. **Render Node (`PawnRenderNode`):**
   - Creates meshes and materials for rendering a piece of a pawn
   - Functions called when a pawn's graphics are dirtied
   - Can have child nodes that inherit state (offset, rotation, scale, active, etc.)
   - Core features:
     - Initializes meshes and materials for parallel rendering
     - Provides shortcuts to the attached render node worker
     - Gets colors, shaders, textures, and meshes for rendering
     - Creates graphics used by workers

4. **Render Node Worker (`PawnRenderNodeWorker`):**
   - Functions called each time a pawn is drawn
   - Stateless; code written to be performant and run each frame
   - Only one instance of each worker type exists
   - Core features:
     - Decides if node should be drawn
     - Handles pre-draw and post-draw events
     - Determines offset, rotation, scale, pivot, and layer
     - Gets final materials
     - Appends draw requests to the final list

5. **Render Sub Worker (`PawnRenderSubWorker`):**
   - Additive workers for mod compatibility
   - Similar functionality to Render Node Workers
   - Useful when mods need to modify specific aspects of rendering

### Animations:

- **Animation Def (`AnimationDef`):**
  - Defines a dictionary of render node tags to animation parts
  - Set in RaceProperties via the `<startingAnimation>` tag

- **Animation Parts:**
  - Can define keyframes OR a custom worker class:
    - **Keyframes:** Define offset, angle, scale, and tick offset from start
    - **Animation Worker:** Custom class with methods for calculating values at current tick
  - Can define a pivot point for rotation
  - Started via `SetAnimation` method in pawn renderer
  - **Note:** Animations disable pawn graphic caching (performance impact with many animated pawns)

### Dynamic Render Nodes:

- Render trees can be built dynamically at runtime
- `PawnRenderNodeTagDef` allows easier referencing of specific parts
- Apparel, hediffs, traits, and genes can add nodes to a pawn

### Parallel Drawing & Dynamic Draw Phases:

RimWorld implements parallel rendering in three distinct phases:

1. **EnsureInitialized:**
   - Called on the main thread
   - Prepare render node trees for parallel rendering
   - Create materials and meshes needed for rendering
   - Most initialization code runs once when graphics are dirtied

2. **ParallelPreDraw:**
   - Runs off the main thread
   - Handles expensive calculations (offsets, matrices, etc.)
   - Game state won't change during this phase
   - Cannot create/access materials and meshes here

3. **Draw:**
   - Final phase on the main thread
   - Iterates over previously created draw requests
   - Renders final graphics on screen
   - Calls any required events

### Drawing Methods:

| Method | Purpose |
|-----------------|-------------|
| `DrawAt(drawLoc, flip)` | Standard drawing method |
| `DrawAtNow(drawLoc, flip)` | Immediate rendering without parallelism |
| `DynamicDrawPhaseAt(phase, drawLoc, flip)` | Entry point for parallel rendering |

- **Key Points:**
  - Override `DrawAt()` to customize or prevent drawing
  - Use `DrawAtNow()` for immediate rendering needs
  - Implement `DynamicDrawPhaseAt()` for performance-critical rendering

### Migration Guide for Modders:

1. **If you were previously using direct drawing approaches:**
   - Override `DrawAt()` instead of older methods
   - Use the provided `drawLoc` parameter instead of `DrawPos`
   - Call `Comps_PostDraw()` at the end if overriding in a `ThingWithComps` subclass

2. **For pawn rendering customization:**
   - Use the node-based system instead of direct modifications
   - Create custom `PawnRenderNodeWorker`/`PawnRenderSubWorker` classes
   - Define `PawnRenderNodeProperties` in XML for your custom rendering needs
   - Use `PawnRenderNodeTagDef` to reference existing nodes

3. **For parallel rendering:**
   - Override `DynamicDrawPhaseAt()` for maximum performance
   - Handle the three phases appropriately (initialization, pre-draw, draw)
   - When drawing contained pawns, route the phase to them

### Documentation References:

For more detailed information, refer to the [official documentation from Ludeon](https://docs.google.com/document/d/e/2PACX-1vRpvIu49_CpHFMCk-8vLuzIsLqbFFDnf0DNFlYRpwAYXjNlYH6ZudPoox7Gj6K8wuCLPDCWkWEGARrO/pub). 
