
## Localization - Cheat Sheet

**Language Files:**

- Location: `Mods/MyModFolder/Languages/[Language]`.
- Language folders: `English`, `French (Français)`, `German (Deutsch)`, etc.

**File Types:**

- **DefInjected:** Translations for XML Def fields.
    - Location: `Languages/[Language]/DefInjected/[DefType]`.
    - Folder structure mirrors Def types (e.g., `ThingDef`, `RecipeDef`).
    - Root tag: `<LanguageData>`.
    - Nodes: Match XML structure of Def tags to translate (e.g., `<Beer.label>`).

- **Keyed:** Translations for C# code strings and UI elements.
    - Location: `Languages/[Language]/Keyed`.
    - Filename: Arbitrary XML file name.
    - Root tag: `<LanguageData>`.
    - Nodes: `<KeyString>Translation</KeyString>` (e.g., `<ClickToJumpToProblem>Click to jump</ClickToJumpToProblem>`).
    - Key Prefix: Use mod-specific prefixes for uniqueness (e.g., `<ARR_UsePotionOn>`).

    - **Using Keyed Strings in C#:**
        - `string translatedString = "KeyString".Translate();`
        - Returns key if translation missing (glitched text in Dev Mode).

    - **String Arguments:**
        - Indexed Arguments (XML): `<Example_KeyString>Text with {0}, {1}, {2}</Example_KeyString>`.
        - Translate with Arguments (C#): `"Example_KeyString".Translate(arg0, arg1, arg2);`.
        - Arguments: `ToString()` used to convert variables to strings for translation.

- **Strings:** Word lists for RulePackDefs (name/sentence generation).
    - Location: `Languages/[Language]/Strings`.
    - Purpose: Define word lists for procedural text generation.

**Key Points:**

- Folder Structure: Follow specific folder paths for correct loading.
- Filenames: Case-sensitive, follow naming conventions.
- XML Structure:  `<LanguageData>` root, specific node structure for each file type.
- Keyed Strings: Essential for mod UI and translatable C# text.
- `Translate()` Method:  Used in C# to access keyed strings.
- String Arguments:  Inject dynamic content into translations.
- Prefixes: Use mod-specific prefixes for Keyed strings to avoid conflicts.
```
