## Transpiler Hints - Cheat Sheet

**Debugging Transpilers:**

- Transpiler debugging is complex due to IL code manipulation.
- Counting instructions accurately is crucial to avoid exceptions.

**Helpful Examples:**

- Search GitHub for "Rimworld Harmony Transpiler" to find community examples.
  - [GitHub Search Link](https://github.com/search?o=desc&q=Rimworld+Harmony+Transpiler&s=indexed&type=Code)

**Debugging Tools:**

- **IL Code Printing:**  Use code snippet to output modified IL code for inspection.

**Debugging Code Snippet:**

```csharp
public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions, ILGenerator generator)
{
    var l = XTranspiler(instructions, generator).ToList(); // Actual transpiler: XTranspiler
    string s = "Code:";
    int i = 0;
    foreach (var c in l)
    {
        if (c.opcode == OpCodes.Call || c.opcode == OpCodes.Callvirt)
        {
            Log.Warning("" + i + ": " + c); // Highlight Call/Callvirt operations
        }
        else
        {
            Log.Message("" + i + ": " + c);
        }
        s += "\n" + i + ": " + c;
        i++;
        yield return c;
    }
    Log.Error(s); // Full code output for text editor analysis
}
```

**Snippet Explanation:**

- `XTranspiler()`: Replace with your actual transpiler method name.
- `ToList()`: Converts `IEnumerable` to `List` for easier iteration.
- `Log.Warning()`: Highlights `Call` and `Callvirt` opcodes in output.
- `Log.Message()`: Standard output for other opcodes.
- `Log.Error(s)`: Outputs complete IL code as error log for easy copying.
- `yield return c`: Returns original `CodeInstruction` after logging.

**Usage Tip:**

- Utilize the provided code snippet to output and analyze generated IL code.
- Inspect the output for correctness, especially instruction counting and opcode sequences.
- Search GitHub for example transpilers to understand implementation patterns.
