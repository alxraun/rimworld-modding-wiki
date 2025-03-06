**Game Data Loading Sequence (Based on PlayDataLoader.cs):**

1.  **`GlobalTextureAtlasManager.ClearStaticAtlasBuildQueue()`**: Clears the static texture atlas build queue.
2.  **`GraphicDatabase.Clear()`**: Clears the graphics database.
3.  **`LoadedModManager.LoadAllActiveMods()`**: Loads all active mods.
4.  **`LanguageDatabase.InitAllMetadata()`**: Initializes language metadata.
5.  **`Parallel.ForEach(typeof(Def).AllSubclasses(), ...)`**: Parallel processing of all `Def` subclasses:
    -   **`GenGeneric.InvokeStaticMethodOnGenericType(typeof(DefDatabase<>), defType, "AddAllInMods")`**: Adds all Defs from mods to the global `DefDatabase` databases.
6.  **`DirectXmlCrossRefLoader.ResolveAllWantedCrossReferences(FailMode.Silent)`**: Resolves cross-references between non-implied Defs (in silent mode, without errors logged at this stage).
7.  **`DefOfHelper.RebindAllDefOfs(earlyTryMode: true)`**: Rebinds `DefOf` (early try mode).
8.  **`TKeySystem.BuildMappings()`**: Builds `TKeySystem` mappings (translation key system).
9.  **`BackstoryTranslationUtility.LoadAndInjectBackstoryData(LanguageDatabase.activeLanguage.AllDirectories)`**: Loads and injects backstory data for translation (legacy).
10. **`LanguageDatabase.activeLanguage.InjectIntoData_BeforeImpliedDefs()`**: Injects selected language data into game data (early pass, before implied Defs).
11. **`ColoredText.ResetStaticData()`**: Resets `ColoredText` static data.
12. **`DefGenerator.GenerateImpliedDefs_PreResolve()`**: Generates implied Defs (pre-resolve stage).
13. **`DirectXmlCrossRefLoader.ResolveAllWantedCrossReferences(FailMode.LogErrors)`**: Resolves cross-references between Defs created by implied Defs (with error logging).
14. **`DirectXmlCrossRefLoader.Clear()`**: Clears `DirectXmlCrossRefLoader`.
15. **`DefOfHelper.RebindAllDefOfs(earlyTryMode: false)`**: Rebinds `DefOf` (final).
16. **`ResetStaticDataPre()`**: Calls methods to reset static data (pre-resolve stage).
17. **Resolve Def References**:
    -   **`DefDatabase<ThingCategoryDef>.ResolveAllReferences(onlyExactlyMyType: true, parallel: true)`**: Resolves references for `ThingCategoryDef`.
    -   **`DefDatabase<RecipeDef>.ResolveAllReferences(onlyExactlyMyType: true, parallel: true)`**: Resolves references for `RecipeDef`.
    -   **`foreach (Type item in typeof(Def).AllSubclasses())...`**: For all other `Def` subclasses except `ThingDef`, `ThingCategoryDef`, `RecipeDef`:
        -   **`GenGeneric.InvokeStaticMethodOnGenericType(typeof(DefDatabase<>), item, "ResolveAllReferences", true, false)`**: Invokes the static `ResolveAllReferences` method.
    -   **`DefDatabase<ThingDef>.ResolveAllReferences()`**: Resolves references for `ThingDef`.
18. **`DefGenerator.GenerateImpliedDefs_PostResolve()`**: Generates implied Defs (post-resolve stage).
19. **`ResetStaticDataPost()`**: Calls methods to reset static data (post-resolve stage).
20. **`if (Prefs.DevMode)`**: If DevMode is enabled:
    -   **`Parallel.ForEach(typeof(Def).AllSubclasses(), ...)`**: Parallel processing of all `Def` subclasses:
        -   **`GenGeneric.InvokeStaticMethodOnGenericType(typeof(DefDatabase<>), defType, "ErrorCheckAllDefs")`**: Checks all Defs for errors.
21. **`KeyPrefs.Init()`**: Initializes keyboard preferences.
22. **`ShortHashGiver.GiveAllShortHashes()`**: Assigns short hashes to all Defs.
23. **`LongEventHandler.ExecuteWhenFinished(...)`**: Executes after long events are finished:
    -   **`SolidBioDatabase.LoadAllBios()`**: Loads all bios (PawnBios).
24. **`LongEventHandler.ExecuteWhenFinished(...)`**: Executes after long events are finished:
    -   **`LanguageDatabase.activeLanguage.InjectIntoData_AfterImpliedDefs()`**: Injects selected language data into game data (post-implied Defs, final pass).
    -   **`GenLabel.ClearCache()`**: Clears `GenLabel` cache.
25. **`LongEventHandler.ExecuteWhenFinished(...)`**: Executes after long events are finished:
    -   **`StaticConstructorOnStartupUtility.CallAll()`**: Calls all static constructors of classes with the `[StaticConstructorOnStartup]` attribute.
    -   **`if (Prefs.DevMode)`**: If DevMode is enabled:
        -   **`StaticConstructorOnStartupUtility.ReportProbablyMissingAttributes()`**: Reports possible missing attributes (for debugging).
    -   **`GlobalTextureAtlasManager.BakeStaticAtlases()`**: "Bakes" static texture atlases.
    -   **`AbstractFilesystem.ClearAllCache()`**: Clears all filesystem caches.
    -   **`GC.Collect(int.MaxValue, GCCollectionMode.Forced)`**: Forces garbage collection.
26. **`LongEventHandler.ExecuteWhenFinished(Log.ResetMessageCount)`**: Resets the log message count after all stages are complete.

**Warning:** Do not Harmony patch these methods for custom loading. Use appropriate methods instead.
