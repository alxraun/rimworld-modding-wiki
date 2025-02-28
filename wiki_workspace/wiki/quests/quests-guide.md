
## Quest Modding - Cheat Sheet

**Overview:**

- Quest modding is complex, two main approaches:
    - "Proper" (Extensible): Uses `QuestScriptDef`, `QuestNode`, `Slate`.
    - "Modern" (Simplified): Single `QuestNode` for all `QuestPart`s.
- "Proper" system offers more extendibility.

**Types of Quest Elements:**

**Generation Time (Builders - Instanced, Not Scribed):**

- **QuestScriptDef:** Root container for a quest.
    - Contains `root` `QuestNode`.
- **QuestNode:** Logic for quest generation.
    - **Slate-Modifiers:** Manipulate `Slate` data (context-data).
        - Retrieve pawns, factions, maps.
    - **QuestPart-Generators:** Create `QuestPart` instances.
    - **Methods:**
        - `TestRunInt()`: Dry run for generation checks, emulates `RunInt()`, modifies temporary `Slate`.
        - `RunInt()`: Actual execution, modifies `Slate` data, creates `QuestPart` instances.
- **Slate:** `Dictionary<string, object>` - data transport for quest generation.
    - Contains pawns, maps, points, etc.
- **SlateRef:** Controls `Slate` access, XML-defined key names.

**Run Time (Produced Objects - Quest Instance):**

- **Quest:** Result of `QuestScriptDef` generation.
    - Contains `QuestPart` list.
    - Listed in Quests tab.
- **QuestPart:** Logic blocks of a `Quest`.
    - React to quest `Signal`s.
- **Signal:** String identifier for quest events.
    - Optional `NamedArgument`: Keyed object data.

**Quest Generation Process (Random Quests):**

1. **QuestScriptDef Selection:** Game selects random `QuestScriptDef` from `DefDatabase` by `rootSelectionWeight`.
2. **TestRunInt() Check:** `root` `QuestNode`'s `TestRunInt()` executed with emulated `Slate`.
3. **QuestGen Activation:** `QuestGen.Generate()` called with valid `QuestScriptDef`.
4. **Slate Population:** `QuestGen.slate` populated with initial `Slate` values (Map, points).
5. **QuestNode Execution:** `QuestNode` call stack: `QuestGen.Generate()` -> `(QuestScriptDef)root.Run()` -> `(QuestNode)root.Run()` -> `(QuestNode)this.RunInt()`.
6. **Quest Finalization:** `QuestPart`s populated, `Quest` fully generated and available.
```
