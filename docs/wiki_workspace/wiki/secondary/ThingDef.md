```markdown
## ThingDef - Cheat Sheet

**Purpose:** Define properties of almost all in-game "things" (tangible & intangible).

**Location:** XML files in `/Defs` folder.

**Learning Tag Meanings:**

- **Decompiler (Recommended):**
    - Use a decompiler (ILSpy, dnSpy) to inspect `ThingDef` C# class.
    - Search for XML tag names in decompiled code.
    - Analyze code usage to understand tag effects.
    - Example: Search "intricate" to understand `<intricate>` tag.

**Finding Tag Meaning Example (`<intricate>`):**

1. **Identify Tag:**  `<intricate>`.
2. **Decompile & Search:** Search decompiler for "intricate".
3. **Analyze Code Usage:**
    - `PlayerItemAccessibilityUtility.CacheAccessibleThings`
    - `Thing.SmeltProducts` -> determines smelting products.
4. **Deduce Meaning:** `<intricate>true</intricate>` for `ThingDef` = item is lost upon smelting (e.g., Components).

**General Approach for Unknown Tags:**

1. **Identify XML Tag:** (e.g., `<DeteriorationRate>`).
2. **Text Editor (Find in Files):** Search for tag name in RimWorld `Data` and mod folders to see examples and usage contexts.
3. **Decompiler (Analyze):** Search and analyze tag name in decompiled C# code to understand its function and effects.

**Adding More Tags (Beyond Vanilla):**

- **DefModExtensions (Recommended):** Use `DefModExtension` (C#) to add custom fields to `ThingDef` and other Defs for maximum compatibility.

**Key Takeaways:**

- `ThingDef` XML structure defines game object properties.
- No complete, official tag documentation exists.
- Decompilers are essential for understanding tag behavior.
- Analyze code usage to infer XML tag meanings and effects.
- Use `DefModExtensions` (C#) to add custom functionality and data.
```
