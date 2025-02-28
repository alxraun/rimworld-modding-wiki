```markdown
## XML Defs - Cheat Sheet

**Defs Overview:**

- Core of RimWorld content definition.
- XML files in `/Mods/Core/Defs` (and mod folders).
- Blueprints for in-game objects (items, plants, etc.).
- Link XML data to C# game logic.
- Turn generic entities into specific game elements (e.g., generic plant -> potato plant).

**Structure:**

- Root Node: `<Defs>`.
- Def Types: XML elements like `<ThingDef>`, `<RecipeDef>`, `<ResearchProjectDef>`.
- C# Correspondence: Each Def Type matches a C# class inheriting from `Def`.
- XML Tags: Represent fields of the corresponding C# Def class.
    - Simple Values: `<defName>value</defName>`, `<label>value</label>` (strings, integers).
    - Complex Values: Nested tags for classes like `IngestibleProperties` (`<ingestible>`).
    - Hierarchy: `<Defs> > <Def Type> > <fieldTag> > <childNode>`.

**Example Structure:**

```xml
<Defs>
	<ThingDef>
		<defName>SomeName</defName>
		<ingestible>
			<ingestSound>Slurp</ingestSound>
		</ingestible>
	</ThingDef>
</Defs>
```

**Common XML Errors (See also: Troubleshooting Tutorial):**

- **Syntax Errors:**  Formatting, unclosed tags, case sensitivity.
- **Cross-reference Errors:**  Incorrect or missing `defName` references. "Could not resolve cross-reference: No DefType named DefName found".
- **Cascading Errors:** One XML error can cause a chain of subsequent errors.

**Solving XML Errors:**

- Fix Errors Incrementally: Start with the first error reported in logs.
- Validate XML: Use XML editor features or online validators to check for syntax.
- Check Player.log: Consult the log file for detailed error messages and locations.
```
