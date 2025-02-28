```markdown
## PatchOperations - Cheat Sheet

**Purpose:** Modify XML Def content without overwriting, ensuring mod compatibility.

**Location:** XML files in `Patches` folder within mod.

**Structure:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<Patch>
  <!-- PatchOperations go here -->
</Patch>
```

**XPath Basics:**

- Target XML nodes for PatchOperations.
- Syntax: `Defs/DefType[predicate]/nodeToTarget`.
- `Defs/`: Root for all Def XML.
- `DefType`: Type of Def (e.g., `ThingDef`, `RecipeDef`).
- `[predicate]`: Conditional matching (e.g., `[defName="Wall"]`).
- `nodeToTarget`: XML tag to modify (e.g., `statBases`).
- Attribute Targeting: `Defs/ThingDef[@Name="ShelfBase"]/stuffCategories`.
- Parent Targeting: `Defs/ThingDef[@ParentName="ApparelBase"]`.

**PatchOperation Types:**

- **XML Node Operations:**
    - `PatchOperationAdd`: Add child node (`<value>`) to XPath target. `<order>`: `Prepend` or `Append` (default).
    - `PatchOperationInsert`: Insert sibling node (`<value>`) relative to XPath target. `<order>`: `Prepend` (default) or `Append`.
    - `PatchOperationRemove`: Delete XPath target node.
    - `PatchOperationReplace`: Replace XPath target node with `<value>`.

- **XML Attribute Operations:**
    - `PatchOperationAttributeAdd`: Add attribute (`<attribute>`, `<value>`) if not present.
    - `PatchOperationAttributeSet`: Set/overwrite attribute (`<attribute>`, `<value>`).
    - `PatchOperationAttributeRemove`: Remove attribute (`<attribute>`).

- **Special Operations:**
    - `PatchOperationSequence`: Execute list of operations; aborts on failure.
    - `PatchOperationAddModExtension`: Add `DefModExtension` to Def.
    - `PatchOperationSetName`: Rename XML node (`<name>`).

- **Conditional Operations:**
    - `PatchOperationFindMod`: Conditional operations based on mod/DLC presence (`<mods>`, `<match>`, `<nomatch>`). Uses mod `name`, not `packageId`.
    - `PatchOperationConditional`: Conditional operations based on XPath test (`<xpath>`, `<match>`, `<nomatch>`).
    - `PatchOperationTest`: Test XPath validity within `PatchOperationSequence`. (Obsolete - use `PatchOperationConditional`).

**Tips and Tricks:**

- Patch Order: Run after Def load, in mod load order.
- Patching Scope: Before Def inheritance.
- `Inherit="False"`: Prevent child from inheriting parent tag value.
- `or` in XPath: Target multiple nodes: `[defName="A" or defName="B"]`.
- `/text()` in XPath: Target tag text content for `PatchOperationReplace`.

**Common Issues / Troubleshooting:**

- Case Sensitivity: XPath, XML tags are case-sensitive.
- XML Syntax: Ensure valid XML structure; use validators.
- `Insert` vs `Add`: `Insert` for siblings, `Add` for children.
- XPath Targets: XML data structure, not file paths.
- Unique Nodes: Non-`li` XML nodes must be unique at each level.

**Obsolete Notes (From Original Wiki):**

- PatchOperationTest & `<success>` tag in `PatchOperationSequence` are obsolete; use `PatchOperationConditional`.
- `<success>` options: `Always` (suppress errors - generally avoid), `Normal`, `Invert`, `Never`.
- `MayRequire` in `PatchOperationSequence` child operations for conditional loading.
- PatchOperationFindMod uses mod `name`, not `packageId`.
```
