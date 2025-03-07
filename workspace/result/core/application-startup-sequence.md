## RimWorld Application Startup Sequence

**Root Method:** `Verse.PlayDataLoader.LoadAllPlayData(bool)` (called by `Verse.Root.Start()`)

**Load Order Steps:**

1. `Verse.GlobalTextureAtlasManager.ClearStaticAtlasBuildQueue()`
2. `Verse.GraphicDatabase.Clear()`
3. `Verse.LoadedModManager.LoadAllActiveMods()`
4. `Verse.LanguageDatabase.InitAllMetadata()`
5. `Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> DefDatabase<>.AddAllInMods`
6. `Verse.DirectXmlCrossRefLoader.ResolveAllWantedCrossReferences(FailMode.Silent)`
7. `RimWorld.DefOfHelper.RebindAllDefOfs(earlyTryMode: true)`
8. `Verse.TKeySystem.BuildMappings()`
9. `Verse.BackstoryTranslationUtility.LoadAndInjectBackstoryData()`
10. `Verse.LanguageDatabase.activeLanguage.InjectIntoData_BeforeImpliedDefs()`
11. `Verse.ColoredText.ResetStaticData()`
12. `Verse.DefGenerator.GenerateImpliedDefs_PreResolve()`
13. `Verse.DirectXmlCrossRefLoader.ResolveAllWantedCrossReferences(FailMode.LogErrors)`
14. `Verse.DirectXmlCrossRefLoader.Clear()`
15. `RimWorld.DefOfHelper.RebindAllDefOfs(earlyTryMode: false)`
16. `Verse.PlayDataLoader.ResetStaticDataPre()`
17. **Resolve Def References:**
    - `DefDatabase<ThingCategoryDef>.ResolveAllReferences()`
    - `DefDatabase<RecipeDef>.ResolveAllReferences()`
    - `Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> DefDatabase<>.ResolveAllReferences`
    - `DefDatabase<ThingDef>.ResolveAllReferences()`
18. `Verse.DefGenerator.GenerateImpliedDefs_PostResolve()`
19. `Verse.PlayDataLoader.ResetStaticDataPost()`
20. `Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> DefDatabase<>.ErrorCheckAllDefs`
21. `Verse.KeyPrefs.Init()`
22. `Verse.ShortHashGiver.GiveAllShortHashes()`
23. `RimWorld.SolidBioDatabase.LoadAllBios()`
24. `Verse.LanguageDatabase.activeLanguage.InjectIntoData_AfterImpliedDefs()`
25. `Verse.StaticConstructorOnStartupUtility.CallAll()`
26. `Verse.GlobalTextureAtlasManager.BakeStaticAtlases()`
27. `RimWorld.IO.AbstractFilesystem.ClearAllCache()`, `System.GC.Collect(...)`
28. `Verse.Log.ResetMessageCount`

**Warning:** Do not Harmony patch these methods for custom loading. Use appropriate methods instead.
