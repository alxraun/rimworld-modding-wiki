## Modding Mechanisms Cheat Sheet

### 1. XML Defs

**Назначение:** Декларативное определение игрового контента (вещи, рецепты, фракции и т.д.) в XML файлах.

**Структура:**
- Файлы находятся в папке `Defs/` внутри мода.
- Корневой элемент: `<Defs>`.
- Каждый Def определяется тегом `<DefType>` (например, `<ThingDef>`, `<RecipeDef>`).
- Теги внутри `<DefType>` соответствуют публичным полям в связанном C# Def классе.
- XML файлы должны начинаться с `<?xml version="1.0" encoding="utf-8"?>`.

**Наследование:**
- `Abstract="True"` в `<Def Name="ParentName" Abstract="True">` определяет шаблон Def, который не загружается напрямую.
- `ParentName="ParentDefName"` в `<Def ParentName="ParentDefName">` наследует от родительского Def.
- Дочерние Def наследуют и могут переопределять свойства родительского Def.
- `Inherit="False"` на теге в дочернем Def предотвращает наследование для этого конкретного тега.

**Пользовательские Defs:**
- Создавайте новые типы Def, создавая подклассы существующих классов Def в C#.
- Связывайте пользовательские классы Def в XML, используя атрибут `Class`: `<ThingDef Class="YourNamespace.YourCustomDefClass">`.
- Получайте доступ к пользовательским полям в C#, выполняя приведение типов: `var customDef = thing.def as YourNamespace.YourCustomDefClass;`.

**Доступ к Defs в C#:**
- **DefOf:** Статический класс с атрибутом `[DefOf]`, автоматически заполняемые статические поля для прямого доступа к Def (например, `MyDefOf.ThingDefName`). Требуется `DefOfHelper.EnsureInitializedInCtor`.
- **DefDatabase:** Используйте `DefDatabase<DefType>.GetNamed("defName")` для поиска Def во время выполнения.
### 2. C# Компоненты

**Назначение:** Добавление модульной функциональности к игровым объектам (Things, Hediffs, Maps, Worlds, Games).

**Типы:**
- **ThingComp:** Для `ThingWithComps`, добавляет поведение и данные к конкретным Things.
- **HediffComp:** Для `HediffWithComps`, специфичен для Hediffs (эффекты здоровья).
- **MapComponent:** Поведение и управление данными на уровне карты.
- **WorldComponent:** Область действия уровня мира, поведение игрового мира.
<!-- - **GameComponent:** Область действия уровня игры, поведение игровой сессии. -->
- **StorytellerComp:** Настраивает поведение Storyteller.

**Структура:**
- **CompProperties class:** Определяет статические данные и структуру XML, наследуется от `CompProperties`, устанавливает `compClass` на тип класса Comp.
- **Comp class:** Содержит логику и данные экземпляра, наследуется от `ThingComp`, `HediffComp` и т.д.
- Доступ к `CompProperties` через свойство `Props` в классе Comp: `YourCompProperties Props => (YourCompProperties)props;`.

**Добавление компонентов в XML:**
- Для `ThingWithComps` и `HediffWithComps` используйте тег `<comps>` в Def XML.
- Добавьте `<li>` с `Class="YourNamespace.YourCompProperties"` внутри `<comps>`.

**Ключевые методы (переопределить в классе Comp):**
- `PostSpawnSetup(bool respawningAfterLoad)`: Вызывается, когда Thing/Hediff появляется или загружается.
- `CompTick()`: Вызывается каждый игровой тик (1/60 сек) для активных компонентов.
- `CompTickRare()`: Вызывается каждые 250 тиков для менее частых обновлений.
- `PostExposeData()`: Для сохранения и загрузки данных компонента.
- `CompGetGizmosExtra()`: Добавляет гизмо (кнопки) к пользовательскому интерфейсу объекта.

**Доступ к компонентам в C#:**
- `thing.Map.GetComponent<MyMapComponent>()` (для MapComponent).
- `Find.World.GetComponent<MyWorldComponent>()` (для WorldComponent).
- `Current.Game.GetComponent<MyGameComponent>()` (для GameComponent).
- `thing.GetComp<YourComp>()` (для ThingComp).
- `hediff.TryGetComp<YourHediffComp>()` (для HediffComp).

### 3. Harmony Patching

**Назначение:** Изменение существующего игрового кода во время выполнения без изменения исходного кода, для совместимости и изменения поведения.

**Типы патчей:**
- **Prefix:** Выполняет код *до* оригинального метода. Может пропустить выполнение оригинального метода, вернув `false` (используйте с осторожностью).
- **Postfix:** Выполняет код *после* оригинального метода, гарантированное выполнение. Может изменять возвращаемое значение оригинального метода через `ref __result`.
- **Transpiler:** Непосредственно манипулирует IL-кодом метода для сложных изменений.

**Начальная загрузка:**
- Инициализируйте Harmony в классе с атрибутом `[StaticConstructorOnStartup]` для раннего патчинга.
- Используйте `Harmony.PatchAll()` для автоматического патчинга всех методов с атрибутами Harmony в сборке, или используйте ручной патчинг.

**Нацеливание на метод:**
- Используйте `AccessTools.Method(typeof(ClassName), nameof(MethodName))` для получения `MethodInfo` для патчинга, особенно для перегруженных методов.

**Методы патчей:**
- Статические методы с определенными параметрами: `__instance`, `__params`, `__result`, `__exception`, `__originalMethod`.
- Методы Prefix могут возвращать `bool`, чтобы пропустить оригинальное выполнение.
- Методы Postfix могут изменять `ref __result`.

**Ключевые моменты:**
- Избегайте чрезмерного использования; сначала рассмотрите альтернативы, такие как Comps или подклассы.
- Проверяйте журналы Harmony (`HarmonyInstance.DEBUG = true`) на наличие проблем с патчингом.
- Помните о потенциальном встраивании пропатченных методов.

### 4. Subclassing Defs

**Назначение:** Создание специализированных типов Def с расширенной функциональностью и данными, наследующих от существующих классов Def.

**Реализация:**
- Создайте новый класс C#, который наследуется от существующего класса Def (например, `class MyCustomThingDef : ThingDef`).
- Добавьте пользовательские поля и свойства к подклассу.
- В XML используйте атрибут `Class` в определении Def, чтобы связать с пользовательским подклассом: `<ThingDef Class="YourNamespace.MyCustomThingDef">`.

**Использование:**
- Для сложных модификаций Def, когда добавление Comps или DefModExtensions недостаточно.
- Чтобы ввести новые XML-теги, соответствующие пользовательским полям в подклассе.
- Доступ к пользовательским полям в C#, приведя `def` к типу пользовательского подкласса.

**Соображения:**
- Может вызвать проблемы совместимости, если несколько модов создают подклассы одного и того же типа Def.
- Менее гибко, чем использование Comps для добавления поведения.
- Требуется приведение типов для доступа к членам, специфичным для подкласса.
### 5. XML патчинг

**Назначение**: Изменение существующих XML Defs без их перезаписи, для обеспечения совместимости модов.

**Механизм**:
- Используйте XML-файлы патчей, расположенные в папке `Patches/` мода.
- Файлы патчей содержат корневой элемент `<Patch>` и теги `<Operation>` для модификаций.
- Указывайте целевые XML-узлы, используя выражения XPath.

**Основы XPath**:
- `Defs/DefType[predicate]/nodeToTarget` для выбора узлов.
- `DefType`: Тип Def (например, `ThingDef`, `RecipeDef`).
- `[predicate]`: Условия для выбора (например, `[defName="Wall"]`).
- `nodeToTarget`: Тег для изменения (например, `statBases`).

**Типы PatchOperation**:
- **PatchOperationAdd**: Добавляет дочерний узел.
- **PatchOperationInsert**: Вставляет соседний узел.
- **PatchOperationRemove**: Удаляет узел.
- **PatchOperationReplace**: Заменяет узел.
- **PatchOperationAttributeAdd/Set/Remove**: Изменяет XML-атрибуты.
- **PatchOperationSequence**: Выполняет список операций последовательно.
- **PatchOperationConditional**: Выполняет операции условно на основе XPath-теста.
- **PatchOperationFindMod**: Выполняет операции условно на основе наличия мода/DLC.
- **PatchOperationAddModExtension**: Добавляет `DefModExtension` к Def.

**Example Structure:**
```xml
<Patch>
  <Operation Class="PatchOperationReplace">
    <xpath>Defs/ThingDef[defName="Gun_AssaultRifle"]/verbs/li/defaultProjectile</xpath>
    <value>
      <defaultProjectile>Bullet_Explosive</defaultProjectile>
    </value>
  </Operation>
</Patch>
```

**Ключевые моменты:**
- XPath чувствителен к регистру.
- Используйте XML-валидаторы для проверки синтаксиса.
- Тестируйте патчи постепенно и проверяйте `Player.log` на ошибки.
- Для лучшей совместимости модов, патчинг важнее перезаписи Defs.
