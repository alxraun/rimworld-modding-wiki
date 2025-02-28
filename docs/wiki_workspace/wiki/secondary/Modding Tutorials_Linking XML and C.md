```markdown
## Linking XML and C# - Cheat Sheet

**I. Using C# from XML**

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

---

**II. Using XML from C#**

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
```
