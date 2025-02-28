```markdown
## Def Classes - Cheat Sheet

**Vanilla defClasses (C# & XML):**

- **Structure:** C# class (`Def` inheritance) defines XML structure.
- **Tags:** Public C# fields (e.g., `public SomeType someTagName;`) become XML tags (`<someTagName>`).
- **Subtags:** Nested classes (e.g., `StuffProperties`) create subtags within XML.
- **Lists:** `List<T>` in C# creates XML lists with `<li>` items.
    - `StatModifier` in lists: Special case, uses tag name as `StatDef`, tag value as `float`.

**Making Custom Def Classes:**

- **Purpose:** Create new Def types with custom fields and behavior.
- **XML Requirement:**
    - `<ThingDef Class="MyNamespace.MyCustomDefName">` in XML to link to custom C# class.
- **Code Requirement:**
    - `class MyCustomDefName : ThingDef` (or other Def type) in C#.
- **Accessing Custom Fields (C#):**
    - Casting: `var def = thing.def as MyCustomDefName;`

**Simple Example (Custom Weapon Def):**

- **XML (`ThingDefs.xml`):**
  ```xml
  <ThingDef ParentName="BaseHumanGun" Class="MyNamespace.MyCustomDef_ThingDef">
    <defName>MyCoolNewGun</defName>
    <thingClass>MyNamespace.MyCoolNewGun</thingClass>
    <myNewFloat>1.0</myNewFloat>
    <myNewBool>true</myNewBool>
    <myNewThingDefList>
      <li>Steel</li>
    </myNewThingDefList>
  </ThingDef>
  ```
- **C# (`MyCustomDef_ThingDef.cs`):**
  ```csharp
  namespace MyNamespace
  {
      public class MyCustomDef_ThingDef : ThingDef
      {
          public float myNewFloat;
          public bool myNewBool;
          public List<ThingDef> myNewThingDefList = new List<ThingDef>();
      }
  }
  ```
- **Accessing Custom Def in Code:**
  ```csharp
  MyCustomDef_ThingDef def = weapon.def as MyNamespace.MyCustomDef_ThingDef;
  float floatValue = def.myNewFloat;
  ```

**Further Example (Abstract Custom Gun Def):**

- Use Abstract Defs to create families of custom Defs.
- Define base properties in Abstract Def, inherit in concrete Defs.

**When NOT to Use Custom Def Classes:**

- **Compatibility Issues:** Overwriting existing Defs can cause conflicts.
- **Limited Scope:** Defs are XML-driven; functionality is restricted to XML context.
- **Alternative:** Consider `ThingComp` or `DefModExtension` for more flexible, compatible solutions.

**Inheritance with Custom Def Classes:**

- `Class` attribute must be specified in every inheriting Def in XML.
- Example:
  ```xml
  <ThingDef Name="BaseThing" Class="MyNamespace.MyCustomClass" Abstract="True">
  </ThingDef>

  <ThingDef ParentName="BaseThing" Class="MyNamespace.MyCustomClass">
  </ThingDef>
  ```

**Key Considerations:**

- `Class` attribute links XML to C# class.
- `defName` must be unique.
- Use namespaces to organize code and XML.
- Be mindful of compatibility when modifying core Defs.
- Consider alternatives like `ThingComp` and `DefModExtension` for better compatibility and flexibility.
- Inheritance in XML requires explicit `Class` declaration in every child Def.
```
