## RimWorld Application Startup Sequence - Cheat Sheet

**Root Method:** `Verse.PlayDataLoader.LoadAllPlayData(bool)` (called by `Verse.Root.Start()`)

**Load Order Steps:**

1. **Load mod config data:** Active mod list (`Verse.ModsConfig constructor`).
2. **Build mod list:** From mod folders (`Verse.ModLister.RebuildModList()`).
3. **Initialize mods:** Basic mod setup (`Verse.LoadedModManager.InitializeMods()`).
4. **Load mod assemblies:** DLLs, Mod subclass instances, queues asset loading (`Verse.LoadedModManager.LoadModContent()`, `Verse.LoadedModManager.CreateModClasses()`).
5. **Load Def XML files:** Parse XML (`Verse.LoadedModManager.LoadModXML()`).
6. **Combine Def XML:** Single unified XML document (`Verse.LoadedModManager.CombineIntoUnifiedXML()`).
7. **Load translation keys (Defs):** Rarely used (`Verse.TKeySystem.Parse()`).
8. **Load & error check patches:** Patch validation (`Verse.LoadedModManager.ErrorCheckPatches()`).
9. **Apply XML Patches:** `PatchOperations` (`Verse.LoadedModManager.ApplyPatches()`).
10. **Register Def inheritance:** `Name`, `ParentName` (`Verse.XmlInheritance.TryRegister()`).
11. **Apply Def inheritance:** Inheritance processing (`Verse.LoadedModManager.ParseAndProcessXML()`).
12. **Load Defs into Def classes:** XML -> C# Defs, register cross-refs (`Verse.LoadedModManager.ParseAndProcessXML()`).
13. **Warn on patch failures:** Patch error reporting (`Verse.LoadedModManager.ClearCachedPatches()`).
14. **Load Language metadata:** Language data initialization (`Verse.LanguageDatabase.InitAllMetadata()`).
15. **Store Defs in `DefDatabase`:** Global Def storage (`Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> AddAllInMods`).
16. **Store Defs in `DefOf` classes:** Static Def access (`RimWorld.DefOfHelper.RebindAllDefOfs(true)`).
17. **Build translation key mappings:** Translation system setup (`Verse.TKeySystem.BuildMappings()`).
18. **Inject DefInjected translations (round 1):** Pre-implied Defs (`Verse.LanguageDatabase.activeLanguage.InjectIntoData_BeforeImpliedDefs()`).
19. **Generate implied defs (pre-resolve):** Blueprints, Meat, etc. (`Verse.DefGenerator.GenerateImpliedDefs_PreResolve();`).
20. **Resolve cross-references:** Def lookups (`Verse.DirectXmlCrossRefLoader.ResolveAllWantedCrossReferences()`).
21. **Store Defs in `DefOf` fields (round 2):** DefOf population, error on missing Defs (`RimWorld.DefOfHelper.RebindAllDefOfs(false)`).
22. **Reload player knowledge database:** Learning Helper setup (`RimWorld.PlayerKnowledgeDatabase.ReloadAndRebind()`, `RimWorld.LessonAutoActivator.Reset()`).
23. **Reset static data (load):** Initial data reset (`Verse.LoadedModManager.DoPlayLoad()`).
24. **Resolve references:** Def `ResolveReferences` calls (ThingCategoryDefs, RecipeDefs, Defs, ThingDefs) (`Verse.DefDatabase<...>.ResolveAllReferences(...)`, `Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> ResolveAllReferences`).
25. **Generate implied defs (post-resolve):** Key bindings (`RimWorld.DefGenerator.GenerateImpliedDefs_PostResolve()`).
26. **Reset static data (smoothing):** Smoothing setup (`Verse.LoadedModManager.DoPlayLoad()`).
27. **Log config errors (Dev mode):** Def validation logging (`Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> ErrorCheckAllDefs`).
28. **Load KeyBindings:** Key prefs initialization (`Verse.KeyPrefs.Init()`).
29. **Assign short hashes:** Hash assignment for Defs (`Verse.ShortHashGiver.GiveAllShortHashes()`).
30. **Load audio files:** Mod sound loading (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
31. **Load textures:** Mod texture loading (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
32. **Load strings:** Mod string loading (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
33. **Load asset bundles:** Mod asset bundle loading (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
34. **Load PawnBios:** Backer pawns, name lists (`RimWorld.SolidBioDatabase.LoadAllBios()`).
35. **Inject DefInjected translations (round 2):** Post-implied Defs, translation validation (`Verse.LanguageDatabase.activeLanguage.InjectIntoData_AfterImpliedDefs()`).
36. **Call `StaticConstructorOnStartup`:** Static constructors execution (`Verse.StaticConstructorOnStartupUtility.CallAll()`).
37. **Bake static atlases:** Texture atlas baking (`Verse.GlobalTextureAtlasManager.BakeStaticAtlases()`).
38. **Force garbage collection:** Memory cleanup (`RimWorld.IO.AbstractFilesystem.ClearAllCache()`, `System.GC.Collect(...)`).

**Warning:** Do not Harmony patch these methods for custom loading. Use appropriate methods instead. Consult #mod-development on RimWorld Discord for load issue questions.

