```markdown
- Mods Folder Location:
    - Windows: `C:\Program Files (x86)\Steam\steamapps\common\RimWorld\Mods`
    - Mac: `~/Library/Application Support/Steam/steamapps/common/RimWorld/RimWorldMac.app/Mods`
    - Linux (standalone): `~/.steam/steam/steamapps/common/RimWorld/Mods`
    - Linux (GoG): `/home/<user>/GOG/Games/RimWorld/game/Mods/`
- Mod Folder Structure (Example):
    - `Mods`: Main folder
        - `MyModFolder`: Mod's subfolder
            - `About`: Required - Mod identification
                - `About.xml`: Required - Mod metadata (name, author, id, versions, description)
                - `Preview.png`: Required - Mod preview image (640x360 PNG recommended, <1MB)
                - `ModIcon.png`: Optional - Loading screen icon (32x32 or 64x64 PNG)
            - `Assemblies`: Optional - Custom C# DLLs
            - `Defs`: Optional - XML Def files for content
            - `Languages`: Optional - Localization files
                - Subfolders: `English`, `French`, etc.
                - Subfolders inside language: `DefInjected`, `Keyed`, `Strings`
            - `Patches`: Optional - XML PatchOperation files for modifying Defs
            - `Sounds`: Optional - Custom sound files (Ogg recommended)
            - `Textures`: Optional - Custom texture files (PNG recommended)
- Versioned Folders (Optional):
    - `1.1`, `1.2`, `1.3`, `1.4`, `Common`
    - For version-specific content.
    - RimWorld loads first compatible versioned folder + `Common` + root.
    - v1.0: Does not recognize `Common`; `1.0` folder disables root folder loading.
    - Most specific path wins in case of file conflicts.
- LoadFolders.xml (Optional):
    - Custom folder loading rules.
    - Overrides default versioned folder loading (except v1.0).
    - Example:
        ```xml
        <loadFolders>
          <v1.4>
            <li>/</li>
            <li>1.4</li>
            <li IfModActive="AnotherMod.PackageId">OptionalFolder</li>
          </v1.4>
        </loadFolders>
        ```
    - Use `IfModActive`/`IfModNotActive` for conditional loading.
    - `_steam` suffix for Steam Workshop version compatibility in `IfModActive`.
- Miscellaneous:
    - `Core` & DLC folders: Similar to mod folders.
    - `Source` folder: For sharing source code; GitHub recommended.

**Key Notes:**

- Folder & filenames are case-sensitive.
- `About` folder is required.
- Use namespace folders or prefixes in `Sounds` & `Textures` to avoid collisions.
- `LoadFolders.xml` for advanced version and dependency management.
- `packageId` from `About.xml` is used for mod identification in `LoadFolders.xml`.
- `Preview.png` max size 1MB for Steam Workshop.
```
