```markdown
## Modding Compatibility - Cheat Sheet

**Mod Conflicts:**

- Occur when multiple mods alter the same game content.
- Common causes:
    - Overwriting abstract bases.
    - Overwriting Defs.
    - Destructive Harmony prefixes.
- Result: Unintended/undesired game behavior.

**Avoiding Mod Conflicts:**

- **Defensive Patching:** Limit mod scope, patch defensively.
    - XML: Use XPath for targeted edits.
    - C#: Null-checking, avoid destructive prefixes (use postfixes if possible).
- **Mod Load Order:**
    - "Bottom wins" general rule is simplified.
    - Actual load order: XML -> XPath Patches -> C#.
    - Core loads first.
    - Load order matters for overwrites but not for well-designed patches.
- **Don't Overwrite:**
    - Abstract bases.
    - Vanilla Defs (use XPath instead).
    - Mod Defs (use XPath, PatchOperationFindMod, PatchOperationConditional).
- **C# Considerations:**
    - Null References: Check for nulls extensively, especially with external mod interactions.
    - Destructive Prefixes: Avoid; use postfixes or limit scope with checks.
    - Weird Interactions: Handle case-by-case.

**Defensive Patching Techniques (XML):**

- **PatchOperationSequence + PatchOperationTest + PatchOperationAdd:**
    ```xml
    <Operation Class="PatchOperationSequence">
        <operations>
            <li Class="PatchOperationTest">
                <xpath>XPath/To/Test/Target</xpath>
                <success>Invert</success> <!-- Check for absence -->
            </li>
            <li Class="PatchOperationAdd">
                <xpath>XPath/To/Add/To</xpath>
                <value> <newNode/> </value>
            </li>
        </operations>
    </Operation>
    ```
    - Adds node only if it doesn't exist.

- **PatchOperationConditional:**
    ```xml
    <Operation Class="PatchOperationConditional">
        <xpath>XPath/To/Check/Target</xpath>
        <nomatch Class="PatchOperationAdd">
            <xpath>XPath/To/Add/IfNoMatch</xpath>
            <value> <newNode/> </value>
        </nomatch>
        <match Class="PatchOperationAdd">
            <xpath>XPath/To/Add/IfMatch</xpath>
            <value> <newNode/> </value>
        </match>
    </Operation>
    ```
    - Different operations based on XPath match/no-match.

**Mod Load Order Details:**

- Load Order Stages: XML -> XPath -> C#.
- Dependencies: Use `<modDependencies>` in `About.xml`.
- Missing Dependencies: Causes errors, prevents mod loading.
- Load Order Warnings/Errors: Use `<loadBefore>`, `<loadAfter>`, `<forceLoadBefore>`, `<forceLoadAfter>` in `About.xml`.

**C# Compatibility Issues:**

- Destructive Prefixes: Avoid; use postfixes or scoped checks.
- Null References: Implement thorough null checks.
- Weird Interactions: Address individually.

**Key Modding Principle:** Cooperate with other modders for compatibility.
```
