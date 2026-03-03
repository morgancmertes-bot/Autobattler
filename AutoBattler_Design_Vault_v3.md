# Auto Battler — Design Vault v3
**Last Updated:** 2026-03-03  
**Status:** Most complete version to date — includes all xlsx ability data, all layout system docs, all novel systems  
**GitHub Workflow:** Download → upload to repo → replace previous version → next session Claude fetches raw URL

---

# TABLE OF CONTENTS
1. [Core Architecture Rules](#1-core-architecture-rules)
2. [Currency System](#2-currency-system)
3. [Unit Line System](#3-unit-line-system)
4. [Combat Systems](#4-combat-systems)
5. [Novel Systems — Designed, Needs Implementation](#5-novel-systems)
6. [Buildings & Full Ability Specs](#6-buildings--full-ability-specs)
7. [Enemy Generation System](#7-enemy-generation-system)
8. [Tag System](#8-tag-system)
9. [Upgrade & Meta Systems](#9-upgrade--meta-systems)
10. [Run Loop & Economy](#10-run-loop--economy)
11. [Save / Load System](#11-save--load-system)
12. [Battle Controller & Outcome](#12-battle-controller--outcome)
13. [Support Actions & XP System](#13-support-actions--xp-system)
14. [Map & Battle Vision — Current Problems & Future Design](#14-map--battle-vision)
15. [Ability Deployment System](#15-ability-deployment-system)
16. [UI Screens](#16-ui-screens)
17. [Known Bugs, Gaps & Backlog](#17-known-bugs-gaps--backlog)
18. [Future Chatbot Specializations](#18-future-chatbot-specializations)
19. [Technical Notes — GameMaker](#19-technical-notes--gamemaker)

---

# 1. CORE ARCHITECTURE RULES

## Unit Creation — NON-NEGOTIABLE
- All units (player AND enemy) MUST be created via `create_unit_data(template_id)`
- `stats` = immutable template stats
- `current_stats` = mutable combat state
- Required fields: `template_id`, `faction`, `stats`, `current_stats`, position data
- FORBIDDEN: Manually constructing unit structs, enemies skipping `create_unit_data()`

## Templates vs Instances
- Templates define base identity and stats (e.g. Knight)
- Instances have their own run-local state: unique name, `veterancy_xp`, `level`, `current_stats`, buffs/debuffs
- Multiple instances of same template can exist simultaneously at different XP/levels (e.g. four Knights at levels 1, 3, 5, 10)

## Enemy Generation — ONE PLACE ONLY
- Enemy generation occurs in EXACTLY ONE place: **purchase screen Create event**
- DO NOT generate enemies in Start Battle button or anywhere else
- Hard-won fix — generation in 3 places caused preview/battle unit divergence

## Boot System
- `obj_game_boot` / `rm_boot` centralizes ALL global variable initialization
- Eliminates parse-time vs runtime conflicts
- All template loading at boot time — do NOT init globals or load templates elsewhere

---

# 2. CURRENCY SYSTEM

## Six Meta Currencies (expanded from original 4)
| Currency | Domain | Upgrade Examples |
|---|---|---|
| **Rubies** | Magic damage | Spell damage, fireball effects |
| **Emeralds** | Healing | Healer effectiveness, regen |
| **Stone** | Buildings/meta | Starting gold, castle health |
| **Steel** | Melee/armor | Melee damage, armor values |
| **Wood** | Ranged/flying | Archer damage, flying unit stats |
| **Sapphire** | Support/buffs | Bannerman aura, temp buffs |

**Resource Assignment Rule (for ability costs):**  
Steel=Melee, Stone=Buildings/Global, Wood=Ranged, Rubies=Magic Damage, Sapphires=Buffs/Support, Emeralds=Healing

**Gold** = run-only currency (does not persist between runs)

**⚠️ Known Issue:** Home UI not yet updated to show 6 currencies — still shows 4. Decision log: "Expanded currencies from 4 → 6 to diversify upgrade correlations."  
**⚠️ Known Issue:** Army Training label shows "IRON" — should say "STEEL". Variable is already `global.steel`.

---

# 3. UNIT LINE SYSTEM

## Unit Lines
- Identified by `(building, slot)`
- Same character across tiers — same face/hair, more armor each tier
- Example Castle Slot 1: Squire (T1) → Knight (T2) → Knight Captain (T3)
- Upgrades target the LINE, not specific tier
- Upgrade key format: `"<building>:slot<slot>:<upgrade_id>"`
- Example: `"Castle:slot1:hp_percent"`

## Building Structure
- 4 slots × 3 tiers = 12 units per building
- Grand Cathedral exception: 3 slots = 9 units
- Non-unit buildings (Goldmine, Quarry, Target Range, Blacksmith) — no slots

## Unit-Producing Building Upgrade Layout (5-row grid)
- Row 1: Building/meta upgrades
- Row 2: Slot 1 line upgrades
- Row 3: Slot 2 line upgrades
- Row 4: Slot 3 line upgrades
- Row 5: Slot 4 line upgrades

## Multi-Upgrade Pricing Rule
ALL multi-step upgrades double in price each time:  
Base 15 → 30 → 60 → 120 → 240 → 480 → 960 → 1920 → 3840 → 7680

---

# 4. COMBAT SYSTEMS

## Projectile System ✅ Implemented
- Accuracy stat default: 80% (global baseline for all ranged units: 20° cone = ±10° off center)
- Cone angle formula: `(1 - accuracy/100) * 90`
  - 100% accuracy = 0° spread (perfect), 0% accuracy = 90° max spread
- Each projectile has individual accuracy roll
- Team-based collision: friendly projectiles pass through allies, hit enemies
- Sprite origins MUST be CENTER for rotation
- Projectiles render at negative depth to appear above background
- **⚠️ Note:** Global baseline 20° cone for all ranged was in previous iteration and needs reimplementing — currently missing

## Multi-Projectile System ✅ Designed
- Multi 1/2/3 = number of simultaneous projectiles
- Each projectile has independent accuracy roll
- Fire in **concave arc (fan/parenthesis curve)** — NOT a straight line
- Arc spread evenly spaced across total spread angle (default 20°)
- Unit template needs: `multi_count` and `multi_spread` fields
- `attack_type` becomes `"Ranged Multi"` or `"Magic Multi"`

## Armor & Damage Types
- Physical damage: reduced by armor stat
- **Magic damage: ignores armor entirely — CONFIRMED RULE**
- Units with "Magic" tag always deal armor-ignoring damage

## Parry & Block
- **Parry (melee units, type:Melee):** 5% global baseline — 50% damage reduction vs melee attacks
- **Block (tank units):** 10% global baseline — complete damage negation from all types
- **Knockback:** 50% chance baseline
  - Knockback Power = push distance in px (formula: 0.5× melee dmg OR 0.25× ranged/magic dmg)
  - Knockback Chance = % odds of triggering
  - Push direction = opposite of attack angle
- **⚠️ Status:** Parry/Block/Knockback system designed but NOT yet implemented in code

## Combat Resolution Order — NEEDS DOCUMENTATION
**⚠️ Missing:** Step-by-step resolution of a single hit:
Attack roll → accuracy check → parry check → block check → armor reduction → damage type → final damage → apply debuffs → XP  
This needs to be written definitively so all ability interactions are unambiguous.

## Healer Repositioning Logic (Shared by Acolyte Mend + Hymnist Hymn of Solace)
1. Heal lowest HP% friendly unit in range
2. If no damaged unit in range: walk 300px from nearest enemy, retarget
3. If no damaged unit on field: follow nearest Tank/Melee DPS/Ranged DPS 80px behind
4. If no allies remain: flee to spawn location

## Healing Rules
- Healing is NOT reduced by armor/defense
- Heal formula: `hp = min(hp + amount, max_hp)`
- `healing_other` field on all healer unit templates = legacy placeholder — superseded by ability system (zero out or ignore once abilities implemented)

---

# 5. NOVEL SYSTEMS — DESIGNED, NEEDS IMPLEMENTATION

## 5A. Facing / Directional System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Shadow Adept (Enclave Slot 4), future Adventurer's Guild Rogue, backstab

### Rules
- Every unit has a facing direction (GameMaker `direction` variable, in degrees)
- **Front Arc:** ±67.5° from facing (135° total)
- **Back Arc:** everything else (225°)

### GML Implementation
```gml
function is_attacking_from_behind(attacker_id, defender_id) {
    var attack_angle = point_direction(defender_id.x, defender_id.y, attacker_id.x, attacker_id.y);
    var angle_diff = angle_difference(defender_id.direction, attack_angle);
    return (abs(angle_diff) > 67.5);
}

// In damage calculation:
var crit_bonus = 0;
if (is_attacking_from_behind(attacker_id, target_id)) {
    crit_bonus = attacker.back_attack_crit_modifier; // default 0
}
var final_crit_chance = attacker.crit_chance + crit_bonus;
```

---

## 5B. Puddle System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** All 4 Elemental Portal lines, Rune Scribe (Enclave)

### Rules
- Circular zone on battlefield with defined radius and duration
- Units inside receive effect every second (tick-based)
- Buff puddles: refresh buff duration every 1s while unit is inside
- Debuff puddles: apply/refresh debuff every 1s
- When unit LEAVES: buff/debuff expires after its listed duration
- Puddles render BELOW units (positive depth)
- Moving puddles (e.g. Tornado) use same system with position update each step

### Puddle Object Properties
```
position (x, y)
radius (px)
duration (seconds — use -1 for permanent)
type (string: "fire", "storm", "tornado", "rune", etc.)
effect (buff/debuff struct applied per tick)
tick_rate (default 1s)
owner (unit instance that created it)
```

### New Status Effects from Puddles
- **Burning (1s):** Damage immunity from same puddle source while active; 2× damage from Ember Wisp unlock
- **Thunderstruck (5s full stun):** Cannot function; counts as stun for all synergies; 2× damage from Waterling unlock
- **Dorothy (10s):** 2× damage from Gustling unlock; knocked away by Tornado, Gustlings chase to follow up

---

## 5C. Ballista Shot System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Construction Yard — Heavy Crossbow Tower line  
**Note:** "The spectacle IS the ability" — no upgrades, no unlock needed

### What the Player Sees
- Oversized bolt (~150% size at launch) travels at half normal arrow speed
- Bolt **shrinks** from 150%→100% size during flight — this is the ONLY timing info for the player
- Lands → sticks as rotated sprite aligned to travel direction
- Near-lethal to anything directly hit
- Fires every 7 seconds (confirmed)
- Always renders on TOP of everything (most negative depth in game)
- "Every unit it passes over creates a rollercoaster of relief and distress"

### Implementation Steps
1. Tower picks **RANDOM** valid target in range (NOT closest)
2. Invisible 150px radius accuracy zone centered on target
3. **Drop Point** = random point within accuracy zone (pure chaos, no bias)
4. Calculate **Damage Tail**: 10px wide × 30px long rectangle along tower→Drop Point vector, tip at Drop Point, shaft extending BEHIND toward tower
5. Projectile spawns, travels toward Drop Point at half speed, shrinks during flight
6. On landing: remove projectile, place stuck sprite (rotated to travel angle)
7. Instantly check Damage Tail zone — apply full damage to all units in it
8. *(Future)* Killed units: `death_angle` set to bolt travel direction for skewer death animation

### Stats
| Tier | Damage | CD |
|---|---|---|
| T1 | 350 | 7s |
| T2 | 500 | 7s |
| T3 | 650 | 7s |

---

## 5D. Just Me and You — Wall Trap
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Elemental Portal — Pebblekin line  
**⚠️ Note:** The Pebblekin line is in Elemental Portal (not Construction Yard as earlier assumed)

### Mechanics
- Spawns 60px wide × 60px deep × 10px thick stone wall centered on nearest enemy
- Units HIT by the spawning wall: stunned 2s + pushed to outside of wall
- One unit caught dead center is TRAPPED inside
- Wall blocks ALL projectiles and line of sight for duration
- 10px thick = real collision object (needs wall collision object in GMS2)

### Stats
| Tier | Wall Duration | CD | Spawn Damage (unlock) |
|---|---|---|---|
| T1 | 10s | 60s | 20 |
| T2 | 10s | 45s | 30 |
| T3 | 10s | 30s | 40 |

**Unlock: "Crunch"** — 500 Rubies + 500 Sapphires  
Units struck by spawning wall also take damage (20/30/40 by tier, scales with tier not upgrades)

**⚠️ Open Question:** Does tornado (Gustling) bounce off battlefield edges or disappear? Needs decision before implementation.

---

## 5E. Shadow Adept — Teleport Behind
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Enclave Slot 4, future Adventurer's Guild Rogue

### State Machine
```
STATE 1 — APPROACH
  Path toward nearest enemy melee unit normally

STATE 2 — STUCK CHECK
  Every step: check if position delta < 5px over 0.5s
  OR: reached melee range of target
  → Either condition: trigger TELEPORT

STATE 3 — TELEPORT BEHIND
  var behind_angle = target.direction + 180;
  var tx = target.x + lengthdir_x(35, behind_angle);
  var ty = target.y + lengthdir_y(35, behind_angle);
  Snap position to (tx, ty). Play VFX.
  → Enter ATTACKING state

STATE 4 — ATTACKING
  Every frame: check is_attacking_from_behind(self, target)
  TRUE → apply DOUBLE DAMAGE modifier
  FALSE (target rotated) → wait 1s, re-enter TELEPORT state
  1 second delay prevents flickering
```

---

## 5F. Vision Void
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Enclave — Initiate line unlock

### Rules
- 40px diameter black circular zone placed on battlefield
- Ranged units CANNOT draw line of sight THROUGH the void
- **Enemy melee inside:** "Alone in the Dark" — cannot block or parry, lasts 2s, refreshes every 1s while inside
- **Allied units inside:** "Comforting Void" — +3 HP/s, lasts 10s, refreshes every 1s while inside

---

## 5G. Flag / Zone Suggestion System
**Status:** CONCEPT ONLY — needs full design before implementation  
**Required by:** Future larger maps, story maps, tactical gameplay

### Design Intent
As maps grow larger, units need soft directional guidance. This is an **indirect suggestion system** — NOT direct control. Preserves auto-battler feel while giving player meaningful input.

### Proposed Mechanics
- Player places **Flag objects** on the map during battle
- Types: **Attack Here**, **Defend Here**, **Rush Here** (names TBD)
- Units with no current priority target bias movement toward relevant flags
- Units actively in combat ignore flags
- Flags are suggestions, not orders — attacked unit still responds to attacker
- Flags may have influence radius rather than point destination
- **⚠️ Needs full design spec before implementation**

### Why This Exists
- Current flat arena has no spatial strategy
- Larger story maps require units to understand positional priorities
- Towers especially need to "cover this approach" vs "cover that one"
- Without flags, large maps become unreadable chaos

---

## 5H. Summon / Pet System
**Status:** DESIGNED, NOT IMPLEMENTED  
**Required by:** Beast Kennel (Falconer + Beast Handler lines), Enclave Spirit Caller

### Rules
- Summoned units have their OWN stat blocks (separate from handler)
- Flying summons: `Flying` tag, no ground collision
- Dead summons are re-summoned on cooldown (one at a time for multi-summon)
- Upgrades on the handler line improve summon HP and damage
- Handler's own `damage`/`attack` stats may be irrelevant if handler only summons
- **⚠️ Needs full system design — stat blocks for each animal type needed**

---

# 6. BUILDINGS & FULL ABILITY SPECS

## LINE ID FIX — CRITICAL BUG
The following buildings have WRONG `line_id` values in the game data. All T2/T3 units are currently pointing to the wrong line. The "Correct line_id" column from the audit is the fix needed:

**Pattern:** T1 unit of each line has correct line_id. T2 and T3 often point to a different line's ID.  
**Fix required in:** All unit template files for these buildings: Academy, Barracks, Beast Kennel, Chapel, Construction Yard, Elemental Portal, Enclave, Grand Cathedral, Grove, Magic School, Monster Lair, Mountain Keep, Outpost  
**Castle, Siege Workshop, Mountain Keep Slot 1:** ✅ Already correct across all tiers

---

## ACADEMY
**Line IDs:** `student_line`, `alchemist_line`, `archivist_line`, `apprentice_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Student | Scholar | Mastermind |
| 2 | Alchemist | Chemist | Transmuter |
| 3 | Archivist | Lorekeeper | Grand Historian |
| 4 | Apprentice Mage | Spellwright | Arcane Professor |

### Slot 1 — Student Line
**PRIMARY: "My Notes!"**
- On death: grants XP to nearby friendly units (NOT self)
- T1: 1 XP / 150px | T2: 2 XP / 200px | T3: 3 XP / 250px
- Upgrade ×5: +1 XP on death | Custom cost curve: 30/60/240/480/960 Sapphires
- No unlock

### Slot 2 — Alchemist Line
**PRIMARY: "Corrode"**
- On hit: applies Corrode stack (5s). Stacks follow buff rules on enemy units. Removes armor and increases damage taken.
- T1: -1 armor, +10% dmg taken | T2: -2 armor, +20% | T3: -3 armor, +30%
- Upgrade ×10: +2% bonus damage taken (all tiers) | Rubies, cost doubles from 15

**UNLOCK: "Mega Flasks"** — 240 Rubies  
Flasks 50% larger, explode on impact: damage + Corrode in 120px AoE. Replaces single-target hit.

### Slot 3 — Archivist Line
**PRIMARY: "Recorded Tactics"**
- Aura. Nearby allies get +10% XP (does NOT apply to self)
- T1: 120px | T2: 160px | T3: 200px
- Upgrade ×10: +5px radius AND +1% XP bonus | Cost doubles from 15

**UNLOCK: "Into the Histories!"** — 540 Sapphires  
Requires unit level 3. If an ally dies within range: ALL other affected allies gain +10% stacking damage until battle ends. Stacks per death.

### Slot 4 — Apprentice Mage Line
**PRIMARY: "Shared Insight"**
- On hit: debuff to target. Stacks infinitely. Timer refreshes on each hit. ALL stacks drop if 5s passes with no hit.
- T1: +10% dmg debuff | T2: +20% | T3: +30%
- Upgrade ×10: +1% debuff AND +1s duration | Rubies, cost doubles from 15
- ⚠️ Balance note: May need 3-unit stack cap if OP — monitor in playtesting
- No unlock

---

## BARRACKS
**Line IDs:** `spearman_line`, `footman_line`, `scout_line`, `militia_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Spearman | Halberdier | Sentinel |
| 2 | Footman | Man-at-Arms | War Marshal |
| 3 | Scout | Ranger | Pathfinder |
| 4 | Militia | Veteran | Warborn |

### Building Ability
**"Bunk Beds"** — Building-level unlock  
When a Barracks unit dies, it can respawn at the Barracks.  
T1: 1 respawn | T2: 2 respawns | T3: 3 respawns (max 5 with upgrades)  
Upgrade ×5: +1 respawn per purchase | Cost: 150 Stone + 150 Steel + 150 Wood (doubles each purchase)

### Slot 1 — Spearman Line
**PRIMARY: "Phalanx Thrust"**
- Every 3s: deals +3 armor-ignoring damage. Hits all units in a line in front.
- T1: 120px line | T2: 160px | T3: 200px
- Upgrade ×10: +5% line width (stacks multiplicatively) | Steel, cost doubles from 15

**UNLOCK: "Almighty Push"** — 240 Steel + 240 Sapphires  
Permanent: Unit has 300% Knockback Power.

### Slot 2 — Footman Line
**PRIMARY: "Press the Line!"**
- On first combat entry (once): unit gains +10% Parry, +10% Block, +10% Knockback Chance, +10% Crit, +10% Dmg Reduction
- T1: 2s | T2: 3s | T3: 4s duration
- Upgrade ×10: +1s duration | Steel, cost doubles from 15
- Stack example: 5% global parry + 10% "I Believe!" + 10% "Press the Line!" (when active) = 25% total

**UNLOCK: "I Believe!"** — 240 Steel + 240 Emeralds  
Permanent buff: +10% Parry, +10% Block, +2× Knockback Power. Stacks with Press the Line! when active.

### Slot 3 — Scout Line
**PRIMARY: "Over Here!"**
- Taunts nearest ranged unit at 300% Scout's range for duration. Taunted unit: +50% range, -50° accuracy. Scout moves perpendicular to taunt angle 1s then fires at target.
- While taunt ON COOLDOWN: Scout fire rate ×2, accuracy -120°
- T1: 3s taunt / 30s CD | T2: 4s / 30s | T3: 5s / 30s
- Upgrade ×10: -1.5s cooldown | Wood, cost doubles from 15
- No unlock — already complex

### Slot 4 — Militia Line
**PRIMARY: "Massed Ranks"**
- Passive: gains +10% bonus damage per friendly Barracks unit alive (cap: 4 units, +40% max base)
- T1/T2/T3: same mechanic, scales with tier stats
- Upgrade ×10: +1% per Barracks unit | 500 Steel + 500 Sapphires, cost doubles

**UNLOCK: "Unwashed Masses"** — 120 Steel + 120 Sapphires + 120 Stone + 120 Emeralds  
**REQUIRES Bunk Beds unlocked at Barracks.**  
On death: spawns (unit level - 1) units of SAME type at same level. Spawned units also have this ability.  
⚠️ Level 10 Militia = 9 level-9 units, each spawning 8... INTENTIONAL exponential meme. Hard counter: Solar Beam.

---

## BEAST KENNEL
**Line IDs:** `pup_line`, `falconer_line`, `beast_handler_line`, `horned_cub_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Pup | War Hound | Alpha Fang |
| 2 | Falconer | Skywatcher | Storm Talon |
| 3 | Beast Handler | Packmaster | Wildcaller |
| 4 | Horned Cub | Rambeast | Thunderhorn |

### Slot 1 — Pup Line
**PRIMARY: "Take Down"**
- Every CD: stuns target 3s (cannot attack, flee, or use abilities) + drags enemy backwards at Pup's move speed for 3s
- T1: 30s CD | T2: 25s | T3: 20s
- Upgrade ×10: -1s cooldown | Sapphires, cost doubles from 15

**UNLOCK: "Go for the Throat!"** — 540 Steel + 240 Sapphires  
Requires unit level 3. During Take Down: if enemy is at 20% HP or below, enemy is killed when stun ends. Execute mechanic.

### Slot 2 — Falconer Line ⚠️ STAT REDESIGN NEEDED
**PRIMARY: "Fast & Featherous"**
- Summons a bird (flying melee, no collision) that attacks Falconer's target
- T2 name: "2 Fast 2 Featherous"
- Bird resummon CD on death: TBD
- Upgrade ×10: +Bird HP, +Bird Damage | Cost doubles
- ⚠️ Falconer/Skywatcher currently identical to Beast Handler/Packmaster (copy-paste error). Storm Talon wrongly set as Magic AOE. ALL need stat redesign as Summoner type. See stat review for proposed changes.

**Bird tag spec:** T1 bird: `not_spawnable, Summoned, Flying, Melee, Physical, Single`  
T2/T3 bird: `not_spawnable, Summoned, Flying, Melee, Magic, Single` (Magic = ignores armor; Melee = gets parry chance)

### Slot 3 — Beast Handler Line ⚠️ STAT REDESIGN NEEDED
**PRIMARY: "Unauthorized Menagerie"**
- Summons ground animals: T1: 2 Badgers | T2: 3 Snow Wolves | T3: 4 Lions
- Dead summons re-summoned one at a time on cooldown
- Upgrade ×10: +Summon HP, +Summon Damage | Cost doubles
- No unlock
- ⚠️ Each animal type (Badger/Wolf/Lion) needs own stat block

**Summon tag specs:**
- Badger: `not_spawnable, core2, Summoned, DPS, Melee, Physical, Single`
- Snow Wolf: `not_spawnable, core3, Summoned, DPS, Melee, Physical, Single`
- Lion: `not_spawnable, core4, Summoned, DPS, Melee, Physical, Single`

### Slot 4 — Horned Cub Line
**PRIMARY: "Charge!"**
- On line of sight: charges target, gaining +10px speed every 0.25s
- On collision: deals 1×/2×/3× move speed as damage (T1/T2/T3). Both units stunned 5s. 100% KB Chance, 2× KB Power.
- Upgrade ×10: +10px base speed | Steel, cost doubles from 15

**UNLOCK: "It's Too Big!"** — 250 Steel + 250 Stone + 500 Sapphires (⚠️ cost TBD — needs Morgan confirmation)  
Charge collision: 110px AoE damage. Survivors: -50% move speed for 5s.

---

## CASTLE
**Line IDs:** `squire_line`, `archer_line`, `shield_bearer_line`, `banner_carrier_line`  
✅ All line_ids correct in game data

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Squire | Knight | Knight Captain |
| 2 | Archer | Crossbowman | Marksman |
| 3 | Shield Bearer | Tower Guard | Bulwark Champion |
| 4 | Banner Carrier | Standard Knight | Royal Vexillum Master |

### Slot 1 — Squire Line
**PRIMARY: "Cleave"**
- Cone AoE damage in front of unit
- T1: 70° cone | T2: 90° | T3: 110°
- Upgrade ×10: +5° to all tiers | 15 Steel, cost doubles

**UNLOCK: "Concentrated Parry"** — 120 Steel + 120 Sapphires  
Requires unit level 3. Only triggers vs melee attackers. +30% parry for 5s. CD: 25s/20s/15s by tier.

### Slot 2 — Archer Line
**PRIMARY: "Hunting"**
- Target nearest support/magic unit. Arrows ignore melee collision for 10s.
- T1: 30s CD | T2: 25s | T3: 20s
- Upgrade ×10: -1s CD to all tiers | 15 Wood, cost doubles

### Slot 3 — Shield Bearer Line
**PRIMARY: "Fresh" (Passive)**
- While above HP threshold: take 20% less damage from all sources
- T1: 80% threshold | T2: 70% | T3: 60%
- Upgrade ×10: -2% threshold to all tiers | 15 Steel, cost doubles

**UNLOCK: "Mockery"** — 60 Steel + 60 Emeralds  
Requires unit level 2. AoE taunt in 120px radius around self every 10s.

### Slot 4 — Banner Carrier Line
**PRIMARY: "Inspired" (Aura)**
- Units within 200px get attack + move speed buff. Lingers 10s after bannerman dies.
- T1: +10% | T2: +15% | T3: +20%
- Upgrade ×10: +1% to all tiers | 15 (cost doubles)
- **Buff stacking rule:** Same tier refreshes. Higher tier overwrites. Lower tier ignored (does not refresh higher). On higher expiry, lower can apply.

---

## CHAPEL
**Line IDs:** `acolyte_line`, `lightbringer_line`, `penitent_line`, `hymnist_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Acolyte | Priest | High Cleric |
| 2 | Lightbringer | Sun Knight | Dawn Paladin |
| 3 | Penitent | Zealot | Crusader |
| 4 | Hymnist | Cantor | Choirmaster |

### Current Stats
| Unit | Type | HP | Armor | Dmg | Atk Spd | Range | Tags |
|---|---|---|---|---|---|---|---|
| Acolyte | Support | 22 | 0 | 1 | 1.8 | 140 | magic,support |
| Priest | Support | 28 | 1 | 1 | 2.0 | 160 | magic,support |
| High Cleric | Support | 36 | 2 | 2 | 2.2 | 180 | magic,support |
| Lightbringer | Melee | 55 | 2 | 3 | 1.9 | 50 | melee ⚠️ attack_area:90 on Melee Single makes no sense |
| Sun Knight | Melee | 70 | 3 | 4 | 2.0 | 55 | melee |
| Dawn Paladin | Melee | 85 | 4 | 5 | 2.1 | 55 | melee |
| Penitent | Melee | 40 | 1 | 6 | 1.3 | 50 | melee (DPS/hybrid) |
| Zealot | Melee | 50 | 2 | 8 | 1.4 | 50 | melee |
| Crusader | Melee | 60 | 3 | 11 | 1.5 | 55 | melee |
| Hymnist | Support | 18 | 0 | 0 | 2.4 | 170 | aoe,magic,support |
| Cantor | Support | 24 | 0 | 0 | 2.6 | 190 | aoe,magic,support |
| Choirmaster | Support | 30 | 1 | 0 | 2.8 | 210 | aoe,magic,support |

### Slot 1 — Acolyte Line
**PRIMARY: "Mend"**
- Heals lowest HP friendly unit in range for X HP/s. Uses healer repositioning logic.
- T1: 4 HP/s, 140px | T2: 8 HP/s, 170px | T3: 12 HP/s, 200px
- Upgrade ×8: +5px range, +0.25 HP/s, -5% CD | Emeralds, cost doubles from 15

**UNLOCK: "Anointed Mend"** — 1000 Emeralds  
Requires level 3. Mend also heals all allies within 60px of primary target for 25% of Mend's value (after all buffs).

### Slot 2 — Lightbringer Line
**PRIMARY: "Battle Buddy"**
- Picks nearest melee unit within 100px (random if equidistant). "Battle Buddy" buff for 60s: buddy receives 25% of Lightbringer's damage as healing, buddy's parry +20%.
- Upgrade ×10: +3% healing transferred, +3% parry | Emeralds, cost doubles from 15
- Reselects if buddy dies

**UNLOCK: "Blessed Weapons"** — 840 Emeralds  
Requires level 3. Permanent: Lightbringer AND current Battle Buddy both gain +4 damage and all attacks become Magic type (ignores armor). Updates when buddy changes.

### Slot 3 — Penitent Line
**PRIMARY: "Holy Onslaught"**
- Passive self-heal 1/2/3 HP/s. Bonus damage = % missing HP (e.g. 50% missing = +50% damage, scales to 99%).
- Absorbs 20% of all damage dealt to friendly units within 60px onto self. (Intentional — dies faster but hits harder.)
- Upgrade ×10: -5% self-heal CD | Emeralds, cost doubles from 15

**UNLOCK: "Glorious Purpose!"** — 1200 Emeralds  
Requires level 3. Once per battle: when unit would die, becomes fully invulnerable to ALL damage, spells, effects, abilities for 10 seconds. Full immunity — nothing gets through.

### Slot 4 — Hymnist Line
**PRIMARY: "Hymn of Solace"**
- Every 2s: heals ALL friendly units within range for X HP. Uses healer repositioning logic.
- T1: 2 HP, 140px | T2: 4 HP, 140px | T3: 6 HP, 140px
- Upgrade ×10: +5px range, +0.25 HP per cast | Emeralds, cost doubles from 15

**UNLOCK: "Grand Chorus"** — 1500 Emeralds  
Requires level 3. All friendly units in Hymn range also gain +2 damage and heal 1 HP on every attack. Range scales with Hymn upgrades.

---

## CONSTRUCTION YARD
**Line IDs:** `archer_tower_line`, `heavy_crossbow_tower_line`, `apprentice_tower_line`, `ritual_tower_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Archer Tower | Crossbow Tower | Marksman Tower |
| 2 | Heavy Crossbow Tower | Ballista Tower | Dragon Slayer Tower |
| 3 | Apprentice Tower | Mage Tower | Archmage Tower |
| 4 | Ritual Tower | Battle Ritual Tower | War Ritual Tower |

### Current Stats (⚠️ ALL towers currently type:Melee — WRONG. All need type:Tower/Ranged fix)
| Unit | HP | Armor | Dmg | Atk Spd | Range | Notes |
|---|---|---|---|---|---|---|
| Archer Tower | 350 | 6 | 40 | 2.5 | 650 | ✅ Stats look reasonable. Multi T1=1 arrow |
| Crossbow Tower | 500 | 8 | 65 | 2.5 | 800 | ✅ Multi T2=2 arrows |
| Marksman Tower | 650 | 10 | 90 | 2.5 | 950 | ✅ Multi T3=3 arrows |
| Heavy Crossbow Tower | 350 | 6 | 100 | 7 | 900 | ✅ T1 intentional — high dmg, very slow |
| Ballista Tower | 500 | 8 | 65 | 2.5 | 800 | ⚠️ BROKEN — copy of Crossbow T2 |
| Dragon Slayer Tower | 650 | 10 | 90 | 2.5 | 950 | ⚠️ BROKEN — copy of Marksman T3 |
| Apprentice Tower | 350 | 6 | 28 | 1.6 | 650 | ✅ Fast, low dmg, magic |
| Mage Tower | 500 | 8 | 65 | 2.5 | 800 | ⚠️ BROKEN — copy of Crossbow T2 |
| Archmage Tower | 650 | 10 | 90 | 2.5 | 950 | ⚠️ BROKEN — copy of Marksman T3 |
| Ritual Tower | 350 | 6 | 28 | 10 | 650 | ✅ Slow AOE (220px), ability-based |
| Battle Ritual Tower | 500 | 8 | 45 | 10 | 800 | ✅ Good scaling (280px AOE) |
| War Ritual Tower | 650 | 10 | 62 | 10 | 950 | ✅ Strong T3 (340px AOE) |

### Slot 1 — Archer Tower Line
**Passive: Multi 1/2/3 arrows firing in concave arc (T1/T2/T3)**  
**UPGRADE: "Volley"** — Stone, cost doubles from 15  
+1 damage per arrow, +10px range per upgrade ×10

**UNLOCK: "Full Quiver"** — 600 Stone (⚠️ cost TBD — needs confirmation)  
Permanent: +2 arrows to all tiers (1→3, 2→4, 3→5). Concave arc expands. Spread angle unchanged = tighter grouping.

### Slot 2 — Heavy Crossbow Tower Line
**PRIMARY: "Ballista Bolt"** — See Section 5C for full implementation spec  
- T1: 350 dmg | T2: 500 | T3: 650 | CD: 7s
- **No upgrades, no unlock. The spectacle IS the ability.** Stone resource.

### Slot 3 — Apprentice Tower Line
**PRIMARY: "Chain Lightning"**
- Targets unit in range (prioritizes flying units). Deals damage then jumps to nearby unit within 60px every 0.2s, up to 5 jumps (6 total targets). Each jump -10% damage. "Charged" debuff on hit: 2s, cannot be targeted by Chain Lightning (prevents infinite loop).
- T1: 10 dmg | T2: 24 dmg | T3: 46 dmg | CD: 3s
- Damage per jump: 100%/90%/80%/70%/60%/50%
- Upgrade ×10: +2 damage, +2px jump range (base 60px, max 80px) | Rubies, cost doubles from 15

**UNLOCK: "Overcharged!"** — 850 Rubies  
Requires level 3. Chain Lightning max jumps +3 (5→8 jumps, 6→9 total targets hit).

### Slot 4 — Ritual Tower Line
**PRIMARY: "Solar Beam"**
- Selects RANDOM enemy in range. After 15s charge: fires Solar Beam dealing damage in AoE. Magic damage — ignores armor.
- T1: 30 dmg, 280px diameter | T2: 40 dmg, 320px | T3: 50 dmg, 360px | CD: 15s charge
- ⚠️ Design note: Damage = EXACT base HP of Militia/Veteran/Warborn (30/40/50). One-shots base Militia units. Hard counter to "Unwashed Masses" swarms. Intentional.
- Upgrade ×10: +1 damage, +5px diameter | Stone, cost doubles from 15

---

## ELEMENTAL PORTAL
**Line IDs:** `ember_wisp_line`, `waterling_line`, `gustling_line`, `pebblekin_line`  
**All 4 lines: melee magic units (ignores armor). Corrupted variants paired correctly.**

| Slot | T1 | T2 | T3 | Corrupted T1 | T2 | T3 |
|---|---|---|---|---|---|---|
| 1 (Fire) | Ember Wisp | Flame Spirit | Inferno Herald | Ember Wretch | Flame Revenant | Infernal Herald |
| 2 (Water) | Waterling | Tidelord | Abyss Caller | Mireling | Abyss Tidelord | Drowned Caller |
| 3 (Wind) | Gustling | Wind Dancer | Skyblade | Gale Imp | Wind Reaver | Skyterror |
| 4 (Earth) | Pebblekin | Earthbound | Terra Warden | Pebblefiend | Earthsplitter | Terra Ravager |

### Stats (all confirmed)
| Line | T1 HP | T1 Armor | T1 Dmg | T1 Spd | T1 Move | Role |
|---|---|---|---|---|---|---|
| Ember Wisp | 60 | 1 | 12 | 0.8 | 110 | Fast, fragile, high damage |
| Waterling | 70 | 1 | 14 | 1.5 | 90 | Balanced |
| Gustling | 50 | 1 | 9 | 0.5 | 130 | High move speed, possible flying |
| Pebblekin | 90 | 1 | 25 | 2.0 | 70 | Tanky, slow, earth |

### Slot 1 — Ember Wisp Line (Fire)
**PRIMARY: "Wall of Flame"**
- Spawns 200px wide × 40px tall horizontal wall of flame for 10s.
- Puddle: enemies entering take damage + "Burning" (1s buff). Immune while Burning. On Burning expiry if still inside: damage + reapply.
- T1: 10 dmg/tick, 30s CD | T2: 15 dmg, 25s CD | T3: 20 dmg, 20s CD
- Upgrade ×10: +10px width, +1 dmg/tick | Rubies, cost doubles from 15
- ⚠️ Wall placement TBD: in front of caster, or centered on target?

**UNLOCK: "No Fury Like Me"** — 500 Rubies + 500 Sapphires  
Ember Wisp line deals double damage to targets with "Burning".

### Slot 2 — Waterling Line (Water)
**PRIMARY: "Sudden Showers"**
- On entering melee combat: spawns storm cloud 60px in front, 120px diameter, lasts 20s. Enemies entering: stunned 1s + "Thunderstruck" (5s buff). Immune to further stuns from THIS cloud while Thunderstruck.
- T1: 60s CD | T2: 50s | T3: 40s
- No upgrades

**UNLOCK: "Choking Hazard"** — 500 Rubies + 500 Sapphires  
Waterling line deals double damage to ANY stunned unit (not just Thunderstruck). Very broad — works with Take Down, Just Me and You, all stun sources.

### Slot 3 — Gustling Line (Wind)
**PRIMARY: "Tornado Time"**
- On entering melee combat: spawns 80px diameter tornado 40px in front for 20s.
- Tornado moves independently: picks random direction, travels 50px/s for 5s, then new direction, repeats until expired.
- Enemies entering: disoriented (cannot attack) 2s + 150px knockback + "Dorothy" (10s buff)
- T1/T2/T3: 20s duration, **5 MINUTE CD** (intentional — it's a spectacle event)
- Upgrade ×10: +5s duration (base 20s, max 70s at 10 upgrades) | Rubies, cost doubles from 15
- ⚠️ Open question: Does tornado bounce off battlefield edges or disappear?

**UNLOCK: "Follow the Yellow Brick Road"** — 500 Rubies + 500 Sapphires  
Gustling line deals double damage to units with "Dorothy" buff. Dorothy lasts 10s — Gustlings chase to follow up.

### Slot 4 — Pebblekin Line (Earth)
**PRIMARY: "Just Me and You"** — See Section 5D for full spec  
- T1: 10s wall, 60s CD | T2: 10s, 45s | T3: 10s, 30s | 0 spawn damage

**UNLOCK: "Crunch"** — 500 Rubies + 500 Sapphires  
Units struck by spawning wall take 20/30/40 damage (by tier).

---

## ENCLAVE
**Line IDs:** `initiate_line`, `shadow_adept_line`, `spirit_caller_line`, `rune_scribe_line`  
⚠️ Note: Slots are numbered differently from earlier documentation — see actual data below:

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Initiate | Mystic | Seer |
| 2 | Shadow Adept | Veilwalker | Night Sage |
| 3 | Spirit Caller | Soulbinder | Astral Warden |
| 4 | Rune Scribe | Glyphmaster | Arcforge Arcanist |

### Slot 1 — Initiate Line
**PRIMARY: "Find Weakness"**
- Targets closest enemy melee. Debuff: -2 armor + 3/6/9 magic damage per hit from ANY source.
- Duration: 30s | CD: 2 min
- Upgrade ×10: +0.5 magic damage per hit, +5s duration | Price doubles

**UNLOCK: "Vision Void"** — 500 Sapphires + 500 Emeralds  
See Section 5F for full spec.

### Slot 2 — Shadow Adept Line
**PRIMARY: "Shadow Step"** — See Section 5E for full state machine  
Teleports behind target, deals double damage from back arc.

**UNLOCK: "Non-Threatening"** — TBD cost  
Cannot be targeted if any other valid target exists within 200px of the attacker. Override enemy target selection.

### Slot 3 — Spirit Caller Line
**PRIMARY: "Summon Spirit"**
- Spawns flying creature: Wisp (T1) / Ghost (T2) / Death Avatar (T3)
- Attacks Spirit Caller's current target
- Resummon on death: 30s/25s/20s by tier
- Upgrade ×10: +5 HP, +1 damage, +5px Spirit Caller targeting range | Price doubles

**UNLOCK: "Haunting Aura"** — 500 Sapphires  
Creature becomes 60px diameter aura instead of direct attacker. Enemy units in aura: "Haunted" = +40% knockback damage taken, -5% parry rate.

### Slot 4 — Rune Scribe Line
**PRIMARY: "Rune Circle"**
- Targets friendly melee without "Energized" buff. Places 80px diameter rune, lasts 30s, 5s CD.
- "Energized" while on rune (refreshes every 1s): +5/10/15% parry, +5/10/15% move speed, +5/10/15% attack speed, +1/2/3 HP/s
- Upgrade ×10: +1% all stats, +0.5 HP/s | Price doubles

**UNLOCK: "Second Surge!"** — 200 Sapphires + 800 Emeralds  
When "Energized" expires naturally: unit healed for 20% max HP.

---

## GRAND CATHEDRAL
**Line IDs:** `cleric_line`, `confessor_line`, `martyr_line`, `seraphic_avenger_line`  
**3 slots only (9 units total) — exception building**

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Cleric | Battle Cleric | Hierophant |
| 2 | Confessor | Inquisitor | Grand Inquisitor |
| 3 | Martyr | Ascetic | Living Relic |
| 4 | Seraphic Avenger | Flamewing Justicar | Archon of the Final Flame |

*Full ability descriptions: to be scraped from xlsx*

---

## GROVE
**Line IDs:** `sproutling_line`, `dryad_line`, `acorn_scout_line`, `seed_sower_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Sproutling | Vinekin | Thorn Warden |
| 2 | Dryad | Grovekeeper | Sylvan Matron |
| 3 | Acorn Scout | Barkstrider | Forest Sentinel |
| 4 | Seed Sower | Bloomcaller | Verdant Sage |

**⚠️ GROVE AUDIT NEEDED BEFORE ABILITY DESIGN:**  
`acorn_scout_line` slot 3 has duplicate entries — `acorn_scout` AND `acorn_lurker` both assigned slot:3 tier:1. Same at T2 (Barkstrider+Barkreaver) and T3 (Forest Sentinel+Forest Reaper). Lurker variants have Ranged Single attack — likely corrupted units incorrectly mixed into player line. Full Grove audit required first.

*Ability descriptions: partial (Sproutling line: Sap Skin → Rooted Recovery → Thorns). Rest needs design.*

---

## MAGIC SCHOOL
**Line IDs:** `spark_adept_line`, `rune_novice_line`, `frost_acolyte_line`, `emberling_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Spark Adept | Lightning Savant | Stormcaller Archon |
| 2 | Rune Novice | Glyph Weaver | Arcane Sigilmaster |
| 3 | Frost Acolyte | Icebinder | Cryomancer |
| 4 | Emberling | Flame Adept | Pyromancer |

*Abilities: 0/12 — needs full design*

---

## MONSTER LAIR
**Line IDs:** `hatchling_line`, `kobold_line`, `cave_imp_line`, `slime_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Hatchling | Drake | Sky Serpent |
| 2 | Kobold | Kobold Warrior | Kobold Chieftain |
| 3 | Cave Imp | Tunnel Fiend | Abyssal Howler |
| 4 | Slime | Ooze | Gelatinous Monarch |

*Abilities: 0/12 — needs full design*

---

## MOUNTAIN KEEP
**Line IDs:** `stonekin_line`, `tunnel_scout_line`, `crystal_adept_line`, `mason_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Stonekin | Rockguard | Obsidian Sentinel |
| 2 | Tunnel Scout | Cavern Striker | Deep Delver |
| 3 | Crystal Adept | Geomancer | Earth Shaper |
| 4 | Mason | Architect | Bastion Master |

*Abilities: 0/12 — needs full design*

---

## OUTPOST
**Line IDs:** `lookout_line`, `courier_line`, `border_guard_line`, `signalman_line`

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Lookout | Watchman | Sentinel Scout |
| 2 | Courier | Messenger | Swift Herald |
| 3 | Border Guard | Patrolman | Frontier Marshal |
| 4 | Signalman | Beacon Keeper | Skyflare Warden |

*Abilities: 0/12 — needs full design*

---

## SIEGE WORKSHOP
**Line IDs:** `light_ram_line`, `bombard_crew_line`, `iron_husk_line`, `siege_engineer_line`  
✅ All line_ids correct across all tiers

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Light Ram | Reinforced Ram | Thunder Ram |
| 2 | Bombard Crew | Cannonade | Grand Bombard |
| 3 | Iron Husk | Steel Golem | Titan Automaton |
| 4 | Siege Engineer | War Mechanic | Doom Artificer |

*Abilities: 0/13 — needs full design*

---

## MISSING BUILDINGS (~5 total)
Completely absent from codebase and design docs — need to identify and recreate:
- **Adventurer's Guild** — Contains Rogue unit (needs facing system + invisibility + backstab)
- **Ballistic Range** — Ranged units
- **[3 more TBD]** — Morgan to confirm which buildings are missing
- Future: **Volley Gun** noted as a standalone unit for one of the missing buildings (organ gun style, Multi tag firing cannon balls)

---

# 7. ENEMY GENERATION SYSTEM

## High-Level Flow
`generate_enemy_army_with_accumulated_dp()`:
1. Ensure templates valid/loaded
2. Read `total_dp = global.enemy_dp_accumulated`, `quest_type = global.current_quest.bucket_type`
3. Initialize `global.enemy_army_data = []`
4. Spawn enemy Castle first via `create_unit_data("Castle")`
5. Split DP into buckets via `get_dp_budget_percentages(round_num)` — buckets: `core`, `counter`, `spice`, `tower`, `siege`
6. For each bucket: `spawn_from_bucket(...)` fills army using affordable templates from that bucket
7. Unit cap: **30 units + Castle**

## Spawning Rules
- For each unit: decide corrupted vs normal via `calculate_corruption_percent(round_num, quest_type, dp_budget, unit_tier)`
- Corruption chance increases with: later rounds, Greed quest type, higher DP budget, higher unit tier
- ⚠️ Unit tier influence may not be fully wired yet

## Hard Invariants
- `global.enemy_dp_accumulated` must be defined before calling generation
- All enemy units created via `create_unit_data()` — no manual struct construction
- DP spending never goes negative
- Unit cap enforced at 30 + Castle

## Known Issue
- All units currently tagged `core` → enemy AI buys wrong parity (1 endgame unit vs many squires)
- Fix blocked until full tag overhaul completes (after abilities phase)

---

# 8. TAG SYSTEM

**Status:** ❌ BROKEN — all units currently just tagged `core`. Enemy parity purchasing completely broken.  
**Fix:** After abilities phase complete, do full tag overhaul for ALL units.

## New Taxonomy (7 tag types, comma-separated)
```
Format: spawn_eligibility, spawn_bucket, object_category, role, attack_distance, damage_type, damage_area
Example: player_spawnable, core3, Unit, DPS, Melee, Physical, Single
```

### Tag Type 1 — Spawn Eligibility
`player_spawnable`, `enemy_spawnable`, `not_spawnable`

### Tag Type 1.1 — Spawn Sorting Buckets (currently all broken as `core`)
`core1` through `core10` (enemy buys many cheap → few expensive), `utility1/2/3`, `spice` (rare/flavour)

### Tag Type 2 — Object Category
`Building`, `Unit`, `Tower`, `Terrain`, `Summoned`, `Corrupted`

### Tag Type 3 — Unit Role
`DPS`, `Tank`, `Healer`, `Support`, `Siege`, `Summoner`, `Flying`

### Tag Type 4 — Attack Distance
`Melee`, `Ranged`, `Multi`

### Tag Type 4.1 — Damage Type
`Physical`, `Magic` (Magic = ignores armor)

### Tag Type 4.2 — Damage Area
`Single`, `Cleave`, `AOE`, `Puddle`

## Tanks Needing `Tank` Tag (30 units = 15 player + 15 corrupted)
Shield Bearer line, Footman line, Lightbringer line, Sproutling line, Stonekin line

---

# 9. UPGRADE & META SYSTEMS

## Meta Upgrade Scopes (Three Types)
1. **Economy/Meta** — Global or run-start constants. Key: `Economy:<upgrade_id>`
2. **Building-wide** — Affects whole building. Key: `<building>:<upgrade_id>`
3. **Unit Line** — Affects all tiers of a slot. Key: `<building>:slot<slot>:<upgrade_id>`

## Run Snapshot Model (Critical)
- Meta upgrades purchased AFTER a run starts do NOT affect that run
- At difficulty confirmation: `run_upgrade_snapshot = copy(global.army_upgrades)`
- All stat previews and unit instantiation during that run use the snapshot
- Upgrades purchased via Army Training only apply to the NEXT new run

## Stacking Rules
- All stat upgrades are additive
- Percent upgrades calculated from BASE stats (do NOT compound)
- Example: Base HP 100, +10% HP at 3 levels = +30 HP = 130 total
- UI must display: `Base + Bonus` (e.g. "HP: 100 + 30")

## Starting Gold
- Base: 100 gold
- Upgrades add fixed amounts (e.g. +25)
- Enables early strategic risks

---

# 10. RUN LOOP & ECONOMY

## Run Loop (currently in obj_battle_controller)
**Victory:** Give quest rewards → current_round += 1 → if round > 100: endgame (placeholder: room_restart()) → else: rm_quest_select  
**Defeat:** reset_enemy_dp() → current_round = 1 → meta resources persist → rm_unit_purchase

## Quest Economy
- **RP** = Resource Points (quest reward budget)
- **DP** = Difficulty Points (enemy budget)
- **5 RP → 1 DP** baseline conversion
- **1 gold = 1 RP | 1 meta resource = 5 RP**
- Quest reward types: Meta-heavy (35% gold) / Normal (55% gold) / Gold-heavy (65% gold)
- Greed Tier 1: +7% enemy DP gain | Greed Tier 2: +15% DP gain
- Each quest awards exactly **2 of the 6 meta resources**
- Total quest combinations: 15 pairs × 3 reward mixes × 3 greed tiers = **135 combinations**
- Base quest RP scales up slightly each round

## Gold Mine (Run Building)
- Cost: 50 gold | Output: +10 gold/round
- Upgradable: each upgrade +10 gold/round, costs scale

## Quarry (Meta Building)
- Output: 2 random meta resources/round base
- Upgradable up to 20 levels: +2 meta resources/level
- Player has NO control over resource type — random
- Timing: awarded as bonus added to quest rewards on battle success

---

# 11. SAVE / LOAD SYSTEM

## Two Save Domains
1. **Meta Save (Persistent):** Player profile, meta currencies, army_upgrades, ownership
2. **Run Save (Active Run):** Current run state + run_upgrade_snapshot

## Save Slots
- Files: `save_<slot>.json`
- Slot: `global.current_save_slot`
- `save_game()` must default to `global.current_save_slot` (NOT hardcoded slot 1)

## Run Save Contents
- Run progression: `current_round`, `current_difficulty`, `reward_multiplier`, `run_active`
- Enemy scaling: `enemy_dp_accumulated`
- Quest state: `current_quest` struct
- Economy: `gold`, run buildings (Gold Mine level, Quarry level)
- Player army: `player_army_data[]` — each unit has `template_id`, `unit_name`, `level`, `veterancy_xp`, placement data
- `run_upgrade_snapshot` — CRITICAL: copy of army_upgrades at run start
- `pending_outcome` — if exists, route to rm_battle_outcome on load

## Meta Save Contents
- `player_name`, `player_gender`, `player_level`
- Currencies: rubies, emeralds, stone, steel, wood, sapphire
- `army_upgrades` (map: upgrade_key → level)
- `units_owned`, `buildings_owned`, `upgrades_purchased`

## Never Save
Templates, registries, ds_map ids, instance ids — NEVER serialize these

## Checkpoint Rule
- Save during unit purchase → resume at unit purchase
- Save during combat → resume at **beginning** of last saved combat round (not mid-action)

## Pending Outcome Data Contract
`global.pending_outcome` struct (set by battle controller when combat ends):
- `result` ("victory"/"defeat"), `round_num`, `rewards_committed` (bool, default false)
- Victory only: `rewards` { gold_gained, dp_gained, meta_gained { rubies, emeralds, stone, steel, wood, sapphire } }
- `player_dead_count`, `enemy_dead_count`

## Known Gaps in Save System
- ⚠️ gender not yet persisted (patch needed)
- ⚠️ round/difficulty not yet persisted (patch needed)
- ⚠️ inventory/army/buildings not yet persisted (patch needed)
- See `patch_plan_save_gender_starting_building.md` for fix details

---

# 12. BATTLE CONTROLLER & OUTCOME

## Battle Controller Responsibilities
- Initialize battle state
- Determine turn order
- Execute unit actions
- Apply damage and effects
- Detect unit death
- Detect battle end (all enemy units dead = victory / all player units dead = defeat)
- Construct `global.pending_outcome`
- Hand off to Outcome Screen

## Hard Invariants
- No unit may be missing `current_stats`
- Battle resolution must NOT depend on frame rate
- Battle speed must NOT alter logic paths or outcomes

## Outcome Screen (rm_battle_outcome)
**This is the authoritative commit point for all battle results.**  
- Displays Victory/Defeat, casualty summary, reward breakdown
- Options: Continue, Save, Save & Quit
- **Legal save point**
- Rewards are NOT applied until player presses Continue

## Reward Apply Rule (Two-Phase)
1. **Compute (Pure):** Calculate rewards, store in `pending_outcome.rewards`. Do NOT modify globals.
2. **Apply (Commit):** On Continue — if `rewards_committed == false`: apply rewards, set `rewards_committed = true`. This ensures rewards can NEVER be applied more than once.

## Battle Speed System
- `global.battle_speed` (0.5×, 1×, 2×, 4×)
- Speed modifies time progression ONLY — never damage math, RNG, or win conditions
- Safe to toggle mid-battle

---

# 13. SUPPORT ACTIONS & XP SYSTEM

## Support Action Types
- **HEAL:** Restores HP to ally (clamped to max HP). Not reduced by armor.
- **BUFF:** Applies timed modifier to ally

## Support XP Credit Window
- When a unit successfully heals or buffs an ally: ally gains a support credit entry for the supporter
- Duration: **3000ms (3 seconds)**, refreshed on reapplication
- This credit causes supporters to share XP from kills made by the supported unit

## Kill XP Distribution
On enemy death:
1. `xp_total = dead_unit.xp_death`
2. Eligible contributors = damage contributors ∪ supporters of the killer (within valid 3s window)
3. Remove duplicates, remove dead units
4. `xp_share = xp_total / contributor_count`
5. Each contributor: `veterancy_xp += xp_share` → check level up

## Unit Leveling
- XP stored per instance (`unit_data.veterancy_xp`)
- Level-up behavior defined PER UNIT TEMPLATE (no global scaling table)
- XP and level are run-only (do NOT persist across runs)
- Multiple instances of same template can have different levels

## Training XP (Separate)
- Units can gain XP during unit purchase phase via gold spend
- Catch-up/optimization mechanic — unrelated to meta progression

---

# 14. MAP & BATTLE VISION

## Current State (Testing Only)
- Single flat battle arena
- Units meet in the middle and fight
- No terrain, no objectives, no positional strategy
- Towers have no spatial advantage over ground units
- No player agency during battle

## The Problem
**"Why do towers even exist?"**  
In the current flat arena, towers have zero advantage. No high ground, no chokepoints, no approaches to cover. The battle screen doesn't create conditions for towers — or most strategic unit placement — to feel meaningful.

Current format risks being **unentertainaing** as the game develops:
- No player agency during battle (watch and hope)
- No spatial variety
- No story context for fights
- No reason to care about unit positioning

## The Vision — Larger, Story-Driven Maps
Maps need to grow and evolve to support:
- **Terrain:** chokepoints, high ground, walls, bridges
- **Environmental storytelling:** maps change based on story context
- **Multiple approaches:** enemies come from different directions
- **Meaningful tower placement:** towers covering specific lanes
- **Objectives:** defend a gate, hold a position, escort something

## Flag / Zone Suggestion System
See Section 5G for full design spec.  
**This system must be designed before maps expand** — without it, larger maps become uncontrollable chaos.

---

# 15. ABILITY DEPLOYMENT SYSTEM

**Status:** CONCEPT — needs full design  
**Required by:** Mid-battle player agency, complex ability interactions

## Design Intent
Player has **indirect, imprecise tactical input** during battle — NOT direct unit control. Preserve auto-battler identity while giving meaningful decision points mid-battle.

## Proposed Mechanics
1. Player clicks ability icon from UI panel
2. Player places **target marker** on battlefield
3. System finds nearest unit that:
   - Has that ability
   - Is within a certain % of cooldown completion (e.g. within 20% of ready)
4. That unit attempts to move to marker and use ability there
5. If stuck or pulled into combat before reaching marker: uses ability where they are

## Key Design Principles
- Player places **intent**, not orders
- Units can be interrupted by combat
- Cooldown threshold prevents sending a near-CD unit across the map uselessly
- Creates interesting decisions: "Do I send my cleric across the battlefield for a clutch heal, risking they get stuck?"
- Works naturally with existing pathfinding and stuck-detection systems

## Open Questions
- Does marker persist if unit dies before reaching it?
- Can multiple units respond to same marker?
- Applies to all abilities or only "deployable" flagged ones?
- UI: how does player know which units are near-cooldown-ready?

---

# 16. UI SCREENS

## Home Screen
- Navigation hub: new run, continue run, load saves, army training, settings, quit
- "Continue Run" only available if `global.run_active == true`
- New Game / Start New Run: use next available slot
- Continue: uses latest active slot among 1-10
- ⚠️ Currency panel partially implemented — missing 2 resources (4 of 6 shown)

## Player Identity Screen
- Name input (letters + spaces only, max 20 chars)
- King / Queen gender selection
- "Begin Your Reign" only enabled when name AND gender are set
- On confirm: set globals, reset run, save, go to difficulty select
- ⚠️ Open question: Where is gendered starting building granted? (Expected: reset_run() or first-run initialization)

---

# 17. KNOWN BUGS, GAPS & BACKLOG

## 🔴 Active Bugs
| Priority | Item | Status |
|---|---|---|
| NOW | line_id WRONG for T2/T3 in most buildings | Fix building-by-building |
| NOW | All tower types set to "Melee" instead of "Tower/Ranged" | Fix during unit type audit |
| NOW | Army Training label shows "IRON" instead of "STEEL" | Code change only |
| NOW | Ballista Tower T2/T3 = copy-paste of Archer Tower T2/T3 | Needs stat redesign |
| NOW | Apprentice Tower T2/T3 = copy-paste | Needs stat redesign |
| NOW | Falconer/Skywatcher = identical to Beast Handler/Packmaster | Full stat redesign needed |
| NOW | Storm Talon wrongly set as Magic AOE (should be Ranged Summoner) | Full stat redesign needed |
| NOW | All units tagged only `core` — enemy parity purchasing broken | Blocked until tag overhaul |
| NOW | Gender, round, difficulty not persisted in save | Patch plan exists |
| NOW | Parry/Block/Knockback system designed but not implemented | Implement in combat overhaul |
| NOW | Global 20° inaccuracy cone missing from reimplementation | Needs adding |
| NOW | Magic damage ignoring armor not yet enforced in code | Implement in combat overhaul |
| NOW | `healing_other` field still active — superseded by ability system | Zero out once abilities implemented |

## 🟡 Design Pending
| Item | Notes |
|---|---|
| Horned Cub "It's Too Big!" unlock cost | Likely Steel-heavy, value TBD |
| Archer Tower "Full Quiver" unlock cost | Stone, value TBD |
| Wall of Flame placement offset | Centered on caster? In front? On target? |
| Tornado boundary behavior | Bounce or disappear at edges? |
| Falconer/Handler/Summon stat blocks | Needs Morgan's input on values |
| Grove full ability design | Must fix slot collision bug first |
| Grand Cathedral full ability descriptions | Data in xlsx but not yet scraped here |

## 🟠 Systems Needing Implementation
| System | Section | Blocked By |
|---|---|---|
| Parry/Block/Knockback | Section 4 | Unit type audit |
| Facing/Directional | Section 5A | Nothing — ready to implement |
| Puddle System | Section 5B | Nothing — ready to implement |
| Ballista Shot | Section 5C | Nothing — ready to implement |
| Just Me and You (wall) | Section 5D | Puddle/collision system |
| Shadow Adept Teleport | Section 5E | Facing system |
| Vision Void | Section 5F | LOS system |
| Flag/Zone Suggestions | Section 5G | Map system |
| Ability Deployment | Section 15 | Ability system |
| Multi-Projectile System | Section 4 | Projectile system |
| Summon/Pet System | Section 5H | Beast Kennel abilities |
| Global inaccuracy cone | Section 4 | Nothing |
| Chain Lightning "Charged" debuff | Section 6 | Status effect system |
| Combat resolution order documentation | Section 4 | Design decision |

## 🔵 Future Work
| Item | Notes |
|---|---|
| ~5 missing buildings | Identify, then design from scratch |
| Adventurer's Guild Rogue | Needs facing + invisibility system |
| Volley Gun standalone unit | After missing buildings identified |
| Map system | Full design needed before implementation |
| Marketplace (meta currency trading) | 5:1 or 10:1 painful exchange ratio |
| Death sprite rotation for Ballista | Requires `death_angle` variable on units |
| Narrative Bible | Currently empty |
| All 6 remaining buildings needing ability design | Magic School, Monster Lair, Mountain Keep, Outpost, Siege Workshop + 1 more |

---

# 18. FUTURE CHATBOT SPECIALIZATIONS

Once vault is complete enough, spin up specialized sessions for:

**Storytelling / Narrative** — needs: Narrative Bible, map designs, faction lore, unit backstories  
→ Requires: Narrative Bible written, story context established

**Game Balance** — needs: all unit stats, all ability values, economy tuning  
→ Requires: full xlsx data + all ability descriptions complete

**Unit "Feel" Design** — needs: movement, animation, sound, visual feedback specs  
→ Requires: all abilities designed, map system defined

**Map Design** — needs: map system documented, flag system designed, story context  
→ Requires: Section 14 fully expanded

**None of these are ready yet.** Vault must be substantially more complete first.

---

# 19. TECHNICAL NOTES — GAMEMAKER

## Depth System
- Lower depth = renders in FRONT
- Background rect: high positive depth (behind everything)
- Puddles: positive depth (below units)
- Projectiles: -10 to -100 (above background)
- Ballista bolt: most negative depth in game (always on top)

## Key Fixed Bugs (Do Not Regress)
1. Enemy generation in ONE place only — purchase screen Create event
2. Projectile depth must be negative
3. Boot system handles all global var init — do NOT init globals elsewhere
4. Sprite origins centered for all projectiles

## Coordinate System
- Purchase screen enemy preview zone: x=1184–1568
- Battle coordinates: x=994–1920
- Enemy generation maps purchase coords to battle coords

## Buff Stacking (Authoritative)
- Same tier: refresh duration
- Higher tier applied: overwrite lower, refresh duration
- Lower tier applied to higher: IGNORE (do NOT refresh higher duration)
- On higher expiry while lower source still active: lower can apply on next tick

---

*End of document.*  
*Session workflow: Start → fetch raw GitHub URL → Claude reads vault → continue work.*  
*End of session: Download updated vault → upload to GitHub → replace previous version.*
