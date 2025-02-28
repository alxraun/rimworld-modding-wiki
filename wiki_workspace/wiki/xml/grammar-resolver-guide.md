## GrammarResolver - Cheat Sheet

**Purpose:**
- Customize text strings dynamically based on game objects (pawns, things, factions).
- Used for descriptions, tooltips, messages.
- Employs symbols within strings for dynamic content.

**Symbols:**
- Format: `{KEY_subsymbol}`.
    - `KEY`: Object identifier (e.g., `PAWN`, `FACTION`, `THING`).
    - `subsymbol`: (Optional) Specifies object property and formatting.

**Thing Subsymbols:**
- `{THING}`: Indefinite label (e.g., "a chair").
- `{THING_label}`: `def.label` (e.g., "chair").
- `{THING_labelShort}`: Short label.
- `{THING_definite}`: "the" + label (e.g., "the chair").
- `{THING_indefinite}`: "a" or "an" + label (e.g., "a chair").
- `{THING_labelPlural}`: Plural label (e.g., "chairs").
- `{THING_labelPluralDef}`: "the" + plural label (e.g., "the chairs").
- `{THING_labelPluralIndef}`: Indefinite plural label (e.g., "chairs").
- `{THING_pronoun}`: Pronoun (e.g., "it").
- `{THING_possessive}`: Possessive pronoun (e.g., "its").
- `{THING_objective}`: Objective pronoun (e.g., "it").
- `{THING_factionName}`: Faction name (for Things with factions).
- `{THING_gender}`: Gender symbol.

**Pawn Subsymbols (Extends Thing Subsymbols):**
- `{PAWN_labelShort}`: Pawn's short name (e.g., "Alice").
- `{PAWN_nameFull}`: Full name, defaults to `kindIndef` (e.g., "Alison 'Alice' Abbey").
- `{PAWN_nameFullDef}`: Definite full name, defaults to `kindDef`.
- `{PAWN_nameDef}`: Definite name (e.g., "Alice").
- `{PAWN_nameIndef}`: Indefinite name (e.g., "Alice").
- `{PAWN_kind}`: Pawn kind based on pawnkind, gender, lifestage (e.g., "colonist").
- `{PAWN_kindDef}`: Definite kind (e.g., "the colonist").
- `{PAWN_kindIndef}`: Indefinite kind (e.g., "a colonist").
- `{PAWN_kindPlural}`: Plural kind (e.g., "colonists").
- `{PAWN_kindPluralDef}`: Definite plural kind (e.g., "the colonists").
- `{PAWN_kindPluralIndef}`: Indefinite plural kind (e.g., "colonists").
- `{PAWN_kindBase}`: PawnKindDef label (e.g., "colonist").
- `{PAWN_lifestage}`: Lifestage noun (e.g., "adult").
- `{PAWN_lifestageDef}`: Definite lifestage noun (e.g., "the adult").
- `{PAWN_lifestageIndef}`: Indefinite lifestage noun (e.g., "an adult").
- `{PAWN_lifestageAdjective}`: Lifestage adjective (e.g., "adult").
- `{PAWN_title}`: Backstory title (e.g., "chef").
- `{PAWN_titleDef}`: Definite title (e.g., "the chef").
- `{PAWN_titleIndef}`: Indefinite title (e.g., "a chef").
- `{PAWN_humanlike}`: "Humanlike" or blank.

**Faction Subsymbols:**
- `{FACTION}`: Faction name (e.g., "Alicetopia").
- `{FACTION_name}`: Faction name (e.g., "Alicetopia").
- `{FACTION_pawnSingular}`: Faction pawn singular label (e.g., "colonist").
- `{FACTION_pawnSingularDef}`: Definite singular pawn label (e.g., "the colonist").
- `{FACTION_pawnSingularIndef}`: Indefinite singular pawn label (e.g., "a colonist").
- `{FACTION_pawnsPlural}`: Faction pawn plural label (e.g., "colonists").
- `{FACTION_pawnsPluralDef}`: Definite plural pawn label (e.g., "the colonists").
- `{FACTION_pawnsPluralIndef}`: Indefinite plural pawn label (e.g., "colonists").

**Misc Subsymbols (Strings & Integers):**

- **Strings:**
    - No subsymbol: Printed as-is.
    - `plural`, `pluralDef`, `pluralIndef`, `definite`, `indefinite`, `pronoun`, `possessive`, `objective`, `gender`: Resolver's best guess for noun subsymbols.
- **Integers:**
    - No subsymbol: Printed as integer.
    - `ordinal`: Integer as ordinal (e.g., "1st").
- **Other Objects:**
    - No subsymbol: `ToString()` method invoked.

**Invoking Resolver (C#):**

- `.Formatted(object.Named("KEY"))`
    - `string`: Text with symbols.
    - `object`: Object to resolve symbols against.
    - `KEY`: Symbol key (e.g., "PAWN", "FACTION").

**AdjustedFor Resolver (C# - Battle Log):**

- `.AdjustedFor(pawn, "KEY")`
    - `string`: Text with symbols.
    - `pawn`: Pawn to resolve symbols against.
    - `KEY`: Symbol key.
- Supported keywords: `nameFull`, `nameShort`, `nameShortDef`, `definite`, `nameShortIndef`, `indefinite`, `pronoun`, `possessive`, `objective`, `factionName`, `kind`, `title`.
