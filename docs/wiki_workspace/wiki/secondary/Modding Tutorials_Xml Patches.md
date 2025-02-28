```markdown
## XML Patches - Cheat Sheet

**Goal of Patching:**
- Modify existing game values (Defs) via XML.
- Non-destructive modding for compatibility.

**Useful Tools:**
- RimWorld Dev Mode: Debug Inspector for XPath discovery.
- XML Code Editor: VSCode, Notepad++ with XML support for syntax highlighting and error checking.
- RimWorld Dotnet Template (optional): For mod setup.
- Code Snippets (optional): For faster XML creation.

**Patching Basics:**

- Patch Files: XML files in `/Patches` folder.
- Root Tag: `<Patch>`.
- Operation Tag: `<Operation Class="PatchOperationType">`.
    - `Class`: Specifies PatchOperation type (e.g., `PatchOperationReplace`).
    - `xpath`: XML path to target value.
    - `value`: New XML value to replace or add.

**Patching with XML - Example (Revolver Damage):**

```xml
<Patch>
    <Operation Class="PatchOperationReplace">
        <xpath>/Defs/ThingDef[defName = "Bullet_Revolver"]/projectile/damageAmountBase</xpath>
        <value>
            <damageAmountBase>100</damageAmountBase>
        </value>
    </Operation>
</Patch>
```

- `PatchOperationReplace`: Replaces the node at XPath with `value`.
- `xpath`: `/Defs/ThingDef[defName = "Bullet_Revolver"]/projectile/damageAmountBase` - Targets `damageAmountBase` of `Bullet_Revolver` `ThingDef`.
- `value`: `<damageAmountBase>100</damageAmountBase>` - New damage value.

**Patching Sequence - Example (Explosive Assault Rifle):**

```xml
<Patch>
    <Operation Class="PatchOperationSequence">
        <operations>
            <li Class="PatchOperationReplace">
                <!-- Replace Projectile -->
            </li>
            <li Class="PatchOperationAdd">
                <!-- Add Explosive Properties -->
            </li>
        </operations>
    </Operation>
</Patch>
```

- `PatchOperationSequence`: Executes operations in order; stops on failure.
- `PatchOperationReplace (within Sequence)`: Changes Assault Rifle's projectile.
- `PatchOperationAdd (within Sequence)`: Adds explosive properties (`forcedMissRadius`, `targetParams`) to Assault Rifle's verbs.

**Other Patching Operations (See Modding Tutorials/PatchOperations for details):**

- `PatchOperationAdd`: Adds child node.
- `PatchOperationRemove`: Removes node.
- `PatchOperationInsert`: Inserts sibling node.
- `PatchOperationAttributeAdd`: Adds attribute if missing.
- `PatchOperationAttributeSet`: Sets/overwrites attribute.
- `PatchOperationAttributeRemove`: Removes attribute.
- `PatchOperationConditional`: Conditional patching based on XPath test.
- `PatchOperationFindMod`: Conditional patching based on mod presence.

**Key Patching Elements:**

- `Patch`: Root tag for patch files.
- `Operation`: Specifies patching action and type.
- `Class`: PatchOperation class name.
- `xpath`: XML Path Language for node targeting.
- `value`: New XML content for adding or replacing.
- `operations`: (PatchOperationSequence) List of PatchOperations.
- `success`: (PatchOperationSequence) Error handling behavior (use with caution).

**Tips:**

- Use Deep Inspection Mode: Find XPath easily.
- Reference Vanilla XML: Locate values for patching in game files (`Data` folder).
- Test Patches Incrementally: Start with simple patches and build complexity.
- Check Player.log: For patch errors and debugging.
- Use Code Snippets: Speed up XML structure creation.
```
