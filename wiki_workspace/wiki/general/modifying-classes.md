```markdown
## Modifying Classes - Cheat Sheet

**Changing Methods:**
- Use Harmony for method modification.

**Adding Fields/Methods to Classes:**
- Not directly possible (even with Harmony).
- Alternatives:
    - Subclassing.

**Adding Fields to Defs:**
- Use `DefModExtension`.
- Allows adding custom fields to any Def via XML.
- Access via `def.GetModExtension<ExtensionType>()`.

**Adding Behavior to Thing Class:**
- For `ThingWithComps`, use `ThingComp`.
- `ThingComp`: Modular, for data storage and functionality.

**Adding Behavior to Hediff Class:**
- For `HediffWithComps`, use `HediffComp`.
- Similar to `ThingComp` but for Hediffs.

**Overriding Non-Overridden Methods (Harmony):**
- Not possible directly with Harmony.
- Harmony patches require an existing method to patch.
- Alternatives:
    - Patch Base Class: Patch parent class and use `__instance is SubClass` check.
    - Subclass & Replace: Subclass target class, replace instantiation with Harmony.

**Patching Base Class of Subclass (Harmony):**
- Patch parent class method.
- Use `__instance is SubClass` to apply patch logic only to specific subclasses.
- Example:
  ```csharp
  if (!(__instance is SubClass))
      return;
  ```
```
