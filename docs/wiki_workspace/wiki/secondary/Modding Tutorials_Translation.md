```markdown
## Modding Tutorials/Translation - Cheat Sheet

**Language Files:**

- Location: `Mods/MyModFolder/Languages/LanguageName/`.
- Language Codes: `English`, `French (Français)`, `German (Deutsch)`, etc.

**Types of Language Files:**

- **DefInjected:** Translate fields in XML Defs.
    - Location: `Languages/LanguageName/DefInjected/DefType/FileName.xml`.
    - Folder Structure: Mirrors Def type hierarchy (e.g., `ThingDef`, `RecipeDef`).
    - Root Tag: `<LanguageData>`.
    - Node Structure: Matches XML structure of Def tags to translate.
    - Example: `<ThingDef_DefName.label>Translated Label</ThingDef_DefName.label>`.

- **Keyed:** Define strings for C# code and UI.
    - Location: `Languages/LanguageName/Keyed/AnyFileName.xml`.
    - Filename & Subfolders: Arbitrary (for organization).
    - String Keys: Must be unique (prefix recommended).
    - Root Tag: `<LanguageData>`.
    - Example: `<MyMod_StringKey>Translated String</MyMod_StringKey>`.
        - Using Keyed Strings in C#: `"MyMod_StringKey".Translate()`.
        - String Arguments (Indexed): `<Example_KeyString>String {0} {1}</Example_KeyString>`.
        - Usage with Arguments: `"Example_KeyString".Translate(arg1, arg2)`.

- **Strings:** Word lists for RulePackDefs (name/sentence generation).
    - Location: `Languages/LanguageName/Strings/AnyFileName.xml`.

**Key Points:**

- `DefInjected`: For Def XML translations.
- `Keyed`: For C# code strings and UI text.
- Prefix String Keys: To ensure uniqueness and avoid conflicts.
- Use `Translate()`: In C# to use Keyed strings.
- Indexed Arguments: For dynamic strings in Keyed files `{0}`, `{1}`, etc.
```
