## XML and C# Integration - Cheat Sheet

### Using C# from XML

**A. Classy Design Pattern:**
- RimWorld exposes Def classes in XML.
- Look for tags like `workerClass`, `thingClass`.
- Example:
  ```xml
  <BiomeDef>
    <workerClass>BiomeWorker_BorealForest</workerClass>
  </BiomeDef>
  ```

**B. "Classy" Pattern (Class Attribute Override):**
- Overwrite default Def class or field class using `Class` attribute.
- Useful for Components, GenSteps, etc.
- Example:
  ```xml
  <comps>
    <li Class="CompProperties_Forbiddable"/>
  </comps>
  ```
  ```xml
  <GenStepDef Class="MyNamespace.MyCustomGenStepDefClass">
  ```
- **Note:** Use DefModExtensions for adding fields to Defs instead of overwriting Def classes.

**C. Defining Custom Types (Self-Defined):**
- Supply custom C# type directly in XML, e.g., AlienRace.
- Example:
  ```xml
  <AlienRace.ThingDef_AlienRace ParentName="BasePawn">
    <alienRace>
      </alienRace>
  </AlienRace.ThingDef_AlienRace>
  ```

**D. Serializing Custom Classes:**
- `LoadDataFromXmlCustom` method in C# class.
- RimWorld uses this method to parse XML for your custom type.
- Limitations: Doesn't work for structs (no constructor).
- Example:
  ```csharp
  public class RotR_cost
  {
      public void LoadDataFromXmlCustom(XmlNode xmlRoot) { ... }
  }
  ```

**E. Common Issues:**
- Namespace.Class must be exact.
- Assembly containing the type must be supplied.

### Using XML from C#

**A. DefOf:**
- Annotated class `[DefOf]` auto-fills static fields with Defs.
- Access Defs directly: `MyDefOf.JustOneExampleDefName`.
- **Pros:**
    - Runtime fast.
    - Easy to use.
    - Startup error detection (early warnings).
- **Cons:**
    - Direct XML referencing (less flexible to XML changes).
    - DefOf fields are `null` until initialized (startup issues).
    - Minor negative startup time impact.
- Example:
  ```csharp
  [DefOf]
  public static class MyDefOf
  {
      public static SomeDef JustOneExampleDefName;
      static MyDefOf() { DefOfHelper.EnsureInitializedInCtor(typeof(MyDefOf)); }
  }
  ```

**B. DefDatabase:**
- Look up Defs directly using `DefDatabase<DefType>.GetNamed("defName")`.
- Example:
  ```csharp
  SomeDef def = DefDatabase<SomeDef>.GetNamed("JustOneExampleDefName");
  ```
- **Pros:**
    - Null return allowed (silent failure).
    - No need for extra class.
    - String-based flexibility for dynamic Def lookup.
- **Cons:**
    - Null return allowed (potential runtime errors if not handled).
    - Slower than DefOf (negligible if cached in static variable).
    - Typo-prone (string-based defName).

### Modifying Classes

**Changing Methods:**
- Use Harmony for method modification.
- For details on Harmony usage, see the Harmony Guide.

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

**Overriding Non-Overridden Methods:**
- Not possible directly with Harmony.
- Harmony patches require an existing method to patch.
- Alternatives:
    - Patch Base Class: Patch parent class and use `__instance is SubClass` check.
    - Subclass & Replace: Subclass target class, replace instantiation with Harmony.

**Patching Base Class of Subclass:**
- Patch parent class method.
- Use `__instance is SubClass` to apply patch logic only to specific subclasses.
- Example:
  ```csharp
  if (!(__instance is SubClass))
      return;
  ```
