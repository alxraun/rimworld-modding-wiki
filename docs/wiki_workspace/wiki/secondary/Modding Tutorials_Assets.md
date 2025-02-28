```markdown
## Modding Tutorials/Assets - Cheat Sheet

**Purpose:** Extract base assets from RimWorld for modding.

**Tools:**

- UnityEx (RimWorld 1.0 Archives)
    1. Download UnityEx.
    2. Place `UnityEX.exe` in RimWorld directory (optional).
    3. Run `UnityEX.exe`.
    4. Open `.asset` file (e.g., `RimWorld\RimWorldWin64_Data\resources.assets`).
    5. Extract files ("Extract with convert" for `.tex` to `.dds`).
    6. Files in `Unity_Assets_Files` folder (same directory as `.asset`).

- Unity Assets Bundle Extractor (RimWorld 1.0 Archives)
    1. Download Unity Assets Bundle Extractor.
    2. Unzip.
    3. Run `AssetBundleExtractor.exe`.
    4. Open `.asset` file (e.g., `RimWorld\RimWorldWin64_Data\resources.assets`).
    5. Select files.
    6. Plugins -> "Export to .png" or "Export sound" (select files by type).
    7. Choose storage folder.

- AssetStudio (RimWorld 1.5 Archives)
    1. Download AssetStudio (net472 recommended if unsure).
    2. Unzip.
    3. Run `AssetStudioGUI.exe`.
    4. File -> Load file (for `.assets`) or File -> Load folder (for DLC Asset Bundles).
       - `.assets` example: `RimWorld\RimWorldWin64_Data\resources.assets`
       - DLC Asset Bundles example: `RimWorld\Data\Anomaly\AssetBundles`
    5. Asset List tab.
    6. Select files.
    7. Right click -> "Export selected assets" (audio/textures).
    8. Choose storage folder.

**Note:**

- Use [Recommended graphics software](https://rimworldwiki.com/wiki/Modding_Tutorials/Recommended_software#Graphics_Software) for editing extracted assets.
```
