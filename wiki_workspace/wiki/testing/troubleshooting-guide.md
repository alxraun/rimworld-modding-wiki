
## Modding Troubleshooting - Cheat Sheet

**Common XML Errors:**

- **Syntax Errors:**
    - Formatting, unclosed tags, wrong case, whitespace.
    - Use XML editor with validation.
    - Example: `compClass` vs `CompClass`, missing closing tag `</tag>`.
- **Cross-reference Errors:**
    - Def B not found when Def A references it by `defName`.
    - Type and `defName` must match.
    - Error message: "Could not resolve cross-reference: No DefType named DefName".
    - List errors: "Could not resolve cross-reference to DefType named DefName (wanter=tagName)".
- **Duplicate Field/Tag Definitions:**
    - Same field defined twice in a Def (mod conflict).
    - First mod's definition takes precedence.
    - Error message: "XML Def defines the same field twice: fieldName".
- **Missing Type:**
    - XML refers to a C# type not loaded (missing DLL, load error, outdated mod).
    - Error message: "Could not find type named Namespace.ClassName".
- **Referencing Non-Existing Field:**
    - XML tag doesn't exist in the Def's C# class (typo, wrong node, outdated mod).
    - Error message: "XML error: <tagName> doesn't correspond to any field in type DefType".

**Solving XML Errors:**

- Errors Cascade: Fix errors sequentially, starting with the first.
- XML Validation: Use a validating XML editor.
- Check Player.log: For detailed error messages.

**Reading Stacktraces (Example: NullReferenceException):**

- Read Top-Down: Error at the top, call sequence below.
- "Exception in BreadthFirstTraverse": Error catch point.
- "System.NullReferenceException: Object reference...": Type of error - uninitialized object.
- "*wrapper dynamic-method* Verse.Pawn.CheckForDisturbedSleep_Patch1": Harmony patch involved.
- "Gloomylynx.TantrumPatch.CheckForDisturbedSleepPrefix": Namespace.Class.Method causing error.

**Bug Reporting:**

- Provide detailed bug reports to mod authors.
- Include logs (especially HugsLib logs).
- Describe steps to reproduce the error.
- Be polite; modders are volunteers.
- "x breaks" is not helpful; provide specific details.

**Finding Error Cause in Stacktrace:**

- Start reading from the top of the stacktrace.
- Identify the Exception Type (e.g., NullReferenceException).
- Look for mod namespace, class, and method names in the stacktrace.
- Trace back the method calls to understand the error context.
- Use decompilers to examine the code at fault.
```
