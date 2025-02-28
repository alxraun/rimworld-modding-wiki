## Modding Tutorials/Sounds - Cheat Sheet

**Sound System Elements:**

- **Sound Event:** Signal from game code to play a sound. Defined in game code.
- **SoundDef:** XML definition of sound behavior. Controls sound file playback, variations, parameters. Edited in XML or in-game (legacy editor, XML editing recommended).
- **Sound File:** Raw audio file (wav, ogg, mp3) referenced by SoundDef. Created and edited externally.

**Getting Sound Editor Started (Legacy - XML Editing Recommended):**
- Enable Development Mode in game options.
- Access Package Editor (No longer in UI - Edit XML directly).
- Select SoundDef type.
- Choose/create mod package.
- Create/Open SoundDef package.
- Create new SoundDef.
- Select SoundDef for editing.

**Adding Sound to Game (XML Method):**
1. **Identify Sound Event Name:** Find the game event name triggering the desired sound (see external list if available).
2. **SoundDef `defName`:** Set `defName` in your SoundDef XML to match the game event name.
    - Use `eventNames` list for multiple event triggers.
3. **Create Sound File:** Make `.wav`, `.ogg`, or `.mp3` sound file.
4. **Place Sound File:** In `Sounds` folder of your mod.
5. **Edit SoundDef XML:**
    - Add `<subSounds>` list.
    - Add `<li>` within `<subSounds>`.
    - Add `<grains>` list within `<li>`.
    - Add `<li Class="AudioGrain_Clip">` within `<grains>`.
    - Set `<clipPath>` to sound file path relative to mod's root (`Sounds/Path/To/YourSound`, no extension).
6. **Test Sound:** In-game, trigger the sound event.

**How SoundDefs Work:**

- Game code triggers sound events by name.
- Sound system searches loaded mods for matching `SoundDef` by `defName`.
- `SoundDef` controls sound playback behavior (files, parameters, etc.).
- XML defines `SoundDef` properties.

**Sound Parameters:**

- Sound events can pass parameters (e.g., `ImpactSpeed`, `FireSize`, `CameraAltitude`).
- Parameters drive dynamic changes in sound properties (volume, pitch, etc.).
- Applied to: Sustainers (continuous sounds) or OneShots (single play sounds).
- Update Frequency:
    - Once per sample: Parameter value fixed at sound start.
    - Constantly: Parameter value updates every frame (dynamic changes).
- Parameter Examples: `ImpactSpeed` (bullet impact volume), mouse cursor speed (button mouseover pitch).
- Parameter Types: Defined in [Sound events and parameters list](https://docs.google.com/spreadsheet/pub?key=0AgWNUTNe4QC7dGpXa2ZHQTdPSU8yOVdrMllfbV9leEE&output=html) (external link).

**Key SoundDef Elements:**

- `<defName>`: Unique identifier for the SoundDef.
- `<eventNames>`: List of game event names that trigger this SoundDef.
- `<subSounds>`: List of sub-sounds to play.
- `<grains>`: List of audio samples (grains) for a sub-sound.
- `<clipPath>`: Path to the sound file within the mod's `Sounds` folder.
- `<volumeRange>`: Random volume variation.
- `<pitchRange>`: Random pitch variation.
- `<paramMappings>`: Map game parameters to sound properties.
- `<sustainers>` vs. `<oneshots>`: Sound playback behavior.
- `<filters>`: Audio filters (e.g., distance-based volume).

