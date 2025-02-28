```markdown
## XML File Structure - Cheat Sheet

**Basic XML Structure:**

- **XML Declaration:** `<?xml version="1.0" encoding="utf-8"?>` -  Start of every XML file, specifies XML version and encoding.
- **Root Node:** `<Defs>` - Encloses all Defs in the file.
- **Def Tags:** `<Def>` -  Container for a specific Def, type determined by tag name (e.g., `<ThingDef>`, `<RecipeDef>`).

**Keywords:**

- **Abstract="True"**:
    - Used in `<Def Name="DefName" Abstract="True">`.
    - Prevents Def from being loaded into the game.
    - Serves as a template for inheritance.
- **Name="DefName"**:
    - Defines a template/parent Def.
    - Used by other Defs for inheritance via `ParentName`.
- **ParentName="ParentDefName"**:
    - Used in `<Def ParentName="ParentDefName">`.
    - Inherits content from `<Def Name="ParentDefName">`.

**Inheritance Mechanism:**

1. **Parent Def:** `<Def Name="ParentDefName" Abstract="True">` - Defines base properties.
2. **Child Def:** `<Def ParentName="ParentDefName">` - Inherits properties from ParentDefName.
3. **Game Load:**
    - Parent Defs (`Name` & `Abstract="True"`) are templates, not loaded directly.
    - Child Defs (`ParentName`) copy & apply parent's content.
    - Child Def content overwrites parent content if conflicts.

**Stopping Inheritance - `Inherit="False"`:**

- Use `Inherit="False"` in a child tag to prevent inheriting a specific tag's content from the parent.
- Useful for:
    - Overriding specific tags.
    - Especially useful for list tags (`<li>`), where default behavior is to *add* to parent's list.
- Example:

```xml
<ThingDef Name="ThingOne" Abstract="True">
  <comps>
    <li Class = "CompOne">
      <valueA>1</valueA>
    </li>
  </comps>
</ThingDef>

<ThingDef ParentName="ThingOne">
  <comps Inherit= "False">
    <li Class = "CompOne">
      <valueA>2</valueA>
    </li>
  </comps>
</ThingDef>
```

**XML File Structure Template:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<Defs>
	<Def Name="ParentDefName" Abstract="True">
        <!-- Parent Def Properties -->
	</Def>

	<Def ParentName="ParentDefName">
        <!-- Child Def Properties - Inherits from ParentDefName -->
	</Def>

	<!-- More Defs -->
</Defs>
```

**Key Rules:**

- Root element is always `<Defs>`.
- Use `<Def>` tags for definitions.
- Case-sensitive XML.
- `Name` for parent Defs, `ParentName` for child Defs.
- `Abstract="True"` for template Defs.
- `Inherit="False"` for selective inheritance control.
```
