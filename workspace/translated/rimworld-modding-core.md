---
description: "Core" module Rimworld modding documentation
globs: 
alwaysApply: true
---
`\\?\C:\Projects\rimworld\wiki_workspace\wiki\core\about-xml-guide.md`:

```````md
## About.xml - шпаргалка

**Назначение:**
- Идентифицирует мод для RimWorld.
- Определяет названия модов (внутренние и для пользователя).
- Предоставляет описание мода для внутриигрового менеджера модов.
- Указывает зависимости модов и порядок загрузки.

**Расположение:**
- `Mods/YourModFolder/About/About.xml`
- Папка и имя файла чувствительны к регистру.

**Корневой тег:**
- `<ModMetaData>`

**Обязательные теги:**

- `<packageId>YourName.ModName</packageId>`
    - Внутренний идентификатор мода.
    - Буквенно-цифровой, разделенный точками (например, `Author.Mod`).
    - **Глобально уникальный** для всех модов.
    - Нечувствителен к регистру.
    - Используется для зависимостей, порядка загрузки, MayRequire.

- `<name>My Mod Name</name>`
    - Название мода (для пользователя).
    - Не является глобально уникальным, но лучше не менять.
    - Используется `PatchOperationFindMod`.

- `<author>Author Name</author>` / `<authors><li>Author Name</li></authors>`
    - Автор(ы) мода.
    - Имена, разделенные запятыми, или узлы списка.

- `<description>Mod description.</description>`
    - Описание мода в виде обычного текста.
    - Отображается в менеджерах модов и Steam Workshop.
    - Старайтесь делать его относительно коротким для `About.xml`.

- `<supportedVersions><li>1.4</li></supportedVersions>`
    - Версии RimWorld, которые поддерживает мод.
    - Перечислите протестированные версии.
    - Предупреждения для неподдерживаемых версий, но мод все равно может загрузиться.

**Необязательные теги:**

- `<modVersion>1.0</modVersion>` / `<modVersion IgnoreIfNoMatchingField="True">1.0</modVersion>`
    - Строка версии мода для личного отслеживания.
    - `IgnoreIfNoMatchingField="True"` для совместимости со старыми версиями.

- `<modIconPath IgnoreIfNoMatchingField="True">Path/To/Icon</modIconPath>`
    - Путь к иконке мода (экраны загрузки).
    - PNG 32x32, ограниченное количество цветов.

- **ModIcon.png:**
    - Просто поместите файл с именем `ModIcon.png` в папку `About`.
    - Будет автоматически загружен при запуске игры.
    - Должен быть небольшим (32x32 или 64x64 пикселей).
    - Избегайте излишней детализации - используйте плоские цвета или тонкие градиенты, чтобы предотвратить артефакты сжатия.
    - Не требует XML-конфигурации.

- `<url>https://mod-url</url>`
    - Веб-ссылка для информации о моде (Steam Workshop, GitHub и т. д.).

- `<descriptionsByVersion><v1.4>...</v1.4></descriptionsByVersion>`
    - Описания для конкретных версий (не для Steam Workshop).

- `<modDependencies><li>...</li></modDependencies>`
    - Зависимости модов.
    - Предупреждает игроков, если зависимости отсутствуют.
    - Не используется автоматически Steam Workshop "Required Items".
    - `packageId`, `displayName`, `steamWorkshopUrl` в `<li>`.

- `<modDependenciesByVersion><v1.4><li>...</li></v1.4></modDependenciesByVersion>`
    - Зависимости для конкретных версий.

- `<loadBefore><li>packageId</li></loadBefore>`
    - Моды, которые нужно загрузить перед этим модом.
    - Предупреждает игрока, если порядок загрузки неверен.
    - Используйте `packageId`.

- `<loadBeforeByVersion><v1.4><li>packageId</li></v1.4></loadBeforeByVersion>`
    - `loadBefore` для конкретных версий.

- `<forceLoadBefore><li>packageId</li></forceLoadBefore>`
    - Принудительный порядок загрузки; RimWorld не позволяет загружать после указанных модов.
    - Внешние менеджеры модов могут игнорировать.

- `<loadAfter><li>packageId</li></loadAfter>`
    - Моды, которые нужно загрузить после этого мода.
    - Предупреждает игрока, если порядок загрузки неверен.
    - Используйте `packageId`.

- `<loadAfterByVersion><v1.4><li>packageId</li></v1.4></loadAfterByVersion>`
    - `loadAfter` для конкретных версий.

- `<forceLoadAfter><li>packageId</li></forceLoadAfter>`
    - Принудительный порядок загрузки; RimWorld не позволяет загружать перед указанными модами.
    - Внешние менеджеры модов могут игнорировать.

- `<incompatibleWith><li>packageId</li></incompatibleWith>`
    - Несовместимые моды.
    - Предупреждает игрока, если несовместимые моды загружены вместе.
    - Для фундаментальных несовместимостей, а не просто ошибок.

- `<incompatibleWithByVersion><v1.4><li>packageId</li></v1.4></incompatibleWithByVersion>`
    - `incompatibleWith` для конкретных версий.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\core\application-startup-sequence.md`:

```````md
## Последовательность запуска приложения RimWorld - шпаргалка

**Корневой метод:** `Verse.PlayDataLoader.LoadAllPlayData(bool)` (вызывается `Verse.Root.Start()`)

**Этапы порядка загрузки:**

1. **Загрузка данных конфигурации модов:** Список активных модов (`Verse.ModsConfig constructor`).
2. **Построение списка модов:** Из папок модов (`Verse.ModLister.RebuildModList()`).
3. **Инициализация модов:** Базовая настройка модов (`Verse.LoadedModManager.InitializeMods()`).
4. **Загрузка сборок модов:** DLL, экземпляры подклассов Mod, очереди загрузки ресурсов (`Verse.LoadedModManager.LoadModContent()`, `Verse.LoadedModManager.CreateModClasses()`).
5. **Загрузка XML-файлов Def:** Разбор XML (`Verse.LoadedModManager.LoadModXML()`).
6. **Объединение XML Def:** Единый XML-документ (`Verse.LoadedModManager.CombineIntoUnifiedXML()`).
7. **Загрузка ключей перевода (Defs):** Используется редко (`Verse.TKeySystem.Parse()`).
8. **Загрузка и проверка патчей на ошибки:** Проверка патчей (`Verse.LoadedModManager.ErrorCheckPatches()`).
9. **Применение XML-патчей:** `PatchOperations` (`Verse.LoadedModManager.ApplyPatches()`).
10. **Регистрация наследования Def:** `Name`, `ParentName` (`Verse.XmlInheritance.TryRegister()`).
11. **Применение наследования Def:** Обработка наследования (`Verse.LoadedModManager.ParseAndProcessXML()`).
12. **Загрузка Def в классы Def:** XML -> C# Defs, регистрация перекрестных ссылок (`Verse.LoadedModManager.ParseAndProcessXML()`).
13. **Предупреждение о сбоях патчей:** Отчет об ошибках патчей (`Verse.LoadedModManager.ClearCachedPatches()`).
14. **Загрузка метаданных языка:** Инициализация языковых данных (`Verse.LanguageDatabase.InitAllMetadata()`).
15. **Хранение Def в `DefDatabase`:** Глобальное хранилище Def (`Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> AddAllInMods`).
16. **Хранение Def в классах `DefOf`:** Статический доступ к Def (`RimWorld.DefOfHelper.RebindAllDefOfs(true)`).
17. **Построение сопоставлений ключей перевода:** Настройка системы перевода (`Verse.TKeySystem.BuildMappings()`).
18. **Внедрение DefInjected переводов (раунд 1):** Pre-implied Defs (`Verse.LanguageDatabase.activeLanguage.InjectIntoData_BeforeImpliedDefs()`).
19. **Генерация подразумеваемых defs (pre-resolve):** Чертежи, Мясо и т. д. (`Verse.DefGenerator.GenerateImpliedDefs_PreResolve();`).
20. **Разрешение перекрестных ссылок:** Поиск Def (`Verse.DirectXmlCrossRefLoader.ResolveAllWantedCrossReferences()`).
21. **Хранение Def в полях `DefOf` (раунд 2):** Заполнение DefOf, ошибка при отсутствии Def (`RimWorld.DefOfHelper.RebindAllDefOfs(false)`).
22. **Перезагрузка базы данных знаний игрока:** Настройка Learning Helper (`RimWorld.PlayerKnowledgeDatabase.ReloadAndRebind()`, `RimWorld.LessonAutoActivator.Reset()`).
23. **Сброс статических данных (загрузка):** Начальный сброс данных (`Verse.LoadedModManager.DoPlayLoad()`).
24. **Разрешение ссылок:** Вызовы Def `ResolveReferences` (ThingCategoryDefs, RecipeDefs, Defs, ThingDefs) (`Verse.DefDatabase<...>.ResolveAllReferences(...)`, `Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> ResolveAllReferences`).
25. **Генерация подразумеваемых defs (post-resolve):** Горячие клавиши (`RimWorld.DefGenerator.GenerateImpliedDefs_PostResolve()`).
26. **Сброс статических данных (сглаживание):** Настройка сглаживания (`Verse.LoadedModManager.DoPlayLoad()`).
27. **Регистрация ошибок конфигурации (Dev mode):** Ведение журнала проверки Def (`Verse.GenGeneric.InvokeStaticMethodOnGenericType() -> ErrorCheckAllDefs`).
28. **Загрузка KeyBindings:** Инициализация настроек клавиш (`Verse.KeyPrefs.Init()`).
29. **Назначение коротких хешей:** Назначение хешей для Def (`Verse.ShortHashGiver.GiveAllShortHashes()`).
30. **Загрузка аудиофайлов:** Загрузка звуков модов (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
31. **Загрузка текстур:** Загрузка текстур модов (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
32. **Загрузка строк:** Загрузка строк модов (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
33. **Загрузка пакетов ресурсов:** Загрузка пакетов ресурсов модов (`Verse.LoadedModManager.LoadModContent() -> Verse.ModContentPack.ReloadContent() -> VerseModContentPack.ReloadContentInt()`).
34. **Загрузка PawnBios:** Pawns спонсоров, списки имен (`RimWorld.SolidBioDatabase.LoadAllBios()`).
35. **Внедрение DefInjected переводов (раунд 2):** Post-implied Defs, проверка перевода (`Verse.LanguageDatabase.activeLanguage.InjectIntoData_AfterImpliedDefs()`).
36. **Вызов `StaticConstructorOnStartup`:** Выполнение статических конструкторов (`Verse.StaticConstructorOnStartupUtility.CallAll()`).
37. **Выпекание статических атласов:** Выпекание атласов текстур (`Verse.GlobalTextureAtlasManager.BakeStaticAtlases()`).
38. **Принудительная сборка мусора:** Очистка памяти (`RimWorld.IO.AbstractFilesystem.ClearAllCache()`, `System.GC.Collect(...)`).

**Предупреждение:** Не используйте Harmony patch для этих методов для пользовательской загрузки. Вместо этого используйте соответствующие методы. Обратитесь к каналу #mod-development в Discord RimWorld, если у вас есть вопросы по загрузке.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\core\mod-folder-structure.md`:

```````md
## Структура папок модов - шпаргалка

**Корневой каталог установки RimWorld:**

- **Data/:** Основные игровые XML Defs (в основном).
    - `Core/`: Содержит основные игровые XML defs.
- **Mods/:** Здесь расположены подпапки модов.
- **RimWorld*_Data/:** Данные игры, DLL, ресурсы.
    - `Managed/`: DLL с кодом игры (Assembly-CSharp.dll, UnityEngine.CoreModule.dll).
    - `output_log.txt`: Подробный журнал отладки игры.
- **Source/:** Ограниченные примеры исходного кода C# (RimWorld, Verse).

**Расположение папки Mods (по ОС):**
- Windows: `C:\Program Files (x86)\Steam\steamapps\common\RimWorld\Mods`
- Mac: `~/Library/Application Support/Steam/steamapps/common/RimWorld/RimWorldMac.app/Mods`
- Linux (standalone): `~/.steam/steam/steamapps/common/RimWorld/Mods`
- Linux (GoG): `/home/<user>/GOG/Games/RimWorld/game/Mods/`

**Структура папки мода (внутри Mods/):**

- **About/:** Обязательно - Идентификация мода.
    - `About.xml`: Обязательно - Метаданные мода (название, автор, id, версии, описание).
    - `Preview.png`: Рекомендуется - Изображение предварительного просмотра мода (рекомендуется PNG 640x360, <1МБ).
    - `ModIcon.png`: Необязательно - Иконка экрана загрузки (PNG 32x32 или 64x64).
- **Assemblies/:** Необязательно - Пользовательские DLL C#.
- **Defs/:** Необязательно - XML-файлы Def для контента.
    - Подпапки для организации (например, `ThingDefs`, `RecipeDefs`).
- **Languages/:** Необязательно - Файлы локализации.
    - Подпапки для конкретных языков (например, `English`, `French`).
    - Подпапки внутри языка: `DefInjected`, `Keyed`, `Strings`.
- **Patches/:** Необязательно - XML-файлы PatchOperation для изменения Defs.
- **Sounds/:** Необязательно - Пользовательские звуковые файлы (рекомендуются Ogg, MP3, WAV).
- **Textures/:** Необязательно - Пользовательские файлы текстур (PNG, JPG, PSD, DDS).

**Версионные папки (необязательно):**
- `1.1`, `1.2`, `1.3`, `1.4`, `Common`
- Для контента, специфичного для версии.
- RimWorld загружает первую совместимую версионную папку + `Common` + корень.
- v1.0: Не распознает `Common`; папка `1.0` отключает загрузку корневой папки.
- Наиболее конкретный путь имеет приоритет в случае конфликтов файлов.

**LoadFolders.xml (необязательно):**
- Пользовательские правила загрузки папок.
- Переопределяет загрузку версионных папок по умолчанию (кроме v1.0).
- Пример:
    ```xml
    <loadFolders>
      <v1.4>
        <li>/</li>
        <li>1.4</li>
        <li IfModActive="AnotherMod.PackageId">OptionalFolder</li>
      </v1.4>
    </loadFolders>
    ```
- Используйте `IfModActive`/`IfModNotActive` для условной загрузки.
- Суффикс `_steam` для совместимости версий Steam Workshop в `IfModActive`.

**Ключевые папки для C# Modding:**
- **Data/Core:** Ванильные игровые Defs для справки.
- **RimWorld*_Data/Managed:** Игровые DLL для ссылок на C# modding.
- **Mods/YourModName/Assemblies:** Для пользовательского кода C#.

**Важные заметки:**
- Папки и имена файлов чувствительны к регистру.
- Папка `About` с файлом `About.xml` обязательна для распознавания мода.
- Используйте папки пространств имен или префиксы в `Sounds` и `Textures`, чтобы избежать конфликтов.
- `LoadFolders.xml` для расширенного управления версиями и зависимостями.
- `packageId` из `About.xml` используется для идентификации мода в `LoadFolders.xml`.
- `Preview.png` максимальный размер 1МБ для Steam Workshop.
- Папки `Core` и DLC: Аналогичны папкам модов.
- Папка `Source`: Для обмена исходным кодом; рекомендуется GitHub.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\core\modding-fundamentals.md`:

```````md
## Основы моддинга - шпаргалка

**Что такое моды?**
- Пользовательские модификации.
- Изменяют функциональность игры: добавляют, изменяют, удаляют контент.
- Предметы, текстуры, звуки, механики и т. д.
- Улучшают или изменяют игровой процесс.

**Типы модов:**
- **Косметические:** Только визуальные изменения (текстуры, стили). Не влияют на игровой процесс.
    - Примеры: Прически, одежда, цветовые вариации.
- **Пользовательский интерфейс (UI):** Улучшения качества жизни. Упрощают взаимодействие с игрой.
    - Примеры: Расширенный инвентарь, медицинские списки, наложения планирования.
- **Изменения в игре:** Изменяют основной игровой процесс, добавляют сложность, новые механики.
    - Примеры: Новые предметы, оружие, животные, механики, капитальный ремонт.
    - Могут повлиять на баланс игры.

**Получение модов:**
- **Steam Workshop:** (Версия Steam) - Простая загрузка, автоматические обновления.
- **Форумы Ludeon:** Ручная загрузка.
- **Сторонние веб-сайты (GitHub, Nexus Mods и т. д.):** Ручная загрузка, используйте с осторожностью.

**Установка модов:**
- **Steam Workshop:** "Подписаться" - Steam обрабатывает установку и обновления.
- **Ручная установка:**
    1. Загрузите файлы мода (ZIP).
    2. Извлеките в каталог `RimWorld/Mods/`.
        - Windows: `C:\Program Files (x86)\Steam\steamapps\common\RimWorld\Mods`
        - Mac: `~/Library/Application Support/Steam/steamapps/common/RimWorld/RimWorldMac.app/Mods`
        - Linux (standalone): `~/.steam/steam/steamapps/common/RimWorld/Mods`
        - Linux (GoG): `/home/<user>/GOG/Games/RimWorld/game/Mods/`
    3. Активируйте в игре: Главное меню -> Моды.
    4. Перезапустите RimWorld.

**Выбор модов:**
- Выбор игрока: На основе предпочтений и стиля игры.
- Нет "правильного" способа выбора модов.
- Совместимость модов: Большие коллекции модов могут иметь конфликты.
- Типы модов: Косметические, UI, Изменения в игре (баланс варьируется).

**Цели и подходы к моддингу:**

- **Добавить новый Thing:**
    - XML-запись `ThingDef`.
    - Начните с дублирования и изменения существующего `ThingDef` из Core XML.
    - Для похожих Things: Используйте абстрактный `ThingDef` в качестве `ParentName` для наследования.
    - Для зданий/растений: Убедитесь, что тег `category` установлен правильно.

- **Создать новую функциональность:**
    - Код C# для нового поведения.
    - Примените C# к Things через XML.
    - Только новая функциональность: Класс C#.
    - Thing с новой функциональностью:
        - Класс C#, наследующий `Thing`.
        - XML-запись `ThingDef` с `<thingClass>YourNamespace.YourNewClass</thingClass>`.
    - Thing с новыми XML-свойствами:
        - Класс C#, наследующий `ThingDef` для новых XML-полей.
        - XML `ThingDef` с `Class="YourNamespace.YourThingDef"`.
        - ИЛИ: `ThingComp` для существующего `ThingWithComps`, определите `CompProperties`, XML с `Class="YourNameSpace.MyCompProperties"`.

- **Изменить код RimWorld:**
    - XML (Defs): `PatchOperations` для изменения XML Defs (потребление энергии, расположение растений).
    - C# (Методы): `Harmony` для инъекции кода (префиксы, постфиксы, транспайлеры).

**Ключевые моменты:**
- Источники модов: Steam Workshop (самый простой), Форумы, Сторонние (ручная установка).
- Место ручной установки: `RimWorld/Mods/`.
- Активация: Внутриигровое меню модов, требуется перезапуск.
- Совместимость: Возможны конфликты с большими списками модов или перекрывающимися модами.
- Влияние типов модов: Косметические (визуал), UI (интерфейс), Изменения в игре (механики).
