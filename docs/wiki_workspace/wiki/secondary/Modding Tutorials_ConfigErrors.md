```markdown
## ConfigErrors - Developer Cheat Sheet

**Purpose:**
- Report XML configuration errors during RimWorld startup.
- Inform modder/user of XML issues (missing fields, invalid values).

**Usage:**
- Implement `ConfigErrors()` method in custom Def classes and related classes.
- Displayed as red errors on game launch.

**Implementation (Def Subclass):**

```csharp
public class MyOwnDef : Def
{
    float someValue = 4f;
    const float minSomeValue = 0f;
    const float maxSomeValue = 10f;

    public override IEnumerable<string> ConfigErrors()
    {
        foreach(string error in base.ConfigErrors()) // Include base Def errors
            yield return error;

        if(someValue < minSomeValue || someValue > maxSomeValue) // Value range check
            yield return $"{nameof(someValue)} is {someValue} , out of range: {minSomeValue}-{maxSomeValue}!";
    }
}
```
- Override `ConfigErrors()` in custom `Def` subclass.
- Call `base.ConfigErrors()` to include base validations.
- Use `yield return "error message"` for each error condition.

**Implementation (Custom Classes):**

```csharp
public class MyOwnDef : Def
{
    List<MyOwnSpecialType> specialStuffList;

    public override IEnumerable<string> ConfigErrors()
    {
        foreach(string error in base.ConfigErrors())
            yield return error;

        if(specialStuffList == null) // Null list check
            yield return $"Required list {nameof(specialStuffList)} is empty";
        else
            foreach(MyOwnSpecialType specialStuff in specialStuffList) // Nested class errors
                foreach(string error in specialStuff.ConfigErrors())
                    yield return error;
    }
}

public class MyOwnSpecialType
{
    float someValue;
    const float minSomeValue = 0;
    const float maxSomeValue = 10;

    public IEnumerable<string> ConfigErrors()
    {
        if(someValue < minSomeValue || someValue > maxSomeValue) // Value range check in nested class
            yield return $"{nameof(someValue)} is {someValue} , out of range: {minSomeValue}-{maxSomeValue}!";
    }
}
```
- Implement `ConfigErrors()` method in custom classes even if not subclassing `Def`.
- Call nested class `ConfigErrors()` within parent `ConfigErrors()`.

**Fallback (No ConfigErrors Method):**

```csharp
// Example for manual validation
public static void ValidateMyTypes()
{
    foreach(var myTypeInstance in MyTypeInstances) // Iterate instances of custom type
    {
        if(myTypeInstance.someValue < 0)
            Log.Error($"MyType: {myTypeInstance.name} has invalid someValue: {myTypeInstance.someValue}"); // Log error message
    }
}
```
- Iterate instances of custom types.
- Use `Log.Error("error message")` to report issues.
- Call validation method after startup.

**Key Code Elements:**

- `override`:  Modify base `ConfigErrors()` behavior.
- `yield return`: Return single error message in `IEnumerable<string>`.
- String Interpolation (`$"{}"`): Embed variables in error messages.
- `nameof(variable)`: Get variable name as string for error messages.
```
