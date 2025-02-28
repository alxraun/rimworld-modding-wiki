## Modding Tutorials/Testing Mods - Cheat Sheet

**Debug Modes:**

- **Development Mode:**
    - Activation: Options Menu -> Gameplay -> Tick "Development mode".
    - Icons (Top-Right):
        - Debug Log: Opens debug log window.
        - Package Editor: Modifies SoundDefs (limited mod use).
        - View Settings: Graphic overlays, cheats (Fast Research).
        - Debug Actions Menu:
            - Incidents: Trigger raids, events.
            - Actions - Misc: Complete research, tutorial.
            - Tools - General: Explosions, snow, plant growth, AI visualizations.
            - Tools - Pawns: Pawn appearance, levels, health, jobs.
            - Tools - Spawning: Spawn pawns, items, terrain, filth.
            - Autotests: Stress tests (colony spawn, burning, pawn killing).
        - Debug Logging Menu: Toggle game logging in Debug Log.
        - Inspector: Detailed info on hovered objects.
        - God Mode: Toggle free building.
        - Pause on Error: Pause game on log errors.

- **Using Debugger:**
    - RimWorld uses Mono runtime.
    - Debugging resources:
        - Step-by-step tutorial: [link to AwsomeInventory Wiki].
        - Source-level debugger (Brrainz): [link to RimWorld4Debugging Github].

**Testing Specific Content (Debug Actions Menu -> Tools - Spawning / Incidents):**

- **Equipment:** Spawn Weapons (Tools - Spawning -> Spawn weapons). Apparel (same menu).
- **Animals:** Spawn Pawns (Tools - Spawning -> Spawn pawns).
- **Raiders:** Start Raid (Debug Actions Menu -> Incidents).
- **Incidents:** Trigger Incident (Debug Actions Menu -> Incidents).
- **Jobs:** Toggle Job Logging (Actions Menu -> Tools - Pawns -> Toggle job logging). Use View settings -> Draw Pawn Debug for visualization.
- **Lords:**
    - View settings -> Draw Duties: See pawn duties.
    - View settings -> Draw Lords: See Lord actions.
    - View settings -> Log Lord Toil Transitions: Log transitions.

**Tips for Testing:**

- **Quickstart:**
    - Command Line Parameter: `-quicktest` for fast map loading.
    - `autostart.rws` Savefile: Auto-loads if present in save folder.

- **Save Data Override:**
    - Command Line Parameter: `-savedatafolder=C:/Path/To/Folder` to redirect save data.
    - Combine with `-quicktest` for clean test environment.

- **Mod Tools:**
    - HugsLib: Quickstart, hotkey restart.
    - Publisher Plus (Jaxe): File exclusion for uploads.
    - TDBug (AlexTD/Uuugggg): Debugging tools.
    - 4M Mehni's Misc Modifications (Mehni): Various utilities.
    - Create custom tools for specific needs.
