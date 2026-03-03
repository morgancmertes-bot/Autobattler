# Auto Battler Design Vault — v2
**Last Updated:** 2026-03-03  
**Purpose:** Authoritative design reference for all units, abilities, systems, and rules.  
**Instructions for Claude:** Fetch https://raw.githubusercontent.com/morgancmertes-bot/Autobattler/main/AutoBattler_Design_Vault.md at the start of every session. It contains everything.  
**Instructions for Morgan:** Download this file at the end of every session. Upload to GitHub. Replace previous version.

---

# TABLE OF CONTENTS
1. [Core Rules & Architecture](#1-core-rules--architecture)
2. [Currency System](#2-currency-system)
3. [Unit Line System](#3-unit-line-system)
4. [Combat Systems](#4-combat-systems)
5. [Novel Systems — Needs Implementation](#5-novel-systems--needs-implementation)
6. [Buildings & Units](#6-buildings--units)
7. [Enemy System](#7-enemy-system)
8. [Upgrade System](#8-upgrade-system)
9. [Map & Battle Systems — Vision & Current State](#9-map--battle-systems--vision--current-state)
10. [Ability Deployment System](#10-ability-deployment-system)
11. [Run Loop & Meta Systems](#11-run-loop--meta-systems)
12. [Authoritative Folder Audit](#12-authoritative-folder-audit)
13. [Missing Buildings](#13-missing-buildings)
14. [Future Chatbot Specializations](#14-future-chatbot-specializations)

---

# 1. CORE RULES & ARCHITECTURE

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

# 2. CURRENCY SYSTEM

Six currencies total (expanded from original 4):
- **Gold** — primary economy/run currency
- **Sapphires** — magic/ranged unit upgrades
- **Emeralds** — nature/grove/druid upgrades
- **Steel** — melee/military upgrades
- **Stone** — construction/defensive upgrades
- **[6th currency — TBD name]** — category TBD

**⚠️ Known Issue:** Home UI not yet updated to reflect 6-currency system. Still shows 4.  
**Decision log:** "Expanded currencies from 4 → 6 to diversify upgrade correlations."

---

# 3. UNIT LINE SYSTEM

## Line Abilities
- Each slot (1-4) in a building has ONE ability that scales across all 3 tiers
- Described as: `Ability Name — damage/effect at T1/T2/T3`
- Upgrade: costs listed, what improves, how many times upgraded
- Unlock: second ability unlocked via currency spend

## Ability Notation Convention
- `10/15/20` = value at Tier 1 / Tier 2 / Tier 3
- Upgrade listed as: `+X per upgrade, max Y upgrades`
- Price doubles each upgrade level (systematic rule)

---

# 4. COMBAT SYSTEMS

## Projectile System
**Status:** ✅ Implemented  
- Accuracy stat default: 80%
- Cone angle formula: `(1 - accuracy/100) * 90`
  - 100% accuracy = perfect shot, 0% = 90° max spread
- Each projectile has individual accuracy roll
- Team-based collision: friendly projectiles pass through allies, hit enemies
- Sprite origins MUST be CENTER for rotation to work
- Projectiles render at negative depth to appear above background rectangle
- Test unit: Wizard firing fireballs

## Multi Projectile System
**Status:** ✅ Designed, partially implemented  
- **Multi 1** = 1 projectile (standard)
- **Multi 2** = 2 projectiles simultaneously
- **Multi 3** = 3 projectiles simultaneously
- Each projectile has its OWN individual accuracy roll
- Projectiles fire in a **fan/arc pattern** (curved spread, parenthesis shape)
- Fan arc gives natural angular separation to reduce mid-flight collisions
- Example: Archer Tower line is Multi 1/2/3 by tier, unlock adds +2 (becomes Multi 3/4/5)

## Armor & Magic Damage
- Units have Armor stat reducing physical damage
- Magic damage bypasses armor entirely
- Some abilities reduce armor as a debuff

## Knockback Damage
- Knockback has an associated damage value
- Some abilities/debuffs modify knockback damage taken (e.g. "Haunted" debuff: +40%)

## Parry & Block System
- Units have parry rate (%)
- Units have block ability (boolean or stat)
- Some debuffs remove ability to parry/block entirely
- Parry rate can be buffed/debuffed by abilities

## Combat Resolution Order (TO BE DOCUMENTED)
**⚠️ Missing:** Step-by-step resolution of how a single hit resolves:
- Attack roll → accuracy check → parry check → block check → armor reduction → damage type check → final damage → apply debuffs → XP
- This needs to be written out definitively so all ability interactions are unambiguous

---

# 5. NOVEL SYSTEMS — NEEDS IMPLEMENTATION

## 5A. Facing / Directional System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Shadow Adept, Adventurer's Guild Rogue, Backstab mechanic, any future positional ability

### Rules
- Every unit has a facing direction = angle in degrees (GameMaker `direction` variable)
- **Front Arc:** ±67.5° from facing direction (135° total)
- **Back Arc:** Everything else (225° of arc)

### GML Implementation
```gml
// Reusable function — add to scr_combat_functions
function is_attacking_from_behind(attacker_id, defender_id) {
    var attack_angle = point_direction(defender_id.x, defender_id.y, attacker_id.x, attacker_id.y);
    var angle_diff = angle_difference(defender_id.direction, attack_angle);
    return (abs(angle_diff) > 67.5);
}

// Critical hit hook — add to damage calculation
var crit_bonus = 0;
if (is_attacking_from_behind(attacker_id, target_id)) {
    crit_bonus = attacker.back_attack_crit_modifier; // default 0, modified by abilities
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
- Buff puddles: refresh buff duration every second unit remains inside
- Debuff puddles: apply/refresh debuff every second unit remains inside
- When unit LEAVES a puddle, buff/debuff expires after its listed duration
- Puddles are visible to player (ground layer visual, renders BELOW units = positive depth)
- Puddles can stack or interact (defined per ability)

### Puddle Object Properties
```
position (x, y)
radius (px)
duration (seconds — use -1 for permanent until dispelled)
type (string identifier e.g. "fire", "healing", "rune")
effect (buff/debuff struct applied per tick)
tick_rate (default: 1 second)
owner (unit instance that created it)
```

---

## 5C. Ballista Shot System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Construction Yard — Heavy Crossbow Tower line

### What The Player Sees
- Oversized bolt (~50% larger than normal) launches from tower
- Travels at HALF normal projectile speed
- **Shrinks as it approaches landing** — this is the player's ONLY timing information
- Lands → swaps to stuck sprite rotated to travel direction
- Near-lethal or lethal to anything hit
- Fires every 10 seconds
- Always renders on TOP of everything (most negative depth value in the game)
- Creates "a rollercoaster of relief and distress" for every unit it passes over

### Implementation Steps
1. Tower picks **RANDOM** valid target in range (NOT closest — preserves mystery)
2. Invisible 200px radius **accuracy zone** centered on target
3. **Landing point** = random point anywhere in accuracy zone (pure chaos, no bias)
4. **Skewer zone** calculated: rectangle along tower→landing vector
   - Tip of rectangle = landing point
   - Shaft extends BEHIND landing point along travel direction
   - Width: ~10px | Length: ~30px
   - **This rectangle IS the visual bolt shaft — identical space**
5. Projectile spawns, travels toward landing point at half speed
6. Shrink animation plays as projectile approaches
7. Projectile reaches landing → removed → stuck sprite placed, rotated to travel direction
8. **Instantly** check skewer zone for all units → apply damage
9. *(Future)* Killed unit death sprites rotate to align with bolt direction

---

## 5D. Just Me and You — Pebblekin Barrier
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Construction Yard — Pebblekin line

### Mechanics
- Pebblekin places 3 stone objects at equal intervals around a target enemy
- Traps the target — blocks physical movement and LINE OF SIGHT through formation
- Enemy units walking INTO the barrier are PUSHED radially outward
- **Unlock:** Pushed units take damage on ejection (exact value TBD — decided late, not recorded)

### Implementation Notes
- Stone objects = temporary obstacle instances, not combat units
- 3 stones placed at 120° intervals around target
- Must interact with ranged unit line-of-sight targeting system
- Push direction = normalize(unit_position - formation_center) * push_force

---

## 5E. Shadow Adept — Teleport Behind System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Enclave Slot 4, future Adventurer's Guild Rogue

### State Machine
```
STATE 1 — APPROACH
  Path toward nearest enemy melee unit normally

STATE 2 — STUCK CHECK  
  Every step: check if position delta < 5px over 0.5 seconds
  OR: if reached melee range of target
  → Either condition triggers TELEPORT

STATE 3 — TELEPORT BEHIND
  var behind_dist = 35;
  var behind_angle = target.direction + 180;
  var tx = target.x + lengthdir_x(behind_dist, behind_angle);
  var ty = target.y + lengthdir_y(behind_dist, behind_angle);
  Snap position to (tx, ty). Play teleport VFX.
  → Enter ATTACKING state

STATE 4 — ATTACKING
  Every frame: check is_attacking_from_behind(self, target)
  TRUE → apply double damage modifier
  FALSE (target rotated) → wait 1 second, re-enter TELEPORT state
  1 second delay prevents teleport flickering
```

---

## 5F. Vision Void System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Enclave — Initiate line unlock ability

### Rules
- 40px diameter black circular zone placed on battlefield
- Ranged units CANNOT draw line of sight through the void
- **Enemy melee inside void:** debuff "Alone in the Dark"
  - Cannot block or parry
  - Lasts 2s, refreshes every 1s while inside
- **Allied units inside void:** buff "Comforting Void"
  - +3 HP healed per second
  - Lasts 10s, refreshes every 1s while inside

---

## 5G. Flag / Zone Suggestion System
**Status:** DESIGNED IN CONCEPT, NOT IMPLEMENTED  
**Required by:** Future larger maps, story maps, tactical gameplay

### Design Intent
As maps grow larger and more complex, units need soft directional guidance. This is NOT direct control — it is an indirect suggestion system that preserves the auto-battler feel while giving the player tactical input.

### Proposed Mechanics
- Player can place **Flag objects** on the map during or before battle
- Flag types: **Attack Here**, **Defend Here**, **Rush Here** (names TBD)
- Units with no current priority target will bias movement toward relevant flags
- Units actively in combat ignore flags
- Flags are suggestions, not orders — a unit being attacked will still respond to the attack
- Flags may have a radius of influence rather than a point destination
- **This system needs full design before implementation**

### Why This Exists
- Current battle screen is a single flat arena — functional for testing
- As story maps are introduced (see Section 9), units need to understand spatial priorities
- Without flags, larger maps become chaotic and unreadable to the player
- Towers especially need to understand "cover this approach" vs "cover that approach"

---

## 5H. Ability Deployment System
**Status:** DESIGNED IN CONCEPT, NOT IMPLEMENTED  
**See Section 10 for full design**

---

# 6. BUILDINGS & UNITS

---

## ACADEMY
**Status:** ✅ Abilities Complete (Level 1 names only — full descriptions need xlsx scrape)

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

**Ability Names:** Slot 1: Eager Learner → Shared Insight → Final Thesis | Slot 2: Caustic Formula → Volatile Mixture → Perfect Solvent | Slot 3: Recorded Tactics → Preserved Doctrine → Immutable Record | Slot 4: Arcane Bolt → Focused Casting → Arcane Thesis  
*Full descriptions: needs xlsx scrape*

---

## BARRACKS
**Status:** ✅ Abilities Complete (Level 1 names only)

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

**Ability Names:** Slot 1: Phalanx Thrust → Braced Thrust → Breaking Formation | Slot 2: Hold Fast → Press the Line → First In, Last Out | Slot 3: Harass → Bait and Break → False Priority | Slot 4: Conscription → Massed Ranks → Overwhelming Presence  
*Full descriptions: needs xlsx scrape*

---

## BEAST KENNEL
**Status:** ❌ No abilities — needs full design  
*Unit names and stats: needs xlsx scrape*

---

## CASTLE
**Status:** ⚠️ Partial — Slot 4 complete, others partial

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

**Known:** Slot 1: Cleave (T2+) | Slot 3: Unyielding Wall (T3) | Slot 4: Banner Aura → Standard Aura → Royal Banner  
*Full descriptions: needs xlsx scrape*

---

## CHAPEL
**Status:** ✅ Abilities Complete (Level 1 names only)

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

**Ability Names:** Slot 1: Mend → Greater Mend → Anointed Mend | Slot 2: Shielded Guard → Radiant Ward → Battle Buddy | Slot 3: Fervor → Relentless Fervor → Holy Onslaught | Slot 4: Hymn of Solace → Chorus of Solace → Grand Chorus  
*Full descriptions: needs xlsx scrape*

---

## CONSTRUCTION YARD
**Status:** ⚠️ Data in xlsx. Key unit lines and systems documented here.

**Slot 1 — Archer Tower Line**
- T1: Archer Tower (Multi 1) | T2: Archer Tower II (Multi 2) | T3: Archer Tower III (Multi 3)
- Line Ability: Fire Multi X projectiles in fan arc, each with individual accuracy roll
- Unlock: +2 Multi count across all tiers (becomes 3/4/5)

**Slot 2 — Heavy Crossbow Tower Line**
- T1: Heavy Crossbow Tower | T2: Ballista Tower | T3: Dragon Slayer Tower
- Visual progression: oversized crossbow → full ballista → massive ornate ballista
- Line Ability: Ballista Shot — see Section 5C
- Fire rate: 10 seconds

**Slot 3 — Pebblekin Line**
- T1/T2/T3: Pebblekin (names TBD from xlsx)
- Line Ability: "Just Me and You" — see Section 5D
- Unlock: Pushed units take damage on ejection (value TBD)

**Slot 4:** TBD — needs xlsx scrape

---

## ELEMENTAL PORTAL
**Status:** ⚠️ Data in xlsx. Puddle system designed for this building — see Section 5B.  
*Full unit names, stats, ability descriptions: needs xlsx scrape*

---

## ENCLAVE
**Status:** ⚠️ Line abilities fully designed 2026-03-03. T2/T3 unit names TBD.

**Slot 1 — Initiate Line**
- T1: Initiate | T2: [TBD] | T3: [TBD]
- **Line Ability: "Find Weakness"**
  - Targets closest enemy melee unit
  - Debuff: -2 armor + 3/6/9 magic damage per hit from ANY source
  - Duration: 30s | Cooldown: 2 min
  - Upgrade (×10): +0.5 magic damage, +5s duration | price doubles
- **Unlock: "Vision Void"** — 500 Sapphires + 500 Emeralds — see Section 5F

**Slot 2 — Spirit Caller Line**
- T1: Spirit Caller | T2: [TBD] | T3: [TBD]
- **Line Ability: "Summon Spirit"**
  - Spawns flying creature: Wisp (T1) / Ghost (T2) / Death Avatar (T3)
  - Attacks Spirit Caller's current target
  - Resummon on death: 30/25/20s by tier
  - Upgrade (×10): +5 HP, +1 damage, +5px Spirit Caller targeting range | price doubles
- **Unlock: "Haunting Aura"** — 500 Sapphires
  - Creature becomes 60px aura instead of direct attacker
  - Enemy units in aura: "Haunted" = +40% knockback damage, -5% parry rate

**Slot 3 — Rune Scribe Line**
- T1: Rune Scribe | T2: [TBD] | T3: [TBD]
- **Line Ability: "Rune Circle"**
  - Targets friendly melee without "Energized" buff
  - Places 80px diameter rune, lasts 30s, 5s cooldown
  - "Energized" buff while on rune (refreshes every 1s): +5/10/15% parry, +5/10/15% move speed, +5/10/15% attack speed, +1/2/3 HP/s
  - Upgrade (×10): +1% all stats, +0.5 HP/s | price doubles
- **Unlock: "Second Surge!"** — 200 Sapphires + 800 Emeralds
  - When "Energized" expires naturally: heal unit for 20% max HP

**Slot 4 — Shadow Adept Line**
- T1: Shadow Adept | T2: [TBD] | T3: [TBD]
- **Line Ability: "Shadow Step"** — see Section 5E for full state machine
  - Teleports behind target, deals double damage from back arc
- **Unlock: "Non-Threatening"**
  - Cannot be targeted if any other valid target exists within 200px of the attacker

---

## GRAND CATHEDRAL
**Status:** ✅ Abilities Complete (9 units, 3 slots)

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

**Ability Names:** Slot 1: Shield Bubble → Aegis Ward → Sanctuary Veil | Slot 2: Rite of Cleansing → Judgment of Faith → Consecrated Verdict | Slot 3: Last Benediction → Vow of Suffering → Relic Burst  
*Full descriptions: needs xlsx scrape*

---

## GROVE
**Status:** ⚠️ Partial — 3/12 abilities

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

**Known:** Slot 1: Sap Skin → Rooted Recovery → Thorns  
*Slots 2-4 and full descriptions: needs xlsx scrape*

---

## MAGIC SCHOOL
**Status:** ❌ No abilities — needs full design  
*Unit names and stats: needs xlsx scrape*

---

## MONSTER LAIR
**Status:** ❌ No abilities — needs full design  
*Unit names and stats: needs xlsx scrape*

---

## MOUNTAIN KEEP
**Status:** ❌ No abilities — needs full design  
*Unit names and stats: needs xlsx scrape*

---

## OUTPOST
**Status:** ❌ No abilities — needs full design  
*Unit names and stats: needs xlsx scrape*

---

## SIEGE WORKSHOP
**Status:** ❌ No abilities — needs full design  
*Unit names and stats: needs xlsx scrape*

---

# 7. ENEMY SYSTEM

## Generation Rule — CRITICAL
- Enemy generation occurs in EXACTLY ONE place: **purchase screen Create event**
- DO NOT generate in Start Battle button or anywhere else
- Hard-won fix — generation in 3 places caused preview/battle unit divergence

## Enemy Unit Fields
- `Spawn Tags` — filtering (BROKEN — see below)
- `Spawn Weight` — probability weighting
- `Min Round` / `Max Round` — appearance window
- `Max Copies Per Wave` — cap per wave
- `Spawn Role` — team composition role
- `Enemy Tier` — difficulty tier
- `DP/Spawn Notes` — difficulty point notes

## Tag System
**Status:** ❌ BROKEN — corrupted, nearly unusable  
- Most units have incorrect, missing, or merged tags
- Needs full audit and rewrite building by building
- Fix as part of each building's ability pass

---

# 8. UPGRADE SYSTEM

## Army Training Screen
- Organized by building → slot rows
- Each slot row: ability upgrades + ability unlock button
- Upgrades apply to entire unit LINE (all tiers of that slot)

## Upgrade Types
1. **Stat upgrades** — flat or % improvements
2. **Ability upgrades** — improve existing line ability
3. **Ability unlocks** — one-time purchase of second ability

## Pricing Rules
- Single upgrades: fixed cost
- Multi-step upgrades: price doubles each purchase
- Unlock abilities: fixed one-time cost in specified currencies

---

# 9. MAP & BATTLE SYSTEMS — VISION & CURRENT STATE

## Current State
- Single flat battle arena — **testing/prototype only**
- Units meet in the middle and fight
- No terrain, no objectives, no positional strategy
- Towers exist but have no meaningful spatial role in this layout

## The Problem (Identified 2026-03-03)
**"Why do towers even exist?"**  
In the current flat arena, towers have no advantage over ground units. They just sit there. There is no high ground, no chokepoints, no approaches to cover. The current battle screen does not create the conditions for towers — or most strategic unit placement — to feel meaningful.

Additionally, the current format risks feeling **unentertainig** as the game develops because:
- No player agency during battle (watch and hope)
- No spatial variety
- No story context for fights
- No reason to care about unit positioning

## The Vision — Larger, Complex Maps
Maps need to grow and evolve to support:
- **Terrain features** — chokepoints, high ground, walls, bridges
- **Environmental storytelling** — maps that change based on story context
- **Multiple approaches** — enemies can come from different directions
- **Meaningful tower placement** — towers covering specific lanes or choke points
- **Objectives** — not just "kill everything", but defend a gate, hold a position, escort something

## Flag / Zone Suggestion System
**See Section 5G for full design spec**
- Player places flag markers on the map
- Types: Attack Here, Defend Here, Rush Here (names TBD)
- Units bias movement toward flags when not in active combat
- Preserves auto-battler feel — suggestions, not direct orders
- Critical for larger maps to remain readable and tactically interesting

---

# 10. ABILITY DEPLOYMENT SYSTEM

**Status:** DESIGNED IN CONCEPT, NOT IMPLEMENTED  
**Priority:** Medium — needed before abilities become complex enough to feel passive

## Design Intent
The player should have **indirect, imprecise tactical input** during battle. Not direct unit control, but a way to say "I want an ability used over here." This preserves the auto-battler identity (you set up, they fight) while giving the player a meaningful decision point mid-battle.

## Proposed Mechanics
1. Player clicks an ability icon from a UI panel
2. Player places a **target marker** anywhere on the battlefield
3. The system finds the nearest unit that:
   - Has that ability
   - Is within a certain % of cooldown completion (e.g. within 20% of ready)
4. That unit attempts to **move toward the target marker** and use the ability there
5. If the unit gets stuck or into combat before reaching the marker, they use the ability where they are instead
6. The system is **indirect** — the unit tries, but combat may interrupt

## Key Design Principles
- Player places intent, not orders
- Units can be interrupted by combat priority
- Cooldown threshold prevents sending a unit that won't be ready for a long time
- This creates interesting decisions: "Do I send my cleric across the battlefield for a clutch heal, knowing they might get stuck in combat?"
- Works naturally with the existing pathfinding and stuck-detection systems

## Open Questions
- Does the ability marker persist if the unit dies before reaching it?
- Can multiple units respond to the same marker?
- Does this apply to all abilities or only certain "deployable" ones?
- UI design: how does the player know which units are near-cooldown-ready?

---

# 11. RUN LOOP & META SYSTEMS

*(To be filled in from Layout docs once bash stabilizes — SYSTEMS_RUN_LOOP.md, SYSTEMS_ECONOMY_RUN.md, SYSTEMS_ECONOMY_META.md, SYSTEMS_META_OVERVIEW.md, etc.)*

**Known:**
- Game uses a run-based structure (roguelite style)
- Meta currencies (Sapphires, Emeralds, Steel, Stone, + 2 more) persist between runs
- Gold is run-only currency
- Quest rewards exist as a system
- Difficulty scales via DP (Difficulty Points) system
- Save/load system exists and has been implemented

---

# 12. AUTHORITATIVE FOLDER AUDIT

## What the Folder Was For
The Authoritative Work Folder was created to pass design context between chat sessions — a "here is how the game works and what it should be" document so a new Claude instance could get up to speed without re-explaining everything.

## Critical Problems Found

### 🔴 All Design docs are EMPTY
Every file in `/Design/` folder is blank except for one line in the decision log:
- `00_INDEX.md.txt` — empty
- `01_GAME_BIBLE.md.txt` — empty
- `10_SYSTEMS_COMBAT.md.txt` — empty
- `11_SYSTEMS_META.md.txt` — empty
- `12_SYSTEMS_META.md.txt` — empty
- `20_NARRATIVE_BIBLE.md.txt` — empty
**Root cause:** ChatGPT export failure — files were created but content never wrote correctly.

### 🔴 Systems With No Documentation At All
These exist in the game but have zero written spec anywhere:
- Projectile system (accuracy, cone, rotation)
- Ability trigger/cooldown/targeting system
- Combat resolution order (hit → parry → block → armor → damage)
- Facing/directional system (new — designed 2026-03-03)
- Puddle system (new — designed 2026-03-03)
- Ballista Shot mechanic (new — designed 2026-03-03)
- Just Me and You barrier (new — designed 2026-03-03)
- Flag/zone suggestion system (new — designed 2026-03-03)
- Ability deployment system (new — designed 2026-03-03)
- Tag system rules (exists but broken, never properly documented)

### 🟡 Layout Docs That Need Updating
*(Pending full read — bash was unstable during review)*
- `SYSTEMS_META_CURRENCIES.md` — likely says 4 currencies, needs update to 6
- `SYSTEMS_UPGRADES.md` — verify upgrade targeting uses `(building, slot)` key format
- `SYSTEMS_ENEMY_GENERATION_DP.md` — verify it reflects single-location generation fix
- `SYSTEMS_BUFF_STACKING.md` — needs review against new buff/debuff abilities designed today
- `SYSTEMS_BATTLE_CONTROLLER.md` — needs review against current obj_battle_controller implementation
- `UI_HOME_SCREEN.md` — needs update for 6-currency display

### 🟢 Layout Docs That Appear Solid
- `SYSTEM_UNIT_LINES.md` — consistent with current design
- `SYSTEMS_UNIT_DATA.md` — consistent with current design
- `SYSTEMS_SAVE_FORMAT.md` / `SYSTEMS_SAVE_LOAD.md` — implemented and working

### 🔴 Completely Missing Documents (Need To Be Written)
1. `SYSTEMS_PROJECTILE.md` — accuracy, cone, multi-shot, rotation rules
2. `SYSTEMS_ABILITY.md` — how abilities trigger, cooldowns, targeting, deployment
3. `SYSTEMS_COMBAT_RESOLUTION.md` — step by step hit resolution
4. `SYSTEMS_FACING.md` — front/back arc rules and GML implementation
5. `SYSTEMS_PUDDLE.md` — puddle object rules and interactions
6. `SYSTEMS_TAGS.md` — correct tag values, what they mean, how generation uses them
7. `SYSTEMS_MAP.md` — current state, vision, flag system design
8. `SYSTEMS_TOWERS.md` — tower unit rules, why they exist, spatial design intent
9. `GAME_BIBLE.md` — the actual game bible (currently empty)
10. `NARRATIVE_BIBLE.md` — story context (currently empty)

---

# 13. MISSING BUILDINGS

Buildings in the game loader with no unit data (~5 buildings, mostly ranged/flying):
- **Adventurer's Guild** — Rogue unit needs facing + invisibility system (Sections 5A, 5E)
- **Ballistic Range** — ranged units, listed in loader, no unit data exists
- **[3 additional TBD]** — Morgan to confirm which buildings are missing

*All missing buildings need: unit names, stats, abilities, tags — designed from scratch*

---

# 14. FUTURE CHATBOT SPECIALIZATIONS

Once the vault is complete enough, specialized Claude sessions can be spun up for:

- **Storytelling / Narrative** — needs: Narrative Bible, map designs, faction lore, unit backstories
  - Requires: `20_NARRATIVE_BIBLE.md` to be written first
- **Game Balance** — needs: all unit stats, all ability values, economy tuning
  - Requires: full xlsx data + ability descriptions complete
- **Unit "Feel" Design** — needs: movement, animation, sound, visual feedback specs
  - Requires: all abilities designed, map system defined
- **Map Design** — needs: map system documented, flag system designed, story context
  - Requires: Section 9 fully expanded, narrative context

**None of these are ready yet.** The vault needs to be substantially more complete before spinning up specialized sessions.

---

# APPENDIX: TECHNICAL NOTES

## GameMaker Depth System
- Lower depth number = renders in FRONT
- Projectiles: negative depth (-10 to -100) renders above background
- Ballista bolt: most negative depth in game (always on top)
- Puddles: positive depth (renders below units)
- Background rect: high positive depth (behind everything)

## Boot System
- `obj_game_boot` / `rm_boot` centralizes ALL global variable initialization
- Eliminates parse-time vs runtime variable conflicts
- All template loading at boot time — do not load templates anywhere else

## Projectile Sprite Setup
- Sprite origin MUST be CENTER for rotation to work correctly

## Coordinate System
- Purchase screen enemy preview zone: x=1184–1568
- Battle coordinates: x=994–1920
- Enemy generation maps from purchase coords to battle coords

## Key Fixed Bugs (Do Not Regress)
1. Enemy generation in ONE place only (purchase screen Create event)
2. Projectile depth must be negative
3. Boot system handles all global var init — do not init globals elsewhere
4. Sprite origins center for all projectiles

---

*End of document.*  
*Session workflow: Start → paste raw GitHub URL → Claude reads vault → continue work.*  
*End of session: Download updated vault → upload to GitHub → replace previous version.*
