```markdown
## RimWorld Folder Structure - Cheat Sheet

**RimWorld Install Directory Root:**

- **Data/:** Core game XML Defs (mostly).
    - `Core/`: Contains base game XML defs.
- **Mods/:**  Mod subfolders are located here.
- **RimWorld*_Data/:** Game data, DLLs, assets.
    - `Managed/`:  Game code DLLs (Assembly-CSharp.dll, UnityEngine.CoreModule.dll).
    - `output_log.txt`: Detailed game debug log.
- **Source/:** Limited C# source code examples (RimWorld, Verse).

**Mod Folder Structure (within Mods/):**

- **About/:** Mod identification.
    - `About.xml`: (Required) Mod metadata (name, author, id, versions, description).
    - `Preview.png`: (Optional) Mod preview image.
- **Assemblies/:** Custom C# DLLs.
- **Defs/:** XML Def files.
    - Subfolders for organization (e.g., `ThingDefs`, `RecipeDefs`).
- **Languages/:** Localization files.
    - Language-specific subfolders (e.g., `English`).
    - Subfolders within language folders: `DefInjected`, `Keyed`, `Strings`.
- **Patches/:** XML PatchOperation files.
- **Sounds/:** Custom sound files (Ogg, MP3, WAV).
- **Textures/:** Custom texture files (PNG, JPG, PSD, DDS).

**Key Folders for Modding:**

- **Data/Core:** Vanilla game Defs for reference.
- **Mods:** Location for mod subfolders.
- **Mods/YourModName/About:**  Required for mod identification.
- **Mods/YourModName/Assemblies:** For custom C# code.
- **Mods/YourModName/Defs:** For XML content definitions.
- **Mods/YourModName/Languages:** For localization.
- **Mods/YourModName/Patches:** For XML patches.
- **Mods/YourModName/Sounds & Textures:** For custom assets.
- **RimWorld*_Data/Managed:** Game DLLs for C# modding references.

**Important Notes:**

- Mod folders are subfolders within the main `Mods` folder.
- Folder and filenames are case-sensitive.
- `About` folder with `About.xml` is mandatory for mod recognition.
- `Mods` folder location depends on OS and RimWorld version (Steam, GOG, Standalone Linux).
- `RimWorld*_Data/Managed` contains essential DLLs for C# modding.
```
