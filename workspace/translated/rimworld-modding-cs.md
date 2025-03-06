Вот переводы документации Rimworld modding, сгруппированные по файлам, как в оригинале:

---
description: "cs" module Rimworld modding documentation
globs: 
alwaysApply: true
---
`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\assembly-modding-example.md`:

```````md
## Пример Assembly-моддинга - Чит-кодинг генератора темной материи RimWorld и анимированной ветряной турбины.
- Фокус: Изучение assembly и его структуры программы.
- Платформа: Только для Windows.

**Необходимые инструменты:**

- IDE: Рекомендуется IDE (например, Visual Studio).
- Знание C#: Базовое понимание.
- RimWorld: Базовая игра.

**Скачать проект:**

- GitHub: [https://github.com/HaploX1/ExampleDllMod](mdc:https:/github.com/HaploX1/ExampleDllMod)
- Форум Ludeon: [https://ludeon.com/forums/index.php?topic=3408.0](mdc:https:/ludeon.com/forums/index.php?topic=3408.0) (вложение)

**Подготовка (Ручные шаги):**

1. **Извлечь:** Скачанный проект в выбранную папку.
2. **Скопировать Assembly-CSharp.dll:**
   - Из: `RimWorld-Folder\RimWorldWin64_Data\Managed\`
   - В: `YourProjectFolder\RimWorld_ExampleProjectDLL\Source-DLLs\`
3. **Скопировать UnityEngine.CoreModule.dll:**
   - Из: `RimWorld-Folder\RimWorldWin64_Data\Managed\`
   - В: `YourProjectFolder\RimWorld_ExampleProjectDLL\Source-DLLs\`

**Вход в проект (Visual Studio):**

1. **Открыть проект:** Запустите Visual Studio и откройте `<YourExtractionFolder>\RimWorld\_ExampleProjectDLL.sln`.
2. **Изучить:** Структуру проекта и код внутри Visual Studio.
3. **Цель:** Создать мод генератора темной материи.

**Примечание:** Готовый мод с генератором энергии включен в проект.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\code-compatibility.md`:

```````md
## Совместимость кода - Шпаргалка

**Задача:** Создать совместимость между вашим модом и DLL другого мода, используя Harmony (ручное патчинг).

**Требования:**

- [x] Ваш Мод
- [x] Целевой Мод ("OtherMod")
- [x] Harmony
- [x] Настройка "Hello World"
- [x] Готовность к созданию совместимости

**Фрагмент ручного патчинга:**

```csharp
try
{
    ((Action)(() =>
    {
        if (LoadedModManager.RunningModsListForReading.Any(x=> x.Name == "OtherMod"))
        {
            harmony.Patch(AccessTools.Method(typeof(FullNameSpaceOfSomeOtherMod.SomeClass), nameof(FullNameSpaceOfSomeOtherMod.SomeClass.SomeOtherMethod)),
                postfix: new HarmonyMethod(typeof(HarmonyPatches), nameof(PatchOnSomeMethodFromSomeOtherMod_PostFix)));
        }
    }))();
}
catch (TypeLoadException ex) { }
```

**Объяснение:**

- **Цель:** Патчит метод из DLL "OtherMod" условно, если "OtherMod" загружен.
- **`try-catch` Block:** Обрабатывает `TypeLoadException`, если "OtherMod" отсутствует, предотвращая ошибки.
- **Анонимный делегат и Action:** Обходной путь, чтобы избежать прямой зависимости от "OtherMod" во время компиляции.
- **`LoadedModManager.RunningModsListForReading.Any(...)`:** Проверяет, находится ли "OtherMod" в списке активных модов.
- **`harmony.Patch(...)`:** Применяет патч Harmony, если "OtherMod" загружен.
    - `AccessTools.Method(...)`: Получает информацию о методе из "OtherMod" с использованием рефлексии.
    - `postfix: new HarmonyMethod(...)`: Указывает метод постфиксного патча в классе `HarmonyPatches`.

**Ограничения, обрабатываемые фрагментом:**

- **Опциональный Мод:** Обрабатывает случаи, когда "OtherMod" может быть активным/присутствующим или нет.
- **Нет прямой зависимости:** Избегает зависимости от DLL "OtherMod" во время компиляции.
- **Оптимизация компилятора:** Обходит проблемы встраивания компилятором и поведения JIT, которые могут сломать условный патчинг.

**Подводные камни:**

- **AssemblyVersion vs. FileVersion (AssemblyFileVersion):**
    - Избегайте ссылок на конкретную `AssemblyVersion` "OtherMod.dll".
    - Патчите против `FileVersion` (AssemblyFileVersion), если это возможно, или в идеале - без конкретной версии.
    - Изменения `AssemblyVersion` указывают на обратно несовместимые изменения API и могут сломать патчи.
- **Обновления ссылок:** Если "OtherMod" обновляет `AssemblyVersion`, ваш патч может сломаться.

**Лучшая практика:**
- Используйте этот подход ручного патчинга для необязательной совместимости с другими DLL модами.
- Предпочитайте патчинг против `FileVersion`, если необходимо указание версии.
- Рассмотрите возможность вообще не ссылаться на конкретные версии для более надежной совместимости.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\components-pattern.md`:

```````md
## Паттерн Компонентов - Шпаргалка

**Паттерн проектирования компонентов:** Модульная функциональность для объектов RimWorld.

### Типы Компонентов (от наиболее специфичных к наиболее общим):

- **HediffComp:**
    - Поведение, специфичное для Hediff (здоровье).
    - Просто использовать.
    - **Когда использовать:** Функциональность здоровья поселенца, поведение, специфичное для Hediff.

- **ThingComp:**
    - Поведение для конкретных Thing.
    - Мощный; хранение данных, специальная функциональность.
    - Строительный блок моддинга RimWorld.
    - Меньше проблем с совместимостью, чем у пользовательских классов.
    - **Когда использовать:** Функциональность уровня Thing, поведение предметов, хранение данных для Thing.

- **WorldObjectComp:**
    - Поведение для WorldObject.
    - Как ThingComp, но для WorldObject.
    - **Когда использовать:** Функциональность уровня WorldObject.

- **MapComponent:**
    - Поведение на уровне карты.
    - Отслеживает несколько вещей, хранение данных, управление сущностями.
    - Функциональность, специфичная для карты.
    - **Когда использовать:** Отслеживание на уровне карты, управление несколькими сущностями на карте.

- **WorldComponent:**
    - Поведение на уровне мира.
    - Глобальный охват.
    - **Когда использовать:** Отслеживание на уровне мира, поведение игрового мира.

- **GameComponent:**
    - Поведение на уровне игры.
    - Создается при запуске новой игры (обучение, настройка сценария, загрузка из главного меню).
    - Охват уровня игры.
    - **Когда использовать:** Отслеживание на уровне игры, поведение игровой сессии.

- **StorytellerComp:**
    - Поведение для рассказчиков.
    - Логика рассказчика.
    - **Когда использовать:** Настройка поведения рассказчика.

### Добавление Компонентов

**Добавление поведения к классу Thing:**
- Для `ThingWithComps` используйте `ThingComp`.
- `ThingComp`: Модульный подход для хранения данных и функциональности.
- **Интеграция с XML:**
  ```xml
  <comps>
    <li Class="YourNamespace.YourCompProperties"/>
  </comps>
  ```

**Добавление поведения к классу Hediff:**
- Для `HediffWithComps` используйте `HediffComp`.
- Аналогично `ThingComp`, но для Hediff.
- **Интеграция с XML:**
  ```xml
  <comps>
    <li Class="YourNamespace.YourHediffCompProperties"/>
  </comps>
  ```

### Структура Компонента (ThingComp в качестве примера)

**C# классы:**
1. **Класс CompProperties** - определяет статические данные и структуру XML
   ```csharp
   public class YourCompProperties : CompProperties
   {
       public bool yourProperty = true;
       
       public YourCompProperties()
       {
           compClass = typeof(YourComp);
       }
   }
   ```

2. **Класс Comp** - содержит логику и данные экземпляра
   ```csharp
   public class YourComp : ThingComp
   {
       public YourCompProperties Props => (YourCompProperties)props;
       
       // Переопределение методов
       public override void CompTick() { /* ваш код */ }
       public override void PostSpawnSetup(bool respawningAfterLoad) { /* ваш код */ }
       public override void PostExposeData() { /* сохранение данных */ }
       public override IEnumerable<Gizmo> CompGetGizmosExtra() { /* элементы UI */ }
   }
   ```

**Важные методы, доступные для переопределения:**
- `PostSpawnSetup(bool)`: Вызывается при создании или загрузке.
- `CompTick()`: Вызывается каждый тик (1/60 сек) для активных компонентов.
- `CompTickRare()`: Вызывается каждые 250 тиков для менее частых обновлений.
- `PostExposeData()`: Для сохранения/загрузки данных.
- `PostDrawExtraSelectionOverlays()`: Для рисования наложений.
- `PostDestroy(DestroyMode, Map)`: Вызывается при уничтожении Thing.
- `CompGetGizmosExtra()`: Добавляет кнопки/иконки к выбранному объекту.
- `CompInspectStringExtra()`: Добавляет информацию в инспектор объекта.

**Заметки по реализации CompUseEffect:**
- Метод `CanBeUsedBy` использует `AcceptanceReport` для возвращаемого типа.
- Пример реализации:
  ```csharp
  public override AcceptanceReport CanBeUsedBy(Pawn p)
  {
      if (p.Downed)
      {
          return "Cannot use: pawn is downed";
      }
      return true;
  }
  ```
- Возвращайте `true` для успеха или строковое сообщение для неудачи.
- Этот подход объединяет статус успеха/неудачи с сообщением причины.

### Сравнение с другими подходами

**ThingComp vs DefModExtension:**

**Когда использовать ThingComp:**
- Необходимы данные, специфичные для экземпляра (различаются для каждого экземпляра Thing)
- Необходимо подключиться к событиям tick/update
- Необходимо добавить функциональность в Thing (предметы, поселенцы, здания)
- Необходимо добавить элементы UI (гизмо, инспекторы)
- Необходимо сохранять/загружать данные с отдельными Thing

**Когда использовать DefModExtension:**
- Необходимы глобальные настройки для всех Thing одного типа Def
- Необходимо добавить данные в Def, не являющиеся Thing (RecipeDef, ResearchDef и т. д.)
- Необходимо легковесное хранение данных без поведения
- Необходимо избежать накладных расходов системы Comp

**ThingComp vs Harmony:**

**Когда использовать ThingComp:**
- Добавление новой функциональности к существующим Thing
- Хранение данных для конкретных экземпляров
- Модульное расширение функциональности
- Приоритизация совместимости с другими модами

**Когда использовать Harmony:**
- Изменение существующего поведения игры
- Патчинг конкретных методов
- Глобальные изменения поведения, не связанные с конкретными объектами
- Изменение общеигровых алгоритмов

Для получения подробной информации о Harmony обратитесь к руководству Harmony.

### Лучшие практики

- **Производительность:** Используйте `CompTickRare()` вместо `CompTick()`, когда это возможно.
- **Ясность кода:** Соблюдайте принцип единственной ответственности, каждый Comp должен делать что-то конкретное.
- **Совместимость:** Проверяйте наличие других Comps при взаимодействии, не полагайтесь на определенный порядок загрузки.
- **SaveData:** Всегда переопределяйте PostExposeData() для сохранения переменных экземпляра.
- **Cleanup:** Освобождайте ресурсы в PostDestroy(), если это необходимо.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\debug-actions.md`:

```````md
## DebugActions - Шпаргалка

**Цель:**
- Легко вызывать статические методы из меню отладки в режиме разработки.
- Доступно через кнопку шестеренки в строке отладки.

**Использование:**
- Добавьте атрибут `[DebugAction]` к статическим методам.
- Доступно в меню Debug Actions (режим разработчика).

**Параметры атрибута `[DebugAction]`:**
- **Категория (string):**
    - Организует действия в меню отладки.
    - Используется для группировки связанных действий.
- **Текст кнопки (string):**
    - Текст, отображаемый на кнопке отладки.
- **`actionType` (перечисление DebugActionType):**
    - Определяет поведение выполнения действия.
    - Возможные значения:
        - `Action`: Прямой вызов метода, любое состояние игры.
        - `ToolMap`: Мышь-таргетер, вызывает метод при каждом клике по карте, только для карты.
        - `ToolMapForPawns`: Мышь-таргетер, вызывает метод при клике по поселенцу, передает `Pawn p`, только для карты.
        - `ToolWorld`: Мышь-таргетер, вызывает метод при клике по клетке мира, только карта мира.
- **`allowedGameStates` (перечисление AllowedGameStates):**
    - Состояния игры, в которых DebugAction виден.
    - Комбинируемые значения перечисления:
        - `Invalid`: Никогда.
        - `Entry`: Главное меню.
        - `Playing`: После запуска игры.
        - `WorldRenderedNow`: После загрузки мира.
        - `IsCurrentlyOnMap`: Вход на карту.
        - `HasGameCondition`: Вход на карту, активное игровое событие.
        - `PlayingOnMap`: `Playing` И `IsCurrentlyOnMap`.
        - `PlayingOnWorld`: `Playing` И `WorldRenderedNow`.

**Значения перечисления `AllowedGameStates`:**
- `Invalid`: Никогда.
- `Entry`: Главное меню.
- `Playing`: В игре.
- `WorldRenderedNow`: Мир загружен.
- `IsCurrentlyOnMap`: На карте.
- `HasGameCondition`: Игровое событие активно на карте.
- `PlayingOnMap`: В игре и на карте.
- `PlayingOnWorld`: В игре и на карте мира.

**Примечание:**
- Декомпилируйте RimWorld, чтобы найти больше примеров `DebugAction`.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\expose-data.md`:

```````md
## Сохранение в RimWorld (ExposeData) - Шпаргалка

**Основная концепция:**
- Метод `ExposeData()` (переопределение virtual) в классах, таких как `Thing`, `ThingComp`, `HediffComp`, `LordJob`, `JobDriver`.
- Методы `Scribe` внутри `ExposeData()` для сохранения отдельных значений.

**Общие параметры метода Scribe:**
- `ref item`: Ссылка на переменную для сохранения (передается по `ref`).
- `"label"`: Уникальный ключ сохранения (string, появляется в файле сохранения).
- Дополнительные параметры: Значение по умолчанию, принудительное сохранение, аргументы конструктора и т. д.

**Сохранение типов данных:**

- **Простые значения (Scribe_Values.Look):**
    - Типы: `int`, `float`, `bool`, `long`, `double`, `sbyte`, Enums, `string`, `System.Type`, Vector Types, `Rect`, `Color`, `ColorInt`, `Rot4`, `CellRect`, `CurvePoint`, `FloatRange`, `IntRange`, `QualityRange`, `PublishedFileId_t`.
    - Параметры: `ref value`, `"label"`, `defaultValue` (опционально, для загрузки значений по умолчанию), `forceSave` (опционально, boolean).
    - Пример: `Scribe_Values.Look(ref foo, "foo", 10);` // int foo, значение по умолчанию 10, если нет сохранения.

- **Defs (Scribe_Defs.Look):**
    - Для: Сохранение ссылок на Defs.
    - Параметры: `ref defField`, `"label"`.
    - Пример: `Scribe_Defs.Look(ref myDef, "myDef");`

- **Things (Scribe_Deep.Look, Scribe_References.Look):**
    - Базовый класс: `Thing` (Pawn, Building, Mote, Plant и т. д.).
    - Два метода сохранения:
        - `Scribe_References.Look`: Для `ILoadReferenceable` Things (заспавнены, сохранены в другом месте).
        - `Scribe_Deep.Look`: Для `IExposable` Things (не заспавнены, не сохранены в другом месте).

    - **Сохранение IExposable Things (Scribe_Deep.Look):**
        - Для: Классов, реализующих `IExposable`.
        - Параметры: `ref exposableItem`, `saveDestroyedThings` (опционально, boolean), `"label"`, `ctorArgs` (опционально, аргументы конструктора для создания экземпляра).
        - Пример: `Scribe_Deep.Look(ref myExposable, "myExposable");`

    - **Сохранение ILoadReferenceable Things (Scribe_References.Look):**
        - Для: Классов, реализующих `ILoadReferenceable` (существующие в мире).
        - Параметры: `ref referenceableItem`, `"label"`, `saveDestroyedThings` (опционально, boolean).
        - Пример: `Scribe_References.Look(ref myReference, "myReference");`

- **TargetInfo (Scribe_TargetInfo.Look):**
    - Для: `TargetInfo`, `LocalTargetInfo`, `GlobalTargetInfo`.
    - Параметры: `ref targetInfo`, `"label"`, `saveDestroyedThings` (опционально, boolean), `defaultValue` (опционально, TargetInfo).
    - Пример: `Scribe_TargetInfo.Look(ref myLocation, "target");`

- **Коллекции (Scribe_Collections.Look):**
    - Для: `List`, `Dictionary`, `HashSet`, `Stack`.
    - Обязательно: Укажите `LookMode` для элементов коллекции.
    - Словари: Если LookMode для ключа или значения - `Reference`, укажите `ref` списки для ключей/значений.
    - Параметры: `ref collection`, `"label"`, `LookMode elementLookMode`, (опциональные параметры для словарей).
    - Примеры:
        - `Scribe_Collections.Look(ref list1, "list1", LookMode.Value);` // List<string>
        - `Scribe_Collections.Look(ref dict1, "dict1", LookMode.Value, LookMode.TargetInfo);` // Dictionary<string, LocalTargetInfo>
        - `Scribe_Collections.Look(ref dict2, "dict2", LookMode.Reference, LookMode.Value, ref list2, ref list3);` // Dictionary<Faction, int> со ссылочными списками для ключей и значений
        - `Scribe_Collections.Look(ref stack1, "stack1", LookMode.Reference);` // Stack<Thing>

**Важные примечания:**
-  Параметр `label` должен быть уникальным внутри метода `ExposeData()`.
-  Для `Scribe_Values.Look` значение по умолчанию устанавливается при загрузке, если ключ сохранения отсутствует.
-  Для `Scribe_Deep.Look` аргументы конструктора можно передать с помощью `ctorArgs`.
-  `Scribe_References.Look` требует, чтобы объект был сохранен в другом месте.
-  Коллекции требуют указания `LookMode` для элементов. Для словарей могут потребоваться дополнительные списки для сохранения.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\game-component-guide.md`:

```````md
## GameComponent, WorldComponent, MapComponent - Шпаргалка

**Общая информация о компонентах:**

- **Цель:** Добавление пользовательского кода и функциональности на разных уровнях (Игра, Мир, Карта).
- **Реализация:** Наследуйтесь от `GameComponent`, `WorldComponent` или `MapComponent`.
    - Реализуйте конструктор: `public MyComponent(TYPE type) : base(type) { }` (TYPE = Game, World или Map).
- **Доступ:** Используйте `GetComponent<MyComponent>()` из:
    - `Current.Game` (GameComponent)
    - `Find.World` (WorldComponent)
    - `Find.CurrentMap` или `thing.Map` или `thing.MapHeld` (MapComponent)
- **Преимущества:**
    - Хорошо поддерживается RimWorld.
    - Совместимость с сохранениями (в основном).
    - Функциональность глобального уровня.
    - Сохранение данных (`ExposeData`).
    - Управляемые RimWorld вызовы методов (например, `Update`, `Tick`).
    - Универсальная функциональность.
    - Всегда доступен.
- **Недостатки:**
    - Удаление мода: Безвредная одноразовая ошибка при загрузке.
    - Отсутствие функциональности, раскрытой через XML.

**Доступ к компонентам:**

- **MapComponent:**
    ```csharp
    thing.Map.GetComponent<MyMapComponent>();         // От Thing на карте
    thing.MapHeld.GetComponent<MyMapComponent>();     // От ThingHolder
    Find.CurrentMap.GetComponent<MyMapComponent>();  // От видимой карты

    // Метод безопасного доступа:
    public static MyMapComponent GetMapComponentFor(Map map) { /* проверки на null и создание экземпляра */ }
    ```

- **WorldComponent:**
    ```csharp
    Find.World.GetComponent<MyWorldComponent>();
    ```

- **GameComponent:**
    ```csharp
    Current.Game.GetComponent<MyGameComponent>();
    ```
    - **Загвоздка:** Конструктору компонента игры требуется параметр `Game`.

**Работа со списками поселенцев:**
- Используйте стандартный `List<Pawn>` для сохранения ссылок на поселенцев в компонентах
  ```csharp
  public List<Pawn> savedPawns = new List<Pawn>();
  ```
- Списки поселенцев автоматически сериализуются при сохранении в ExposeData

**Переопределяемые методы:**

- **TYPEComponentUpdate:** Обновление кадра, даже когда игра на паузе. Используется для обновлений, зависящих от частоты кадров (визуальные эффекты), а не для игровой логики.
- **TYPEComponentTick:** Обновление игрового тика. Обновления игровой логики.
- **TYPEComponentOnGUI:** Обновление кадра, когда TYPE виден. Рендеринг GUI (не WorldComponent).
- **ExposeData:** Сохранение/загрузка данных компонента.
- **FinalizeInit:** Вызывается после создания экземпляра TYPE, после конструктора. Хорошо подходит для инициализации, безопаснее, чем конструктор, для обработки ошибок.
- **StartedNewGame:** Только для GameComponent. При запуске новой игры.
- **LoadedGame:** Только для GameComponent. При загрузке игры.
- **MapGenerated:** Только для MapComponent. При генерации карты.
- **MapRemoved:** Только для MapComponent. При удалении карты.

**Заметки по использованию:**

- Проверки на null для `Map` имеют решающее значение, особенно для MapComponent.
- Выберите тип компонента на основе области действия (Игра > Мир > Карта > Thing).
- Переопределите методы по мере необходимости для желаемого поведения.
- Для сохранения данных реализуйте `ExposeData`.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\harmony-guide.md`:

```````md
## Harmony Шпаргалка

**Harmony:** Библиотека патчинга во время выполнения для методов .NET/Mono в RimWorld. Лучшая практика для модификации кода.

**Интеграция:**
- Добавьте Harmony как ссылку в C# проект.
- НЕ включайте `0Harmony.dll` в Assemblies мода.
- Добавьте Harmony как зависимость Steam.
- Избегайте HugsLib для базового Harmony; используйте только при необходимости функций HugsLib.

**Типы патчей:**

- **Prefix:**
    - Запускается **перед** оригинальным методом.
    - Тип возвращаемого значения: `void` или `bool`.
    - `bool` возвращает `false`: Пропускает оригинальный метод (используйте с осторожностью - риск совместимости).
    - Может помешать запуску других префиксов.

- **Postfix:**
    - Запускается **после** оригинального метода.
    - Гарантированное выполнение.
    - Может изменять `__result` оригинального метода (передавать по `ref`).
    - Рекомендуется для совместимости.

- **Transpiler:**
    - Изменяет code`.

**Изменение результатов:**

- Изменение возвращаемого значения оригинального метода:
    - Параметр метода Postfix `ref __result`.

**Ошибки:**

- **Злоупотребление:** Рассмотрите альтернативы (подклассы, Comps) перед Harmony.
- **Результат не изменен:** Отсутствует ключевое слово `ref` при изменении `__result`.
- **Возврат значения из Void/Prefix:** Тип возвращаемого значения Harmony != типу возвращаемого значения оригинального метода. Используйте `__result` для префиксов для изменения результата.
- **Неправильный патч метода:** Используйте `AccessTools.Method` или `System.Reflection.MethodInfo` для точной спецификации метода, особенно для перегруженных методов.
- **Патч не применяется:**
    - Проверьте `HarmonyInstance.DEBUG = true` & файл журнала Harmony.
    - Порядок начальной загрузки: `StaticConstructorOnStartup` для ранних патчей.
    - Встраивание: Патченный метод может быть встроенным; сложнее исправить.

**Начальная загрузка:**

- Инициализируйте Harmony в классе `[StaticConstructorOnStartup]`.
- Используйте `Harmony.PatchAll()` или ручное патчинг в статическом конструкторе.

**Ссылки:**

- [Harmony Mod (Steam)](mdc:https:/steamcommunity.com/workshop/filedetails/?id=2009463077)
- [Harmony 2 Releases (GitHub)](mdc:https:/github.com/pardeike/Harmony/releases)
- [Harmony 2 NuGet](mdc:https:/www.nuget.org/packages/Lib.Harmony)
- [Harmony 2 Docs](mdc:https:/harmony.pardeike.net)
- [Alien Races HarmonyPatches Example](mdc:https:/github.com/RimWorld-CCL-Reborn/AlienRaces/blob/master/Source/AlienRace/AlienRace/HarmonyPatches.cs)

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\mod-settings-guide.md`:

```````md
## ModSettings - Шпаргалка

**Цель:**

- Предоставление настраиваемых параметров для пользователей вашего мода в игре.

**Настройка:**

- Требуется два класса:
    - Класс `ModSettings`: Хранит данные настроек.
    - Класс `Mod`: Обрабатывает GUI настроек и загрузку/сохранение.

**Структура кода:**

- **ExampleSettings (Подкласс `ModSettings`):**
    ```csharp
    public class ExampleSettings : ModSettings
    {
        public bool exampleBool;
        public float exampleFloat = 200f;
        public List<Pawn> exampleListOfPawns = new List<Pawn>();

        public override void ExposeData() // Для сохранения настроек
        {
            Scribe_Values.Look(ref exampleBool, "exampleBool");
            Scribe_Values.Look(ref exampleFloat, "exampleFloat", 200f);
            Scribe_Collections.Look(ref exampleListOfPawns, "exampleListOfPawns", LookMode.Reference);
            base.ExposeData();
        }
    }
    ```
    - `ExposeData()`: Сохраняет/загружает настройки на диск, используя `Scribe_Values`, `Scribe_Collections` и т. д.

- **ExampleMod (Подкласс `Mod`):**
    ```csharp
    public class ExampleMod : Mod
    {
        ExampleSettings settings; // Ссылка на настройки

        public ExampleMod(ModContentPack content) : base(content) // Обязательный конструктор
        {
            this.settings = GetSettings<ExampleSettings>();
        }

        public override void DoSettingsWindowContents(Rect inRect) // GUI для настроек (необязательно)
        {
            Listing_Standard listingStandard = new Listing_Standard();
            listingStandard.Begin(inRect);
            listingStandard.CheckboxLabeled("Bool Setting", ref settings.exampleBool, "Tooltip");
            listingStandard.Label("Float Setting");
            settings.exampleFloat = listingStandard.Slider(settings.exampleFloat, 100f, 300f);
            listingStandard.End();
            base.DoSettingsWindowContents(inRect);
        }

        public override string SettingsCategory() // Делает настройки видимыми в меню (обязательно)
        {
            return "Mod Name"; // Используйте .Translate() для локализации
        }
    }
    ```
    - Конструктор (`ExampleMod(ModContentPack content)`): Получает ссылку на настройки: `settings = GetSettings<ExampleSettings>();`
    - `DoSettingsWindowContents(Rect inRect)`: Создает GUI настроек, используя `Listing_Standard` или `Widgets`.
        - `Listing_Standard`: Базовая разметка GUI, использует `Begin()` и `End()`.
        - `Widgets`: Больше контроля, ручное позиционирование Rect.
    - `SettingsCategory()`: Возвращает имя мода для отображения в меню настроек (требуется непустая строка).

**Сохранение настроек:**

- Автоматически: При закрытии окна.
- Необязательно: Переопределите `WriteSettings()` для пользовательского поведения сохранения.

**Доступ к настройкам:**

- Нестатический (на основе экземпляра):
    ```csharp
    LoadedModManager.GetMod<ExampleMod>().GetSettings<ExampleSettings>().settingName
    ```
- Рекомендуется: Сохраните ссылку на настройки в классе `Mod` для облегчения доступа.

**HugsLib vs Vanilla ModSettings:**

- Vanilla ModSettings: Достаточно для большинства случаев, нет внешней зависимости.
- HugsLib: Предлагает альтернативную реализацию настроек (не требуется для базовых настроек).

**Ключевые классы:**

- `ModSettings`: Базовый класс для данных настроек.
- `Mod`: Базовый класс для мода, обрабатывает GUI настроек и загрузку.
- `Listing_Standard`: Базовый класс разметки GUI.
- `Widgets`: Альтернативный класс GUI с большим контролем.
- `Rect`: Структура Unity для позиционирования элементов GUI.
- `Translator`: Для локализации (`"StringKey".Translate()`).

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\rimworld-classes-reference.md`:

```````md
## Полезные классы RimWorld для моддеров - Шпаргалка

**Требования:**
- Декомпилятор (ILSpy, dnSpy, Rider/dotPeek) для изучения кода игры.

**Категории классов:**

- **Gen:** (Классы, начинающиеся с "Gen") - Общие утилиты, методы расширения для:
    - `GenDate`, `GenLocalDate`: Операции, связанные со временем.
    - `GenDraw`: Рисование и графика.
    - `GenAdj`: Расчеты сетки и смежности.
    - `GenText`: Манипуляции с текстом (менее рекомендуется, используйте LINQ).

- **Utility:** (Классы, заканчивающиеся на "Utility") - Утилиты для конкретных задач:
    - `FoodUtility`: Потребление и обработка пищи.
    - `RestUtility`: Управление отдыхом и сном.
    - `PawnUtility`: Действия и проверки, связанные с поселенцами.
    - `MassUtility`: Расчеты массы.
    - `Area*`, `Roof*`, `Snow*`: Утилиты, связанные с областями, крышами и снегом.
    - `AggressiveAnimalIncidentUtility`: Обрабатывает нападения животных (ранее ManhunterPackIncidentUtility).

- **Maker:** (Классы, заканчивающиеся на "Maker") - Создание и управление объектами:
    - Создание: `Hediffs`, `Things`, `Lords`, `Zones`, `Sites`, `Filth`.

- **Tuning:** (Классы, заканчивающиеся на "Tuning") - Игровые константы:
    - Ссылка: Значения баланса игры и настройки (не для динамического доступа к значениям).

- **Find:** - Менеджеры игрового состояния и синглтоны:
    - `LetterStack`: Управление UI писем.
    - `Archive`: История игры и архивирование.
    Story event tracking.
    - `ResearchManager`: Управление системой исследований.
    - `World`: Доступ к состоянию мира.
    - `FactionManager`: Управление фракциями.

- **PawnsFinder:** - Поиск и фильтрация поселенцев.

- **Map:** - Данные и менеджеры уровня карты:
    - Доступ: `ListerBuildings`, `ListerThings`, `LordManagers`, `ZoneManager`.
    - `MapMeshFlagDef`: Определяет флаги сетки для рендеринга карты.
    - `MapMeshFlags`: Статические свойства для доступа к флагам сетки.

- **Pawn:** - Данные и компоненты поселенца:
    - Доступ: `Health`, `Needs`, `Inventory`, `Jobs`, `Story`, `Equipment`.
    - **Рендеринг поселенцев:**
        - `PawnRenderTree`: Система рендеринга поселенцев на основе узлов.
        - `PawnRenderNode`: Представляет узел в дереве рендеринга.
        - `PawnRenderNodeWorker`: Обрабатывает логику рендеринга для узлов.
        - `PawnRenderNodeProperties`: Данные, определяющие, как отображается узел.
        - `PawnRenderTreeDef`: Определяет структуру дерева рендеринга для конкретной расы.
        - `PawnRenderSubWorker`: Аддитивные работники для совместимости модов.
        - `AnimationDef`: Определяет анимацию поселенцев.

- **Thing:** - Базовый класс игрового объекта.
    - Важные методы:
        - `DynamicDrawPhaseAt(phase, drawLoc, flip)`: Точка входа для параллельного рендеринга.
        - `DrawAtNow(drawLoc, flip)`: Немедленный рендеринг без параллелизма.

- **DefDatabase:** - Поиск и доступ к Def:
    - Использование: `DefDatabase<DefType>.GetNamed("defName")`.

- **CellFinder:** - Поиск и фильтрация ячеек карты.

- **Command:** - UI-фреймворк команд (например, Gizmos).

- **Listing_Standard:** - Базовая UI-разметка для настроек и списков.

- **Gizmo:** - Внутриигровые UI-элементы (кнопки, иконки).

- **BoolUnknown:** - Nullable boolean type.

- **LoadedModManager:** - Загрузка и управление модами:
    - Доступ: `LoadedModManager.RunningModsListForReading` (получить загруженные моды).

- **Pair<T1, T2>:** - Generic key-value pair (legacy, consider tuples).

- **Rand:** - Генерация случайных чисел (используйте с осторожностью в мультиплеере).

- **LudeonTK:** - Пространство имен для классов отладки и утилит (перемещено из Verse):
    - `DebugActionAttribute` (aka `[DebugAction]`)
    - `DebugActionNode`
    - `DebugActionType`
    - Различные классы, связанные с отладкой.

**Классы зданий и предметов:**
- `BuildingProperties`: Имеет флаг `isAttachment` для предметов, установленных на стене.
- `IHaulEnroute`: Интерфейс для предметов, которые могут перевозиться несколькими поселенцами.
- `HiddenItemsManager`: Управляет неоткрытыми предметами с тегом `<hiddenWhileUndiscovered>`.
- `Building_MultiTileDoor`: Класс для декоративных дверей, охватывающих несколько плиток.

**Классы Incident Worker:**
- `IncidentWorker_AggressiveAnimals`: Обрабатывает инциденты нападения животных (ранее ManhunterPack).

**Важные расположения пространств имен:**
- `IngredientValueGetter`: Находится в пространстве имен RimWorld.
- Классы, связанные с отладкой: Находятся в пространстве имен LudeonTK.

```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\transpiler-hints.md`:

```````md
## Подсказки для Транспилеров - Шпаргалка

**Отладка Транспилеров:**

- Отладка транспилеров сложна из-за манипуляций с IL-кодом.
- Точный подсчет инструкций имеет решающее значение для избежания исключений.

**Полезные примеры:**

- Поищите "Rimworld Harmony Transpiler" на GitHub, чтобы найти примеры от сообщества.
  - [Ссылка для поиска на GitHub](mdc:https:/github.com/search?o=desc&q=Rimworld+Harmony+Transpiler&s=indexed&type=Code)

**Инструменты отладки:**

- **Печать IL-кода:** Используйте фрагмент кода для вывода измененного IL-кода для проверки.

**Фрагмент кода отладки:**

```csharp
public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions, ILGenerator generator)
{
    var l = XTranspiler(instructions, generator).ToList(); // Фактический транспилер: XTranspiler
    string s = "Code:";
    int i = 0;
    foreach (var c in l)
    {
        if (c.opcode == OpCodes.Call || c.opcode == OpCodes.Callvirt)
        {
            Log.Warning("" + i + ": " + c); // Выделить операции Call/Callvirt
        }
        else
        {
            Log.Message("" + i + ": " + c);
        }
        s += "\n" + i + ": " + c;
        i++;
        yield return c;
    }
    Log.Error(s); // Полный вывод кода для анализа в текстовом редакторе
}
```

**Объяснение фрагмента:**

- `XTranspiler()`: Замените именем вашего фактического метода-транспилера.
- `ToList()`: Преобразует `IEnumerable` в `List` для облегчения итерации.
- `Log.Warning()`: Выделяет опкоды `Call` и `Callvirt` в выводе.
- `Log.Message()`: Стандартный вывод для других опкодов.
- `Log.Error(s)`: Выводит полный IL-код в виде журнала ошибок для легкого копирования.
- `yield return c`: Возвращает исходную `CodeInstruction` после регистрации.

**Совет по использованию:**

- Используйте предоставленный фрагмент кода для вывода и анализа сгенерированного IL-кода.
- Проверьте вывод на корректность, особенно подсчет инструкций и последовательности опкодов.
- Поищите примеры транспилеров на GitHub, чтобы понять шаблоны реализации.

<think>Судя по контексту, задание - перевести на русский язык файл `\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\tweak-value.md`. Файл содержит шпаргалку по использованию функции TweakValue в процессе моддинга игры RimWorld. 

Я уже перевел файл `rimworld-classes-reference.md` и `transpiler-hints.md` на русский раньше, теперь нужно перевести этот файл аналогичным образом.

Я сохраню структуру документа, формат кода и основные концепции, но переведу весь текст на русский язык.</think>```language:workspace/translated/rimworld-modding-cs.md
`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\tweak-value.md`:

```````md
## TweakValue - Шпаргалка

**Цель:**

- Модификация значений во время выполнения.
- Прямая настройка значений в игре без перекомпиляции.
- Идеально для настройки UI и итеративного тестирования значений.

**Применимые поля:**

- Только `static` поля.
- Не `constant` и не `readonly`.
- Типы данных: `float`, `bool`, `int`, `ushort`.

**Использование:**

- Доступ через кнопку "TweakValues" в режиме разработчика (иконка с тире и точками).
- Настройка значений с помощью ползунков в окне TweakValues.
- Изменения временные, не сохраняются при закрытии игры.
- Записывайте хорошие значения для постоянной реализации в моде.

**Синтаксис:**

```csharp
[TweakValue("имяКатегории", минЗначение, максЗначение)]
private static типДанных имяПоля = значениеПоУмолчанию;
```

- `[TweakValue("имяКатегории", минЗначение, максЗначение)]`: Аннотация атрибута.
    - `"имяКатегории"`: Строка для группировки в окне TweakValues.
    - `минЗначение`, `максЗначение`: Необязательные мин/макс значения для ползунка (по умолчанию: 0, 100).
- `private static типДанных имяПоля`: Объявление поля.
    - `static`: Обязательно для работы TweakValue.
    - `private`: Рекомендуется для инкапсуляции (может быть `public` или `protected`).
    - `типДанных`: `float`, `bool`, `int` или `ushort`.
    - `имяПоля`: Имя поля.
    - `значениеПоУмолчанию`: Начальное значение.

**Пример:**

```csharp
#if DEBUG // Рекомендуется: условная компиляция только для отладочных сборок
[TweakValue("UI_Tweaks", -100f, 4000f)]
#endif
private static float exampleFloat = 2000f;
```

**Советы:**

- Используйте `#if DEBUG` или комментарии для релизных сборок, чтобы избежать спама TweakValue.
- Категоризируйте TweakValues с помощью описательных строк `имяКатегории` для организации в окне TweakValues (алфавитный порядок).
- Используйте префиксы типа `AAA` для группировки связанных значений в верхней части списка TweakValues.
- Документируйте хорошие настройки TweakValue, так как они не являются постоянными.

**Связанные инструменты (Ссылки не включены):**

- Method Reloader (Перезагрузчик методов)
- Texture Reloader (Перезагрузчик текстур)

```````
```


```````

`\\?\C:\Projects\rimworld\wiki_workspace\wiki\cs\writing-custom-code.md`:

```````md
## Написание пользовательского кода - Шпаргалка

**Цель:**

- Расширение функциональности RimWorld за пределы XML.
- Создание автономных классов C# или патчей Harmony.

**Создание класса:**

- Настройка IDE: Используйте Visual Studio, Rider или другие IDE.
- Тип проекта: Библиотека классов (.NET Framework 4.8).
- Путь вывода: `(RimWorldInstallFolder)/Mods/(YourModName)/Assemblies`.
- Ссылки: Добавьте `Assembly-CSharp.dll`, `UnityEngine.CoreModule.dll` из `RimWorldWin64_Data\Managed\`.
- Операторы `using`: Импортируйте пространства имен (например, `using System;`, `using RimWorld;`, `using Verse;`).
- `namespace`: Организуйте код, предотвратите конфликты именования. Используйте уникальное пространство имен (например, `YourName.YourModName`).
- Определение класса: `public class MyClassName { ... }`.
- Конструктор: `public MyClassName() { ... }` - Инициализируйте экземпляры класса.

**Пространства имен (полезные):**

- **Общие:**
    - `System.Collections.Generic`: `List<>`, `Dictionary<>`, `IEnumerable<>`.
    - `System.Linq`: LINQ-операции (`.Where()`, и т. д.).
    - `System.Text.RegularExpressions`: RegEx для манипуляции строками.
    - `System.Collections`: Интерфейс `IEnumerable`.
    - `System.Text`: `StringBuilder` для эффективного построения строк.

- **Специфичные для RimWorld:**
    - `RimWorld`: Основные классы RimWorld.
    - `Verse`: Базовые игровые классы, общая функциональность игры.
    - `RimWorld.BaseGen`: Генерация поселений.
    - `RimWorld.Planet`: Классы, связанные с миром.
    - `Verse.AI`: ИИ пешек, работы.
    - `Verse.AI.Group`: ИИ отряда.
    - `Verse.Grammar`: Генерация текста, локализация.
    - `UnityEngine`: GUI, Rect, Color.
    - `LudeonTK`: Отладочные действия и служебные классы, перемещенные из Verse.

**Workflow написания кода:**

1. **Декомпилируйте исходный код:** Изучите существующий игровой код для справки.
2. **Скомпилируйте в DLL:** Используйте IDE (Build/Compile) для создания `.dll` в папке `Assemblies`.
3. **Ссылка в XML (необязательно):** Свяжите код C# с XML Defs, используя `<thingClass>`, `<compClass>` и т. д.
    - Пример: `ThingDef` с пользовательским `thingClass`, указывающим на класс C#.
4. **Протестируйте в игре:** Запустите RimWorld; пользовательские классы должны загрузиться.

**Примеры (Быстрый старт):**

- Hello World Mod: Базовый мод C# для тестирования установки.
- Assembly Modding Example: Пример небольшого проекта моддинга.
- Plague Gun Tutorial: Пошаговое руководство для мода оружия (более сложное).

**Ключевые моменты:**

- Пространства имен: Имеют решающее значение для организации кода и предотвращения конфликтов.
- Директивы `using`: Упростите код, импортируя пространства имен.
- Конструкторы: Инициализируйте экземпляры класса.
- Связывание C# и XML: Мост между кодом и определением контента.
- Отладка: Используйте декомпилированный код для справки; обратитесь за помощью в сообществах моддинга.
- Компиляция: Убедитесь в правильности .NET Framework и настроек вывода.
