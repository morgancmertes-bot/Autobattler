# Auto Battler Design Vault
**Last Updated:** 2026-03-03  
**Purpose:** Authoritative design reference for all units, abilities, systems, and rules.  
**Instructions for Claude:** Read this file at the start of every session. It contains everything.  
**Instructions for Morgan:** Download this file at the end of every session. Upload it next session.

---

# TABLE OF CONTENTS
1. [Core Rules & Architecture](#core-rules)
2. [Currency System](#currencies)
3. [Unit Line System](#unit-lines)
4. [Combat Systems](#combat-systems)
5. [Novel Systems (Needs Implementation)](#novel-systems)
6. [Buildings & Units](#buildings)
7. [Enemy System](#enemy-system)
8. [Upgrade System](#upgrade-system)
9. [Missing Buildings](#missing-buildings)

---

# 1. CORE RULES & ARCHITECTURE {#core-rules}

## Unit Creation
- All units MUST be created via `create_unit_data(template_id)`
- `stats` = immutable template stats
- `current_stats` = mutable combat state
- Required fields: `template_id`, `faction`, `stats`, `current_stats`, `position data`
- FORBIDDEN: Manually constructing unit structs, enemies skipping `create_unit_data()`

## Templates vs Instances
- Unit templates define base identity and base stats (e.g. Knight)
- Instances have their own persistent run-local state:
  - Unique name (player-named or generated)
  - `veterancy_xp` and `level`
  - `current_stats` (mutable combat stats)
  - Per-unit flags/statuses (buffs, debuffs)
- Multiple instances of same template can exist simultaneously at different XP/levels

## Unit Lines
- A **unit line** = same unit across building tiers, identified by `(building, slot)`
- Example Castle Slot 1: Squire (T1) → Knight (T2) → Knight Captain (T3)
- Same character visually — same face/hair, more armor, more ornate weapon each tier
- Upgrades target the LINE not a specific tier — applies to all tiers in that slot
- Upgrade key format: `"<building>:slot<slot>:<upgrade_id>"`
- Example: `"Castle:slot1:hp_percent"`

## Building Structure
- Each unit-producing building has 4 slots, 3 tiers = 12 units per building
- Exception: Grand Cathedral has 3 slots = 9 units
- Non-unit-producing buildings (Goldmine, Quarry, Target Range, Blacksmith) have no slots

## Upgrade UI Layout (Unit-Producing Buildings)
- Row 1: Building/meta upgrades
- Row 2: Slot 1 upgrades
- Row 3: Slot 2 upgrades
- Row 4: Slot 3 upgrades
- Row 5: Slot 4 upgrades

## Multi-Upgrade Pricing Rule
- ALL multi-step upgrades: price doubles each time
- Example: 100g → 200g → 400g → 800g...

---

# 2. CURRENCY SYSTEM {#currencies}

Six currencies total (expanded from original 4):
- **Gold** — primary economy currency
- **Sapphires** — magic/ranged unit upgrades
- **Emeralds** — nature/grove/druid upgrades  
- **Steel** — melee/military upgrades
- **Stone** — construction/defensive upgrades
- **[6th currency TBD]** — Home UI not yet updated to reflect 6-currency system

**Decision log:** "Expanded currencies from 4 → 6 to diversify upgrade correlations."

---

# 3. UNIT LINE SYSTEM {#unit-lines}

## Line Abilities
- Each slot (1-4) in a building has ONE ability that scales across all 3 tiers
- Described as: `Ability Name — damage/effect at T1/T2/T3`
- Upgrade: costs listed, what improves, how many times it can be upgraded
- Unlock: second ability unlocked via currency spend, listed with full cost

## Ability Notation Convention
- `10/15/20` = value at Tier 1 / Tier 2 / Tier 3
- Upgrade listed as: `+X per upgrade, max Y upgrades`
- Price doubles each upgrade level (systematic rule)

---

# 4. COMBAT SYSTEMS {#combat-systems}

## Projectile System
- Accuracy stat: default 80%
- Cone angle formula: `(1 - accuracy/100) * 90`
  - 100% accuracy = perfect shot
  - 0% accuracy = 90° max spread
- Each projectile has individual accuracy roll
- Team-based collision: friendly projectiles pass through allies, hit enemies
- Sprite origins must be CENTER for rotation to work correctly
- Projectiles render at negative depth to appear above background rectangle

## Multi Projectile System
- **Multi 1** = 1 projectile (standard)
- **Multi 2** = 2 projectiles simultaneously  
- **Multi 3** = 3 projectiles simultaneously
- Each projectile has its OWN individual accuracy roll
- Projectiles fire in a **fan/arc pattern** (curved spread, like a parenthesis shape)
- Fan arc gives natural angular separation to reduce mid-flight collisions
- Example: Archer Tower line is Multi 1/2/3 by tier, unlock adds +2 (becomes Multi 3/4/5)

## Armor & Magic Damage
- Units have Armor stat that reduces physical damage
- Magic damage bypasses armor
- Some abilities reduce armor as a debuff

## Knockback Damage
- Knockback has an associated damage value
- Some abilities/debuffs modify knockback damage taken

## Parry & Block System
- Units have parry rate (%)
- Units have block ability (boolean or stat)
- Some debuffs remove ability to parry/block
- Parry rate can be buffed/debuffed by abilities

---

# 5. NOVEL SYSTEMS (NEEDS IMPLEMENTATION) {#novel-systems}

## 5A. Facing / Directional System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Shadow Adept, Adventurer's Guild Rogue, Backstab mechanic

### Rules
- Every unit has a facing direction = angle in degrees (GameMaker `direction` variable)
- **Front Arc:** ±67.5° from facing direction (135° total)
- **Back Arc:** Everything else (225° of arc)

### Implementation
```gml
// Check if attacker is hitting from behind
var attack_angle = point_direction(defender.x, defender.y, attacker.x, attacker.y);
var angle_diff = angle_difference(defender.direction, attack_angle);
// If abs(angle_diff) > 67.5, attacker is in the BACK ARC
var attacking_from_behind = (abs(angle_diff) > 67.5);
```

### Critical Hit Hook (for back attacks)
```gml
var crit_bonus = 0;
if (is_attacking_from_behind(attacker_id, target_id)) {
    crit_bonus = attacker.back_attack_crit_modifier; // default 0
}
var final_crit_chance = attacker.crit_chance + crit_bonus;
```

---

## 5B. Puddle System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Elemental Portal units, Rune Scribe (Enclave)

### Rules
- A puddle is a circular zone on the battlefield with a defined radius
- Puddles have a duration and a type (element, buff, debuff)
- Units standing IN a puddle receive the puddle's effect every second (tick-based)
- Buff puddles: refresh their buff duration every second the unit remains inside
- Debuff puddles: apply/refresh debuff every second unit remains inside
- When unit LEAVES a puddle, buff/debuff expires after its listed duration
- Puddles can stack or interact (to be defined per ability)
- Puddles are visible to the player (visual effect on ground layer, renders BELOW units)

### Puddle Properties
- `position (x, y)`
- `radius` (px)
- `duration` (seconds, -1 = permanent until dispelled)
- `type` (string identifier)
- `effect` (buff/debuff applied per tick)
- `tick_rate` (default: 1 second)
- `owner` (unit that created it, for targeting rules)

---

## 5C. Ballista Shot System (Heavy Crossbow Tower Line)
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Construction Yard — Heavy Crossbow Tower line

### Visual Description (What Player Sees)
- Tower fires an oversized bolt (~50% larger than normal)
- Bolt travels at HALF normal projectile speed
- As bolt approaches landing point it SHRINKS (sells the arc/distance illusion in 2D)
- Bolt lands → swaps to stuck sprite rotated to match travel direction
- Near-lethal damage — this is a SPECTACLE unit, not a spam unit
- Fire rate: 10 seconds
- Bolt always renders on TOP of everything (most negative depth)

### The Trick (Implementation Steps)
1. **Target Selection:** Tower picks a RANDOM valid target in range (NOT closest — player should never know who it's targeting)
2. **Accuracy Zone:** Invisible 200px radius circle centered on random target
3. **Landing Point:** Random point chosen anywhere within accuracy zone (no bias, pure chaos)
4. **Skewer Zone Calculated:** Rectangle along tower→landing point vector, tip at landing point, shaft extending BEHIND landing point
   - Width: ~10px
   - Length: ~30px
   - Orientation: aligned with travel direction
   - This rectangle IS the visual shaft of the stuck bolt — they occupy identical space
5. **Projectile Spawns:** Oversized bolt launches from tower toward landing point at half speed
6. **Shrink Animation:** Bolt scales down as it approaches — this is the player's ONLY timing information
7. **Landing:** Bolt reaches landing point → removed → stuck sprite placed, rotated to travel direction
8. **Skewer Damage:** INSTANTLY check skewer zone rectangle for all units → apply damage
9. **Death Sprite (future/ideal):** Killed units rotate death sprite to align with bolt direction

### Design Intent
- The shrinking bolt is the tension mechanic — big = time to breathe, small = brace yourself
- Every unit it passes over creates "a little rollercoaster of relief and distress"
- Intentionally inaccurate — player has no idea when or where it lands
- Expected to be near-lethal or lethal to anything hit
- Practical counterplay is purely hoping it misses

---

## 5D. Just Me and You — Pebblekin Barrier
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Construction Yard — Pebblekin line

### Visual Description
- Pebblekin places 3 stone units around a target enemy forming a barrier
- The formation traps the enemy unit, isolating it from the rest of the fight
- Blocks LINE OF SIGHT through the formation (ranged units cannot shoot through it)
- Enemy units cannot pass through the barrier physically

### Mechanics
- Enemy units that walk INTO the barrier get PUSHED outward (away from center)
- The push direction is radially outward from the formation center
- Unlock: Units that get PUSHED receive damage on ejection (amount TBD — was decided late in session, not recorded)

### Implementation Notes
- The 3 stone units are placed at equal intervals around the target
- They may need to be treated as temporary obstacle objects rather than combat units
- Line of sight blocking needs to interact with the ranged targeting system

---

## 5E. Shadow Adept — Teleport Behind System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Enclave — Shadow Adept line, future Adventurer's Guild Rogue

### State Machine
```
STATE 1: APPROACH
  Move toward nearest enemy (normal pathfinding)

STATE 2: STUCK CHECK
  Every N steps check if position delta < threshold (5px over 0.5s)
  OR if successfully adjacent to target → trigger TELEPORT

STATE 3: TELEPORT BEHIND
  var behind_dist = 35; // px
  var behind_angle = target.direction + 180;
  var teleport_x = target.x + lengthdir_x(behind_dist, behind_angle);
  var teleport_y = target.y + lengthdir_y(behind_dist, behind_angle);
  Instantly set position. Play teleport VFX.

STATE 4: ATTACKING
  Every frame: check is_attacking_from_behind()
  If true: apply DOUBLE DAMAGE modifier
  If false (target turned): wait 1s then re-teleport (prevents flickering)
```

---

## 5F. Vision Void System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Enclave — Initiate line unlock

### Rules
- 40px diameter black circular zone
- Ranged units CANNOT draw line of sight through the void
- Enemy melee units INSIDE the void: receive debuff "Alone in the Dark"
  - Lose ability to block or parry
  - Debuff lasts 2 seconds, refreshes every 1 second while inside void
- Allied units INSIDE the void: receive buff "Comforting Void"
  - +3 HP healed per second
  - Buff lasts 10 seconds, refreshes every 1 second while inside void

---

# 6. BUILDINGS & UNITS {#buildings}

---

## ACADEMY
**Status:** ✅ Abilities Complete (Level 1 only)  
**Slot abilities:** TBD — data in xlsx, needs scraping

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | Student |
| 1 | 2 | Alchemist |
| 1 | 3 | Archivist |
| 1 | 4 | Apprentice Mage |
| 2 | 1 | Scholar |
| 2 | 2 | Chemist |
| 2 | 3 | Lorekeeper |
| 2 | 4 | Spellwright |
| 3 | 1 | Mastermind |
| 3 | 2 | Transmuter |
| 3 | 3 | Grand Historian |
| 3 | 4 | Arcane Professor |

**Ability Names (from xlsx):**
- Slot 1: Eager Learner → Shared Insight → Final Thesis
- Slot 2: Caustic Formula → Volatile Mixture → Perfect Solvent
- Slot 3: Recorded Tactics → Preserved Doctrine → Immutable Record
- Slot 4: Arcane Bolt → Focused Casting → Arcane Thesis

*Full ability descriptions: needs xlsx scrape*

---

## BARRACKS
**Status:** ✅ Abilities Complete (Level 1 only, 12/13 — one duplicate Ranger row)  

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | Spearman |
| 1 | 2 | Footman |
| 1 | 3 | Scout |
| 1 | 4 | Militia |
| 2 | 1 | Halberdier |
| 2 | 2 | Man-at-Arms |
| 2 | 3 | Ranger |
| 2 | 4 | Veteran |
| 3 | 1 | Sentinel |
| 3 | 2 | War Marshal |
| 3 | 3 | Pathfinder |
| 3 | 4 | Warborn |

**Ability Names (from xlsx):**
- Slot 1: Phalanx Thrust → Braced Thrust → Breaking Formation
- Slot 2: Hold Fast → Press the Line → First In, Last Out
- Slot 3: Harass → Bait and Break → False Priority
- Slot 4: Conscription → Massed Ranks → Overwhelming Presence

*Full ability descriptions: needs xlsx scrape*

---

## BEAST KENNEL
**Status:** ❌ No abilities — needs full ability design  

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | TBD |
| 1 | 2 | TBD |
| 1 | 3 | TBD |
| 1 | 4 | TBD |
| 2 | 1 | TBD |
| 2 | 2 | TBD |
| 2 | 3 | TBD |
| 2 | 4 | TBD |
| 3 | 1 | TBD |
| 3 | 2 | TBD |
| 3 | 3 | TBD |
| 3 | 4 | TBD |

*Needs xlsx scrape for unit names and stats*

---

## CASTLE
**Status:** ⚠️ Partial abilities — Slot 4 complete, Slot 1 partial, Slots 2-3 mostly empty  

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | Squire |
| 1 | 2 | Archer |
| 1 | 3 | Shield Bearer |
| 1 | 4 | Banner Carrier |
| 2 | 1 | Knight |
| 2 | 2 | Crossbowman |
| 2 | 3 | Tower Guard |
| 2 | 4 | Standard Knight |
| 3 | 1 | Knight Captain |
| 3 | 2 | Marksman |
| 3 | 3 | Bulwark Champion |
| 3 | 4 | Royal Vexillum Master |

**Known Abilities:**
- Slot 1: Cleave (T2+) — damage in cone, exact values TBD
- Slot 3: Unyielding Wall (T3 only recorded)
- Slot 4: Banner Aura → Standard Aura → Royal Banner

*Full ability descriptions: needs xlsx scrape*

---

## CHAPEL
**Status:** ✅ Abilities Complete (Level 1 only)  

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | Acolyte |
| 1 | 2 | Lightbringer |
| 1 | 3 | Penitent |
| 1 | 4 | Hymnist |
| 2 | 1 | Priest |
| 2 | 2 | Sun Knight |
| 2 | 3 | Zealot |
| 2 | 4 | Cantor |
| 3 | 1 | High Cleric |
| 3 | 2 | Dawn Paladin |
| 3 | 3 | Crusader |
| 3 | 4 | Choirmaster |

**Ability Names (from xlsx):**
- Slot 1: Mend → Greater Mend → Anointed Mend
- Slot 2: Shielded Guard → Radiant Ward → Battle Buddy
- Slot 3: Fervor → Relentless Fervor → Holy Onslaught
- Slot 4: Hymn of Solace → Chorus of Solace → Grand Chorus

*Full ability descriptions: needs xlsx scrape*

---

## CONSTRUCTION YARD
**Status:** ⚠️ Data in xlsx — needs scrape. Key systems designed in this session.  

### Known Unit Lines
**Slot 1 — Archer Tower Line (Multi-Projectile)**
- T1: Archer Tower (Multi 1)
- T2: Archer Tower II (Multi 2)  
- T3: Archer Tower III (Multi 3)
- **Line Ability:** Fires Multi X projectiles in fan arc pattern, each with individual accuracy
- **Unlock:** +2 to Multi count (becomes Multi 3/4/5)

**Slot 2 — Heavy Crossbow Tower Line (Ballista Shot)**
- T1: Heavy Crossbow Tower
- T2: Ballista Tower
- T3: Dragon Slayer Tower
- Visually: oversized crossbow → full ballista → massive ornate ballista
- Functionally identical across tiers, better stats each tier
- **Line Ability:** Ballista Shot — see Section 5C for full system spec
- Fire rate: 10 seconds

**Slot 3 — Pebblekin Line (Just Me and You)**
- T1: Pebblekin (T1 name TBD)
- T2: Pebblekin (T2 name TBD)
- T3: Pebblekin (T3 name TBD)
- **Line Ability:** "Just Me and You" — see Section 5D for full system spec
- **Unlock:** Pushed units take damage on ejection (damage value TBD)

**Slot 4:** TBD — needs xlsx scrape

*Full unit names, stats, ability descriptions: needs xlsx scrape*

---

## ELEMENTAL PORTAL
**Status:** ⚠️ Data in xlsx — needs scrape. Puddle system invented for this building.  
*Full unit names, stats, ability descriptions: needs xlsx scrape*  
*Puddle system spec: see Section 5B*

---

## ENCLAVE
**Status:** ⚠️ Ability designs documented in THIS session — not yet in xlsx  

### Unit Lines (Designed 2026-03-03)

**Slot 1 — Initiate Line**
- T1: Initiate | T2: [TBD] | T3: [TBD]
- **Line Ability: "Find Weakness"**
  - Targets closest enemy melee
  - Applies debuff: -2 armor + 3/6/9 magic damage per hit from ANY source
  - Duration: 30 seconds | Cooldown: 2 minutes
  - Upgrade (×10): +0.5 magic damage per hit, +5s duration | Price doubles each time
  - **Unlock: "Vision Void"** — see Section 5F for full system spec
  - Unlock Cost: 500 Sapphires + 500 Emeralds

**Slot 2 — Spirit Caller Line**
- T1: Spirit Caller | T2: [TBD] | T3: [TBD]
- **Line Ability: "Summon Spirit"**
  - Spawns flying creature: Wisp (T1) / Ghost (T2) / Death Avatar (T3)
  - Summoned creature attacks Spirit Caller's current target
  - Resummon timer if creature dies: 30/25/20 seconds by tier
  - Upgrade (×10): +5 HP to creature, +1 damage, +5px targeting range on Spirit Caller
  - **Unlock: "Haunting Aura"** — 500 Sapphires
  - Unlock Effect: Summoned creature becomes 60px diameter aura instead of direct attacker
  - Enemy units in aura: debuff "Haunted" = +40% knockback damage taken, -5% parry rate

**Slot 3 — Rune Scribe Line**
- T1: Rune Scribe | T2: [TBD] | T3: [TBD]
- **Line Ability: "Rune Circle"**
  - Targets friendly melee unit WITHOUT "Energized" buff
  - Places 80px diameter rune under them, lasts 30 seconds, 5 second cooldown
  - Units standing on rune get buff "Energized" (refreshes every 1s while on rune):
    - +5/10/15% parry
    - +5/10/15% movement speed
    - +5/10/15% attack speed
    - +1/2/3 HP/s healing
  - Upgrade (×10): +1% to all stats, +0.5 HP/s | Price doubles each time
  - **Unlock: "Second Surge!"** — 200 Sapphires + 800 Emeralds
  - Unlock Effect: When "Energized" expires naturally, unit is healed for 20% max HP

**Slot 4 — Shadow Adept Line**
- T1: Shadow Adept | T2: [TBD] | T3: [TBD]
- **Line Ability: "Shadow Step"**
  - On combat start: paths toward nearest enemy melee
  - If stuck (position delta < 5px over 0.5s) OR reaches melee range: teleports behind target
  - While attacking from BACK ARC: deals DOUBLE DAMAGE
  - Re-teleports if target rotates and Shadow Adept leaves back arc (1s delay)
  - See Section 5E for full system spec
  - **Unlock: "Non-Threatening"**
  - Unit cannot be targeted by enemies if any other valid target exists within 200px of that enemy
  - Implementation: override enemy target selection — skip Shadow Adept if other valid targets exist nearby

---

## GRAND CATHEDRAL
**Status:** ✅ Abilities Complete (Level 1 only, 9 units — 3 slots only)  

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | Cleric |
| 1 | 2 | Confessor |
| 1 | 3 | Martyr |
| 2 | 1 | Battle Cleric |
| 2 | 2 | Inquisitor |
| 2 | 3 | Ascetic |
| 3 | 1 | Hierophant |
| 3 | 2 | Grand Inquisitor |
| 3 | 3 | Living Relic |

**Ability Names (from xlsx):**
- Slot 1: Shield Bubble → Aegis Ward → Sanctuary Veil
- Slot 2: Rite of Cleansing → Judgment of Faith → Consecrated Verdict
- Slot 3: Last Benediction → Vow of Suffering → Relic Burst

*Full ability descriptions: needs xlsx scrape*

---

## GROVE
**Status:** ⚠️ Partial — 3/12 abilities, rest empty  

| Tier | Slot | Unit Name |
|------|------|-----------|
| 1 | 1 | Sproutling |
| 1 | 2 | Dryad |
| 1 | 3 | Acorn Scout |
| 1 | 4 | Seed Sower |
| 2 | 1 | Vinekin |
| 2 | 2 | Grovekeeper |
| 2 | 3 | Barkstrider |
| 2 | 4 | Bloomcaller |
| 3 | 1 | Thorn Warden |
| 3 | 2 | Sylvan Matron |
| 3 | 3 | Forest Sentinel |
| 3 | 4 | Verdant Sage |

**Known Abilities:**
- Slot 1: Sap Skin (T1) → Rooted Recovery (T2) → Thorns (T3)

*Full ability descriptions and slots 2-4: needs xlsx scrape*

---

## MAGIC SCHOOL
**Status:** ❌ No abilities — needs full ability design  
*Unit names and stats: needs xlsx scrape*

---

## MONSTER LAIR
**Status:** ❌ No abilities — needs full ability design  
*Unit names and stats: needs xlsx scrape*

---

## MOUNTAIN KEEP
**Status:** ❌ No abilities — needs full ability design  
*Unit names and stats: needs xlsx scrape*

---

## OUTPOST
**Status:** ❌ No abilities — needs full ability design  
*Unit names and stats: needs xlsx scrape*

---

## SIEGE WORKSHOP
**Status:** ❌ No abilities — needs full ability design  
*Unit names and stats: needs xlsx scrape*

---

# 7. ENEMY SYSTEM {#enemy-system}

## Generation Rule (Critical — Hard Won Fix)
- Enemy generation occurs in EXACTLY ONE place: purchase screen Create event
- DO NOT generate enemies in Start Battle button or any other location
- Preview units and battle units must be identical — generation in multiple places caused them to diverge

## Enemy Unit Fields
- `Spawn Tags` — used for enemy generation filtering (currently broken/corrupted)
- `Spawn Weight` — probability weighting
- `Min Round` / `Max Round` — which rounds this enemy can appear
- `Max Copies Per Wave` — cap per wave
- `Spawn Role` — role in enemy team composition
- `Enemy Tier` — difficulty tier
- `DP/Spawn Notes` — difficulty point notes

## Tag System
**Status:** ❌ BROKEN — tags corrupted, nearly unusable, needs full audit and fix  
Most units have incorrect or missing tags. This needs to be addressed building by building.

---

# 8. UPGRADE SYSTEM {#upgrade-system}

## Army Training Screen
- Shows upgrades organized by building → slot
- Each slot row contains ability upgrades and ability unlocks
- Upgrades apply to entire unit LINE (all tiers of that slot)

## Upgrade Types
1. **Stat upgrades** — flat or % improvements to unit stats
2. **Ability upgrades** — improve existing line ability (damage, duration, etc.)
3. **Ability unlocks** — purchase second ability for the line (one-time cost)

## Pricing
- Single upgrades: fixed cost
- Multi-step upgrades: price doubles each purchase
- Unlock abilities: fixed one-time cost in specified currencies

---

# 9. MISSING BUILDINGS {#missing-buildings}

These buildings exist in the game design but have NO unit data yet (~5 buildings, mostly ranged/flying):
- **Adventurer's Guild** — contains Rogue unit (needs facing/invisibility system, see Section 5A/5E)
- **Ballistic Range** — ranged units (listed in loader, no unit data)
- **[Additional missing buildings TBD]**

*These need to be designed from scratch — unit names, stats, abilities, everything*

---

# APPENDIX: TECHNICAL NOTES

## GameMaker Depth System
- Lower depth number = renders in FRONT
- Projectiles: set depth to negative value (-10 to -100) to render above background
- Ballista bolt: most negative depth of all (always on top)
- Puddles: render BELOW units (positive depth)

## Boot System
- `obj_game_boot` / `rm_boot` centralizes all global variable initialization
- Eliminates parse-time vs runtime variable conflicts
- All template loading happens at boot time

## Projectile Sprite Setup
- Sprite origin MUST be set to CENTER for rotation to work correctly

## Coordinate System
- Purchase screen enemy zone: x=1184-1568
- Battle coordinates: x=994-1920
- Enemy generation maps from purchase coords to battle coords

---

*End of document. When resuming: upload this file, tell Claude to read it, then continue from where you left off.*
