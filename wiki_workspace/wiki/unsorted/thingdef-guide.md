```markdown
## ThingDef - Cheat Sheet

**Purpose of ThingDef:**

- Defines properties and behavior of in-game "things" (items, buildings, pawns, projectiles, etc.).
- Backed by `ThingDef` C# class.
- Configured via XML in Def files.

**Learning Tag Meanings:**

- **Challenge:** 200+ tags, limited official documentation.
- **Method:** Decompilation + Code Analysis.
    1. **Identify Tag:** Find tag in XML (e.g., `<intricate>`).
    2. **Decompile:** Use a decompiler (ILSpy, dnSpy).
    3. **Search:** Search decompiler for tag name (e.g., "intricate").
    4. **Analyze Usage:** Analyze code using tag (e.g., `Thing.SmeltProducts`).
    5. **Infer Meaning:** Deduce tag's function from code context (e.g., `intricate` affects smelting products).

**Example: `intricate` Tag**

- **XML Usage:**
  ```xml
  <ThingDef>
      <defName>ComponentIndustrial</defName>
      <intricate>true</intricate>
  </ThingDef>
  ```
- **Analysis:**
    - Used in `ComponentIndustrial` and `AdvancedComponent` ThingDefs.
    - Read by `PlayerItemAccessibilityUtility.CacheAccessibleThings` and `Thing.SmeltProducts`.
    - `SmeltProducts` checks `intricate` to determine smelting output.
- **Meaning:** Indicates item is lost upon smelting (not recoverable).

**Adding More Tags (Custom Fields):**

- **Method:** Use `DefModExtension` (C#).
- **Benefits:** Compatibility, modularity.
- **Alternative to:** Directly modifying `ThingDef` class (complex, compatibility issues).

**Key Takeaway:**

- `ThingDef` defines core properties of in-game objects.
- Decompilation and code analysis are essential for understanding tag behavior.
- `DefModExtension` is recommended for adding custom data/functionality to ThingDefs.
- No comprehensive tag documentation exists - rely on code analysis and community resources.
```
