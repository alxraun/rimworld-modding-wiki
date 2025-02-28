```markdown
## Modifying Defs - Cheat Sheet

**Methods for Modifying Defs:**

- **1. Overwriting Defs (XML)**
    - **Pros:** Easiest method.
    - **Cons:** Incompatible with other mods, not recommended.
    - **When to Use:** Never, except for personal use and not sharing.

- **2. XPath Changes (XML Patches)**
    - **Pros:** Highly specific, very compatible, non-destructive.
    - **Cons:** Limited to XML-defined Defs, complex operations can be tricky.
    - **When to Use:** For most XML value changes, general compatibility.

- **3. Adding a (Self-Made) Comp (C#)**
    - **Pros:** Flexible, well-supported, highly compatible, adds functionality.
    - **Cons:** Not applicable to all Def types.
    - **When to Use:** Adding new behaviors, non-static data, or functionality to ThingWithComps.

- **4. DefModExtensions (C#)**
    - **Pros:** Simple, lightweight, highly compatible, adds data fields.
    - **Cons:** For static data only.
    - **When to Use:** Adding custom data fields to existing Defs.

- **5. Subclassing Defs (C#)**
    - **Pros:** Powerful, extends Def functionality.
    - **Cons:** Compatibility issues, less flexible than Comps, casting required.
    - **When to Use:** Complex Def modifications where Comps/DefModExtensions are insufficient.

- **6. Custom Defs (C#)**
    - **Pros:** Full control, no compatibility issues within your mod.
    - **Cons:** Implementation from scratch, more work.
    - **When to Use:**  Creating entirely new Def types unique to your mod.

- **7. Checking for Tags (XML/C#)**
    - **Pros:** Lightweight, easy for simple features and compatibility.
    - **Cons:** Hacky, risk of side effects, less robust.
    - **When to Use:** Simple, lightweight feature checks or cross-mod compatibility flags.

- **8. Changing Def Class (XML Patches)**
    - **Pros:** Easy, retains most original Def values.
    - **Cons:** Compatibility issues with Harmony patches targeting original class.
    - **When to Use:** Specific class behavior replacement, consider compatibility impacts.

- **9. Harmony Patching (C#)**
    - **Pros:** Highly flexible for code-level changes.
    - **Cons:** Overuse can be complex, alternatives often better suited.
    - **When to Use:** Modifying game code execution, consider alternatives first.

**Key Considerations:**

- **Compatibility:** Prioritize XPath, Comps, and DefModExtensions for best mod compatibility.
- **Functionality:** Comps for behavior, DefModExtensions for data.
- **Complexity:** Subclassing and Custom Defs for advanced, unique modifications.
- **Performance:** DefOf for optimized Def access in C#.
- **Error Handling:** XML errors cascade; fix the first error first.
```
