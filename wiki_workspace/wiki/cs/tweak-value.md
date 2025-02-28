
## TweakValue - Cheat Sheet

**Purpose:**

- Run-time value modification.
- Direct in-game value tweaking without recompiling.
- Ideal for UI adjustments and iterative value testing.

**Applicable Fields:**

- `static` fields only.
- Non-`constant` and non-`readonly`.
- Data types: `float`, `bool`, `int`, `ushort`.

**Usage:**

- Access via Dev-Mode "TweakValues" button (dashes and dots icon).
- Adjust values using sliders in the TweakValues window.
- Changes are temporary, not saved on game close.
- Note down good values for permanent mod implementation.

**Syntax:**

```csharp
[TweakValue("categoryName", minVal, maxVal)]
private static dataType fieldName = defaultValue;
```

- `[TweakValue("categoryName", minVal, maxVal)]`: Attribute annotation.
    - `"categoryName"`: String for grouping in TweakValues window.
    - `minVal`, `maxVal`: Optional min/max values for slider (defaults: 0, 100).
- `private static dataType fieldName`:  Field declaration.
    - `static`: Required for TweakValue to work.
    - `private`: Recommended for encapsulation (can be `public` or `protected`).
    - `dataType`: `float`, `bool`, `int`, or `ushort`.
    - `fieldName`: Field name.
    - `defaultValue`: Initial value.

**Example:**

```csharp
#if DEBUG // Recommended: Conditional compilation for debug builds only
[TweakValue("UI_Tweaks", -100f, 4000f)]
#endif
private static float exampleFloat = 2000f;
```

**Tips:**

- Use `#if DEBUG` or comments for release builds to prevent TweakValue spam.
- Categorize TweakValues with descriptive `categoryName` strings for organization in the TweakValues window (alphabetical order).
- Use prefixes like `AAA` to group related values at the top of the TweakValues list.
- Document good TweakValue settings as they are not persistent.

**Related Tools (Links not included):**

- Method Reloader
- Texture Reloader
```
