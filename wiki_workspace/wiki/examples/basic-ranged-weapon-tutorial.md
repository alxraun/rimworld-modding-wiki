## XML Patches - Cheat Sheet

**Purpose:** Modify existing game XML Defs without overwriting.

**Key Concepts:**

- **Patch Files:** XML files in `/Patches` folder. Root tag `<Patch>`.
- **Patch Operations:** XML elements within `<Patch>` that define modifications.
- **XPath:** XML Path Language to target specific nodes in Def XML.
- **Value:**  New XML content to add or replace.

**Basic Patch Structure:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<Patch>
  <Operation Class="PatchOperationClass">
    <xpath>XPath Expression</xpath>
    <value>
      <!-- XML value to add/replace -->
    </value>
  </Operation>
</Patch>
```

**Common Patch Operations:**

- **`PatchOperationReplace`**: Replaces targeted XML node with `<value>`.
    - `<xpath>`: Target node to replace.
    - `<value>`: New XML node to replace with.

- **`PatchOperationAdd`**: Adds `<value>` as child node to targeted node.
    - `<xpath>`: Target node to add child to.
    - `<value>`: XML node to add as child.
    - Optional: `<order>Prepend</order>` to add at the beginning.

- **`PatchOperationRemove`**: Removes targeted XML node.
    - `<xpath>`: Target node to remove.

- **`PatchOperationSequence`**: Executes a list of operations; stops on failure.
    - `<operations>`: List of `<li>` containing PatchOperations.
    - `<success>Normal/Always/Invert/Never</success>`: Error handling (mostly obsolete, use PatchOperationConditional).

**XPath Basics:**

- `Defs/DefType[defName="DefName"]/node/to/target`: Path to specific node.
- `Defs`: Root node for Def files.
- `DefType`: Type of Def (e.g., `ThingDef`, `RecipeDef`).
- `[defName="DefName"]`: Predicate to target Defs by `defName`.
- `@AttributeName="Value"`: Predicate to target Defs by attribute value.
- `/node/to/target`: Path to the specific element within the Def.

**Example - PatchOperationReplace:**

```xml
<Operation Class="PatchOperationReplace">
  <xpath>/Defs/ThingDef[defName = "Bullet_Revolver"]/projectile/damageAmountBase</xpath>
  <value>
    <damageAmountBase>100</damageAmountBase>
  </value>
</Operation>
```
- Replaces `damageAmountBase` value of `Bullet_Revolver` ThingDef.

**Example - PatchOperationSequence:**

```xml
<Operation Class="PatchOperationSequence">
  <operations>
    <li Class="PatchOperationReplace">
      </li Class="PatchOperationAdd">
    </operations>
</Operation>
```
- Executes multiple operations sequentially.

**Tips:**

- Use XML editor with validation.
- Test patches incrementally.
- Check `Player.log` for errors.
- Use Debug Inspector (Deep inspection mode) to find XPath of game objects.
- Reference vanilla XML for XPath examples and structure.
- Use code snippets for faster XML creation (e.g., `rwpatch`, `rwbullet`, `rwweapon`).

**Caution:**

- XML is case-sensitive.
- XPath must be precise.
- PatchOperationSequence can hide errors; test operations individually.
- Avoid overuse of `success="Always"` in PatchOperationSequence.
