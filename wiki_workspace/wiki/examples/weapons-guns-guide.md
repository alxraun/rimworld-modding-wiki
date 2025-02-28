## Modding Tutorials/Weapons Guns - Cheat Sheet

**BaseGun and BaseBullet Parents:**

- **BaseGun (Abstract, Name="BaseGun"):** Common properties for all guns.
    - `<category>Item</category>`: Category "Item".
    - `<altitudeLayer>Item</altitudeLayer>`: Render layer.
    - `<useHitPoints>True</useHitPoints>`: Has hitpoints, can be damaged.
    - `<selectable>True</selectable>`: Can be selected.
    - `<alwaysHaulable>True</alwaysHaulable>`: Can be hauled.
    - `<techLevel>Midworld</techLevel>`: Tech level (Midworld, Spacer...).
    - `<thingCategories><li>WeaponsRanged</li></thingCategories>`: Part of WeaponsRanged category.
    - `<smeltProducts><Steel>20</Steel></smeltProducts>`: Produces Steel when smelted.
    - `<statBases>`: Stat modifiers.
        - `<MaxHitPoints>`: Durability.
        - `<Flammability>`: Fire vulnerability.
        - `<DeteriorationRate>`: Deterioration speed.
        - `<SellPriceFactor>`: Sell price modifier.
    - `<graphicData>`: Graphics settings.
        - `<onGroundRandomRotateAngle>35</onGroundRandomRotateAngle>`: Rotation on ground.
    - `<drawGUIOverlay>true</drawGUIOverlay>`: Draws GUI overlay when selected.
    - `<tickerType>Never</tickerType>`: No tick function.
    - `<inspectorTabs><li>ITab_Art</li></inspectorTabs>`: Has ITab_Art inspector tab.
    - `<comps><li>...</li></comps>`: Components.
        - `CompForbiddable`, `CompEquippable`, `CompQuality`, `CompArt`.

- **BaseHumanGun (Abstract, ParentName="BaseGun", Name="BaseHumanGun"):** Inherits from BaseGun, specific for human guns.
    - `<weaponTags><li>Gun</li></weaponTags>`: Weapon tag "Gun".

- **BaseBullet (Abstract, Name="BaseBullet"):** Common properties for all bullets.
    - `<category>Projectile</category>`: Category "Projectile".
    - `<tickerType>Normal</tickerType>`: Has tick function.
    - `<altitudeLayer>Projectile</altitudeLayer>`: Render layer.
    - `<thingClass>Bullet</thingClass>`: ThingClass "Bullet".
    - `<label>bullet</label>`: Label "bullet".
    - `<useHitPoints>False</useHitPoints>`: No hitpoints.
    - `<neverMultiSelect>True</neverMultiSelect>`: Not multi-selectable.
    - `<graphicData><shaderType>Transparent</shaderType></graphicData>`: Shader type "Transparent".

**Gun Definitions (Inherit from BaseHumanGun or BaseGun):**

- **Required Tags:**
    - `<defName>`: Unique identifier (e.g., `Gun_Pistol`).
    - `<label>`: In-game name (lowercase, e.g., `pistol`).
    - `<description>`: Item description.
    - `<graphicData>`: Texture and rendering.
        - `<texPath>`: Texture path (e.g., `Things/Item/Equipment/WeaponRanged/Pistol`).
        - `<graphicClass>`: Graphic class (`Graphic_Single` etc.).
    - `<soundInteract>`: Sound on interaction (`InteractPistol`).
    - `<statBases>`: Weapon stats.
        - `<MarketValue>`: Item value.
        - `<AccuracyTouch>`, `<AccuracyShort>`, `<AccuracyMedium>`, `<AccuracyLong>`: Accuracy at ranges (0.0-1.0).
        - `<RangedWeapon_Cooldown>`: Cooldown time (seconds).
    - `<verbs><li></verbs>`: Ranged attack verbs.
        - `<verbClass>Verb_Shoot</verbClass>`: Verb class "Verb_Shoot".
        - `<hasStandardCommand>true</hasStandardCommand>`
        - `<projectileDef>`: Bullet Def to use (`Bullet_Pistol`).
        - `<warmupTicks>`: Aiming time (ticks).
        - `<range>`: Range (tiles).
        - `<soundCast>`: Firing sound (`ShotPistol`).
        - `<soundCastTail>`: Tail sound (`GunTail_Light`).
        - `<muzzleFlashScale>`: Muzzle flash size.
    - `<tools><li></tools>`: Melee attack tools.
        - `<label>`: Tool label (`handle`, `barrel`).
        - `<capacities><li>DamageType</li></capacities>`: Damage capacities (`Blunt`, `Poke`).
        - `<power>`: Damage power.
        - `<cooldownTime>`: Cooldown time.

**Bullet Definitions (Inherit from BaseBullet):**

- **Required Tags:**
    - `<defName>`: Unique identifier (e.g., `Bullet_Pistol`).
    - `<label>`: In-game name (lowercase, e.g., `pistol bullet`).
    - `<graphicData>`: Texture and rendering.
        - `<texPath>`: Texture path (e.g., `Things/Projectile/Bullet_Small`).
        - `<graphicClass>`: Graphic class (`Graphic_Single` etc.).
    - `<projectile>`: Projectile properties.
        - `<flyOverhead>false</flyOverhead>`
        - `<damageDef>`: Damage Def type (`Bullet`).
        - `<damageAmountBase>`: Base damage amount.
        - `<speed>`: Projectile speed.

**Special Cases (Weapon Modifications):**

- **Burstfire:**
    - `<verbs><li><burstShotCount>`, `<ticksBetweenBurstShots>`: Define burst fire behavior.

- **Sniper Rifles:**
    - `<weaponTags><li>SniperRifle</li></weaponTags>`: Limits usage to specific pawn kinds.

- **Bomb Projectiles:**
    - `<thingClass>Projectile_Explosive</thingClass>`: Explosive projectile class.
    - `<projectile><damageDef>Bomb</damageDef>`, `<explosionRadius>`: Bomb damage and radius.

- **Flame Bullets:**
    - `<projectile><damageDef>Flame</damageDef>`, `<explosionRadius>`, `<postExplosionSpawnThingDef>`, `<explosionSpawnChance>`: Flame damage, radius, puddle spawn.

- **Bigger Muzzleflash:**
    - `<verbs><li><muzzleFlashScale>`: Adjust muzzle flash size.

- **Spacer Tech Level:**
    - `<techLevel>Spacer</techLevel>`: Restricts weapon to Spacer tech level.

- **Turret Guns (Hidden Weapons):**
    - `<menuHidden>true</menuHidden>`, `<canBeSpawningInventory>false</canBeSpawningInventory>`, `<tradeability>None</tradeability>`, `<weaponTags><li>TurretGun</li></weaponTags>`: Hidden, non-tradable turret weapons.

- **Heavy Weapons (Move Speed Penalty):**
    - `<weaponTags><li>MechanoidGunHeavy</li><li>GunHeavy</li></weaponTags>`, `<equippedStatOffsets><MoveSpeed>-0.25</MoveSpeed></equippedStatOffsets>`: Heavy weapon tags and move speed penalty.

- **Doomsday Rocket (Visual and Behavior Changes):**
    - `<thingClass>Projectile_DoomsdayRocket</thingClass>`, `<shaderType>TransparentPostLight</shaderType>`: Special projectile class and shader.
    - `<verbs><li><verbClass>Verb_ShootOneUse</verbClass>`, `<onlyManualCast>true</onlyManualCast>`, `<targetParams><canTargetLocations>true</canTargetLocations></targetParams>`: Single-use, manual cast, location targeting.

- **Miss Radius (Forced Miss):**
    - `<verbs><li><forcedMissRadius>2.0</forcedMissRadius></verbs>`: Forces a miss radius even with accuracy.

