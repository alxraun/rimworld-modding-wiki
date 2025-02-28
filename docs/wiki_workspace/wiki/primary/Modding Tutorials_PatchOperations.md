```markdown
## PatchOperations - Cheat Sheet

**Purpose:** Modify XML Defs without overwriting, ensuring mod compatibility.

**Location:** XML files in `Patches` folder within mod.

**Root Tag:** `<Patch>`.

**Basics:**

- **XPath:** Target XML nodes for modification.
    - Start with `Defs/`.
    - Predicates `[...]` for conditional matching (e.g., `[defName="Wall"]`).
    - Target attributes using `@` (e.g., `[@Name="ShelfBase"]`).
    - XPath targets XML structure, not file paths.
- **PatchOperation Types:** XML nodes within `<Patch>` define operations.

**PatchOperation Types:**

- **XML Node Operations:**
    - `PatchOperationAdd`: Add child node(s) to XPath target.
        - `<value>`: Nodes to add.
        - `<order>`: `Prepend` (before) or `Append` (after, default) existing children.
    - `PatchOperationInsert`: Insert sibling node(s) relative to XPath target.
        - `<value>`: Nodes to insert.
        - `<order>`: `Prepend` (above, default) or `Append` (below) target node(s).
    - `PatchOperationRemove`: Delete XPath target node(s).
    - `PatchOperationReplace`: Replace XPath target node(s).
        - `<value>`: Replacement node(s).

- **XML Attribute Operations:**
    - `PatchOperationAttributeAdd`: Add attribute if not present.
        - `<attribute>`: Attribute name.
        - `<value>`: Attribute value.
    - `PatchOperationAttributeSet`: Set/overwrite attribute value.
        - `<attribute>`: Attribute name.
        - `<value>`: Attribute value.
    - `PatchOperationAttributeRemove`: Remove attribute.
        - `<attribute>`: Attribute name.

- **Special Operations:**
    - `PatchOperationSequence`: Execute list of operations; aborts on failure.
        - `<operations><li>...</li></operations>`: List of operations.
    - `PatchOperationAddModExtension`: Add `DefModExtension` to a `Def`.
        - `<value><li>...</li></value>`: ModExtension definition.
    - `PatchOperationSetName`: Change node name (e.g., for stat blocks).
        - `<name>`: New node name.

- **Conditional Operations:**
    - `PatchOperationFindMod`: Conditional operations based on mod/DLC presence (by mod name).
        - `<mods><li>...</li></mods>`: Mod names to check.
        - `<match>`: Operation if mod(s) are present.
        - `<nomatch>`: Operation if mod(s) are absent.
    - `PatchOperationConditional`: Conditional operations based on XPath target existence.
        - `<xpath>`: XPath to test.
        - `<match>`: Operation if XPath target exists.
        - `<nomatch>`: Operation if XPath target does not exist.
    - `PatchOperationTest`: Test XPath existence within `PatchOperationSequence`. (Obsolete, use `PatchOperationConditional` instead)

**Tips and Tricks:**

- Patch order: Mod list order, after Def loading, before inheritance.
- `Inherit="False"`: Prevent inheritance from parent Defs.
- `or` in XPath: Target multiple nodes (e.g., `[defName="A" or defName="B"]`).
- `/text()` in XPath: Target text content of a tag (e.g., for `PatchOperationReplace`).

**Common Issues:**

- Case-sensitivity: XPath, XML tags, attributes.
- Malformed XML: Unclosed tags, syntax errors (use XML validator).
- Confusing Insert vs Add: `Insert` for siblings, `Add` for children.
- XPath targets XML structure, not file paths.
- Non-unique XML nodes (except `li`): Errors on load.

**Troubleshooting:**

- Check Player.log for errors.
- Verify XPath is correct using Debug Inspector (Deep inspection mode).
- Test patches incrementally.
- Consult RimWorld Discord #mod-development for help.
```
