
## Troubleshooting - Finding Exceptions Cheat Sheet

**For Modders Facing Exceptions (NRE Example):**

**Purpose:** Track down the root cause of exceptions, especially NullReferenceExceptions (NREs).

**Requirements:**

- An Exception (error message/stack trace).
- Decompiler (ILSpy, dnSpy, Rider/dotPeek).

**Example Scenario:**

- Debug Action "Spawn Potatoes" added for demonstration.
- Code spawns potatoes at a pawn's location.
- Goal: Understand how to use decompilers to investigate exceptions.

**Troubleshooting Steps (General Approach):**

1. **Get the Exception:**
   - Obtain the full error message and stack trace from RimWorld's debug log.

2. **Use a Decompiler:**
   - Open `Assembly-CSharp.dll` in a decompiler (ILSpy, dnSpy, Rider/dotPeek).

3. **Analyze the Stack Trace (Example):**
   - Read stack trace from top to bottom.
   - Start analysis ~3 calls before the top for relevant mod code.
   - Identify Exception Type: `System.NullReferenceException` (common for modding).
   - Look for Harmony Patches: `(wrapper dynamic-method) ... _Patch#` indicates Harmony involvement.
   - Locate Error Source: `Namespace.Class.MethodName` (e.g., `Gloomylynx.TantrumPatch.CheckForDisturbedSleepPrefix`).

4. **Understand NullReferenceException (NRE):**
   - "Object reference not set to an instance of an object" - code tries to use a variable that is `null` (not initialized).

5. **Decompile Error Source Code:**
   - Use decompiler to examine the code in the identified `Namespace.Class.MethodName`.
   - Look for lines of code where a variable might be `null` before being used (dereferenced).

6. **Debugging Example (Conceptual):**
   - Imagine `SpawnPotatoesOnPawn(Pawn pawn)` throws NRE.
   - Decompile `SpawnPotatoesOnPawn` method.
   - Check for potential `null` variables: `pawn`, `pawn.Position`, `pawn.Map`, etc.
   - Add null checks (`if (pawn != null)`) to your code to prevent NRE.

**Key Takeaways:**

- Stack traces are read top-down, error at the top.
- NullReferenceExceptions (NREs) are common due to uninitialized variables.
- Decompilers are essential for examining game code and pinpointing error sources.
- Focus on the relevant part of the stack trace (your mod's namespace).
- Use null checks defensively in your code.
```
