```markdown
## Assembly Modding Example - Cheat Sheet

**Description:**

- Tutorial: Creating a RimWorld mod using a DLL assembly (.dll).
- Project Goal: Build dark matter generator and animated wind turbine.
- Focus: Learning assembly and its program structure.
- Platform: Windows-only.

**Needed Tools:**

- IDE: Recommended IDE (e.g., Visual Studio)
- C# Knowledge: Basic understanding.
- RimWorld: Base game.

**Download Project:**

- GitHub: [https://github.com/HaploX1/ExampleDllMod](https://github.com/HaploX1/ExampleDllMod)
- Ludeon Forum: [https://ludeon.com/forums/index.php?topic=3408.0](https://ludeon.com/forums/index.php?topic=3408.0) (attachment)

**Preparations (Manual Steps):**

1. **Extract:** Downloaded project to chosen folder.
2. **Copy Assembly-CSharp.dll:**
   - From: `RimWorld-Folder\RimWorld000Win\RimWorld000Win_Data\Managed\`
   - To: `YourProjectFolder\RimWorld_ExampleProjectDLL\Source-DLLs\`
3. **Copy UnityEngine.dll:**
   - From: `RimWorld-Folder\RimWorld000Win\RimWorld000Win_Data\Managed\`
   - To: `YourProjectFolder\RimWorld_ExampleProjectDLL\Source-DLLs\`

**Enter The Project (Visual Studio):**

1. **Open Project:** Start Visual Studio and open `<YourExtractionFolder>\RimWorld\_ExampleProjectDLL.sln`.
2. **Explore:** Project structure and code within Visual Studio.
3. **Goal:** Create a dark matter generator mod.

**Note:** Finished mod with power generator included in the project.
```
