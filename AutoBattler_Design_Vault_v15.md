# Auto Battler — Design Vault v6
**Internal version: v6 | GitHub filename: AutoBattler_Design_Vault_v15.md**  
**Last updated: 2026-03-20**  
**Status: AUTHORITATIVE — major redesign: 45-building tech tree, Home Castle Screen, unit roster gap analysis**
**Last Updated:** 2026-03-20  
**Status:** v5 — Adds fully documented combat system (crit, parry, block, knockback), complete hit resolution chain, GML pseudocode for all systems, unit polish patch notes (Hatchling line, tower attack types)  
**GitHub Workflow:** Download → upload to repo → replace previous version → next session Claude fetches raw URL

---

## Changelog

### v6 — 2026-03-20 (Design Vault v15)
**MAJOR REDESIGN — Home Castle Screen, 45-Building Tech Tree, Unit Roster Overhaul**

#### Structural Changes
- **Home Castle Screen** replaces old "Unit Purchase Screen" as the primary meta hub
- **45-building tech tree** fully confirmed (5 T1 → 10 T2 → 20 T3 → 10 T4 incl. Omni Fortress)
- **5 building rows** × 3 columns (T1→T2→T3) + 2 large T4 slots on right side of screen
- **Castle** moved to permanent home-screen fixture (top-right, above T4 column) — not purchasable
- **Gold Mine + Quarry** remain top-left and top-center of left side

#### Unit Reassignments
- **Castle (home screen):** inherits old Barracks roster — Footman · Spearman · Scout · Militia
- **Barracks (T1 R2):** gets old Castle units — Squire line · Banner Carrier line (+2 new slots TBD)
- **Armory (T2 R2):** Crossbowman line (renamed from Archer — T1 unit gets hand crossbow) · Shield Bearer line (+2 new slots TBD)
- **Dwarven Skyport (T4):** renamed from Clockwork Skyport — unit lines carry over
- **Siege Workshop units** (Light Ram · Bombard Crew · Iron Husk · Siege Engineer) → new home: Dwarven Smithy T2 (suggested)
- **Mountain Keep units** (Stonekin · Tunnel Scout · Crystal Adept · Mason) → new home: Sawmill T2 or Mason Shop T3 (suggested)
- **Magic School units** (Spark Adept · Rune Novice · Frost Acolyte · Emberling) → new home: Alchemical Workshop T2 (suggested)

#### Progression Rules (CONFIRMED)
- **Path choice is per-run** — T2 building selection locks for the run; reset on run loss
- **Building unlocks persist across runs (meta progression)** — player must unlock before purchasing, but unlocks never reset
- **Buildings must still be purchased each run** — you unlock once, buy each run
- **Run loss = dream failure** — player wakes up, starts fresh but keeps all meta unlocks
- **Thematic framing:** Player fights through dangerous countryside to reach corrupted father; each run = lucid dream; "writing home for reinforcements" each round

#### Roster Gap Analysis
- Current vault unit lines: **69** (17 buildings)
- Target unit lines: **184** (45 buildings × 4 + Castle 4)
- Confirmed carry-over lines: **53**
- Homeless lines needing new building assignment: **12**
- New unit lines needed from scratch: **119** (or ~107 if homeless lines are absorbed)

#### Layout Specs (GameMaker pixel-perfect at 1920×1080)
- Regular building slot: **256 × 128 px** (T1, T2, T3)
- T4 large plot: **608 × 360 px** (2 slots)
- Castle block: **608 × 232 px**
- Gold Mine / Quarry: **256 × 96 px**
- Left/right margin: **96 px**
- Column gaps (T1↔T2, T2↔T3, T3↔T4): **96 px**
- Row gaps (between building rows): **48 px**
- T4 gap (between 2 large slots): **32 px**
- Path zone at bottom: **96 px** = 3 × 32 px tiles
- Castle x: **1152**, T4 x: **1152–1760**, right margin: **160 px**
- Layout reference SVG saved as: `home_castle_screen_mockup_1920x1080.svg`

#### Screen Flow Update
Home Castle Screen → Vision Path Select (lucid dream branching) → Quest Select → Unit Deployment Screen → Battle Map → Outcome

#### Naming Collision — FLAGGED
- "Elven Outpost" (T3 building, Row 2) vs map camp building — one needs renaming
- Suggested: map building renamed to **Field Camp** (`bld_field_camp` in code)

---

### v5 — 2026-03-07
- **ALL BUILDINGS NOW FULLY DESIGNED** — this is the first version where every player building has complete ability specs
- Replaced stub entries (0/12 — needs full design) in §6 with full ability documentation for:
  - **Grove** — Growing Pains, Destructive Nature, In the Shade, Meadow of Tranquility
  - **Magic School** — Sudden Pop, Runic Mine, Frost Wave, Great Ball of Fire
  - **Monster Lair** — Chromatic Scales, Protect the Master!, Something in the Dark, It's All Just Goo
  - **Mountain Keep** — Prismatic Shield, Heh?! Walls?, Prismatic Healing, Boaghdar the Buildar
  - **Siege Workshop** — Ram, Bombard Crew, Iron Husk, Siege Engineer behavioral systems
  - **Outpost** — marked as cut/level-design-only (was placeholder)
- Added **Rangers Lodge** and **Clockwork Skyport** full ability summaries to §6 (previously only existed in appendix)
- Corrected stale note claiming Rangers Lodge/Clockwork Skyport abilities were lost — they are fully designed
- Redundant appendix sections (# BUILDING: GROVE, # BUILDING: MAGIC SCHOOL, etc.) retained at end of file for reference but §6 is now the authoritative location


- **Section 4 (Combat Systems) fully rewritten** — now contains complete authoritative documentation:
  - §4.0 Hit Resolution Chain (12-step authoritative order)
  - §4.1 Accuracy & Inaccuracy Cone System (with GML pseudocode)
  - §4.2 Critical Strike System (with GML pseudocode, unit template fields)
  - §4.3 Parry System (with GML pseudocode, all sources, stacking rules)
  - §4.4 Block System (with GML pseudocode, Tank unit list, pending tag pass)
  - §4.5 Knockback System (with GML pseudocode, power formula, stacking rules)
  - §4.6 Armor & Damage Types (Physical vs Magic formula)
  - §4.7 Full assembled combat resolution GML
  - §4.8–4.9 Healer logic & healing rules
- **Unit patch notes:**
  - Hatchling / Drake / Sky Serpent → `attack_type: "Magic Cone"`, `attack_range: 240`, `attack_area: 90`
  - Archer Tower → `attack_type: "Ranged Multi"`
  - All other towers → `attack_type: "Ranged Single"`
  - All 76 unit_line assignments verified correct (previous session had already patched them)
  - Lightbringer line `attack_area` verified correct (already 0)
  - Warden Ranger `unit_line: "ranger_line"` tier 3 verified correct

### v3 — 2026-03-03
- Added all xlsx ability data (Castle, Academy, Barracks, Beast Kennel, Chapel, all remaining buildings)
- Added all layout system docs (25 system documents)
- Added Rangers Lodge, Clockwork Skyport buildings
- Added Grand Cathedral abilities, full building tech tree

### v2 — prior
- Unit line audit, stat framework, projectile system, XP system, save/load docs

---

# TABLE OF CONTENTS
1. [Core Architecture Rules](#1-core-architecture-rules)
2. [Currency System](#2-currency-system)
3. [Unit Line System](#3-unit-line-system)
3B. [45-Building Tech Tree & Roster Gap](#3b-45-building-tech-tree--roster-gap)
4. [Combat Systems](#4-combat-systems) — Hit Resolution Chain, Accuracy, Crit, Parry, Block, Knockback, Armor
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
16. [UI Screens — Home Castle Screen](#16-ui-screens--home-castle-screen)
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


---

# 3B. 45-BUILDING TECH TREE & ROSTER GAP

## Tech Tree Structure
**5 T1 starters → 10 T2 (2 per row) → 20 T3 (2 per T2) → 10 T4 (cross-row pairs + Omni)**  
Total: 45 purchasable buildings + Castle (permanent home screen fixture, 4 units)

### Row 1 — Guild / Adventurers
```
Guild Hall (T1)
  ├─ Rangers Lodge (T2)
  │    ├─ Ballistics Range (T3) ──────────────────── → Dwarven Skyport (T4, R1+R5)
  │    └─ Menagerie Masters (T3) ─────────────────── → Dragon Stables (T4, R1+R2)
  └─ Adventurers Academy (T2)
       ├─ Dungeoneering Guild (T3) ────────────────── → Great Dungeon (T4, R1+R2)
       └─ Beastkin Encampment (T3) ────────────────── → Cryptid Merchant (T4, R1+R3)
```

### Row 2 — Military / Beasts
```
Barracks (T1)
  ├─ Beast Kennel (T2)
  │    ├─ Stables (T3) ──────────────────────────── → Dragon Stables (T4, R1+R2)
  │    └─ Monster Lair (T3) ──────────────────────── → Great Dungeon (T4, R1+R2)
  └─ Armory (T2)
       ├─ Orc Stronghold (T3) ──────────────────────  → Necropolis (T4, R2+R4)
       └─ Elven Outpost (T3) ───────────────────────  → Oasis (T4, R2+R3)
```
⚠️ "Elven Outpost" (T3 building) name collides with map camp building. Map building should be renamed to **Field Camp**.

### Row 3 — Nature / Faith
```
Herbalist Hut (T1)
  ├─ Grove (T2)
  │    ├─ Ent Forest (T3) ───────────────────────── → Oasis (T4, R2+R3)
  │    └─ Enchanted Garden (T3) ──────────────────── → Cryptid Merchant (T4, R1+R3)
  └─ Chapel (T2)
       ├─ Monastery (T3) ───────────────────────────  → Astral Laboratory (T4, R3+R4)
       └─ Church (T3) ─────────────────────────────── → Grand Cathedral (T4, R3+R4)
```

### Row 4 — Knowledge / Magic
```
School (T1)
  ├─ Academy (T2)
  │    ├─ University (T3) ─────────────────────────  → Grand Cathedral (T4, R3+R4)
  │    └─ Enclave (T3) ────────────────────────────  → Necropolis (T4, R2+R4)
  └─ Alchemical Workshop (T2)
       ├─ Research Facility (T3) ──────────────────── → Engineering Center (T4, R4+R5)
       └─ Elemental Portal (T3) ──────────────────── → Astral Laboratory (T4, R3+R4)
```

### Row 5 — Engineering / Craft
```
Construction Yard (T1)
  ├─ Dwarven Smithy (T2)
  │    ├─ Dwarven Workshop (T3) ──────────────────── → Dwarven Skyport (T4, R1+R5)
  │    └─ Clockwork Armory (T3) ──────────────────── → Dwarven Skyport (T4, R1+R5)
  └─ Sawmill (T2)
       ├─ Rune Smiths (T3) ──────────────────────── → Engineering Center (T4, R4+R5)
       └─ Mason Shop (T3) ──────────────────────── → Engineering Center (T4, R4+R5)
```

## T4 Cross-Row Pairings (10 total)
| T4 Building | Requires | Rows |
|---|---|---|
| Dwarven Skyport | Ballistics Range + Dwarven Workshop OR Clockwork Armory | R1+R5 |
| Dragon Stables | Menagerie Masters + Stables | R1+R2 |
| Great Dungeon | Dungeoneering Guild + Monster Lair | R1+R2 |
| Cryptid Merchant | Beastkin Encampment + Enchanted Garden | R1+R3 |
| Oasis | Elven Outpost + Ent Forest | R2+R3 |
| Necropolis | Orc Stronghold + Enclave | R2+R4 |
| Astral Laboratory | Monastery + Elemental Portal | R3+R4 |
| Grand Cathedral | Church + University | R3+R4 |
| Engineering Center | Research Facility + Rune Smiths OR Mason Shop | R4+R5 |
| **Omni Fortress** | All 5 T3s built, no valid cross-row pair | All rows |

**T4 Math:** 5 rows → 5 T3s per run → floor(5÷2) = **max 2 T4 buildings per run**. One T3 always unpaired.  
**Omni Fortress condition:** Player has all 5 T3s but their combination produces no valid cross-row pair.

## Unit Reassignment Table (from old building structure)
| Unit Lines | Old Home | New Home |
|---|---|---|
| Footman · Spearman · Scout · Militia | Old Barracks | **Castle (home screen)** |
| Squire line · Banner Carrier line | Old Castle | **Barracks T1 R2** |
| Crossbowman line (renamed Archer) · Shield Bearer line | Old Castle | **Armory T2 R2** |
| Spark Adept · Rune Novice · Frost Acolyte · Emberling | Magic School | **Alchemical Workshop T2 R4** (suggested) |
| Stonekin · Tunnel Scout · Crystal Adept · Mason | Mountain Keep | **Sawmill T2 R5 or Mason Shop T3 R5** (suggested) |
| Light Ram · Bombard Crew · Iron Husk · Siege Engineer | Siege Workshop | **Dwarven Smithy T2 R5** (suggested) |
| Clockwork Skyship · Copter · Rifleman · Volley Gunner | Clockwork Skyport | **Dwarven Skyport T4** (rename only) |

## Roster Gap Analysis
| Metric | Value |
|---|---|
| Current vault unit lines | 69 (17 buildings) |
| Target unit lines | 184 (45 buildings × 4 + Castle 4) |
| Confirmed carry-over | 53 lines |
| Homeless (confirmed lines, need new building) | 12 lines |
| **New lines needed from scratch** | **119** (107 if all homeless lines absorbed) |

### Buildings Needing Full Unit Design (31 buildings, ~119 new lines)
| Building | Tier/Row | Theme |
|---|---|---|
| Guild Hall | T1 R1 | Freelancer/civilian base |
| Herbalist Hut | T1 R3 | Nature healing |
| School | T1 R4 | Academic starter |
| Barracks (slots 3+4) | T1 R2 | Light military |
| Armory (slots 3+4) | T2 R2 | Heavy military |
| Alchemical Workshop | T2 R4 | Potions/transmutation |
| Dwarven Smithy | T2 R5 | Heavy forge (may absorb Siege Workshop units) |
| Sawmill | T2 R5 | Wood/stone craft (may absorb Mountain Keep units) |
| Ballistics Range | T3 R1 | Heavy ranged/artillery |
| Menagerie Masters | T3 R1 | Exotic beast taming |
| Dungeoneering Guild | T3 R1 | Delvers/trappers |
| Beastkin Encampment | T3 R1 | Half-beast humanoids |
| Stables | T3 R2 | Cavalry/mounted |
| Orc Stronghold | T3 R2 | Orc warriors |
| Elven Outpost | T3 R2 | Elf units |
| Ent Forest | T3 R3 | Living trees/forest guardians |
| Enchanted Garden | T3 R3 | Fey/magical nature |
| Monastery | T3 R3 | Martial monks |
| Church | T3 R3 | Holy soldiers/paladins |
| University | T3 R4 | Battle-mages/arcane strategists |
| Research Facility | T3 R4 | Scientists/inventors |
| Dwarven Workshop | T3 R5 | Heavy dwarven engineers |
| Clockwork Armory | T3 R5 | Mechanical/golem units |
| Rune Smiths | T3 R5 | Rune-inscribed warriors |
| Mason Shop | T3 R5 | Builders/fortifiers |
| Dragon Stables | T4 | Dragon riders, drake cavalry |
| Great Dungeon | T4 | Dungeon creatures, deep horrors |
| Cryptid Merchant | T4 | Exotic/mythical creatures |
| Oasis | T4 | Elven+ent alliance units |
| Necropolis | T4 | Undead (orc shamanism + dark arcane) |
| Astral Laboratory | T4 | Cosmic/planar units |
| Engineering Center | T4 | Ultimate engineers |
| Omni Fortress | T4 special | Generalist reward — design TBD |


# 4. COMBAT SYSTEMS

---

## 4.0 — Hit Resolution Chain (AUTHORITATIVE ORDER)

Every attack, projectile hit, or ability that deals damage follows this exact resolution order. No exceptions. All ability interactions must be documented relative to which step they modify.

```
1. ACCURACY CHECK        — does the attack/projectile even reach the target?
2. PARRY CHECK           — does the defender partially deflect the hit?
3. BLOCK CHECK           — does the defender fully negate the hit?
4. RAW DAMAGE            — base damage from attacker stats
5. CRIT CHECK            — does the attack deal bonus damage?
6. DAMAGE TYPE           — is damage Physical or Magic?
7. ARMOR REDUCTION       — Physical only: subtract armor from damage (min 0)
8. DAMAGE MODIFIERS      — apply % bonuses/reductions (buffs, debuffs, abilities)
9. FINAL DAMAGE APPLIED  — subtract from target hp
10. ON-HIT EFFECTS        — apply debuffs (Corrode, stun, knockback, etc.)
11. KNOCKBACK CHECK       — does the hit push the target?
12. XP CONTRIBUTION       — register attacker as contributor to target's death pool
```

**Key rules:**
- If PARRY triggers → damage is halved, then continues through the chain (armor still applies, crits still apply to the halved base, on-hit effects still trigger)
- If BLOCK triggers → damage is set to 0, skip steps 5–9. On-hit effects (step 10) do NOT trigger on a blocked hit. Knockback (step 11) does NOT trigger on a blocked hit.
- A hit cannot both PARRY and BLOCK. BLOCK check only runs if PARRY did not trigger.
- CRIT applies to raw damage BEFORE armor reduction. This means crits are more impactful against low-armor targets.
- Magic damage skips step 7 (armor) entirely. All other steps still apply.

---

## 4.1 — Accuracy & Inaccuracy Cone System
**Status:** Projectile system implemented. Global baseline cone NOT YET reimplemented (was in previous iteration, lost in split).

### Rules
- ALL ranged units have a global baseline inaccuracy of **20° cone (±10° off center)**
- Cone angle formula: `(1 - accuracy/100) * 90`
  - 100% accuracy = 0° spread (perfect shot)
  - 0% accuracy = 90° max spread
- Each projectile fired gets an **independent accuracy roll** — multi-projectile units roll separately per projectile
- Friendly projectiles pass through allies, only collide with enemies (team-based collision)
- Sprite origins MUST be set to CENTER for rotation to work correctly
- Projectiles render at **negative depth** to appear above background rectangles

### Per-Unit Accuracy Field
- `accuracy` is a per-unit stat (not yet on all templates — add during unit type audit pass)
- Castle Archer = standard baseline (20° cone)
- Scout idle (Over Here! on cooldown): -120° additional spread (intentionally wild)
- Taunted unit (Over Here! active): -50° additional spread

### Multi-Projectile System ✅ Designed
- `multi_count` = number of simultaneous projectiles
- `multi_spread` = total arc spread in degrees
- Each projectile fires in a **concave arc (fan/parenthesis curve)** — evenly spaced across the spread angle
- Each projectile gets its own independent accuracy roll on top of the arc spread
- `attack_type` = `"Ranged Multi"` or `"Magic Multi"`

### GML Pseudocode
```gml
// Firing a projectile with inaccuracy
function fire_projectile_with_accuracy(shooter, target_x, target_y) {
    var base_angle = point_direction(shooter.x, shooter.y, target_x, target_y);
    var cone_half = (1 - shooter.accuracy / 100) * 90;
    var spread = random_range(-cone_half, cone_half);
    var final_angle = base_angle + spread;
    // spawn projectile at final_angle
}

// Multi-projectile arc
function fire_multi_projectile(shooter, target_x, target_y) {
    var base_angle = point_direction(shooter.x, shooter.y, target_x, target_y);
    var count = shooter.multi_count;
    var spread = shooter.multi_spread;
    for (var i = 0; i < count; i++) {
        var arc_offset = lerp(-spread/2, spread/2, i / max(count - 1, 1));
        var cone_half = (1 - shooter.accuracy / 100) * 90;
        var accuracy_roll = random_range(-cone_half, cone_half);
        var final_angle = base_angle + arc_offset + accuracy_roll;
        // spawn projectile at final_angle
    }
}
```

---

## 4.2 — Critical Strike System
**Status:** NOT YET IMPLEMENTED

### Rules
- **Crit Chance:** % probability that an attack deals bonus crit damage
- **Global baseline crit chance:** 0% (no unit crits by default unless granted by ability/buff)
- **Crit damage multiplier:** 2× base damage (i.e. a crit doubles the raw damage before armor)
- Crit applies at **step 5** of the hit resolution chain — BEFORE armor reduction
- This means crits feel more impactful against soft targets (armor doesn't eat the bonus)

### Where Crit Chance Comes From
| Source | Amount | Notes |
|---|---|---|
| Footman — Press the Line! (active) | +10% crit | Duration: 2s/3s/4s per tier, first combat entry only |
| Facing system — back attack | +back_attack_crit_modifier | Default 0; set per unit for backstab units (e.g. Rogue) |
| Future ability/buff sources | varies | Always additive |

### Stacking
- All crit sources are **additive** (5% + 10% = 15% total)
- Crit chance caps at **100%**

### GML Pseudocode
```gml
function resolve_crit(attacker, raw_damage) {
    var total_crit_chance = attacker.crit_chance; // base stat on unit
    
    // Add back-attack bonus if applicable
    if (is_attacking_from_behind(attacker, target)) {
        total_crit_chance += attacker.back_attack_crit_modifier;
    }
    
    total_crit_chance = min(total_crit_chance, 100);
    
    if (random(100) < total_crit_chance) {
        return raw_damage * 2; // crit multiplier
    }
    return raw_damage;
}
```

### Unit Template Fields Required
```gml
crit_chance: 0,              // % base crit chance (0 = no crits by default)
back_attack_crit_modifier: 0 // bonus crit % when attacking from behind (Rogue etc.)
```

---

## 4.3 — Parry System
**Status:** NOT YET IMPLEMENTED

### What Is Parry?
Parry represents a melee unit partially deflecting an incoming melee blow. When a parry triggers, the hit still lands but damage is **halved**. The hit is not negated — on-hit effects, debuffs, and knockback still apply to the halved damage.

### Rules
- **Who can parry:** Units with the `Melee` attack distance tag
- **Global baseline parry chance:** 5% for all Melee units
- **Parry effect:** 50% damage reduction on the triggering hit
- **Parry only activates against melee attacks** (attack_type contains "Melee")
- Ranged and Magic attacks CANNOT be parried (they can be blocked — see 4.4)
- Parry check runs at **step 2** of hit resolution — before block, before crit, before armor

### Where Parry Chance Comes From
| Source | Amount | Notes |
|---|---|---|
| Global baseline (all Melee units) | 5% | Always active |
| Footman — Press the Line! (active) | +10% | Duration buff, first combat entry |
| Footman — I Believe! (unlock) | +10% | Permanent passive |
| Squire — Concentrated Parry (unlock, active) | +30% | Vs melee attackers only, 5s duration |
| Lightbringer — Battle Buddy (buff on buddy) | +20% base, upgradeable | Applied to buddy melee unit |
| Flying Melee Magic units (birds T2/T3) | 5% baseline | Melee tag = gets parry chance |

**Stack example (Footman with I Believe! + Press the Line! active):**
`5% global + 10% I Believe + 10% Press the Line = 25% parry total`

### Stacking
- All parry sources are **additive**
- Parry chance caps at **95%** (always at least 5% chance to be hit normally)

### GML Pseudocode
```gml
function resolve_parry(attacker, defender, raw_damage) {
    // Only melee attacks can be parried
    if (!string_pos("Melee", attacker.attack_type)) return raw_damage;
    
    // Only Melee-tagged defenders can parry
    if (!string_pos("Melee", defender.spawn_tags)) return raw_damage;
    
    var total_parry = defender.parry_chance; // base stat (5 for melee units)
    // Add buff sources (Battle Buddy, Press the Line, etc.)
    total_parry += defender.current_stats.parry_bonus; // from active buffs
    total_parry = min(total_parry, 95);
    
    if (random(100) < total_parry) {
        // Parry triggered — halve damage, flag for on-hit effects
        defender.last_hit_parried = true;
        return raw_damage * 0.5;
    }
    defender.last_hit_parried = false;
    return raw_damage;
}
```

### Unit Template Fields Required
```gml
parry_chance: 5,   // % for Melee units, 0 for all others
```

---

## 4.4 — Block System
**Status:** NOT YET IMPLEMENTED

### What Is Block?
Block represents a tank unit fully absorbing an incoming attack on their shield. When a block triggers, damage is **completely negated**. Unlike parry, on-hit effects do NOT apply on a blocked hit (the hit never connected).

### Rules
- **Who can block:** Units with the `Tank` role tag
- **Tank units:** Shield Bearer line, Footman line, Lightbringer line, Sproutling line, Stonekin line
- **Global baseline block chance:** 10% for all Tank-tagged units
- **Block effect:** 100% damage negation — damage set to 0
- **Block activates against ALL damage types** (Melee, Ranged, Magic)
- Block check runs at **step 3** — after parry check, before crit and armor
- If parry already triggered on this hit, block check is SKIPPED
- On a blocked hit: no damage, no on-hit effects, no knockback, no debuffs applied

### Where Block Chance Comes From
| Source | Amount | Notes |
|---|---|---|
| Global baseline (Tank units only) | 10% | Always active on Tank-tagged units |
| Footman — Press the Line! (active) | +10% | Duration buff, first combat entry |
| Footman — I Believe! (unlock) | +10% | Permanent passive |

**Stack example (Footman with I Believe! + Press the Line! active):**
`10% global + 10% I Believe + 10% Press the Line = 30% block total`

### Stacking
- All block sources are **additive**
- Block chance caps at **90%** (always at least 10% chance for an attack to get through)

### Tank Tag — Units That Get Global Block
The following 15 player unit lines (+ their corrupted equivalents = 30 total files) need the `Tank` tag in their spawn_tags. This is a **pending code pass**:
- Shield Bearer line (Shield Bearer, Tower Guard, Bulwark Champion)
- Footman line (Footman, Man-at-Arms, War Marshal)
- Lightbringer line (Lightbringer, Sun Knight, Dawn Paladin)
- Sproutling line (Sproutling, Vinekin, Thorn Warden)
- Stonekin line (Stonekin, Rockguard, Obsidian Sentinel)

### GML Pseudocode
```gml
function resolve_block(defender, damage_after_parry) {
    // Only Tank-tagged units can block
    if (!string_pos("Tank", defender.spawn_tags)) return damage_after_parry;
    
    var total_block = defender.block_chance; // base stat (10 for Tank units)
    total_block += defender.current_stats.block_bonus; // from active buffs
    total_block = min(total_block, 90);
    
    if (random(100) < total_block) {
        // Block triggered — full negation, suppress on-hit effects
        defender.last_hit_blocked = true;
        return 0;
    }
    defender.last_hit_blocked = false;
    return damage_after_parry;
}
```

### Unit Template Fields Required
```gml
block_chance: 10,  // % for Tank units, 0 for all others
```

---

## 4.5 — Knockback System
**Status:** NOT YET IMPLEMENTED

### What Is Knockback?
When an attack triggers knockback, the target is physically pushed away from the attacker. This interrupts movement, can push units into bad positioning, and interacts with map edges and other units.

### Rules
- **Knockback Chance:** % probability that a hit triggers knockback
- **Global baseline knockback chance:** 0% (no unit causes knockback by default unless granted)
- **Knockback Power:** How far (in pixels) the target is pushed
  - Melee attacks: `0.5 × attacker raw damage` px push distance
  - Ranged/Magic attacks: `0.25 × attacker raw damage` px push distance
- **Push direction:** Opposite of the attack angle (away from attacker)
- Knockback only triggers if the hit was NOT blocked (blocked hits = no knockback)
- Knockback CAN trigger on a parried hit (the blow still landed, just weaker)
- Knockback check runs at **step 11** — after all damage and effects are resolved

### Where Knockback Chance Comes From
| Source | Amount | Notes |
|---|---|---|
| Global baseline | 0% | No knockback by default |
| Spearman — Almighty Push (unlock) | 300% KB Power modifier | Does NOT raise chance; multiplies push distance |
| Footman — Press the Line! (active) | +10% KB Chance | Duration buff |
| Footman — I Believe! (unlock) | +2× KB Power | Permanent multiplier on push distance |
| Horned Cub — Charge! (on collision) | 100% KB Chance, 2× KB Power | Applies on charge collision |

**Note on Almighty Push:** Spearman's unlock gives 300% Knockback Power, meaning push distance = `3 × (0.5 × raw damage)` for melee = 1.5× raw damage in pixels. This is a large shove.

**Note on I Believe! + Almighty Push stacking:** If a Footman somehow had both (they can't, different units), the multipliers would be additive: `2× + 3× = 5×`. In practice these are on separate unit lines.

### Knockback Power Formula
```
base_push = attack_is_melee ? (raw_damage * 0.5) : (raw_damage * 0.25)
final_push = base_push * knockback_power_multiplier
```

### Stacking
- Knockback Chance sources are **additive**
- Knockback Power multipliers are **additive** (2× + 2× = 4× total, not 2× × 2× = 4×)
- Knockback Chance caps at 100%

### GML Pseudocode
```gml
function resolve_knockback(attacker, defender, raw_damage) {
    // No knockback on blocked hits
    if (defender.last_hit_blocked) return;
    
    var total_kb_chance = attacker.knockback_chance;
    total_kb_chance += attacker.current_stats.knockback_chance_bonus;
    total_kb_chance = min(total_kb_chance, 100);
    
    if (random(100) >= total_kb_chance) return; // no knockback
    
    // Calculate push distance
    var is_melee = string_pos("Melee", attacker.attack_type) > 0;
    var base_push = is_melee ? (raw_damage * 0.5) : (raw_damage * 0.25);
    var power_mult = attacker.knockback_power_multiplier; // default 1.0
    power_mult += attacker.current_stats.knockback_power_bonus; // from buffs
    var final_push = base_push * power_mult;
    
    // Push direction = opposite of attack angle
    var push_angle = point_direction(attacker.x, attacker.y, defender.x, defender.y);
    var push_x = lengthdir_x(final_push, push_angle);
    var push_y = lengthdir_y(final_push, push_angle);
    
    // Apply push (clamped to battlefield bounds)
    defender.x = clamp(defender.x + push_x, BATTLEFIELD_LEFT, BATTLEFIELD_RIGHT);
    defender.y = clamp(defender.y + push_y, BATTLEFIELD_TOP, BATTLEFIELD_BOTTOM);
}
```

### Unit Template Fields Required
```gml
knockback_chance: 0,          // % chance to trigger knockback (0 by default)
knockback_power_multiplier: 1.0, // multiplier on push distance (1.0 = normal)
```

---

## 4.6 — Armor & Damage Types
**Status:** Armor reduction exists in code. Magic ignoring armor NOT YET enforced.

### Rules
- **Physical damage:** `final_damage = max(0, raw_damage - armor)`
- **Magic damage:** Ignores armor entirely. `final_damage = raw_damage` (no subtraction)
- Units with the `Magic` tag in spawn_tags always deal Magic damage
- Units with the `Physical` tag deal Physical damage (default)
- Armor can be **reduced by debuffs** (Corrode: -1/-2/-3 armor per tier)
- Armor cannot go below 0 from debuffs

### Armor Reduction Formula
```gml
function apply_armor_reduction(raw_damage, damage_type, target_armor) {
    if (damage_type == "Magic") return raw_damage; // ignores armor
    return max(0, raw_damage - target_armor);
}
```

---

## 4.7 — Full Combat Resolution GML (Assembled)

This is how a complete hit resolves in code, combining all systems above:

```gml
function resolve_hit(attacker, defender, raw_damage, attack_type) {
    // Step 2 — Parry check
    var damage = resolve_parry(attacker, defender, raw_damage);
    if (defender.last_hit_blocked) {
        // Step 3 — Block check (only if parry didn't fire)
    } else {
        damage = resolve_block(defender, damage);
    }
    
    // If fully blocked — skip to step 11
    if (!defender.last_hit_blocked) {
        // Step 5 — Crit check (applied to damage after parry, before armor)
        damage = resolve_crit(attacker, damage);
        
        // Step 6+7 — Damage type and armor reduction
        var dmg_type = string_pos("Magic", attack_type) > 0 ? "Magic" : "Physical";
        damage = apply_armor_reduction(damage, dmg_type, defender.armor);
        
        // Step 8 — Damage modifiers (% buffs/debuffs)
        damage = apply_damage_modifiers(attacker, defender, damage);
        
        // Step 9 — Apply final damage
        damage = max(0, damage);
        defender.hp -= damage;
        
        // Step 10 — On-hit effects (only if not blocked)
        apply_on_hit_effects(attacker, defender);
    }
    
    // Step 11 — Knockback (only if not blocked)
    resolve_knockback(attacker, defender, raw_damage);
    
    // Step 12 — XP contribution
    register_xp_contribution(attacker, defender, damage);
    
    // Death check
    if (defender.hp <= 0) {
        handle_unit_death(defender);
    }
}
```

---

## 4.8 — Healer Repositioning Logic
Shared by Acolyte (Mend) and Hymnist (Hymn of Solace):

1. Heal lowest HP% friendly unit in range
2. If no damaged unit in range: walk 300px from nearest enemy, retarget
3. If no damaged unit on field: follow nearest Tank/Melee DPS/Ranged DPS 80px behind
4. If no allies remain: flee to spawn location

## 4.9 — Healing Rules
- Healing is NOT reduced by armor or any defensive stat
- Heal formula: `hp = min(hp + amount, max_hp)`
- `healing_other` field on healer templates = **legacy placeholder** — superseded by ability system. Zero out or ignore once abilities are implemented.

---

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
**4 slots, 3 tiers = 12 units total**

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Cleric | Battle Cleric | Hierophant |
| 2 | Confessor | Inquisitor | Grand Inquisitor |
| 3 | Martyr | Ascetic | Living Relic |
| 4 | Seraphic Avenger | Flamewing Justicar | Archon of the Final Flame |

### Slot 1 — Cleric Line
**PRIMARY: "Divine Retribution"**
- Trigger: when unit enters melee combat
- Spawns a puddle centered on the unit, **60px thick × 120px wide**, oriented along the unit's facing direction (spreads to the sides perpendicular to facing)
- Duration: 20s | CD: 60s
- **Puddle buff: "Holy Thorns"** — applied to all friendly units inside puddle, lasts 8s (refreshes every 1s while inside):
  - Take **40% less damage** from all sources
  - Any enemy that hits a unit with Holy Thorns takes **5/7/9 magic damage** (T1/T2/T3) in return
- Upgrade ×10: +1 magic damage (return damage), +1% damage reduction | Cost doubles from 15

**UNLOCK: "Blessed Armament"** — 500 Sapphires + 500 Rubies  
Units affected by Holy Thorns have their weapons blessed: all attacks deal an additional **5/10/15 magic damage** (T1/T2/T3) regardless of normal damage type.

**⚠️ Implementation note:** Puddle orientation requires the facing system (Section 5A) — puddle shape must rotate with unit direction. This is the first ability with a directional puddle rather than a circular one.

---

### Slot 2 — Confessor Line
**PRIMARY: "Discipline is Freedom"**
- Every 10s: targets lowest HP friendly unit within 200px and heals for **80/90/100 HP** (T1/T2/T3)
- Heal then **jumps** to another injured friendly unit within 120px, up to 5 additional jumps (6 units total)
- Each jump reduces heal amount by 10%
- Each unit healed receives **"Disciplined"** buff for 8s:
  - +10% attack speed
  - Cannot be targeted by Discipline is Freedom again while buff is active (prevents double-healing same unit)
- Upgrade ×10: +1 HP healed (base amount), +5px jump distance | Cost doubles from 15

**UNLOCK: "Personal Responsibility"** — 800 Emeralds + 200 Sapphires  
Units with the "Disciplined" buff also gain **+30% parry** for the duration.

---

### Slot 3 — Martyr Line
**PRIMARY: "Take Me Instead!"**
- Trigger: on this unit's death
- The **lowest HP 2/3/4 friendly units** (T1/T2/T3) within 300px receive **Invulnerability** for 6s:
  - Prevents ALL damage
  - Removes all active debuffs instantly on application
  - Prevents new debuffs for the duration
- Units that received Invulnerability cannot receive it again for **20 seconds** (prevents stacking from multiple Martyrs)
- Upgrade ×10: +0.25s invulnerability duration, +5px radius (300px base → 350px max) | Cost doubles from 15

**UNLOCK: "Righteous Vengeance"** — 500 Emeralds + 500 Sapphires  
Units affected by Invulnerability also gain **+30% attack speed** for the duration.

**⚠️ Design note:** With multiple Martyrs in an army the 20s immunity cooldown is critical — prevents a chain of Martyrs from locking an army in permanent invulnerability.

---

### Slot 4 — Seraphic Avenger Line
**PRIMARY: "Not Today!"**
- Every 2 minutes: **resurrects a dead friendly unit** within 300px
- Max resurrections per battle: **2/3/4** (T1/T2/T3)
- Resurrects the unit at (presumably) low HP — exact resurrection HP TBD
- **Cannot be upgraded**

**UNLOCK: "We Need You Now!"** — 3000 Emeralds  
Removes the 2-minute cooldown entirely. Also increases the max resurrections by +1 (becomes 3/4/5 by tier).

**⚠️ Design note:** "We Need You Now!" is an extremely powerful unlock — effectively unlimited resurrection spam. The 3000 Emerald cost is the intended balancing mechanism. Monitor in playtesting.

---

## GROVE
**Line IDs:** `sproutling_line`, `dryad_line`, `acorn_scout_line`, `seed_sower_line`  
**Tier:** 4 | **Prerequisites:** Enclave | **Levels:** 3  
**Theme:** Nature magic — tanky plant tank, poison puddle spreader, mortar-style archer, and a battlefield-wide healing meadow. Strong passive/area control identity.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Sproutling | Vinekin | Thorn Warden |
| 2 | Dryad | Grovekeeper | Sylvan Matron |
| 3 | Acorn Scout | Barkstrider | Forest Sentinel |
| 4 | Seed Sower | Bloomcaller | Verdant Sage |

**⚠️ GROVE AUDIT NEEDED:** `acorn_scout_line` slot 3 has duplicate entries — `acorn_scout` AND `acorn_lurker` both assigned slot:3 tier:1. Same at T2 (Barkstrider+Barkreaver) and T3 (Forest Sentinel+Forest Reaper). Lurker variants have Ranged Single attack — likely corrupted units incorrectly mixed into player line. Audit and separate corrupted variants before implementation.

### Slot 1 — Sproutling Line
**Unit Type:** Melee, Physical, Tank  
**Ability: Growing Pains**  
Every 3 seconds: **taunts all enemies within 200px** and **reflects 5/10/15 damage + heals 4/8/12 HP** per hit received during the taunt window.  
- **Upgrade ×10:** +1 reflect damage AND +1 HP heal per upgrade. Cost doubles from 15. Stone.  
- Max upgrade: reflect 15/20/25 damage, heal 14/18/22 HP per hit.

**Unlock: Rampant Regrowth** — 200 Sapphires + 800 Emeralds  
Passive self-heal: **2/3/4 HP every 0.5 seconds** (T1/T2/T3).

### Slot 2 — Dryad Line
**Unit Type:** Melee, Physical  
**Ability: Destructive Nature**  
60 second cooldown. Spawns an **80px poison puddle** at the Dryad's location tagged **"Return to Earth"** — deals **5/10/15 damage per second for 10 seconds** (T1/T2/T3). Enemies that touch the puddle while it is active **spread it** — a new puddle spawns at their location. Maximum **4-6 puddles** active simultaneously.  
- **Upgrade ×10:** +1 damage/s per upgrade. Cost doubles from 15. Wood.

**Unlock: The Planet Feeds** — 500 Wood + 500 Sapphires  
Units that die while standing in a Return to Earth puddle **spawn a new puddle at their death location**.

### Slot 3 — Acorn Scout Line
**Unit Type:** Ranged, Physical (mortar arc)  
**Ability: In the Shade**  
60 second cooldown. Fires a **mortar-style volley of 10/15/20 arrows** (T1/T2/T3) in arc trajectory landing within a **250px accuracy circle**. Each arrow is a 4px damage box dealing **15/20/25 damage each** (T1/T2/T3).  
- **Upgrade ×10:** +1 arrow per upgrade. Cost doubles from 15. Wood.

**Unlock: Poison Arrows** — 1000 Wood  
Each arrow hit applies a stacking poison: **1 damage/s for 10 seconds per arrow hit**.

### Slot 4 — Seed Sower Line
**Unit Type:** Ranged, Support  
**Ability: Meadow of Tranquility**  
2 minute cooldown. Creates a **400px meadow** lasting **30 seconds**. All friendly units inside receive **1/2/3 HP/s** (T1/T2/T3) and are **immune to all debuffs** (existing debuffs are cleared on entry).  
- **Upgrade ×10:** -2s cooldown AND +2s duration per upgrade. Cost doubles from 15. Emeralds.

**Unlock: New Bloom** — 500 Wood + 500 Emeralds  
Units that remain continuously in the meadow for **10 seconds** receive a burst heal of **20/30/40 HP** (T1/T2/T3).

---

## MAGIC SCHOOL
**Line IDs:** `spark_adept_line`, `rune_novice_line`, `frost_acolyte_line`, `emberling_line`  
**Tier:** 2 | **Prerequisites:** Academy | **Levels:** 3  
**Theme:** Academic magic — four mechanically distinct elemental/arcane schools with no thematic overlap between lines.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Spark Adept | Lightning Savant | Stormcaller Archon |
| 2 | Rune Novice | Glyph Weaver | Arcane Sigilmaster |
| 3 | Frost Acolyte | Icebinder | Cryomancer |
| 4 | Emberling | Flame Adept | Pyromancer |

### Slot 1 — Spark Adept Line
**Ability: Sudden Pop**  
Every normal attack applies a stacking, renewing debuff **"Charged"** — timer resets to 3 seconds on every new stack. Charges accumulate at **10/15/24 per attack** (T1/T2/T3). When target reaches **100 stacks**, it **"Pops"** — 80 damage in 80px AoE centered on target. Charged resets to 0.  
- Hits to pop base: T1=10, T2=7, T3=5  
- **Upgrade ×10:** +1 charge/attack all tiers. Cost doubles from 15. Rubies.  
- Hits to pop at max upgrade: T1=5, T2=4, T3=3  
- **No unlock.**

### Slot 2 — Rune Novice Line
**Ability: Runic Mine**  
Every 3 seconds, places a **ground rune** (80px radius) at a random location within 300px. Runes last 30 seconds. Enemy stepping on rune: explodes for **50/60/70 damage** (T1/T2/T3). New runes must be at least 40px from any existing rune center.  
- **Upgrade ×10:** +3s rune duration per upgrade (30s → 60s max). Cost doubles from 15. Rubies.  
- **No unlock.**

### Slot 3 — Frost Acolyte Line
**Ability: Frost Wave**  
Creates a **200px wide, 10px thick wall of frost** that travels 200px at 100px/second (2s travel). Enemies hit: **30/40/50 damage** (T1/T2/T3) and **-20% movement speed and attack speed for 10 seconds**. Cooldown: 20 seconds.  
- **Upgrade ×10:** +1 damage AND +1% movement/attack speed reduction per upgrade. Cost doubles from 15. Sapphires.

**Unlock: Deep Freeze** — cost TBD (Sapphires)  
Extends all Frost Wave effects by 5 seconds (10s → 15s). Also stuns targets for 3 seconds on hit (before slow applies).

### Slot 4 — Emberling Line
**Ability: Great Ball of Fire**  
Once every 60 seconds: targets unit in range, 6 second charge-up. Launches a **homing missile** at 50px/second with infinite effective range — follows target indefinitely. On hit: **400px AoE** dealing **40/50/60 damage** (T1/T2/T3) to all enemy units in range.  
- **Upgrade ×10:** +2 damage to explosion per upgrade. Cost doubles from 15. Rubies.  
- **No unlock.**  
- Design note: 400px AoE is the largest in the game outside Ritual Tower. 6s charge-up is a readable telegraph. "Infinite homing" means fleeing units still get hit — intentional and funny.

---

## MONSTER LAIR
**Line IDs:** `hatchling_line`, `kobold_line`, `cave_imp_line`, `slime_line`  
**Tier:** 3 | **Prerequisites:** Barracks + Beast Kennel | **Levels:** 3  
**Theme:** Tiny dragon cult. Kobolds worship the Hatchling as a dragon-god. Strong internal synergy — Kobolds fight dramatically harder when their Hatchling is nearby.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Hatchling | Drake | Sky Serpent |
| 2 | Kobold | Kobold Warrior | Kobold Chieftain |
| 3 | Cave Imp | Tunnel Fiend | Abyssal Howler |
| 4 | Slime | Ooze | Gelatinous Monarch |

### Slot 1 — Hatchling Line
**Unit Type:** Flying, Ranged, Magic damage  
⚠️ STAT REDESIGN NEEDED — needs full stat block as flying ranged magic unit.  
**Ability: Chromatic Scales**  
Passive. This unit takes **40/50/60% less magic damage** (T1/T2/T3).  
- **Upgrade ×10:** +2% magic damage reduction per upgrade. Rubies.  
- Max upgrade: 60/70/80% magic damage reduction.  
- **No unlock.**

### Slot 2 — Kobold Line
**Ability: Protect the Master!**  
Passive aura. If **any Hatchling line unit is within 300px**, this unit gains: **+3 Armor, +20% Parry, +20% Block, +20% Attack damage, +20% Attack speed**.  
- **Upgrade ×10:** +1 to each bonus per upgrade (max: +13 Armor, +30% all others). Sapphires.

**Unlock: Glorious Scales** — cost TBD (Sapphires + Rubies)  
Kobolds take **20/25/30% less magic damage** (T1/T2/T3) at all times. Increased by 50% when a Hatchling line unit is within 300px (becomes 30/37.5/45%).

### Slot 3 — Cave Imp Line
**Ability: Something in the Dark**  
Passive on-hit melee. Target receives **"Dark Hallucinations"** debuff — their **next attack has a 50% chance to miss entirely** (attacks a hallucination, 0 damage, no on-hit effects). Debuff consumed on next attack whether it misses or not. Multiple Cave Imps hitting the same target stack debuffs.  
- **Upgrade ×10:** +2% miss chance per upgrade (50% → 70% at max). Sapphires.

**Unlock: Safety in Numbers** — 500 Stone + 500 Sapphires  
⚠️ Pending cohesion/morale system. This unit counts as **2/3/4 units** (T1/T2/T3) for flee and morale calculations.

### Slot 4 — Slime Line
**Unit Type:** Melee, Physical, Tank  
**Visual:** Units swallowed by the Slime are carried inside the model — visually present but trapped within the goo.  
**Ability: It's All Just Goo**  
Passive. Takes **60/70/80% less physical damage** (T1/T2/T3). Every enemy melee hit applies **1/1.5/2 stacks of "Sticky Goo"** debuff. At **10 stacks**: unit is **"Eaten"** — removed from field and carried inside Slime. Swallowed units: cannot act, cannot be targeted. Freed only by killing the Slime or dealing magic damage to it. Max swallowed: **1/2/3** (T1/T2/T3).  
- If round ends with Slime alive: swallowed units die, Slime gains **+50% XP**.  
- If Slime killed: swallowed units released back to fight.  
- **Upgrade ×10:** +1% physical damage reduction per upgrade (max: 70/80/90%). Stone.  
- **No unlock.**  
- Counterplay: magic damage frees all swallowed units simultaneously.

---

## MOUNTAIN KEEP
**Line IDs:** `stonekin_line`, `tunnel_scout_line`, `crystal_adept_line`, `mason_line`  
**Tier:** 3 | **Prerequisites:** Barracks + Construction Yard | **Levels:** 3  
**Theme:** All dwarves. Underground, building-focused. Strong internal synergy — Mason heals towers that Tunnel Scout threatens. Stonekin tanks magic. Crystal Adept creates spreading healing field.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Stonekin | Rockguard | Obsidian Sentinel |
| 2 | Tunnel Scout | Cavern Striker | Deep Delver |
| 3 | Crystal Adept | Geomancer | Earth Shaper |
| 4 | Mason | Architect | Bastion Master |

### Slot 1 — Stonekin Line
**Unit Type:** Melee, Physical, Tank  
**Ability: Prismatic Shield**  
Passive. Takes **25% less physical/melee damage**. Has a **25% chance per hit to reflect magic projectiles** back toward the caster — reflected projectiles are friendly to Stonekin's faction and may hit other enemies along return path.  
- **Upgrade ×10:** +2% to all percentages per upgrade. Stone.  
- Max upgrade: 45% physical reduction, 45% reflect chance.

**Unlock: It's All Mine** — 500 Steel + 500 Stone  
Enemies hit by this unit are **taunted for 2 seconds** and cannot flee during that duration.

### Slot 2 — Tunnel Scout Line
**Unit Type:** Siege, Physical  
**Visual:** While burrowed, a dust cloud/dirt disturbance travels across battlefield — visible but untargetable.  
**Ability: Heh?! Walls?**  
On spawn: **burrows underground** and paths toward the **nearest enemy tower or building**. While burrowed: **untargetable**. As it passes beneath enemy units it leaves an **"Unusual Package"** detonating 2 seconds later for **20/30/40 damage** (T1/T2/T3) in small AoE. On reaching target: unburrows and attacks it. After 4 seconds, plants an **"Extra Special Package"** dealing **500 damage to the building, ignoring all armor** (if killed before 4s, package does NOT detonate).  
- **Upgrade ×10:** +1 Unusual Package damage AND +50 Extra Special Package damage. Stone + Rubies.  
- Max upgrade: 30/40/50 Unusual, 1000 Extra Special.  
- ⚠️ Christmas event: replace packages with presents, giant gift box; Santa outfit on Scout.

**Unlock: Extra Large Packages** — 500 Stone + 500 Rubies  
All packages +30% size and damage.

### Slot 3 — Crystal Adept Line
**Ability: Prismatic Healing**  
Every 30 seconds: spawns a **Crystal** within 40px of the lowest health friendly unit in range. Crystal is stationary, has **10/15/20 HP** (T1/T2/T3), takes full damage from everything. While alive: shines a prismatic beam on the lowest HP friendly unit within **500px**, healing **10/15/20 HP every 2 seconds**.  
- Crystal retargets on death of original target. Does not despawn until killed.  
- **Upgrade ×10:** +1 HP per heal AND -0.1s heal interval per upgrade (min 1.0s). Emeralds.

**Unlock: Crystal Chakra Gardening** — 2000 Emeralds  
If a Crystal survives for 20+ seconds, it spawns a **second lesser Crystal** nearby. Lesser Crystal heals **5/10/15 HP** (T1/T2/T3) on same interval. Max **2 lesser Crystals** per parent.

### Slot 4 — Mason Line
**Ability: Boaghdar the Buildar**  
The Mason finds the **nearest injured player tower**, moves to it, and **heals it at 50 HP/s**. When fully healed: finds next nearest injured tower. If no towers are damaged: idles at last tower healed, applying unlock buff if purchased.  
- **Cannot be upgraded** — intentionally simple. Buy a Mason, your towers live longer.

**Unlock: Improved Tower Management** — 3000 Stone  
While Mason is idling at a tower (no injured towers elsewhere), that tower gains **+10% damage, +10% attack speed, +10% cast speed**.  
- Design note: 1 Engineer ≈ survival, 2 ≈ comfortable, 3 ≈ tower is probably doomed. Mason only heals player towers, not Crystals or enemy structures.

---

## OUTPOST
**Status: CUT AS PLAYER BUILDING — repurposed as level design unit**  
See §8 Level Design Units for full Outpost event description. No player purchasable abilities.

---

## SIEGE WORKSHOP
**Line IDs:** `light_ram_line`, `bombard_crew_line`, `iron_husk_line`, `siege_engineer_line`  
✅ All line_ids correct across all tiers  
**Tier:** 2 | **Prerequisites:** Construction Yard | **Levels:** 3  
**Theme:** Pure siege. These units exist to kill towers and buildings. Behavioral systems rather than ability-driven fighters. All dwarven engineering.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Light Ram | Reinforced Ram | Thunder Ram |
| 2 | Bombard Crew | Cannonade | Grand Bombard |
| 3 | Iron Husk | Steel Golem | Titan Automaton |
| 4 | Siege Engineer | War Mechanic | Doom Artificer |

### Slot 1 — Light Ram Line
**System: Tower Ram**  
Paths directly toward nearest enemy tower/building. **Pushes units aside** (displacement, no damage) without stopping. Immune to taunt/fear. On reaching target: 1s wind-up → slam → 1s cooldown → repeat. Each slam deals **1/5 of target tower's base HP** (ignores all armor). 5 swings kills a same-tier tower (~10 seconds).  
- T1 vs Archer Tower T1 (700 HP): 140 per swing.  
- Partial progress is preserved — a Ram that lands 2 swings before dying has dealt 40% tower damage.  
- Hard counter: melee units that kill it before it reaches the tower. Soft counter: Mason healing faster than it damages.

### Slot 2 — Bombard Crew Line
**System: Siege Ranged — Building Priority**  
Priority check every second: any enemy tower/building within **400px**? If yes → target it, ignore all units. If no → engage nearest enemy unit normally. Switches to building target immediately when one enters 400px range mid-combat. Cannot be taunted off a building target within 400px.

### Slot 3 — Iron Husk Line
**System: Siege Melee — Building Priority**  
Identical building priority logic to Bombard Crew (400px check, ignores units when building in range). Unlike the Ram, deals **normal melee damage to units it passes through** — it's a combat unit that prefers buildings, not a pure siege vehicle. At T3 (Titan Automaton): heavy melee unit that soaks significant damage walking to its target.

### Slot 4 — Siege Engineer Line
**System: Siege Unit Healer — Dwarf**  
Triage loop: finds most injured friendly **siege unit** (Ram, Bombard, Iron Husk) → moves to it → heals at X HP/s (TBD — tune so 1 Engineer ≈ offsetting one defending archer tower's DPS). When target is full → next most injured siege unit. No injured siege units → follows nearest siege unit as bodyguard at 40-60px.  
- **Upgrade ×10:** +X HP/s healing per upgrade. Emeralds.

**Unlock: Reinforced for Battle** — cost TBD (Steel + Emeralds)  
When Siege Engineer heals a siege unit to full HP, that unit gains a **temporary armor buff** (+X armor for Y seconds — TBD). Rewards stacking 2-3 Engineers on one Ram.

**Design target:** 1 Engineer ≈ survival, 2 ≈ comfortable, 3 ≈ the tower is probably doomed.

---

## RANGERS LODGE
**Line IDs:** `stinger_drake_line`, `longbowman_line`, `air_skirmisher_line`, `fledgling_rider_line`  
**Tier:** 3 | **Prerequisites:** Adventurers Academy + Ballistic Range | **Levels:** None  
**Theme:** Aerial rangers and flying creatures. Specializes in air units, ranged physical, poison debuffs, and air-to-air combat. Only building with a dedicated flying healer.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Stinger Drake | Venom Drake | Plague Drake |
| 2 | Longbowman | Warbow | Master Fletcher |
| 3 | Air Skirmisher | Sky Hunter | Drake Sniper |
| 4 | Fledgling Rider | Pegasus Rider | Celestial Skymaster |

### Slot 1 — Stinger Drake Line
**Unit Type:** Flying, Ranged, Physical + Poison  
**Stats:** T1: 50 HP / 3 armor / 11 dmg / 1.1s / 130 move | T2: 95/5/19 | T3: 181/8/33  
**Ability: Poisoned Spike** — 10s CD. Normal ranged attack + poison DoT **4/7/10 dmg/s for 3s** (T1/T2/T3). For DoT duration: **-25% Parry, -25% Block, -25% Attack Speed, -25% Accuracy**.  
- **Upgrade ×10:** +1% to ALL four debuff values per upgrade. Wood.  
**Unlock: Deep Spikes** — 2000 Wood — Doubles DoT and all debuff duration (3s → 6s).

### Slot 2 — Longbowman Line
**Unit Type:** Ranged, Physical  
**Stats:** T1: 40 HP / 2 armor / 12 dmg / 1.3s / 110 move | T2: 76/4/21 | T3: 145/6/36  
**Ability: Power Shots** — 20s CD. Attack speed **-25%** but attack damage **×2** for next **3 attacks**. Speed penalty persists until all shots are fired.  
- **Upgrade ×10:** +1 additional Power Shot per upgrade (3 → 13 at max). Wood.  
**No unlock.**

### Slot 3 — Air Skirmisher Line
**Unit Type:** Ranged, Physical, Anti-air specialist  
**Stats:** T1: 38 HP / 2 armor / 13 dmg / 1.0s / 125 move | T2: 73/4/22 | T3: 139/6/38  
**Ability: Deploy Hunting Location** — Normal archer range by default. When enemy air unit detected in range: 2s deploy animation → **Hunting Mode**: range +200%, damage +20%. Persists until no air units in expanded range. Un-deploys when threat clears.  
**No unlock.**

### Slot 4 — Fledgling Rider Line
**Unit Type:** Flying, Melee/Support  
**Stats:** T1: 62 HP / 4 armor / 10 dmg / 1.2s / 120 move | T2: 118/7/17 | T3: 225/11/29  
**Ability: Blessed Aura** — Passive 200px. All friendly air/flying units: **25% damage reduction, +25% move speed, 1/2/3 HP/s** (T1/T2/T3).  
- **Upgrade ×10:** +1% DR, +1% move speed, +0.5 HP/s per upgrade. Wood + Emeralds.  
**Unlock: Blessings of the Herd** — 2000 Wood + 2000 Emeralds — Flying units in aura for 8 continuous seconds: **+100 HP burst heal**, re-triggers every 8s while in aura.

---

## CLOCKWORK SKYPORT
**Line IDs:** `clockwork_skyship_line`, `clockwork_copter_line`, `dwarven_rifleman_line`, `volley_gunner_line`  
**Tier:** 4 | **Prerequisites:** Rangers Lodge + Mountain Keep | **Levels:** None  
**Theme:** Steampunk dwarven air force. Most technologically advanced building. Rudimentary guns, blimps, homing rockets, and a comically large organ gun.

| Slot | T1 | T2 | T3 |
|---|---|---|---|
| 1 | Clockwork Skyship | Dreadnought Mk II | Leviathan Mk III |
| 2 | Clockwork Copter | Interceptor | Ace |
| 3 | Dwarven Rifleman | Sharpshooter | Deadeye |
| 4 | Volley Gunner | Rotary Volley Gunner | Storm Volley Gunner |

### Slot 1 — Clockwork Skyship Line
**Unit Type:** Flying, Siege  
**Stats:** T1: 70 HP / 8 armor / — dmg / — atk / 80 move | T2: 133/13/— | T3: 253/21/—  
**System: Bombing Run** — Paths to nearest enemy tower/building (400px priority, ignores units). Floats directly over target and drops bombs. Each bomb = **1/2 of same-tier Archer Tower base HP** (ignores armor). **2 bombs kills a same-tier tower**. T1 bomb = 350 damage vs 700 HP tower.  
- **Upgrade ×10:** +X bomb damage. Stone + Steel.  
**No unlock.** Countered by anti-air units — must be intercepted before it reaches towers.

### Slot 2 — Clockwork Copter Line
**Unit Type:** Flying, Ranged, Anti-air  
**Stats:** T1: 35 HP / 2 armor / 60 dmg / 2.0s / 140 move | T2: 67/4/102 | T3: 127/6/173  
**DPS: 30/s — highest in game at T1**  
**System: Homing Rocket** — Targets nearest enemy air unit in range (~300-350px). **Perfect accuracy / hitscan**. Fires every 2s for 30 damage. Rocket homes to target. Idle (no air targets): retreats to own buildings and patrols between them automatically. Does NOT engage ground units.  
- **Upgrade ×10:** +X damage. Steel + Rubies.  
**No unlock.** Hard countered by having no air units.

### Slot 3 — Dwarven Rifleman Line
**Unit Type:** Ranged, Physical, Tower guard  
**Stats:** T1: 45 HP / 4 armor / 13 dmg / 1.0s / 100 move | T2: 86/7/22 | T3: 163/11/38  
**System: Deploy (mirrors Air Skirmisher — ground targets)** — Prefers to station near player towers. Standard archer range normally. When enemy ground unit detected in range: 2s deploy → **Sniper Mode**: range +200%, damage +20%, perfect accuracy/hitscan. Persists until ground threats clear. Un-deploys to tower guard position.  
- **Upgrade ×10:** +X damage. Steel + Wood.  
**No unlock.**

### Slot 4 — Volley Gunner Line
**Unit Type:** Ranged, Physical, Anti-unit siege  
**Visual:** Ribauldequin (organ gun) — multiple barrels fire simultaneously in fan arc.  
**Stats:** T1: 55 HP / 5 armor / 8 dmg per projectile / 10.0s / 85 move | T2: 105/8/8 | T3: 200/13/8  
*(HP/armor scale normally. Damage per projectile stays 8 — tier scaling comes from projectile count)*  
**System: Volley Fire** — Building priority 400px (same as Bombard/Iron Husk). On reaching position: 2s deploy → fires **10/20/30 projectiles simultaneously** (T1/T2/T3) in shotgun spread arc. Each projectile: 8 physical damage. 10s reload. Does not move while deployed and reloading.  
- Against 2 armor: 6 dmg/projectile → 60/120/180 total. Against 8 armor: 0 dmg (all soaked).  
- **Upgrade ×10:** +1 projectile per volley per upgrade (all tiers). Steel + Stone.  
**No unlock.** Pair with Alchemist Corrode to counter heavy armor.

---

## BUILDING TECH TREE (Recovered from Balance_v1_27.xlsx)

| Tier | Building | Dependencies | Has Levels? | Notes |
|---|---|---|---|---|
| 0 | **Castle** | — | No | Starting building |
| 1 | Academy | Castle | Yes (3) | |
| 1 | Armory | Castle | No | Mid-run gold sink: temp melee upgrades (lost on run end) |
| 1 | Arrow Tower | Castle | No | Utility building — no unit lines |
| 1 | Ballistic Range | Castle | No | Mid-run gold sink: temp ranged upgrades (lost on run end) |
| 1 | Barracks | Castle | Yes (3) | |
| 1 | Chapel | Castle | Yes (3) | |
| 1 | Construction Yard | Castle | Yes (3) | |
| 1 | Gold Mine | Castle | No | |
| 1 | Quarry | Castle | No | Generates meta resources per run |
| 2 | Adventurers Academy | Barracks + Academy | No | Rogue, Party Leader, Bard lines |
| 2 | Beast Kennel | Barracks | Yes (3) | |
| 2 | Magic School | Academy | Yes (3) | |
| 2 | Mountain Keep | Barracks + Construction Yard | Yes (3) | |
| 2 | Siege Workshop | Construction Yard | Yes (3) | |
| 3 | Enclave | Chapel + Magic School | Yes (3) | |
| 3 | Monster Lair | Mountain Keep + Beast Kennel | Yes (3) | |
| 3 | Rangers Lodge | Adventurers Academy + Ballistic Range | No | Drake, Hawkcaller, Air Skirmisher lines |
| 4 | Clockwork Skyport | Rangers Lodge + Mountain Keep | No | Skyship, Copter, Rifleman, Volley Gunner lines |
| 4 | Elemental Portal | Magic School + Enclave | Yes (3) | |
| 4 | Grand Cathedral | Chapel + Magic School + Enclave | Yes (3) | Requires 3 dependencies |
| 4 | Grove | Enclave | Yes (3) | |

**Total: 22 buildings**

### Critical Notes
- **Grand Cathedral** requires Chapel + Magic School + Enclave — minimum 4-building prerequisite chain. Most gated building in the game. Validates the Seraphic Avenger "We Need You Now!" power level.
- **Outpost** is NOT in this building list — present in current game data but absent here. May have been added after this file or renamed. Needs investigation.
- **Adventurers Academy, Rangers Lodge, Clockwork Skyport** — all `No Levels` (single-purchase). Their units are special/unique tiers.
- **Armory + Ballistic Range** — run-only temporary upgrade buildings, not persistent.

### Newly Confirmed Unit Lines (Previously Unknown)
**Adventurers Academy:**
- Slot 1: Rogue / Shadowblade / Master Thief
- Slot 2: Party Leader / Veteran Leader / Legendary Commander
- Slot 3: Bard / Troubadour / Virtuoso

**Rangers Lodge:**
- Slot 1: Stinger Drake / Venom Drake / Plague Drake
- Slot 2: Hawkcaller / Skycaller / Stormcaller
- Slot 3: Air Skirmisher / Sky Hunter / Drake Sniper

**Clockwork Skyport:**
- Slot 1: Clockwork Skyship / Dreadnought (Mk II) / Leviathan (Mk III)
- Slot 2: Clockwork Copter / Interceptor / Ace
- Slot 3: Dwarven Rifleman / Sharpshooter / Deadeye
- Slot 4: Volley Gunner / Rotary Volley Gunner / Storm Volley Gunner

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

# 16. UI SCREENS — HOME CASTLE SCREEN

## Screen Flow (Updated)
```
Title / Home Screen
  → Player Identity Screen (name + King/Queen)
  → Home Castle Screen  ← PRIMARY META HUB (new)
  → Vision Path Select (lucid dream branching — future)
  → Quest Select
  → Unit Deployment Screen (renamed from "Unit Purchase Screen")
  → Battle Map
  → Battle Outcome
```

## Home Screen (Title)
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

---

## Home Castle Screen — FULL SPEC

### Concept
The player's home castle grounds. A persistent visual hub showing all buildings the player can invest in across runs. Gold is spent here to purchase buildings and send units to the deployment screen.

### Thematic Framing
Player writes home for reinforcements each round. The castle is the home base — always present, never destroyed. Each run = a lucid dream/vision of the journey ahead. Failing a run = the dream ends in failure; player wakes up and starts over, but keeps all meta unlocks.

### Layout (1920×1080 pixel-perfect)

#### Zone Map
```
┌─────────────────────────────────────────────────────────────┬─────────────────┐
│  [Gold Mine 256×96]    [castle wall crenellations]          │                 │
│                                                             │  CASTLE         │
│     T1 col      T2 col      T3 col                         │  608 × 232 px   │
│    256×128    256×128     256×128     [T4 Large Plot        │                 │
│  ──────────  ──────────  ──────────   608 × 360 px]        │                 │
│  Row 1 (guild)                                              │                 │
│  Row 2 (military)                                           ├─────────────────┤
│  Row 3 (nature)                                             │ [T4 Large Plot  │
│  Row 4 (knowledge)                                          │  608 × 360 px]  │
│  Row 5 (craft)          [Quarry 256×96]                     │                 │
├─────────────────────────────────────────────────────────────┴─────────────────┤
│  Walking path — 3 × 32px tiles = 96px                                         │
└───────────────────────────────────────────────────────────────────────────────┘
```

#### Pixel Coordinates
| Element | X | Y | W | H |
|---|---|---|---|---|
| Gold Mine | 96 | 4 | 256 | 96 |
| Quarry | 800 | 4 | 256 | 96 |
| Castle | 1152 | 0 | 608 | 232 |
| T1 column | 96 | — | 256 | 128 |
| T2 column | 448 | — | 256 | 128 |
| T3 column | 800 | — | 256 | 128 |
| T4 slot 1 | 1152 | 232 | 608 | 360 |
| T4 slot 2 | 1152 | 624 | 608 | 360 |
| Path zone | 0 | 984 | 1920 | 96 |

#### Row Y Positions (building top edge)
| Row | Y | Bottom |
|---|---|---|
| Row 1 | 152 | 280 |
| Row 2 | 328 | 456 |
| Row 3 | 504 | 632 |
| Row 4 | 680 | 808 |
| Row 5 | 856 | 984 |

#### Gaps
| Gap | Value |
|---|---|
| Left/right margin | 96 px |
| Column gap (T1↔T2, T2↔T3, T3↔T4) | 96 px |
| Row gap (between building rows) | 48 px |
| T4 inter-slot gap | 32 px |
| Path = 3 × 32px tileset tiles | 96 px |

### Castle (Permanent Fixture)
- Not purchasable. Always present. Cannot be destroyed.
- Units: Footman · Spearman · Scout · Militia (inherited from old Barracks)
- Visually occupies top-right of screen, above the T4 column
- Castle building sprite: 608 × 232 px slot (Morgan draws custom castle art)
- ⚠️ Placeholder sprite: Barracks.png scaled to 608×232 until castle art is made

### Building Slot Placeholder
- All building slots use **Barracks.png scaled to the slot dimensions** as placeholder
- T1/T2/T3 slots: scale Barracks.png to 256×128 px
- T4 large plots: scale Barracks.png to 608×360 px
- Replace with proper building art as art gets made — no code change needed, just swap sprite

### Building Progression Rules
- **T1 buildings:** Purchased directly — available from run start once unlocked
- **T2 buildings:** Requires T1 built first in same row; player picks ONE path (locked for the run)
- **T3 buildings:** Requires T2 built first; player picks ONE path (locked for the run)
- **T4 buildings:** Appear only when player has both required T3 buildings from two different rows
- **Max T4 per run:** 2 (floor(5 rows ÷ 2) = 2). One T3 always left unpaired.
- **Omni Fortress:** Special T4 that appears if all 5 T3s are built but no valid cross-row pair exists

### Meta Persistence Rules
- **Building unlocks persist across runs** — once unlocked, always available for purchase in future runs
- **Buildings must be purchased each run** — unlock is permanent, purchase is per-run
- **Path commitment is per-run** — choosing T2A vs T2B locks for the run; resets on loss
- **Run loss = dream failure** — all run purchases reset, but meta unlocks remain
- **Encourages variety** — players will try different row combinations each run to discover all 45 buildings

### Gold Flow
- Gold is spent on the Home Castle Screen to:
  1. Purchase/construct buildings (one-time unlock cost + per-run build cost)
  2. Send units to the Unit Deployment Screen (per-unit cost)

### T4 Slot Display Logic
```
if (t4_slot_unlocked && valid_pair_exists) → show T4 building in slot
if (t4_slot_unlocked && no_pair)           → show "Omni Fortress" if all 5 T3s built
if (!t4_slot_unlocked)                     → show locked/empty slot
```

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
| NOW | Battle controller fallback enemy generation (v2.3.1 GPT hotfix) — CONFLICTS with §1 hard rule | Remove fallback from obj_battle_controller/Create_0.gml |
| NOW | Outpost still purchasable in building list — should be level-design only | Remove from spawnable building list |
| NOW | Archer sprite check `spr_arrow5` failing — Archers shoot fireballs | Fix sprite check; confirm sprites in build |

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
| Omni Fortress unit design | What 4 units does it provide? What is its unique ability? |
| Barracks T1 slots 3+4 | 2 new unit lines needed |
| Armory T2 slots 3+4 | 2 new unit lines needed |
| 119 new unit lines | Names (T1/T2/T3), abilities, stats for all new buildings — see §3B |
| "Elven Outpost" naming collision | T3 building vs map camp — rename map building to Field Camp |
| Guild Hall T1 unit design | Theme TBD (freelancer/civilian base) |
| Herbalist Hut T1 unit design | Theme: nature healing |
| School T1 unit design | Theme: academic starter |

## 🟠 Systems Needing Implementation
| System | Section | Blocked By |
|---|---|---|
| Home Castle Screen GML | §16 | Art (placeholder Barracks.png ready) |
| Parry/Block/Knockback | §4 | Unit type audit |
| Facing/Directional | §5A | Nothing — ready to implement |
| Puddle System | §5B | Nothing — ready to implement |
| Ballista Shot | §5C | Nothing — ready to implement |
| Just Me and You (wall) | §5D | Puddle/collision system |
| Shadow Adept Teleport | §5E | Facing system |
| Vision Void | §5F | LOS system |
| Flag/Zone Suggestions | §5G | Map system |
| Ability Deployment | §15 | Ability system |
| Multi-Projectile System | §4 | Projectile system |
| Summon/Pet System | §5H | Beast Kennel abilities |
| Global inaccuracy cone | §4 | Nothing |
| Chain Lightning "Charged" debuff | §6 | Status effect system |
| Building unlock persistence system | §16 | GML architecture |
| T4 slot display logic | §16 | Building unlock system |
| Army Upgrader Stage 2 | §9 | Stage 1 complete |

## 🔵 Future Work
| Item | Notes |
|---|---|
| 119 new unit lines (names + abilities + stats) | See §3B roster gap table — design in building order T1→T4 |
| Home Castle Screen art | Castle, Mine, Quarry, all 45 building sprites — use Barracks.png placeholder |
| Map system / Field Camp building | Full design needed; Field Camp replaces "Outpost" on maps |
| Marketplace (meta currency trading) | 5:1 or 10:1 painful exchange ratio |
| Death sprite rotation for Ballista | Requires `death_angle` variable on units |
| Narrative Bible | Story of player fighting toward corrupted father |
| Vision Path Select screen | Lucid dream branching — future screen in flow |
| Gold Mine economy exploit patch | Grand Cathedral T3 achievable by round 30 — gate prerequisite enforcement |

---
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

---

# RECOVERED DATA: Balance_v1_27.xlsx (Old Pre-Hallucination Snapshot)

## ⚠️ Data Quality Notes
This data is from an early snapshot (~January 2026) before ChatGPT began hallucinating. Values marked `placeholder` or `Unknown` building were partially generated. Unit stats for Castle/Barracks/Chapel/Academy lines are most reliable. Higher-tier building unit stats are mostly placeholder copies.

---

## TOWERS (Complete — from Towers_v1)

### Player Towers (built via Construction Yard)

| Tower | Tier | Type | Range | AOE | Speed | HP | DMG | Unlock | Effective Window |
|---|---|---|---|---|---|---|---|---|---|
| Archer Tower | 1 | Ranged Single | 700px | — | 1.2s | 250 | 18 | Construction Yard | R1–30 |
| Crossbow Tower | 2 | Ranged Single | 800px | — | 1.8s | 450 | 28 | Construction Yard | R1–60 |
| Marksman Tower | 3 | Ranged Single | 900px | — | 2.2s | 650 | 40 | Construction Yard | R1–90 |
| Heavy Crossbow Tower | 1 | Ranged Single | 1400px | — | 12s | 300 | 200 | CY + Siege Workshop | R25–35 |
| Heavy Crossbow Tower | 2 | Ranged Single | 1500px | — | 10s | 500 | 280 | CY + Siege Workshop | R40–55 |
| Heavy Crossbow Tower | 3 | Ranged Single | 1600px | — | 9s | 750 | 360 | CY + Siege Workshop + Grove | R60–75 |
| Apprentice Tower | 1 | Magic Multiple (3) | 650px | — | 0.7s | 240 | 6 | CY + Academy | R20–30 |
| Apprentice Tower | 2 | Magic Multiple (4) | 700px | — | 0.75s | 420 | 8 | CY + Magic School | R35–55 |
| Apprentice Tower | 3 | Magic Multiple (5) | 750px | — | 0.8s | 600 | 10 | CY + Magic School (Max) | R60–70 |
| Ritual Tower | 1 | Magic AOE (∞) | 900px | 260px | 20s | 320 | 25 | CY + Enclave | R30–45 |
| Ritual Tower | 2 | Magic AOE (∞) | 950px | 320px | 22s | 520 | 32 | CY + Enclave (Max) | R50–70 |
| Ritual Tower | 3 | Magic AOE (∞) | 1000px | 420px | 26s | 800 | 40 | CY + Grand Cathedral (Max) | R70–90 |

**Notes:**
- Heavy Crossbow Tower has comically slow projectile — spectacle weapon, not DPS
- Heavy Crossbow Tower T3 "launches an extra big tree bolt" — visual set piece
- Ritual Tower T3 is gated behind Grand Cathedral Max Level — among the most expensive unlocks in the game
- Towers are ineffective after Round 90 by design; Rounds 91–100 are unit-combat-only

### Enemy Towers

| Tower | Tier | Type | Range | AOE | Speed | HP | DMG | Window |
|---|---|---|---|---|---|---|---|---|
| Gallows Spire | 1 | Ranged Single | 750px | — | 1.1s | 500 | 35 | Late |
| Impaler Spire | 2 | Ranged Single | 850px | — | 1.6s | 800 | 45 | Late |
| Black Ballista | 3 | Ranged Single | 950px | — | 2.0s | 1200 | 60 | Late |
| Cataclysm Harpoon | 1 | Ranged Single | 1500px | — | 12s | 650 | 320 | Rare late |
| Dreadspike Launcher | 2 | Ranged Single | 1600px | — | 10s | 900 | 420 | Rare late |
| World-Piercer Engine | 3 | Ranged Single | 1700px | — | 9s | 1400 | 560 | R80–90 only |
| Hex Choir | 1 | Magic Multiple (3) | 700px | — | 0.65s | 550 | 12 | R60–80 |
| Warp Conclave | 2 | Magic Multiple (4) | 750px | — | 0.7s | 850 | 14 | R70–90 |
| Profane Arcanum | 3 | Magic Multiple (5) | 800px | — | 0.75s | 1200 | 18 | R80–90 |
| Oblivion Monolith | 1 | Magic AOE (∞) | 950px | 320px | 22s | 700 | 35 | R80–100 |
| Cathedral of Ash | 2 | Magic AOE (∞) | 1000px | 420px | 24s | 1000 | 45 | R85–100 |
| Final Invocation | 3 | Magic AOE (∞) | 1100px | 520px | 26s | 1400 | 55 | R90–100 |

Enemy towers are stronger than player equivalents (higher HP/DMG). Intentionally designed to end runs, not stall them — Ritual Resolution Rule prevents infinite stalemates.

---

## DP & ECONOMY SYSTEM (Authoritative Formulas)

### RP Base Formula by Stage
| Stage | Rounds | RP Formula (Normal) |
|---|---|---|
| 1 | 1–10 | `RP = 20 + 2*Round` |
| 2 | 11–20 | `RP = 40 + 4*(Round-10)` |
| 3 | 21–30 | `RP = 80 + 6*(Round-20)` |
| 4 | 31–60 | `RP = 140 + 10*(Round-30)` |
| 5 | 61–90 | `RP = 440 + 16*(Round-60)` |
| 6 | 91–100 | `RP = 920 + 30*(Round-90)` |

### DP Gain Formula (Locked)
`DP_gain = ceil_to_5((Baseline_RP + Optional_RP) * TierMultiplier * DifficultyMultiplier * GreedMultiplier)`
`DP_RATE = 0.9` (enemy DP = RP_Total * 0.9 * DP_Tax)

### Tier Multipliers by Round
| Rounds | Tier Mult |
|---|---|
| 1–10 | 0.5× |
| 11–30 | 1× |
| 31–60 | 1.5× |
| 61–90 | 2× |
| 91–100 | 3× |

### Greed Multipliers
| Quest Type | Spawn Chance | Available From | RP Mult | DP Tax |
|---|---|---|---|---|
| Normal | 30% | Round 1 | 1.0× | 1.00 |
| Greed I | 50% | Round 1 | 1.4× | 1.08 |
| Greed II | 20% | Round 5 | 1.8× | 1.15 |

### Difficulty Multipliers (Placeholder)
| Mode | Mult |
|---|---|
| Normal | 1× |
| Hard | 1.25× |
| Very Hard | 1.5× |
| Doomed | 2× |

### DP Bucket Allocations by Round Band
| Band | Core% | Counter% | Spice% | Tower% Cap | Siege% Min |
|---|---|---|---|---|---|
| 1–10 | 75 | 15 | 10 | 0 | 0 |
| 11–30 | 70 | 20 | 10 | 0 | 0 |
| 31–60 | 65 | 25 | 10 | 5 | 0 |
| 61–90 | 60 | 25 | 10 | 10 | 0 |
| 91–100 | 55 | 25 | 10 | 10 | 15 |

### Spawn Tag Vocabulary
| Tag | Meaning | Notes |
|---|---|---|
| `core` | Default wave pressure | Main DP spend |
| `frontline` | Damage soakers/blockers | Often paired with core |
| `backline` | Ranged/magic DPS | Dive targets |
| `diver` | Backline hunters | Counter bucket, spawns after R31 |
| `support` | Heals/buffs/debuffs | Enemy support should be rare |
| `siege` | Anti-tower/anti-boss | Min enforced after R91 |
| `tower` | Static building/tower | Enemy towers capped by DP budget |
| `summon` | Summoned unit | Not DP-spent directly |
| `boss` | Boss unit | Forced spawns, not DP-spent |
| `player_only` | Never spawn for enemy | Safety filter |
| `not_spawnable` | Header row | Filter out of spawner entirely |

### Early Quest Gold Bands (Locked v1.4)
| Rounds | Gold-heavy | Balanced | Meta-heavy |
|---|---|---|---|
| 1–3 | 30–35 | 18–22 | 8–12 |
| 4–5 | 38–45 | 22–28 | 10–15 |
| 6–7 | 42–50 | 25–30 | 12–18 |
| 8–10 | 45–55 | 28–35 | 15–22 |

### Tier 1 Building Costs (Locked)
| Building | Gold Cost |
|---|---|
| Barracks | 40 |
| Gold Mine | 50 |
| Construction Yard | 50 |
| Arrow Tower (unit, not building) | 50 |
| Chapel | 60 |
| Academy | 80 |

### Castle Unit Costs (Locked)
| Unit | Gold Cost |
|---|---|
| Squire | 15 |
| Archer | 20 |
| Shield Bearer | 25 |
| Banner Carrier | 35 |

---

## GOLD MINE ARMY TRAINING UPGRADES (Rows 310–316)
These upgrades belong in the Gold Mine building section of Army Training:

| Upgrade | Type | Effect | Base | Per Level | Max | Cost/Level | Total |
|---|---|---|---|---|---|---|---|
| Greed I Chance | Utility | Increase Greed I quest appearance chance | 50% | +5% | 4 | 25 Emeralds | 100 Emeralds |
| Greed II Chance | Utility | Increase Greed II quest appearance chance | 20% | +3% | 4 | 40 Emeralds | 160 Emeralds |
| Quest Slot 4 | Utility | Unlock 4th quest choice | 3 slots | +1 | 1 | 50 Emeralds | 50 Emeralds |
| Quest Slot 5 | Utility | Unlock 5th quest choice | 4 slots | +1 | 1 | 100 Emeralds | 100 Emeralds |
| Starting Gold | Economic | Start runs with bonus gold | 100g | +25g | 4 | 15 Rubies | 60 Rubies |
| Gold Mine Yield | Economic | Increase gold mine output per round | +10g | +2g | 10 | 10 Rubies | 100 Rubies |
| Gold Mine Upgrade Cap | Economic | Reduce gold mine upgrade cost cap | 50g cap | -5g | 5 | 20 Rubies | 100 Rubies |

---

## PSEUDOCODE — CASTLE MECHANICS (from Pseudocode_v1)

### Cohesion (Morale-lite)
Formation stability bonus. Upgrade hook: Castle Row0 ColB ("Feudal Organization").
- `cohesion_radius_px` = 170 (default)
- `cohesion_value` = clamp(nearby_allies, 0, 6) / 6.0
- Damage reduction = `cohesion_value * COHESION_DR_BASE` (default 0.08)
- Attack speed bonus = `cohesion_value * COHESION_AS_BASE` (default 0.06)
- Max cohesion (6 allies within 170px) = 8% DR + 6% attack speed
- Intentionally small — stability not a win button

### Parry (Melee Only)
Defensive proc vs melee hits. Upgrade hook: Squire Row1 ColB ("Parry Training").
- `parry_chance` = 0..1
- `parry_reduction` = 0.60 (60% damage reduction on parry)
- `parry_riposte_bonus` = 0.25 (+25% next attack damage)
- On parry: also gain 10% faster attack briefly (1.0s)
- Supports guaranteed parry (first hit) as upgrade option
- Melee-only — magic/ranged hits cannot be parried

### Buff Carryover (Promotion Retention)
When a unit promotes (tier up), some buffs carry over scaled by `buff_carryover_pct`.
- XP always retains 100%
- HP% optionally preserved
- Permanent buffs NOT carried over
- Debuffs NOT carried over by default
- Buff magnitude and remaining_time both scaled by carryover_pct

### Power Strike (Knight Captain)
Long-cooldown heavy hit. Upgrade hook: Knight Captain Row3 ColB ("Power Strike").
- Suggested cooldown: 8–14s (readable on screen)
- Unlocked at rank 5 of Power Strike upgrade
- Deals `base_damage * power_strike_mult`
- If power strike available → use it instead of normal attack

---

## DESIGN PRINCIPLES (from Design_Summary_v1)

- Attack speed is time-based: 1.0 = 1 attack/second; 0.5 = twice as fast; 2.0 = slower
- Battlefield: 1920×1080px, divided into thirds (player / no-man's land / enemy)
- Units ~40×40px, buildings ~80×80px; units can overlap ~50% to reduce pathing jams
- AOE radius in pixels
- Default leveling growth: +6 HP, +0.4 Armor, +0.5 Damage per XP level; units rarely exceed level 5 without support
- **All systems modify WHEN the player loses, not WHETHER**
- Buildings respawn each round at full HP, do not gain XP, do not move, fight only after army is dead, are sunk-cost investments
- Thorn Warden (Grove T3) hard-counters Corrupted Barracks T3 — this relationship must be preserved during balance

### Tower Relevance Windows
| Tier | Effective Until |
|---|---|
| T1 towers | ~Round 30 |
| T2 towers | ~Round 60 |
| T3 towers | ~Round 90 |
| R91–100 | Towers functionally irrelevant |

### Ritual Resolution Rule (Anti-Infinite)
If player has no active units remaining: display "There are no more forces to stop the ritual." Enemy ritual timer accelerates ~20× and triggers unavoidable destruction sequence ending round as a loss. Prevents infinite tower-vs-tower stalemates. Enemy towers are mirrors of player towers but stronger — never smarter.

---

## HALLUCINATED UNITS — DISCARDED (Former Grand Cathedral Draft)
Rows 274–309 in Army Training Costs contained 12 units with `Building: Unknown`:
Consecrator, Judgment Bearer, Peoples Defender, Field Reclaimer, Sanctifier, Retribution Paladin, Bastion of Faith, Grave Warden, High Consecrator, Justiciar Paladin, Martyr Guardian, Resurrection Justiciar

**Confirmed hallucinated** — these were ChatGPT's drift on the Grand Cathedral before the session collapsed. They share naming conventions and implied ability schemes with the real Grand Cathedral lines (Cleric/Confessor/Martyr/Seraphic Avenger) but are not canonical. The real Grand Cathedral unit names were finalized and are documented above. **These 12 names are discarded.**

The Grand Cathedral was the last building fully designed with ChatGPT before the hallucination spiral. Adventurers Academy, Rangers Lodge, and Clockwork Skyport have since been **fully designed with Claude** — all abilities, stats, and unlocks are documented in this vault. The ChatGPT data loss has been recovered.


---

# BUILDING: ADVENTURERS ACADEMY
**Tier:** 2 | **Prerequisites:** Barracks + Academy | **Levels:** None (no level upgrades)
**Theme:** D&D tropes, comic relief, gold extraction. Every unit in this building has some relationship with money. One slot is an explicit joke unit with no upgrade path.

---

## Slot 1 — Rogue Line

| Tier | Unit |
|---|---|
| T1 | Rogue |
| T2 | Shadowblade |
| T3 | Master Thief |

**Ability: Steal**
The Rogue imparts the debuff "Empty Pockets" onto any unit it attacks. When the debuff is applied, the Rogue receives **15% of the target unit's gold value** immediately.

- T1/T2/T3 base: 15% of unit value as gold on debuff application
- **Upgrade ×10:** +2% gold received per upgrade (15% → 35% at max)
- Cost doubles from base each upgrade

**Design Notes:**
- "Empty Pockets" is a one-time-per-unit proc — cannot be re-applied to the same target. Prevents infinite farming of high-value tanks that survive prolonged combat.
- On-hit gold stealing is small and constant, making it feel like pickpocketing rather than burst economy. Thematically distinct from Shadowy Figure (corpse looting) and Party Leader unlock (last-hit bounty).
- Future consideration: a "mark" variant that allows re-application after a cooldown window.

**Unlock: Stealth**
Cost: 300 Iron + 300 Sapphires
Unit becomes **permanently invisible** until it attacks. The unit will attempt to path around behind an enemy unit and attack from behind.
- Invisibility breaks on attack, reapplies after a short delay (TBD)
- Pairs with the facing/directional system (Section 5A) for backstab positioning
- Functions as a loot delivery tool — invisible unit can slip past combat to reach high-value targets
- ⚠️ Requires facing system implementation before unlock is fully functional

---

## Slot 2 — Party Leader Line

| Tier | Unit |
|---|---|
| T1 | Party Leader |
| T2 | Veteran Leader |
| T3 | Legendary Commander |

**Ability: XP Farming**
Friendly units within **300px** gain an additional **30/40/50% XP** (T1/T2/T3) from all sources.

- **Upgrade ×10:** +2% XP bonus per upgrade (30/40/50% → 50/60/70% at max)
- Cost doubles from base each upgrade

**Design Notes:**
- Aura is purely additive — does not stack multiplicatively with other XP bonuses
- Strong synergy with high-kill-count units; accelerates level-ups for frontline fighters
- Multiple Party Leaders in range: soft cap consideration recommended (diminishing returns or hard cap on XP bonus stacking)

**Unlock: Clean Those Pockets**
Cost: 300 Stone + 300 Wood + 300 Sapphires
Any unit within the XP Farming aura gains **10% of enemy unit's gold value on last hit**.
- Applies to all friendly units in 300px, not just the Party Leader itself
- Stacks with Rogue's Steal (different trigger — last hit vs. on-hit debuff)
- Wood cost is intentionally off-theme — adventurers burn through resources indiscriminately. Justified.

---

## Slot 3 — Bard Line

| Tier | Unit |
|---|---|
| T1 | Bard |
| T2 | Troubadour |
| T3 | Virtuoso |

**Ability: Silver Tongue**
Friendly units near the Bard gain **1/2/3 HP per second** (T1/T2/T3) within a **300px aura**. Enemy units that remain within the aura for **10 seconds** tip the Bard **1/2/3 gold**.

- **Upgrade ×10:** +0.5 HP/sec healing AND +0.5 gold per tip per upgrade
- Cost doubles from base each upgrade

**Design Notes:**
- Enemy tipping is the funniest mechanic in the building. It is small enough to read as flavor at low upgrade levels but compounds interestingly at max.
- Multiple Bard stacking: soft cap recommended on gold tipping (enemies should not tip more than their actual value over a fight). Hard cap suggestion: enemy cannot tip more than 50% of its gold value total per round.
- The healing aura is honest and useful. The gold mechanic is the identity hook.
- 10-second tip timer means fast enemies or enemies that die quickly don't contribute much — rewards building a defensive line that keeps enemies alive in the aura briefly.

**Unlock: Crowd Work**
Cost: 500 Wood + 500 Emeralds
A **random enemy unit** currently under the effect of Silver Tongue takes **30 points of physical ranged damage** and tips an **additional 2 gold**.
- Triggers on a cooldown (TBD — suggest 8–12s)
- "Crowd Work" targets random — cannot be aimed. Keeps it chaotic and on-theme.
- Damage is physical ranged — interacts with armor and relevant resistances

---

## Slot 4 — Tinkerer Line

| Tier | Unit |
|---|---|
| T1 | Tinkerer |
| T2 | Artificer |
| T3 | Siege Artificer |

**Ability: Summon Rifle Construct**
The Tinkerer summons a **Rifle Construct** companion that follows its owner and attacks independently.

**Rifle Construct Stats by Tier:**
| Tier | HP | Damage | Attack Speed | Range |
|---|---|---|---|---|
| T1 | 30 | 5 | 1.0s | 250px |
| T2 | 50 | 10 | 1.0s | 250px |
| T3 | 70 | 15 | 1.0s | 250px |

**Rifle Construct Behavior:**
- Follows the Tinkerer and maintains **20–40px distance** at all times
- Default targeting: attacks a **random enemy** within 250px
- Priority override: if any enemy unit attacks the Tinkerer, the Rifle Construct immediately switches to attacking that unit
- If the Tinkerer dies, the Rifle Construct continues fighting until destroyed
- Repositions between rounds (follows Tinkerer to new position)

**Upgrade ×10:** +1 HP and +1 damage to the Rifle Construct per upgrade
- Cost doubles from base each upgrade

**Design Notes:**
- The Tinkerer is intentionally weaker than a basic Archer in raw personal stats (slower fire, less accurate) — the value proposition is the Rifle Construct providing a second simultaneous attacker.
- T1 feels unreliable but the T2 jump is dramatic — suddenly two things are shooting.
- Random targeting on the Construct can feel bad (ignores high-threat targets for low-HP stragglers). Future consideration: upgrade path that adds "prioritize lowest HP" or "prioritize attacker" as a toggleable mode.
- The Construct is a summon — tagged `summon`, not DP-spent directly.

**Unlock: Gatling Turret**
Cost: 200 Iron + 800 Wood
The Rifle Construct's attack damage is **reduced by 3** but its **rate of fire increases by 300%** (attack speed 1.0s → 0.25s).
- Net DPS increases significantly despite lower per-hit damage
- Shreds low-armor targets and swarms; weaker against high-armor single targets
- The damage reduction is a real tradeoff — not a pure upgrade. Players who want burst keep the Rifle Construct; players who want sustained pressure take Gatling Turret.

---

## Slot 5 — Shadowy Figure (NO UPGRADE PATH)

| Tier | Unit |
|---|---|
| T1 | Just a Dwarf |
| T2 | Average Adventurer |
| T3 | Somewhat Tall Man |

**What it actually is:** Goblins in a trenchcoat. One goblin at T1. Two goblins at T2. Three goblins at T3. The disguise gets progressively taller and more implausible with each tier. This is a D&D trope and the one moment of comic relief in an otherwise serious game. Do not attempt to make it competitive. That would ruin the joke.

**Ability: Loot**
When any unit (friendly or enemy) dies within reach, the Shadowy Figure will path to the corpse and attempt to loot it, recovering a **% of that unit's gold cost** for the player.

- Loot % TBD (suggest 10–20% of gold value)
- Must physically reach the corpse — positional, not automatic
- Competes with other Shadowy Figures and Rogues for corpse access
- Has no combat ability worth mentioning

**NO UPGRADE PATH. NO UNLOCK. IT JUST LOOTS BODIES.**

**Design Notes:**
- This unit should not be tuned for combat viability. It is not supposed to win fights.
- The comedy comes from it waddling across a battlefield toward a fresh corpse while everything else is on fire.
- Death behavior: the stack collapses. There is no clean death animation suggestion here — whatever is funniest.
- Do not add upgrades. Do not add unlocks. Do not add synergy bonuses. Leave it alone.

---

## Building Unlock: Shadowy Figure
The Shadowy Figure unit line is unlocked via a **building-level unlock** on the Adventurers Academy itself (not tied to any unit row). Cost TBD.


---

# RECOVERED DATA: unit_lines_audit_2.xlsx

## ⚠️ Data Quality Note
This file is a more recent and more complete snapshot than Balance_v1_27.xlsx. Ability specs for Castle, Academy, Barracks, Beast Kennel, and Chapel are fully documented and should be treated as authoritative. Unit line IDs, tag taxonomy, and systems backlog are confirmed.

---

## UNIT LINE ID CORRECTIONS (from Unit Lines sheet)

The following buildings have incorrect `line_id` assignments in code. T1 is always correct — T2/T3 are frequently wrong. These need a code pass after abilities phase.

### Academy
| Slot | Unit | Current line_id | Correct line_id |
|---|---|---|---|
| 1 | Scholar (T2) | archivist_line | student_line ✗ |
| 1 | Mastermind (T3) | archivist_line | student_line ✗ |
| 2 | Transmuter (T3) | apprentice_line | alchemist_line ✗ |
| 3 | Lorekeeper (T2) | student_line | archivist_line ✗ |
| 3 | Grand Historian (T3) | student_line | archivist_line ✗ |
| 4 | Arcane Professor (T3) | alchemist_line | apprentice_line ✗ |

### Barracks
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Halberdier (T2) | footman_line | spearman_line ✗ |
| 1 | Sentinel (T3) | militia_line | spearman_line ✗ |
| 2 | Man-at-Arms (T2) | militia_line | footman_line ✗ |
| 2 | War Marshal (T3) | scout_line | footman_line ✗ |
| 3 | Pathfinder (T3) | footman_line | scout_line ✗ |
| 4 | Veteran (T2) | spearman_line | militia_line ✗ |
| 4 | Warborn (T3) | spearman_line | militia_line ✗ |

### Beast Kennel
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Alpha Fang (T3) | beast_handler_line | pup_line ✗ |
| 2 | Skywatcher (T2) | horned_cub_line | falconer_line ✗ |
| 3 | Wildcaller (T3) | pup_line | beast_handler_line ✗ |
| 4 | Rambeast (T2) | falconer_line | horned_cub_line ✗ |

### Chapel
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Priest (T2) | hymnist_line | acolyte_line ✗ |
| 1 | High Cleric (T3) | penitent_line | acolyte_line ✗ |
| 3 | Crusader (T3) | hymnist_line | penitent_line ✗ |
| 4 | Cantor (T2) | acolyte_line | hymnist_line ✗ |
| 4 | Choirmaster (T3) | acolyte_line | hymnist_line ✗ |

### Construction Yard
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Crossbow Tower (T2) | heavy_crossbow_tower_line | archer_tower_line ✗ |
| 1 | Marksman Tower (T3) | heavy_crossbow_tower_line | archer_tower_line ✗ |
| 2 | Balista Tower (T2) | apprentice_tower_line | heavy_crossbow_tower_line ✗ |
| 2 | Dragon Slayer Tower (T3) | archer_tower_line | heavy_crossbow_tower_line ✗ |
| 3 | Mage Tower (T2) | ritual_tower_line | apprentice_tower_line ✗ |
| 4 | Battle Ritual Tower (T2) | archer_tower_line | ritual_tower_line ✗ |

**Note:** Construction Yard has unit names not previously documented — Balista Tower, Dragon Slayer Tower, Mage Tower, Battle Ritual Tower, Archmage Tower, War Ritual Tower. These appear to be the T2/T3 upgrade names for the tower lines.

### Elemental Portal
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Flame Spirit (T2) | gustling_line | ember_wisp_line ✗ |
| 1 | Inferno Herald (T3) | gustling_line | ember_wisp_line ✗ |
| 2 | Tidelord (T2) | pebblekin_line | waterling_line ✗ |
| 2 | Abyss Caller (T3) | ember_wisp_line | waterling_line ✗ |
| 3 | Wind Dancer (T2) | waterling_line | gustling_line ✗ |
| 3 | Skyblade (T3) | pebblekin_line | gustling_line ✗ |
| 4 | Earthbound (T2) | ember_wisp_line | pebblekin_line ✗ |
| 4 | Terra Warden (T3) | waterling_line | pebblekin_line ✗ |

### Enclave
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Mystic (T2) | rune_scribe_line | initiate_line ✗ |
| 1 | Seer (T3) | spirit_caller_line | initiate_line ✗ |
| 2 | Veilwalker (T2) | spirit_caller_line | shadow_adept_line ✗ |
| 3 | Soulbinder (T2) | shadow_adept_line | spirit_caller_line ✗ |
| 3 | Astral Warden (T3) | rune_scribe_line | spirit_caller_line ✗ |
| 4 | Glyphmaster (T2) | initiate_line | rune_scribe_line ✗ |
| 4 | Arcforge Arcanist (T3) | initiate_line | rune_scribe_line ✗ |

### Grand Cathedral
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Battle Cleric (T2) | confessor_line | cleric_line ✗ |
| 1 | Hierophant (T3) | martyr_line | cleric_line ✗ |
| 2 | Inquisitor (T2) | seraphic_avenger_line | confessor_line ✗ |
| 3 | Ascetic (T2) | cleric_line | martyr_line ✗ |
| 3 | Living Relic (T3) | seraphic_avenger_line | martyr_line ✗ |
| 4 | Flamewing Justicar (T2) | martyr_line | seraphic_avenger_line ✗ |
| 4 | Archon of the Final Flame (T3) | cleric_line | seraphic_avenger_line ✗ |

### Grove
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Thorn Warden (T3) | seed_sower_line | sproutling_line ✗ |
| 2 | Grovekeeper (T2) | seed_sower_line | dryad_line ✗ |
| 4 | Bloomcaller (T2) | dryad_line | seed_sower_line ✗ |
| 4 | Verdant Sage (T3) | sproutling_line | seed_sower_line ✗ |

### Magic School
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 2 | Glyph Weaver (T2) | frost_acolyte_line | rune_novice_line ✗ |
| 2 | Arcane Sigilmaster (T3) | emberling_line | rune_novice_line ✗ |
| 3 | Icebinder (T2) | rune_novice_line | frost_acolyte_line ✗ |
| 4 | Pyromancer (T3) | rune_novice_line | emberling_line ✗ |

### Monster Lair
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Drake (T2) | cave_imp_line | hatchling_line ✗ |
| 1 | Sky Serpent (T3) | slime_line | hatchling_line ✗ |
| 2 | Kobold Warrior (T2) | hatchling_line | kobold_line ✗ |
| 3 | Tunnel Fiend (T2) | slime_line | cave_imp_line ✗ |
| 4 | Ooze (T2) | kobold_line | slime_line ✗ |
| 4 | Gelatinous Monarch (T3) | hatchling_line | slime_line ✗ |

### Mountain Keep
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 2 | Cavern Striker (T2) | iron_husk_line | tunnel_scout_line ✗ |
| 2 | Deep Delver (T3) | iron_husk_line | tunnel_scout_line ✗ |
| 3 | Geomancer (T2) | mason_line | crystal_adept_line ✗ |
| 3 | Earth Shaper (T3) | mason_line | crystal_adept_line ✗ |
| 4 | Architect (T2) | crystal_adept_line | mason_line ✗ |
| 4 | Bastion Master (T3) | crystal_adept_line | mason_line ✗ |

### Outpost
| Slot | Unit | Current | Correct |
|---|---|---|---|
| 1 | Watchman (T2) | signalman_line | lookout_line ✗ |
| 1 | Sentinel Scout (T3) | courier_line | lookout_line ✗ |
| 2 | Swift Herald (T3) | signalman_line | courier_line ✗ |
| 3 | Patrolman (T2) | lookout_line | border_guard_line ✗ |
| 4 | Beacon Keeper (T2) | border_guard_line | signalman_line ✗ |
| 4 | Skyflare Warden (T3) | lookout_line | signalman_line ✗ |

---

## ABILITY SPECS (Authoritative — from Abilities sheet)

### Castle

**Squire Line — Cleave**
- Primary: Cone AoE damage in front of unit. 70°/90°/110° cone by tier. 5s CD.
- Upgrade ×10: +5° to all tiers. Cost doubles from 15. Steel.
- Unlock "Concentrated Parry": Req level 3. Melee only. +30% parry for 5s. CD 25s/20s/15s per tier. Cost: 120 Steel + 120 Sapphires.

**Archer Line — Hunting**
- Primary: Target nearest support/magic unit. Arrows ignore melee collision for 10s. CD 30s/25s/20s per tier.
- Upgrade ×10: -1s CD all tiers. Cost doubles from 15. Wood.
- No unlock.

**Shield Bearer Line — Fresh**
- Primary: Passive. While above HP threshold take 20% less damage from all sources. Threshold 80%/70%/60% per tier.
- Upgrade ×10: -2% HP threshold all tiers. Cost doubles from 15. Stone.
- Unlock "Mockery": Req level 2. AoE taunt 120px radius every 10s. Cost: 60 Steel + 60 Emeralds.

**Banner Carrier Line — Inspired**
- Primary: Aura. Units within 200px get +10%/+15%/+20% atk+move speed buff by tier. Lingers 10s after bannerman dies. Buff stacking rules apply (same tier refreshes, higher overwrites, lower ignored).
- Upgrade ×10: +1% to all tiers. Cost doubles from 15.

---

### Academy

**Student Line — My Notes!**
- Primary: On death, grants XP to nearby friendly units (not self). 1XP/120px → 2XP/160px → 3XP/200px by tier.
- Upgrade ×5: +1 XP on death. Custom cost curve: 30/60/240/480/960 Sapphires (not doubling formula).
- No unlock.

**Alchemist Line — Corrode**
- Primary: On hit applies Corrode stack for 5s. -1 armor +10% dmg taken / -2 armor +20% / -3 armor +30% by tier.
- Upgrade ×10: +2% bonus damage taken all tiers. Cost doubles from 15. Rubies.
- Unlock "Mega Flasks": Flasks 50% larger, explode on impact, 120px AoE damage + Corrode. Cost: 240 Rubies.

**Archivist Line — Recorded Tactics**
- Primary: Aura. Nearby allies +10% XP. 120px/160px/200px radius by tier. Does not apply to self.
- Upgrade ×10: +5px radius AND +1% XP per upgrade. Cost doubles from 15.
- Unlock "Into the Histories!": Req level 3. If ally dies in range, all other affected allies gain +10% stacking damage until battle ends. Cost: 540 Sapphires.

**Apprentice Mage Line — Shared Insight**
- Primary: On hit applies stacking magic damage vulnerability. Stacks infinitely. Timer refreshes on hit. ALL stacks drop if 5s passes without a hit. +10%/+20%/+30% magic dmg taken by tier.
- Upgrade ×10: +1% magic vulnerability AND +1s duration. Cost doubles from 15. Rubies.
- No unlock. Balance note: monitor for stack cap (suggest 3-unit max if OP).

---

### Barracks

**Building Ability — Bunk Beds**
- On unit death: unit can respawn at Barracks. Respawns per purchase: 1/2/3/4/5 (5 max purchases).
- Cost: 150 Stone + 150 Steel + 150 Wood per purchase (doubles each time).
- Required by: Militia "Unwashed Masses" unlock.

**Spearman Line — Phalanx Thrust**
- Primary: Every 3s deals +3 damage ignoring armor. Hits all units in line in front. 120px/160px/200px line by tier.
- Upgrade ×10: +5% line width (multiplicative stacking). Cost doubles from 15. Steel.
- Unlock "Almighty Push": 300% Knockback Power (permanent passive). Cost: 240 Steel + 240 Sapphires.

**Footman Line — Press the Line!**
- Primary: On first combat entry (once): +10% Parry, +10% Block, +10% Knockback Chance, +10% Crit, +10% Dmg Reduction for 2s/3s/4s by tier.
- Upgrade ×10: +1s duration. Cost doubles from 15. Steel.
- Unlock "I Believe!": Permanent: +10% Parry, +10% Block, +2× Knockback Power. Stacks with Press the Line! Cost: 240 Steel + 240 Emeralds.

**Scout Line — Over Here!**
- Primary: Taunts nearest ranged unit at 300% Scout range for 3s/4s/5s. Taunted unit gains +50% range and -50° accuracy. Scout moves perpendicular to taunt angle 1s then fires at target. While on cooldown: Scout fire rate ×2, accuracy -120°. CD 30s all tiers.
- Upgrade ×10: -1.5s cooldown. Cost doubles from 15. Wood.
- No unlock (ability already complex).

**Militia Line — Massed Ranks**
- Primary: Passive. +10% bonus damage per friendly Barracks unit alive. Cap: 4 units (+40% max base).
- Upgrade ×10: +1% per Barracks unit per upgrade. Cost 500 Steel + 500 Sapphires (doubles).
- Unlock "Unwashed Masses": On death spawns (unit level - 1) units of same type at same level. Spawned units also have this ability. REQUIRES Bunk Beds. WARNING: exponential spawn potential — intentional meme mechanic at high levels. Cost: 120 Steel + 120 Sapphires + 120 Stone + 120 Emeralds.

---

### Beast Kennel

**Pup Line — Take Down**
- Primary: Every CD seconds stuns enemy 3s (cannot attack/flee/use abilities). Drags enemy backwards at Pup move speed for 3s. CD 30s/25s/20s by tier.
- Upgrade ×10: -1s cooldown. Cost doubles from 15. Sapphires.
- Unlock "Go for the Throat!": Req level 3. During Take Down: if enemy at 20% HP or below, enemy is killed when stun ends. Cost: 540 Steel + 240 Sapphires.

**Falconer Line — Fast & Featherous**
- Primary: Summons a bird (flying melee, no unit collision) that attacks Falconer's target. If bird dies, resummon cooldown applies. T2 name: "2 Fast 2 Featherous".
- Upgrade ×10: +Bird HP and +Bird Damage. Cost doubles from 15.
- ⚠️ STAT REDESIGN NEEDED — see Stat Review below. Handler = Ranged Support/Summoner. Bird stats TBD.

**Beast Handler Line — Unauthorized Menagerie**
- Primary: Summons ground animals. T1: 2 Badgers. T2: 3 Snow Wolves. T3: 4 Lions. Dead summons resummoned one at a time on cooldown.
- Upgrade ×10: +Summon HP and +Summon Damage (all summon types). Cost doubles from 15.
- No unlock.
- ⚠️ STAT REDESIGN NEEDED — each animal type needs own stat block. Awaiting Morgan input.

**Horned Cub Line — Charge!**
- Primary: On line of sight charges target, gaining +10px speed every 0.25s. On collision: deals 1×/2×/3× move speed as damage by tier. Both units stunned 5s. 100% Knockback Chance, 2× Knockback Power.
- Upgrade ×10: +10px base speed. Cost doubles from 15. Steel.
- Unlock "It's Too Big!": Charge collision causes 110px AoE damage. Survivors slowed 50% for 5s. Cost: 250 Steel + 250 Stone + 500 Sapphires. ⚠️ Cost marked TBD in backlog — this is the working number.

---

### Chapel

**Acolyte Line — Mend**
- Primary: Heals lowest HP friendly unit in range for 4/8/12 HP/s at 140/170/200px range by tier. Repositioning logic (shared with Hymnist): 1) Heal lowest HP unit in range. 2) If no damaged unit in range: walk 300px from nearest enemy, retarget. 3) If no damaged unit on field: follow nearest Tank/Melee/Ranged 80px behind. 4) If no allies remain: flee to spawn.
- Upgrade ×8: +5px range AND +0.25 HP/s AND -5% CD per purchase. Cost doubles from 15. Emeralds.
- Unlock "Anointed Mend": Req level 3. Mend also heals all allies within 60px of primary target for 25% of Mend's healing value (after all buffs). Cost: 1000 Emeralds.

**Lightbringer Line — Battle Buddy**
- Primary: Picks nearest melee unit within 100px. Applies "Battle Buddy" buff 60s: buddy receives 25% of Lightbringer's damage as healing, buddy's parry +20%. Reselects if buddy dies.
- Upgrade ×10: +3% healing AND +3% parry per purchase. Cost doubles from 15. Emeralds.
- Unlock "Blessed Weapons": Req level 3. Permanent: Lightbringer AND current Battle Buddy both gain +4 damage and all attacks become Magic type (ignores armor). Updates when buddy changes. Cost: 840 Emeralds.
- ⚠️ Lightbringer base stats suspect — see Chapel Stat Review below.

**Penitent Line — Holy Onslaught**
- Primary: Passive 1/2/3 HP/s self-heal by tier. Bonus damage = % missing HP (50% missing = +50% damage, scales to 99%). Absorbs 20% of all damage dealt to friendlies within 60px onto self.
- Upgrade ×10: -5% heal cooldown. Cost doubles from 15. Emeralds.
- Unlock "Glorious Purpose!": Req level 3. Once per battle: when this unit would die it becomes fully invulnerable to ALL damage, spells, effects, abilities for 10 seconds. Cost: 1200 Emeralds.

**Hymnist Line — Hymn of Solace**
- Primary: Every 2s heals ALL friendly units within range for 2/4/6 HP at 140px AoE by tier (AoE radius not listed as scaling — same repositioning logic as Mend).
- Upgrade ×10: +5px range AND +0.25 HP per heal. Cost doubles from 15. Emeralds.
- Unlock "Grand Chorus": Req level 3. All friendly units within Hymn range gain +2 damage and heal 1 HP on every attack. Range scales with Hymn of Solace upgrades. Cost: 1500 Emeralds.

---

## CHAPEL STAT REVIEW

Lightbringer line stats are suspect — attack_area on Melee Single makes no sense. healing_other field is legacy placeholder (superseded by ability system).

| Unit | HP | Armor | Dmg | Atk Spd | Range | Area | Move | Status |
|---|---|---|---|---|---|---|---|---|
| Acolyte | 22 | 0 | 1 | 1.8s | 140 | 0 | 110 | ✅ Reasonable |
| Priest | 28 | 1 | 1 | 2.0s | 160 | 0 | 105 | ✅ Reasonable |
| High Cleric | 36 | 2 | 2 | 2.2s | 180 | 0 | 100 | ✅ Reasonable |
| Lightbringer | 55 | 2 | 3 | 1.9s | 50 | 90 | 95 | ⚠️ attack_area:90 on Melee Single — wrong |
| Sun Knight | 70 | 3 | 4 | 2.0s | 55 | 110 | 90 | ⚠️ Same issue |
| Dawn Paladin | 85 | 4 | 5 | 2.1s | 55 | 130 | 85 | ⚠️ Same issue |
| Penitent | 40 | 1 | 6 | 1.3s | 50 | 90 | 120 | ✅ Fast melee DPS |
| Zealot | 50 | 2 | 8 | 1.4s | 50 | 110 | 115 | ✅ Good scaling |
| Crusader | 60 | 3 | 11 | 1.5s | 55 | 130 | 110 | ✅ Solid T3 |
| Hymnist | 18 | 0 | 0 | 2.4s | 170 | 120 | 105 | ✅ Pure healer |
| Cantor | 24 | 0 | 0 | 2.6s | 190 | 140 | 100 | ✅ Scaling right |
| Choirmaster | 30 | 1 | 0 | 2.8s | 210 | 160 | 95 | ✅ Scaling right |

**Awaiting input:** Lightbringer line proposed stat corrections. Penitent intended as DPS or DPS/healer hybrid?

---

## BEAST KENNEL STAT REDESIGN (Awaiting Input)

Falconer/Skywatcher stats are identical to Beast Handler/Packmaster (copy-paste error). Storm Talon wrongly set as Magic AOE. All 6 handler units + all summon stat blocks need redesign.

**Proposed structure (needs Morgan's approval on numbers):**
- Falconer/Skywatcher/Storm Talon: Ranged Support/Summoner type. Decent HP, low/no personal damage, medium move speed.
- Bird (T1): Flying melee, no collision. Fast. Own stat block.
- Bird (T2/T3): Flying Melee Magic (ignores armor, still gets parry chance).
- Beast Handler/Packmaster/Wildcaller: Same structure as Falconer but ground summons.
- Badger (T1 summon), Snow Wolf (T2 summon), Lion (T3 summon): each needs own stat block.

---

## SYSTEMS & BACKLOG (Confirmed)

### NOW — Immediate Actions
- **Army Training 'IRON' label**: Change display label to STEEL. Variable is already `global.steel`.
- **Horned Cub "It's Too Big!" cost**: 250 Steel + 250 Stone + 500 Sapphires (working number — confirm).

### CONFIRMED SYSTEMS
- **Ability upgrade cost formula**: Base 15, doubles each purchase, 10 max. Full curve: 15/30/60/120/240/480/960/1920/3840/7680.
- **Unlock ability rules**: One-time purchase. No upgrades. Activates second cooldown ability. Separate cooldown from primary.
- **Resource assignment**: Steel=Melee, Stone=Buildings/Global, Wood=Ranged, Rubies=Magic Damage, Sapphires=Buffs/Support, Emeralds=Healing.
- **Buff stacking**: Same tier refreshes. Higher tier overwrites. Lower tier ignored (does not refresh). On higher expiry, lower can apply.
- **Magic damage ignores armor**: CONFIRMED. Must be enforced in combat damage calculation.
- **Global ranged inaccuracy**: 20° cone (±10°) baseline for ALL ranged units. Scout idle: -120° additional.
- **Knockback**: Power = push distance in px (0.5× melee dmg or 0.25× ranged/magic dmg). Chance = % odds of trigger. Direction = opposite attack angle.
- **Healer repositioning logic**: Shared by Mend and Hymn of Solace — 4-step priority (heal in range → walk from enemy → follow tank/melee/ranged → flee to spawn).
- **healing_other field**: Legacy placeholder superseded by ability system. Zero out or ignore once abilities implemented.
- **Blessed Weapons dynamic update**: When Lightbringer's buddy changes, +4 dmg and Magic type must transfer to new buddy.
- **Holy Onslaught absorption**: Penitent absorbs 20% of damage dealt to friendlies within 60px onto itself. Intentional — synergizes with Glorious Purpose.
- **Flying Melee Magic tag rule**: Bird T2/T3 = Melee tag (gets parry chance) + Magic tag (ignores armor). Both apply simultaneously.

### LATER — Blocked Until Abilities Phase Complete
- Unit type audit (many incorrectly tagged as Melee)
- Global parry/block/knockback system implementation
- Tank tag addition (Shield Bearer, Footman, Lightbringer, Sproutling, Stonekin lines — 30 files)
- Global tank block chance (value TBD)
- Accuracy stat — add as per-unit field during unit type audit
- Shared Insight stack cap monitoring
- Full tag system overhaul — all units need retagging
- Enemy spawn bucket system (currently all 'core' = broken parity purchasing)
- Falconer & Beast Handler full stat redesign + summon system
- Flying unit system (no ground collision)

---

## TAG SYSTEM (Authoritative — from Tag System sheet)

All tags follow format: `spawn_eligibility, spawn_bucket, object_category, role, attack_distance, damage_type, damage_area`

**Type 1 — Spawn Eligibility:** `player_spawnable`, `enemy_spawnable`, `not_spawnable`

**Type 1.1 — Spawn Buckets (CURRENTLY BROKEN — all units = core1):**
`core1` through `core10` (weakest to strongest), `utility1`/`utility2`/`utility3`, `spice`

**Type 2 — Object Category:** `Building`, `Unit`, `Tower`, `Terrain`, `Summoned`, `Corrupted`

**Type 3 — Role:** `DPS`, `Tank`, `Healer`, `Support`, `Siege`, `Summoner`, `Flying`

**Type 4 — Attack Distance:** `Melee`, `Ranged`, `Multi`

**Type 4.1 — Damage Type:** `Physical` (affected by armor), `Magic` (ignores armor — CONFIRMED)

**Type 4.2 — Damage Area:** `Single`, `Cleave`, `AOE`, `Puddle` (persistent ground effect — new type)

**Example — Falconer:**
`player_spawnable, enemy_spawnable, core2, Unit, Summoner, Ranged, Physical, Single`

**Example — Bird T2/T3:**
`not_spawnable, core2, Summoned, Flying, Melee, Magic, Single`


---

# BUILDING: GROVE
**Tier:** 3 | **Prerequisites:** Enclave | **Levels:** 3
**Theme:** Nature, growth, poison, sustain. Hard-counters Corrupted Barracks T3 units — this relationship must be preserved during balance.

**Confirmed unit names (from unit_lines_audit_2.xlsx):**
- Slot 1: Sproutling / Vinekin / Thorn Warden
- Slot 2: Dryad / Grovekeeper / Sylvan Matron
- Slot 3: Acorn Scout / Barkstrider / Forest Sentinel
- Slot 4: Seed Sower / Bloomcaller / Verdant Sage

---

## Slot 1 — Sproutling Line

**Ability: Growing Pains**
Every 3 seconds, taunts all enemy units within **200px**, forcing them to target the Sproutling. Every attack made against the Sproutling deals **5/10/15 damage** back to the attacker (T1/T2/T3) and heals the Sproutling for **4/8/12 HP**.

- **Upgrade ×10:** +1 reflect damage AND +1 heal per upgrade. Cost doubles from 15. Stone (tank line).
- ⚠️ Stats note: Sproutling should have ~30% more HP and ~10% less damage than the Earth Elemental (Pebblekin line). Current stats suspected wrong — verify against Pebblekin baseline before implementing.
- Tank tag applies to this line (see backlog — add `Tank` tag to Sproutling line, 3 player + 3 corrupted files).

**Unlock: Rampant Regrowth**
Cost: 200 Sapphires + 800 Emeralds
This unit heals itself for **2/3/4 HP every 0.5 seconds** (T1/T2/T3).

---

## Slot 2 — Dryad Line

**Ability: Destructive Nature**
60-second cooldown. Creates a **poison puddle** (80px radius) at target location. Units that walk into the puddle receive the debuff **"Return to Earth"**: they take **5/10/15 damage per second** (T1/T2/T3) for 10 seconds. "Return to Earth" **spreads to other units of the same faction on touch** — when a unit receives the debuff from another unit, a **new Destructive Nature puddle** is created at the infected unit's location.

- **Upgrade ×10:** +1 damage/sec to Return to Earth. Cost doubles from 15. Wood (ranged/nature line).
- ⚠️ **Max puddle cap required**: chain spread can tile the entire field if uncapped. Suggested cap: **4–6 active puddles** from a single Dryad at once. Oldest puddle despawns when cap is exceeded.

**Unlock: The Planet Feeds**
Cost: 500 Wood + 500 Sapphires
Units that die while under the effect of "Return to Earth" **also spawn a Destructive Nature puddle** at their death location.

---

## Slot 3 — Acorn Scout Line

**Ability: In the Shade**
60-second cooldown. Using the **mortar/arc system** (same as Heavy Crossbow Tower flying projectile system): creates an **accuracy circle** (250px diameter) centered on a target unit. The Acorn Scout fires a **high arc volley** of **10/15/20 arrows** (T1/T2/T3) at a rate of one every 0.2 seconds. 2 seconds after the first arrow is fired, arrows begin landing on their target points, creating **4px damage boxes** on impact. Landing points are checked in order of creation. Each arrow deals **15/20/25 damage** if it hits (T1/T2/T3).

- **Upgrade ×10:** +1 arrow per volley. Cost doubles from 15. Wood.
- Implementation note: Uses the planned flying projectile arc system. Damage boxes are small (4px) — the spread within the 250px accuracy circle is what makes the ability feel like a rain of arrows rather than a precision shot.

**Unlock: Poison Arrows**
Cost: 1000 Wood
Units hit by an arrow receive a **stacking "Poison Arrow" debuff** that deals **1 damage per second for 10 seconds** per stack.

---

## Slot 4 — Seed Sower Line

**Ability: Meadow of Tranquility**
2-minute cooldown. Creates a large **400px radius meadow puddle**. Friendly units inside receive:
- **1/2/3 HP per second** (T1/T2/T3)
- **Immunity to all negative status effects and debuffs**
- **Removal of all current negative status effects and debuffs** on entry

Lasts **30 seconds**.

- **Upgrade ×10:** -2 seconds cooldown AND +2 seconds duration per upgrade. Cost doubles from 15. Emeralds (healing line).

**Unlock: New Bloom**
Cost: 500 Wood + 500 Emeralds
If a friendly unit spends **10 continuous seconds** inside the Meadow, they are healed for a burst of **20/30/40 HP** (T1/T2/T3).


---

# BUILDING: MAGIC SCHOOL
**Tier:** 2 | **Prerequisites:** Academy | **Levels:** 3
**Theme:** Academic magic — four distinct elemental/arcane schools. Each line has a mechanically distinct ability with no thematic overlap between lines.

**Confirmed unit names (from unit_lines_audit_2.xlsx):**
- Slot 1: Spark Adept / Lightning Savant / Stormcaller Archon
- Slot 2: Rune Novice / Glyph Weaver / Arcane Sigilmaster
- Slot 3: Frost Acolyte / Icebinder / Cryomancer
- Slot 4: Emberling / Flame Adept / Pyromancer

---

## Slot 1 — Spark Adept Line

**Ability: Sudden Pop**
Every normal attack by the Spark Adept line applies a stacking, renewing debuff called **"Charged"** to the target. The debuff timer resets to **3 seconds** on every new stack applied. Charges accumulate at **10/15/24 charges per attack** (T1/T2/T3). When the target reaches **100 Charged stacks**, the unit **"Pops"** — dealing **80 damage** in an **80px AoE** centered on the target. The Charged debuff is then consumed and resets to 0.

**Charges to Pop by tier (base):**
- T1 (10/attack): 10 hits to pop
- T2 (15/attack): 7 hits to pop
- T3 (24/attack): 5 hits to pop

**Upgrade ×10:** +1 charge per attack added to all tiers. Cost doubles from 15. Rubies (magic damage line).

**Charges to Pop at max upgrade (+10/attack):**
- T1 (20/attack): 5 hits to pop
- T2 (25/attack): 4 hits to pop
- T3 (34/attack): 3 hits to pop

**No unlock.**

**Design Notes:**
- "Charged" timer refreshes on every hit — a single Spark Adept maintaining sustained fire will reliably pop targets. Multiple Spark Adepts hitting the same target stack charges faster.
- 80px AoE on pop punishes enemy units clumping near a charged target.
- The charge rate escalation across tiers is intentional and upgrade-aware — T3 at max upgrades pops in 3 hits, which is aggressive but gated behind significant investment.
- Charged debuff is faction-agnostic on the target — any unit can be charged regardless of type.

---

## Slot 2 — Rune Novice Line

**Ability: Runic Mine**
Every **3 seconds**, the Rune Novice places a **ground rune** (80px radius) at a **random location within 300px** of the unit. Runes last **30 seconds**. When an enemy unit steps on a rune, the rune **explodes** dealing **50/60/70 damage** (T1/T2/T3) to the triggering unit.

**Placement rules:**
- New runes must be placed at least **40px from any existing rune center** (max 50% overlap on 80px diameter runes)
- If no valid non-overlapping position is found within 300px after a few placement attempts, that cycle is skipped
- Up to ~10 runes can exist simultaneously at base duration before the oldest begin expiring

**Upgrade ×10:** +3 seconds to rune duration per upgrade (30s → 60s at max). Cost doubles from 15. Rubies.

**No unlock.**

**Design Notes:**
- 50% overlap cap keeps the battlefield readable — runes at 80px diameter need 40px clearance between centers.
- At max upgrade (60s duration, 3s placement rate) a single Rune Novice can maintain up to ~20 runes simultaneously — significant area denial.
- Runes are placed randomly within range, not player-targeted. Creates unpredictable mine fields rather than precise traps.
- Future consideration: a targeting upgrade that biases placement toward enemy units or high-traffic paths.

---

## Slot 3 — Frost Acolyte Line

**Ability: Frost Wave**
Creates a **200px wide, 10px thick wall of frost** that travels **200px at 100px/second** (2 second travel time). Units hit by the wall take **30/40/50 damage** (T1/T2/T3) and have their **movement speed and attack speed reduced by 20% for 10 seconds**. Cooldown: **20 seconds**.

- **Upgrade ×10:** +1 damage AND +1% additional movement/attack speed reduction per upgrade. Cost doubles from 15. Sapphires (slow/debuff line).

**Design Notes:**
- 100px/s travel speed means units locked in melee combat will almost never dodge it — the animation is the point. Pun intended and logged.
- 200px wide wall covers a significant cross-section of the battlefield.
- 20s cooldown gives breathing room — this will likely be tuned in playtesting.
- At max upgrade: +10 damage (40/50/60 total) and +10% reduction (30% movement/attack speed reduction for 10s).

**Unlock: Deep Freeze**
Cost: TBD (Sapphires)
Extends the duration of all Frost Wave effects by **5 seconds** (10s → 15s). Additionally, units that receive the Frost Wave damage are **stunned for 3 seconds** (cannot attack, move, or use abilities).

**Design Notes:**
- Stun is applied on hit, before the slow takes effect — the 3s stun is the immediate impact, the 15s slow is the lingering consequence.
- Deep Freeze makes Frost Wave a significant crowd control tool rather than a pure slow.

---

## Slot 4 — Emberling Line

**Ability: Great Ball of Fire**
Once every **60 seconds**, the Emberling targets a unit within normal casting range and begins a **6 second charge-up**. The charge only initiates if a valid target is within range. Once launched, the **Great Ball of Fire** is a **homing missile** with **infinite effective range** — it travels at **50px/second** and will chase its target indefinitely regardless of distance until it connects. When the target is hit, it **explodes in a 400px AoE** dealing **40/50/60 damage** (T1/T2/T3) to all enemy units in range.

- **Upgrade ×10:** +2 damage to the explosion per upgrade. Cost doubles from 15. Rubies (magic damage line).

**No unlock.**

**Design Notes:**
- 6 second charge-up is a readable telegraph — the Emberling is visibly winding up, giving opponents a window of awareness even if they can't react.
- "Infinite range homing" means a fleeing unit will still be hit eventually. This is intentional and also funny.
- 400px AoE is enormous — the largest AoE in the game outside of Ritual Tower. Positioning the target unit near a cluster maximizes value.
- 50px/s travel speed means a unit 400px away takes 8 seconds to hit after launch. For very distant targets the ball is visually present on screen for a long time — dramatic.
- At max upgrade: +20 damage (60/70/80 total explosion damage).
- Cooldown is 60s flat — no upgrade path on cooldown reduction. The ability is already very powerful; upgrades only touch damage.


---

# BUILDING: MONSTER LAIR
**Tier:** 3 | **Prerequisites:** Barracks + Beast Kennel | **Levels:** 3
**Theme:** Kobolds worship dragons — a tiny dragon cult building. The Hatchling is the dragon-god, Kobolds are the devoted servants, Cave Imps are the sneaky monsters in the dark, and the Slime is the terrifying goo monster. All D&D tropes.

**Confirmed unit names (from unit_lines_audit_2.xlsx):**
- Slot 1: Hatchling / Drake / Sky Serpent
- Slot 2: Kobold / Kobold Warrior / Kobold Chieftain
- Slot 3: Cave Imp / Tunnel Fiend / Abyssal Howler
- Slot 4: Slime / Ooze / Gelatinous Monarch

---

## Slot 1 — Hatchling Line

**Unit Type:** Flying, Ranged, Magic damage
⚠️ **STAT REDESIGN NEEDED** — Hatchling line is almost certainly broken in game data. Needs full stat block as flying ranged magic unit. Flag for stat audit pass alongside Falconer/Beast Handler redesign.

**Ability: Chromatic Scales**
Passive. This unit takes **40/50/60% less magic damage** (T1/T2/T3).

- **Upgrade ×10:** +2% magic damage reduction per upgrade. Cost doubles from 15. Rubies.
- Max upgrade: 60/70/80% magic damage reduction.

**No unlock.**

**Design Notes:**
- Chromatic Scales creates a hard magic-resistant flying unit — very tanky against magic, but fully exposed to physical damage. Clean vulnerability/resistance identity.
- Pairs thematically and mechanically with Kobold "Glorious Scales" unlock — Kobolds near Hatchlings also gain magic resistance, reinforcing the dragon-worship fantasy.
- At max upgrade an T3 Sky Serpent takes only 20% of incoming magic damage. Physical units are its hard counter.

---

## Slot 2 — Kobold Line

**Ability: Protect the Master!**
Passive aura. If **any Hatchling line unit** is within **300px**, this unit gains:
- +3 Armor
- +20% Parry
- +20% Block
- +20% Attack damage
- +20% Attack speed

- **Upgrade ×10:** +1 to each bonus per upgrade (so at max: +13 Armor, +30% Parry, +30% Block, +30% Attack, +30% Attack speed). Cost doubles from 15. Sapphires (buff/support line).

**Design Notes:**
- Bonus applies if ANY Hatchling line unit is within 300px — not unit-specific. Multiple Kobolds near one Hatchling all receive the buff simultaneously.
- ⚠️ Balance note: stacking multiple Kobolds around a single Hatchling is the intended win condition for this building but monitor — at max upgrades a cluster of Kobolds near a T3 Sky Serpent becomes extremely dangerous. The Hatchling's physical vulnerability is the counterplay.
- The fantasy is clear: the Kobolds fight dramatically harder when their dragon-god is watching.

**Unlock: Glorious Scales**
Cost: TBD (Sapphires + Rubies — thematic mix)
Kobolds permanently take **20/25/30% less magic damage** (T1/T2/T3). This reduction is **increased by 50%** when a Hatchling line unit is within 300px.

**Design Notes:**
- Base: 20/25/30% magic resistance at all times.
- Near Hatchling: 30/37.5/45% magic resistance (base × 1.5).
- Reinforces the dragon-cult theme — the Hatchling's presence literally makes the Kobolds more dragonlike.
- Combined with Protect the Master! bonuses, a Kobold near its Hatchling is a significantly different unit than one fighting alone.

---

## Slot 3 — Cave Imp Line

**Ability: Something in the Dark**
Passive on-hit. When a Cave Imp line unit hits a target with a **melee attack**, the target receives the **"Dark Hallucinations"** debuff. The target's **next attack has a 50% chance to miss entirely** — the unit swings at a hallucination instead of its actual target.

- **Upgrade ×10:** TBD — suggest +2% miss chance per upgrade (50% → 70% at max). Cost doubles from 15. Sapphires.
- No direct damage component. Pure debuff utility.

**Design Notes:**
- "Miss" means the attack animation plays but deals 0 damage and applies no on-hit effects — the unit attacked a hallucination.
- One debuff application per melee hit. Debuff is consumed on the target's next attack whether it misses or not.
- Multiple Cave Imps hitting the same target can stack the debuff — each application is a separate 50% miss check on a separate upcoming attack.
- The visual sell: unit winds up to attack, swings confidently at empty air. Readable and funny.

**Unlock: Safety in Numbers** *(Conditional — requires cohesion/morale system)*
Cost: 500 Stone + 500 Sapphires
This unit **counts as 2/3/4 units** (T1/T2/T3) for flee and morale calculations.

**Design Notes:**
- ⚠️ This unlock is flagged as **pending cohesion system implementation**. The morale/flee system is a "someday" feature — this unlock should be built but gated until the system exists.
- Intent: a single Cave Imp makes the army feel less outnumbered than it actually is, delaying flee thresholds. A T3 Abyssal Howler counts as 4 units for morale purposes — a small squad with a few of them feels much larger to the flee calculation.

---

## Slot 4 — Slime Line

**Unit Type:** Melee, Physical, Tank
**Visual:** Units swallowed by the Slime are carried **inside the model** — visually present but trapped within the goo. Magic damage applied to the Slime frees swallowed units. Physical damage bounces off.

**Ability: It's All Just Goo**
Passive. This unit receives **60/70/80% less physical damage** (T1/T2/T3).

Every time an enemy unit hits this unit in melee, that unit receives **1/1.5/2 stacks** of the **"Sticky Goo"** debuff (T1/T2/T3). If a unit accumulates **10 stacks** of Sticky Goo, it is **"Eaten"** — removed from the field and carried inside the Slime model until the Slime is killed.

**Swallowed unit rules:**
- Swallowed units cannot act, cannot be targeted, and cannot be freed except by killing the Slime or dealing magic damage to it
- **Magic damage** applied to the Slime frees all currently swallowed units (they are released at the Slime's location)
- Maximum swallowed units: **1/2/3** (T1/T2/T3 — Slime / Ooze / Gelatinous Monarch)
- If the round ends and the Slime is **not killed**: all swallowed units die, and the Slime gains **50% bonus XP**
- If the Slime **is killed** mid-round: swallowed units are released and return to the fight

- **Upgrade ×10:** +1% physical damage reduction per upgrade (60/70/80% → 70/80/90% at max). Cost doubles from 15. Stone (tank line).

**No unlock.**

**Design Notes:**
- The clear counterplay is deliberate and readable: **throw magic at it to free your people**. Physical damage is nearly useless. This creates genuine decision-making — do you pivot to magic units mid-round?
- The Gelatinous Monarch (T3) eating up to 3 units simultaneously while shrugging off physical damage is the intended terrifying set piece. It should feel like a crisis.
- Sticky Goo stacks accumulate from any melee hit — fast-attacking melee units reach 10 stacks much faster than slow heavy hitters. A Scout or Pup that charges in will get eaten quickly.
- 50% XP bonus on round-end with swallowed units creates an interesting dynamic — a Slime that survives with prisoners becomes a bigger threat next round.
- At T1 (1.0 stack/hit, cap 1): a unit gets eaten after 10 melee hits — slow enough to be avoidable with positioning.
- At T3 (2.0 stacks/hit, cap 3): a unit gets eaten after 5 melee hits — fast enough to feel dangerous.
- ⚠️ "Carrying units inside model" is a visual implementation note — the swallowed unit sprite should be visible but non-interactive inside the Slime's body.


---

# BUILDING: MOUNTAIN KEEP
**Tier:** 3 | **Prerequisites:** Barracks + Construction Yard | **Levels:** 3
**Theme:** All dwarves. Tough, underground, building-focused. Strong internal synergy — Mason heals towers that Tunnel Scout threatens, Stonekin tanks magic, Crystal Adept creates a field of healing objects that reward planning.

**Confirmed unit names (from unit_lines_audit_2.xlsx):**
- Slot 1: Stonekin / Rockguard / Obsidian Sentinel
- Slot 2: Tunnel Scout / Cavern Striker / Deep Delver
- Slot 3: Crystal Adept / Geomancer / Earth Shaper
- Slot 4: Mason / Architect / Bastion Master

---

## Slot 1 — Stonekin Line

**Ability: Prismatic Shield**
Passive. This unit takes **25% less physical and melee damage**. Additionally, has a **25% chance per hit to reflect magic projectiles and abilities** back toward the unit that fired them. Reflected projectiles/abilities are now treated as **friendly to the Stonekin's faction** — they may hit other enemy units along their return path.

- **Upgrade ×10:** +2% to all percentages per upgrade. Cost doubles from 15. Stone.
- Max upgrade: 45% physical/melee damage reduction, 45% reflect chance.

**Unlock: It's All Mine**
Cost: 500 Steel + 500 Stone
Enemies hit by this unit are **taunted for 2 seconds** and **cannot flee** during that duration.

**Design Notes:**
- Reflect applies per projectile individually, not per ability cast — a multi-projectile ability could have some projectiles reflected and others land.
- Reflected projectiles travel back at original speed toward the original caster. Friendly fire between enemies on the return path is intentional and creates chaotic moments.
- Pairs with the magic-heavy enemy compositions that would normally shred tank units — the reflect chance makes stacking magic against this unit risky.
- Tank tag applies. Add to tank block pass.

---

## Slot 2 — Tunnel Scout Line

**Unit Type:** Siege/DPS, Physical
**Visual:** While burrowed, a small dust cloud or dirt disturbance effect travels across the battlefield indicating position without making the unit targetable. Unit is fully **untargetable while underground**.

**Ability: Heh?! Walls?**
On spawn: unit **burrows underground** and pathfinds toward the **nearest enemy tower or building**. While burrowing, the unit is **untargetable**. As the Tunnel Scout passes beneath enemy units it leaves an **"Unusual Package"** that detonates **2 seconds later** dealing **20/30/40 damage** (T1/T2/T3) in a small AoE at that location.

When the Tunnel Scout reaches its target tower/building it **unburrows** and begins attacking it directly. After **4 seconds** of being at the target, it plants an **"Extra Special Package"** that detonates dealing **500 damage** to the tower/building, **ignoring all resistances and armor**.

- **Upgrade ×10:** +1 damage to Unusual Packages AND +50 damage to Extra Special Package per upgrade. Cost doubles from 15. Stone + Rubies.
- Max upgrade: 30/40/50 Unusual Package damage. 1000 Extra Special Package damage.

**Unlock: Extra Large Packages**
Cost: 500 Stone + 500 Rubies
Increases the **size and damage of all packages by 30%** — both Unusual Packages and the Extra Special Package.

**Design Notes:**
- Untargetable while burrowed — the dust cloud visual is essential to give the player awareness without giving counterplay until the Scout surfaces.
- The 4 second delay before Extra Special Package detonation gives a window to kill the Scout after it surfaces — if killed before detonation the package does not go off.
- Primary purpose: hard tower/building assassination. Secondary: chip damage to any units standing along the path.
- ⚠️ **Christmas note:** During Christmas event, replace Unusual Packages with presents, Extra Special Package with a giant gift box, and put a Santa outfit on the Tunnel Scout. Flagged for seasonal content pass.
- Internally countered by Mason line (same building) — a Mason healing a tower can offset or outlast the Extra Special Package if the Scout is killed in time.

---

## Slot 3 — Crystal Adept Line

**Ability: Prismatic Healing**
Every **30 seconds**, spawns a **Crystal** within 40px of the lowest health friendly unit currently in range. The Crystal is a **stationary summoned object** — it cannot move, cannot attack, and has **10/15/20 HP** (T1/T2/T3). It takes full damage from everything and dies in one hit from most units, two hits from weaker units.

While alive, the Crystal shines a **prismatic beam** on the lowest health friendly unit within its **500px range**, healing it for **10/15/20 HP every 2 seconds**.

- **Upgrade ×10:** +1 healing per heal AND -0.1s heal interval per upgrade. Cost doubles from 15. Emeralds.
- Max upgrade: 20/25/30 HP per heal, interval down to 1.0s (from 2.0s).

**Crystal rules:**
- Does not move after placement — stationary for its entire lifespan
- Retargets to lowest HP friendly unit in range if original target dies
- Does not despawn when its original target dies — lingers until killed
- Cannot be controlled or repositioned
- Tagged as: `not_spawnable, Summoned` — its own object category, not a Tower (yet — see future consideration below)

**Unlock: Crystal Chakra Gardening**
Cost: 2000 Emeralds
If a Crystal survives for longer than **20 seconds**, it **spawns a second lesser Crystal** nearby. The lesser Crystal heals for **5/10/15 HP** (T1/T2/T3) on the same interval as the parent. Maximum **2 lesser Crystals** per parent Crystal.

**Design Notes:**
- The late-game dynamic: if the enemy has no AoE, multiple Crystal Adepts produce a spreading prismatic healing lattice across the battlefield. Enemies that don't plan for it find their damage output increasingly negated.
- Crystals are intentionally fragile — any stray AoE, cleave, or splash damage will clear them. The power is conditional on surviving.
- ⚠️ **Future consideration:** Flag Crystals as `Tower` type so siege units (including Tunnel Scout) prioritize their destruction. Deferred — implement when siege targeting is revisited.
- The 2000 Emerald unlock cost reflects how powerful a self-propagating healing field becomes at scale.

---

## Slot 4 — Mason Line

**Ability: Boaghdar the Buildar**
*(Ability name is the unit's personality — the Mason is a dwarf who takes tower repair very seriously)*

The Mason targets and moves to the **nearest injured (damaged) player tower**. While adjacent to that tower the Mason **heals it for 50 HP per second**. When the tower is **fully healed**, the Mason moves to the **next nearest injured tower** and repeats. If **no towers are damaged**, the Mason idles at the last tower it healed, granting it a passive buff for as long as it remains there (see unlock).

**Triage loop:**
1. Find nearest injured player tower → move to it → heal at 50 HP/s
2. Tower reaches full HP → find next nearest injured tower
3. No injured towers → idle at last tower, apply unlock buff if purchased
4. New tower takes damage → immediately route to it

**Cannot be upgraded.** No upgrade path on healing value — the Mason is simple by design.

**Unlock: Improved Tower Management**
Cost: 3000 Stone
While the Mason is **idling at a tower** (no injured towers elsewhere), that tower gains:
- +10% damage
- +10% attack speed
- +10% cast speed

**Design Notes:**
- Primary purpose: tower sustain against siege units. Hard counter to Tunnel Scout (same building) if the Mason reaches the tower before the Extra Special Package detonates.
- Intentionally simple — no upgrades, no stack of numbers. Buy a Mason, your towers live longer. That's the whole pitch.
- Multiple Masons with multiple towers: each Mason runs its own independent triage loop. In theory 1 Mason + 5 towers = Mason running a circuit healing them all in sequence. Satisfying to watch.
- 3000 Stone unlock cost is high — the buff is minor (+10% on each stat) but permanent while the Mason is idle. It's a luxury purchase for players who've already stabilized their tower game.
- ⚠️ Mason only heals **player towers** — not enemy towers, not Crystals, not buildings.


---

# LEVEL DESIGN UNITS (Not Player Buildings)

These are units, buildings, and objects that exist in the world but are **not purchasable by the player**. They appear as map events, allied factions, environmental storytelling, or neutral objects on specific levels. They do not appear on the purchase screen or army builder.

## Outpost
Originally designed as a player building — cut. No other building or unit depends on it as a prerequisite.

**Repurposed as:** A level-specific allied faction event. On levels where an Outpost appears:
- The Outpost is pre-placed on the map, not built by the player
- Has a Watchman (guard unit) and possibly an Archer Tower defending it
- A Courier unit spawns and pathfinds toward the player's castle carrying a message or resource delivery
- Player has no control over any of it — it just happens around them
- Delivers world-building, flavor, and potentially a resource bonus or intel item when the Courier arrives

**Unit IDs that live here:** Lookout, Watchman, Sentinel Scout, Courier, Messenger, Swift Herald, Border Guard, Patrolman, Frontier Marshal, Signalman, Beacon Keeper, Skyflare Warden

## Other Likely Level Design Units (Placeholder — expand as needed)
The following are examples of units/objects that would fit this category. None are designed yet.

- **Unarmed Villagers** — civilians on village/town levels. No combat stats. Possibly a "protect the villagers" objective unit.
- **Farms / Fences** — environmental objects on rural levels. Destructible terrain or background flavor.
- **Merchant Cart** — neutral object that can be looted by either side for a gold bonus.
- **Captured Prisoner** — an ally unit that needs to be rescued before enemy units reach it.
- **Wandering Traveler** — neutral NPC that joins the player's side if kept alive long enough.

⚠️ None of the above are designed — just category placeholders. Expand this section as level design develops.


---

# BUILDING: SIEGE WORKSHOP
**Tier:** 2 | **Prerequisites:** Barracks + Construction Yard | **Levels:** 3
**Theme:** Pure siege. These units exist to kill towers and buildings. They are not standard combat units and behave as systems rather than ability-driven fighters. All dwarven engineering aesthetic.

**Confirmed unit names (from unit_lines_audit_2.xlsx):**
- Slot 1: Light Ram / Reinforced Ram / Thunder Ram
- Slot 2: Bombard Crew / Cannonade / Grand Bombard
- Slot 3: Iron Husk / Steel Golem / Titan Automaton
- Slot 4: Siege Engineer / War Mechanic / Doom Artificer

---

## Design Note — Siege Units as Systems

These four lines do not use the standard ability/upgrade/unlock format. They are **behavioral systems** — they have a job, they do it, and their stats scale by tier. This section documents their AI behavior and interaction rules rather than ability specs.

---

## Slot 1 — Light Ram Line

**System: Tower Ram**

The Ram has one objective: destroy the nearest enemy tower or building. It does not engage units by choice.

**Movement behavior:**
- On spawn: paths directly toward the nearest enemy tower or building
- If an enemy unit is in its path: the Ram **pushes them aside** (pure displacement, no damage) and continues without stopping
- The Ram cannot be redirected by taunt or fear effects — it ignores all crowd control that would change its target
- If the tower it is targeting is destroyed before it arrives, it re-evaluates and paths to the next nearest tower

**Attack behavior (Ram Swing cycle):**
- On reaching target: plays a **1 second wind-up animation**, then slams
- Each slam deals exactly **1/5 of the target tower's base HP** (ignoring all armor and resistances)
- After each slam: **1 second cooldown**, then wind-up begins again
- Full cycle per swing: 2 seconds (1s wind-up + 1s slam/cooldown)
- **5 swings kills a same-tier tower in approximately 10 seconds**
- Damage scales with tier — each tier's Ram is tuned against same-tier towers

**Damage values (1/5 tower base HP):**
- Light Ram (T1) vs Archer Tower T1 (700 HP): 140 per swing
- Reinforced Ram (T2) vs towers T2: proportional
- Thunder Ram (T3) vs towers T3: proportional

**Design Notes:**
- Push displacement has no damage component — units shoved aside are unharmed but repositioned
- Partial progress is intentional and meaningful — a Ram that lands 2 swings before dying has dealt 40% damage to a tower, which the Siege Engineer can preserve
- The Ram cannot be taunted, feared, or redirected — its only vulnerability is being killed before it reaches its target
- Hard counter: melee units that can kill it before it reaches the tower. Soft counter: healing the tower faster than the Ram damages it (Mason)

---

## Slot 2 — Bombard Crew Line

**System: Siege Ranged — Building Priority**

The Bombard Crew is a ranged siege unit that **strongly prefers towers and buildings** as targets but will engage units if no buildings are accessible.

**Targeting behavior:**
- **Priority check (every second):** Is any enemy tower or building within **400px**? If yes → target it, ignore all units
- If no tower/building within 400px → engage nearest enemy unit as normal ranged combat
- When a tower/building enters 400px range mid-combat: immediately drops unit target and switches to building
- Cannot be taunted off a building target while one is within 400px

**Design Notes:**
- 400px priority radius prevents the Bombard Crew from getting bogged down in unit combat when towers are reachable
- The 400px check runs continuously — a Bombard Crew fighting a unit will immediately pivot the moment a tower enters range
- Paired with Rams: Bombard softens towers at range while Rams close in for the kill
- Stats from xlsx exist — damage, range, attack speed carry over from existing data

---

## Slot 3 — Iron Husk Line

**System: Siege Melee — Building Priority**

The Iron Husk is a melee siege unit that behaves identically to the Bombard Crew in terms of building priority, but engages in melee rather than ranged.

**Targeting behavior:**
- **Priority check (every second):** Is any enemy tower or building within **400px**? If yes → path toward it and attack it, ignore all units
- If no tower/building within 400px → engage nearest enemy unit as normal melee combat
- When a tower/building enters 400px range: immediately drops unit target and moves toward building
- Pushes through units to reach buildings similarly to the Ram (but does deal normal melee damage to units it passes through unlike the Ram)

**Design Notes:**
- Unlike the Ram, the Iron Husk deals normal damage to units it encounters — it's a combat unit that happens to prefer buildings, not a pure siege vehicle
- The 400px priority radius is shared with Bombard Crew for consistency — both siege combat units use the same targeting logic
- At T3 (Titan Automaton) this is a heavy melee unit with significant HP — it will soak damage while walking to a tower

---

## Slot 4 — Siege Engineer Line

**System: Siege Unit Healer/Buffer — Dwarf**

The Siege Engineer is a dwarf support unit that follows siege units and keeps them alive. Thematically equivalent to the Mason (which heals towers) but for siege units instead.

**Healing behavior (triage loop):**
1. Find the most injured friendly **siege unit** (Ram, Bombard, Iron Husk) within range → move to it → heal at **X HP/s** (value TBD — suggest 30-40 HP/s, tune against Ram HP)
2. Target siege unit reaches full HP → find next most injured siege unit
3. No siege units are damaged → follow the nearest siege unit as a bodyguard at 40-60px distance
4. New siege unit takes damage → immediately route to it

**Upgrade:** The Siege Engineer **can** be upgraded — unlike the Mason which is intentionally simple, the Engineer has a combat role worth enhancing.
- **Upgrade ×10:** +X HP/s healing (value TBD). Cost doubles from 15. Emeralds.

**Unlock: Reinforced for Battle**
Cost: TBD (Steel + Emeralds)
When the Siege Engineer heals a siege unit to full HP, that unit gains a **temporary armor buff** (+X armor for Y seconds — values TBD). Rewards the "stack 2-3 Engineers on one Ram" playstyle by making a fully-healed Ram progressively tankier each time it's topped up.

**Design Notes:**
- Core fantasy: buy 2-3 Siege Engineers to escort a Ram, keep it alive through 5 full swings, kill the tower. The Engineers are what make the Ram a reliable threat rather than a kamikaze unit.
- Healing value should be tuned so that **1 Engineer approximately offsets the damage a Ram takes from one defending archer tower** — meaning 1 Engineer = survival, 2 Engineers = comfortable, 3 Engineers = the tower is probably doomed
- The bodyguard idle behavior means Engineers automatically reposition with the siege line rather than standing still
- ⚠️ Healing values (HP/s, upgrade increment, unlock armor values) are TBD — need tuning against actual Ram/Iron Husk HP stats from xlsx


---

# UNIT STAT FRAMEWORK (Authoritative)

## Core Philosophy
A unit IS the person/creature at all tiers — the Squire becomes the Knight becomes the Knight Captain. They get better gear and more experience, not a different body. Attack speed and move speed are personality traits of the unit, not progression stats. Fast things are fast their whole lives. Critters that grow up (drakes, cubs) are the exception and should be handled case by case.

## Scaling Multipliers (apply each tier, round UP to nearest whole number)
- **Damage:** ×1.7 per tier (T1 → T2 → T3)
- **HP:** ×1.9 per tier
- **Armor:** ×1.6 per tier
- **Attack Speed:** Does NOT scale — fixed for the unit line
- **Move Speed:** Does NOT scale — fixed for the unit line

## T1 Baseline Constraints
- **Max DPS at T1:** 15 (this is the HIGH end — reserved for single-target magic/DPS units)
- **HP range:** 30–75 (30 = fragile support, 75 = front-line tank)
- **Attack Speed range:** 0.7s (very fast) to 1.5s+ (slow heavy hitter)

## Armor Categories
| Category | Armor Value | Examples |
|---|---|---|
| None / Cloth | 0–1 | Healers, pure support, fragile casters |
| Light | 2–4 | Ranged units, scouts, fast melee |
| Medium | 3–6 | Standard melee, cavalry, mixed fighters |
| Heavy | 7–9 | Frontline tanks, heavy infantry |
| Shield Bearer | 10 | Shield Bearer line only — unique cap |

*Note: ranges overlap by design — a sturdy light unit and a lightly armored medium unit can share values.*

## DPS Reference by Role
| Role | Target DPS | Notes |
|---|---|---|
| Pure support / healer | 0–4 | Damage is irrelevant, may be 0 |
| Ranged support | 4–8 | Fires occasionally, not the point |
| Flying cavalry / buffer | 6–9 | Support unit that can fight |
| Standard ranged | 8–12 | Bread and butter ranged DPS |
| Standard melee | 8–13 | Bread and butter melee DPS |
| Heavy melee / tank DPS | 10–14 | Hits hard but slow |
| Single target magic / top DPS | 12–15 | Cap — reserve for dedicated damage dealers |

## Scaling Example — Pegasus Rider Line
| Tier | HP | Armor | Damage | Atk Speed | Move Speed | DPS |
|---|---|---|---|---|---|---|
| T1 Fledgling Rider | 62 | 4 | 10 | 1.2s | 120 | 8.3 |
| T2 Pegasus Rider | 118 | 7 | 17 | 1.2s | 120 | 14.2 |
| T3 Celestial Skymaster | 225 | 11 | 29 | 1.2s | 120 | 24.2 |

*Note: T3 DPS exceeds the T1 cap of 15 — this is expected and correct. The 15 DPS cap applies only to T1 baselines. T2 and T3 scale naturally from there.*

## ⚠️ Full Roster Audit Required
All existing units need their T1 stats verified against this framework and their T2/T3 recalculated using the multipliers. This is a post-abilities-phase task. Priority order: Castle → Barracks → Chapel → Academy → Beast Kennel → Elemental Portal → Enclave → Grand Cathedral → Grove → Magic School → Monster Lair → Mountain Keep → Siege Workshop → Rangers Lodge → Clockwork Skyport.

Known problem units already flagged:
- Falconer / Beast Handler lines — copy-paste error, full redesign needed
- Lightbringer line — attack_area:90 on Melee Single makes no sense
- Hatchling line — almost certainly broken, needs flying ranged magic stat block
- All tower units — currently tagged Melee, wrong
- Stonekin line — should be ~30% more HP, ~10% less damage than Pebblekin


---

# BUILDING: RANGERS LODGE
**Tier:** 3 | **Prerequisites:** TBD | **Levels:** 3
**Theme:** Aerial rangers and flying creatures. Specializes in air units, ranged physical damage, poison debuffs, and air-to-air combat. The only building with a dedicated flying support/healer unit.

**Confirmed unit names:**
- Slot 1: Stinger Drake / Venom Drake / Plague Drake
- Slot 2: Longbowman / Warbow / Master Fletcher
- Slot 3: Air Skirmisher / Sky Hunter / Drake Sniper
- Slot 4: Fledgling Rider / Pegasus Rider / Celestial Skymaster

---

## Slot 1 — Stinger Drake Line

**Unit Type:** Flying, Ranged, Physical damage + Poison debuff

**Stats (T1 baseline — flying unit, medium-light build):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Stinger Drake | 50 | 3 | 11 | 1.1s | 130 |
| T2 Venom Drake | 95 | 5 | 19 | 1.1s | 130 |
| T3 Plague Drake | 181 | 8 | 33 | 1.1s | 130 |

**Ability: Poisoned Spike**
10 second cooldown. Fires a ranged attack dealing normal damage. The target receives a **poison DoT** dealing **4/7/10 damage per second for 3 seconds** (T1/T2/T3). For the **duration of the DoT** the target also suffers:
- -25% Parry
- -25% Block
- -25% Attack Speed
- -25% Accuracy

- **Upgrade ×10:** +1% to ALL four debuff values per upgrade. Cost doubles from 15. Wood.
- Max upgrade: -35% Parry, -35% Block, -35% Attack Speed, -35% Accuracy for the DoT duration.

**Unlock: Deep Spikes**
Cost: 2000 Wood
Doubles the duration of Poisoned Spike — DoT and all debuffs last **6 seconds** instead of 3.

**Design Notes:**
- The debuff package is tied strictly to the DoT duration — when the poison expires, all four debuffs expire simultaneously.
- At max upgrade + Deep Spikes: 6 seconds of -35% to four stats is a significant combat debuff. Pairs well with heavy melee units that can exploit the reduced parry/block window.
- Flying unit — no ground collision, ignores melee unit positioning.

---

## Slot 2 — Longbowman Line

**Unit Type:** Ranged, Physical damage

**Stats (T1 baseline — ranger, light armor):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Longbowman | 40 | 2 | 12 | 1.3s | 110 |
| T2 Warbow | 76 | 4 | 21 | 1.3s | 110 |
| T3 Master Fletcher | 145 | 6 | 36 | 1.3s | 110 |

**Ability: Power Shots**
Once every 20 seconds: the Longbowman's **attack speed is reduced by 25%** and their **attack damage is doubled** for the next **3 attacks**. The attack speed reduction persists until all 3 Power Shots have been fired, then snaps back to normal immediately.

- **Upgrade ×10:** +1 additional Power Shot per upgrade. Cost doubles from 15. Wood.
- Max upgrade: 13 Power Shots per activation.

**No unlock.**

**Design Notes:**
- The attack speed penalty during Power Shots means each shot takes longer to fire but hits twice as hard. Net DPS during the window is slightly lower than normal but burst damage per shot is doubled.
- At max upgrade (13 shots at 25% reduced speed) the activation window is long — the Longbowman is in "Power Shot mode" for an extended period every 20 seconds.
- Clean ability — no unlock needed, the upgrade path tells the whole story.

---

## Slot 3 — Air Skirmisher Line

**Unit Type:** Ranged, Physical damage, Anti-air specialist

**Stats (T1 baseline — light, fast, specialist):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Air Skirmisher | 38 | 2 | 13 | 1.0s | 125 |
| T2 Sky Hunter | 73 | 4 | 22 | 1.0s | 125 |
| T3 Drake Sniper | 139 | 6 | 38 | 1.0s | 125 |

**Ability: Deploy Hunting Location**
The Air Skirmisher operates at **standard archer range** by default and attacks whatever is in range normally. When an enemy **air unit is detected within standard range**, the unit **deploys** (2 second animation, fully stops moving) and enters **Hunting Mode**:
- Range increases by **200%** (massive radius, effectively covering most of the battlefield)
- Damage increases by **20%**
- Hunting Mode persists until **no enemy air units remain within the expanded range**
- When all air targets leave range, unit un-deploys and returns to standard behavior

**No unlock.**

**Design Notes:**
- Standard range is normal archer range — the unit functions as a regular ranged attacker when no air threats are present.
- The 200% range increase on deploy means once an air unit is spotted the Skirmisher can track it across almost the entire battlefield.
- The 2 second deploy animation is a readable telegraph — the unit visibly commits before the range spike activates.
- Hunting Mode stays active as long as air targets remain in the expanded radius — it doesn't reset between individual kills.
- Hard counter to flying units. Soft against ground-only compositions since it functions as a normal archer in that case.

---

## Slot 4 — Fledgling Rider Line

**Unit Type:** Flying, Melee/Support, Magic buff aura

**Stats (T1 baseline — flying cavalry, medium armor, support-oriented damage):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Fledgling Rider | 62 | 4 | 10 | 1.2s | 120 |
| T2 Pegasus Rider | 118 | 7 | 17 | 1.2s | 120 |
| T3 Celestial Skymaster | 225 | 11 | 29 | 1.2s | 120 |

**Ability: Blessed Aura**
Passive aura. All friendly **air/flying units within 200px** receive:
- **25% damage reduction**
- **+25% movement speed**
- **1/2/3 HP per second healing** (T1/T2/T3)

- **Upgrade ×10:** +1% damage reduction AND +1% movement speed AND +0.5 HP/s healing per upgrade. Cost doubles from 15. Wood + Emeralds.
- Max upgrade: 35% damage reduction, +35% movement speed, 6/7/8 HP/s healing by tier.

**Unlock: Blessings of the Herd**
Cost: 2000 Wood + 2000 Emeralds
Flying units that remain within the Blessed Aura for **8 continuous seconds** receive a **burst heal of 100 HP**. This **re-triggers every 8 seconds** as long as the unit stays within the aura without leaving.

**Design Notes:**
- This is the **only flying healer in the game** — the 100 HP burst on the unlock is intentionally generous because anti-air units hit hard and flying units that survive engagements need to recover quickly before re-engaging.
- The aura only affects flying units — ground units standing beneath it receive no benefit.
- The 8 second re-trigger on the unlock rewards keeping your air units grouped around the Pegasus Rider between engagements. Units that dip in and out of the aura reset their 8 second timer.
- Paired with Air Skirmisher (same building): the Skirmisher hunts enemy air while the Pegasus Rider keeps your own air units alive. Strong internal building synergy.
- The Celestial Skymaster at max upgrades with unlock is a significant force multiplier for any air-heavy army — 35% damage reduction + healing on a flying cavalry unit that can also fight is the payoff for investing in the full line.


---

# BUILDING: CLOCKWORK SKYPORT
**Tier:** 3 | **Prerequisites:** Construction Yard + Siege Workshop | **Levels:** 3
**Theme:** Steampunk dwarven air force. The most technologically advanced building in the game. Rudimentary guns, blimps, homing rockets, and a comically large organ gun. Everything here is mechanical, loud, and dwarf-built.

**Confirmed unit names:**
- Slot 1: Clockwork Skyship / Dreadnought Mk II / Leviathan Mk III
- Slot 2: Clockwork Copter / Interceptor / Ace
- Slot 3: Dwarven Rifleman / Sharpshooter / Deadeye
- Slot 4: Volley Gunner / Rotary Volley Gunner / Storm Volley Gunner

---

## Slot 1 — Clockwork Skyship Line

**Unit Type:** Flying, Siege, Physical damage (bombing)
**Visual:** A large armored blimp. Heavily built, slow moving, visually imposing. Floats directly over its target to drop payload.

**Stats (T1 baseline — heavily armored siege platform, not a fighter):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Clockwork Skyship | 70 | 8 | — | — | 80 |
| T2 Dreadnought Mk II | 133 | 13 | — | — | 80 |
| T3 Leviathan Mk III | 253 | 21 | — | — | 80 |

*Damage is handled by the bombing system below, not standard attacks.*

**System: Bombing Run**
The Skyship has one objective: destroy enemy towers and buildings.

**Movement behavior:**
- Paths toward nearest enemy tower or building
- Floats **directly over the target** — positions itself above the building
- While hovering: takes full incoming damage but its heavy armor soaks most of it
- If target is destroyed: immediately re-evaluates and moves to next nearest tower/building
- **Building priority radius:** 400px (same as Bombard/Iron Husk) — will not be distracted by units

**Bomb drop behavior:**
- On reaching position directly above target: drops one **comically large bomb**
- Each bomb deals **exactly 1/2 of the target tower's base HP** (ignoring all armor and resistances) — tuned against same-tier Archer Tower
- **Bomb damage values (1/2 Archer Tower base HP by tier):**
  - T1 vs Archer Tower T1 (700 HP): 350 per bomb
  - T2 vs towers T2: proportional
  - T3 vs towers T3: proportional
- **2 bombs kills a same-tier Archer Tower**
- Cooldown between bombs: TBD — suggest 4-6 seconds (tune against survivability)

**Upgrade ×10:** +X bomb damage per upgrade. Cost doubles from 15. Stone + Steel.

**No unlock.**

**Design Notes:**
- The Skyship is the hardest single tower-killer in the game — 2 shots is faster than the Ram's 5 swings but the Skyship must physically float over the target making it vulnerable to anti-air (Clockwork Copter, Air Skirmisher, Drake Sniper)
- Its heavy armor means ground units struggle to kill it — you specifically want air units or the Copter to intercept it
- Intentional counterplay: at least one Copter + one Air Skirmisher/Drake Sniper to stop it from removing towers

---

## Slot 2 — Clockwork Copter Line

**Unit Type:** Flying, Ranged, Physical damage (homing rocket), Anti-air specialist
**Visual:** A small fast mechanical helicopter. Not heavily armored — it's fast and it doesn't miss.

**Stats (T1 baseline — fast, fragile, specialist):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Clockwork Copter | 35 | 2 | 60 | 2.0s | 140 |
| T2 Interceptor | 67 | 4 | 102 | 2.0s | 140 |
| T3 Ace | 127 | 6 | 173 | 2.0s | 140 |

**DPS: 30 per second — highest DPS unit in the game at T1**

**System: Homing Rocket**
- Fires a **homing rocket** at the nearest enemy **air unit** within range
- Range: between standard archer range and mage range (~300-350px suggested — shorter than an archer's 25% range advantage over mages)
- **Perfect accuracy — hitscan, never misses**
- Fires once every **2 seconds** at **30 damage per shot**
- Rocket homes to target — if target moves the rocket follows until impact

**Idle behavior (no air targets in range):**
- Does NOT engage ground units
- Retreats toward own buildings and **patrols between them automatically**
- Remains airborne and visible — at minimum interesting to watch

**Upgrade ×10:** +X damage per upgrade. Cost doubles from 15. Steel + Rubies.

**No unlock.**

**Design Notes:**
- 30 DPS is intentionally the highest in the game — this is a specialist that earns it by being fragile, short-ranged, and useless against ground units
- Hitscan means no projectile travel time — the damage is instant on fire
- Hard countered by having no air units — the Copter becomes an expensive parade float
- Soft countered by overwhelming it with many cheap air units before it can kill them all
- The patrol idle behavior is a deliberate design choice — it keeps the unit visually active and makes the Skyport feel alive even between engagements

---

## Slot 3 — Dwarven Rifleman Line

**Unit Type:** Ranged, Physical damage, Tower guard specialist
**Visual:** A dwarf with a rifle. Deploys into a braced firing position.

**Stats (T1 baseline — sturdy guard, medium armor):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Dwarven Rifleman | 45 | 4 | 13 | 1.0s | 100 |
| T2 Sharpshooter | 86 | 7 | 22 | 1.0s | 100 |
| T3 Deadeye | 163 | 11 | 38 | 1.0s | 100 |

**System: Deploy Hunting Location** *(mirrors Air Skirmisher — ground targets)*

**Positioning behavior:**
- **Prefers to station near a player tower** — if a tower is within range, moves adjacent to it and guards it
- If no towers are available: acts as a normal ranged unit

**Targeting behavior:**
- At **standard archer range**: functions as normal ranged unit
- When an enemy **ground unit** enters standard range: **deploys** (2 second animation, fully stops moving) and enters **Sniper Mode**:
  - Range increases by **200%**
  - Damage increases by **20%**
  - **Perfect accuracy** while deployed — hitscan, no spread
- Sniper Mode persists until no enemy ground units remain within expanded range
- Un-deploys and returns to tower guard positioning when threat clears

**Upgrade ×10:** +X damage per upgrade. Cost doubles from 15. Steel + Wood.

**No unlock.**

**Design Notes:**
- The "scariest tower guard" identity comes from the combination of deploy range (200% increase covers enormous ground), perfect accuracy, and the tower-adjacent positioning — an attacker approaching a tower finds a deployed Rifleman with massive range waiting for them
- Mirrors the Air Skirmisher exactly but for ground targets — clean parallel design within the same building
- The 2 second deploy animation means the first unit to enter standard range gets one normal shot before Sniper Mode activates

---

## Slot 4 — Volley Gunner Line

**Unit Type:** Ranged, Physical, Anti-unit siege
**Visual:** Based on a Ribauldequin (organ gun) — multiple barrels arranged in a fan/arc pattern. All barrels fire simultaneously. Visually large, mounted on a wheeled frame operated by a dwarf crew.

**Stats (T1 baseline — heavy siege crew, slow but devastating):**
| Tier | HP | Armor | Damage | Atk Speed | Move Speed |
|---|---|---|---|---|---|
| T1 Volley Gunner | 55 | 5 | 8 per projectile | 10.0s | 85 |
| T2 Rotary Volley Gunner | 105 | 8 | 8 per projectile | 10.0s | 85 |
| T3 Storm Volley Gunner | 200 | 13 | 8 per projectile | 10.0s | 85 |

*HP and armor scale normally. Damage per projectile stays at 8 — the tier scaling comes from projectile count.*

**System: Volley Fire**

**Targeting behavior (mirrors Bombard/Iron Husk — building priority):**
- **Priority check:** Is any enemy tower or building within **400px**? If yes → deploy adjacent to it and fire at it
- If no tower/building within 400px → deploy adjacent to nearest enemy unit cluster and fire at units
- When a building enters 400px range mid-combat: switches target to building

**Deploy behavior:**
- On reaching target position: **deploys** (2 second animation) then fires
- After firing: **10 second reload** before next volley
- Does not move while deployed and reloading

**Volley behavior:**
- Fires **10/20/30 projectiles simultaneously** (T1/T2/T3) in a **shotgun spread arc**
- Projectiles travel in a fan pattern — wide spread, not converging on a single point
- Each projectile deals **8 physical damage** — armor soaks most of it per hit
- Against a lightly armored target (2 armor): 6 damage per projectile → 60/120/180 total damage per volley
- Against a heavily armored target (8 armor): 0 damage per projectile (all soaked) — hard counter
- The **quantity** is the threat, not individual projectile damage — crowds of low-armor units get shredded

**Upgrade ×10:** +1 projectile per volley per upgrade (all tiers). Cost doubles from 15. Steel + Stone.

**No unlock.**

**Design Notes:**
- Armor is the hard counter — a heavily armored unit absorbs every shot. Pair with Alchemist (Corrode debuff reduces armor) to make the Volley Gunner effective against armored targets
- Devastating against swarms of low-armor units — Militia Unwashed Masses, summoned animals, spirit clones, the Slime's freed prisoners
- The 10 second reload is the critical vulnerability window — any unit that survives the volley has 10 seconds to close the gap before the next one
- Projectile spread arc means positioning matters — units clustered together take more total hits than spread-out units
- ⚠️ T3 Storm Volley Gunner at max upgrade fires 40 projectiles simultaneously — at 8 damage each against unarmored targets that is 320 damage in a single volley. Monitor in playtesting.

