# Activity and Social/Quirky Skills

Game design document for the Activity skills (Agility, Prayer training, Dungeoneering, Sailing) and Social/Quirky skills (Socializing, Bank Standing, Pet Raising). Each skill follows the activity pattern: perform an action in a specific context to earn XP. All skills and subsystems are toggleable per world.

---

## Activity Skill Pattern

Activity skills differ from gathering/processing/combining skills. Instead of "tool + node = product," activity skills follow "perform action in context = XP." The context is the key differentiator -- a course, a dungeon floor, a voyage, a prayer altar. The player does not produce a tradeable output; they gain XP, unlock access, and earn unique rewards.

| Property | Description |
|---|---|
| Context | Where the activity takes place (course, floor, altar, sea) |
| Action | What the player does (run obstacles, clear rooms, offer bones, navigate) |
| Completion | What counts as a finished unit (lap, floor clear, bone offered, voyage landed) |
| XP Award | Per-action, per-completion, or both |
| Failure | Whether actions can fail and what happens (damage, time loss, retry) |
| Scaling | How difficulty and reward scale with level |
| Unique Rewards | Currency, unlocks, or items exclusive to this skill |

---

## 1. Agility

Source: OSRS. Controls run energy, grants shortcut access, and trains through obstacle courses.

### 1.1 Run Energy

Run energy is a 0-10000 integer (displayed as 0-100% to the player). Running drains energy; walking and standing restore it.

#### Drain Formula

```
drain_per_tick = floor((67 + weight) * (300 - agility_level) / 300)
```

- `weight` is the player's total equipment weight in kg (minimum 0, negative weight counts as 0)
- At 0 kg and 99 Agility, drain is ~45/tick. At 30 kg and 1 Agility, drain is ~97/tick.
- A game tick is 0.6 seconds. Running consumes 1 drain unit every 2 ticks (1.2 seconds).

#### Restoration Formula

```
restore_per_tick = floor(agility_level / 6 + 8)
```

- Restoration occurs every tick while walking or standing still.
- At 99 Agility, restore is ~24/tick. At 1 Agility, restore is ~8/tick.
- A 99 Agility player runs ~49% longer and restores ~65% faster than a level 1.

#### Weight Impact

| Weight (kg) | Effect |
|---|---|
| 0 or below | Minimum drain rate. Negative weight does not reduce further |
| 1 - 30 | Linear increase in drain rate per the formula |
| 30+ | Continues scaling but rarely reached with graceful |

#### Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| base_drain_factor | integer | 67 | Base constant in drain formula |
| weight_factor | float | 1.0 | Multiplier on weight contribution to drain |
| restore_divisor | integer | 6 | Divisor in restoration formula |
| restore_base | integer | 8 | Additive base in restoration formula |
| min_weight_floor | integer | 0 | Minimum effective weight (set negative to allow sub-zero) |

### 1.2 Graceful Outfit

Weight-reducing outfit purchased with marks of grace. Full set grants a 30% bonus to run energy restoration rate.

| Piece | Marks of Grace | Weight Reduction (kg) |
|---|---|---|
| Hood | 35 | -3 |
| Top | 55 | -5 |
| Legs | 60 | -4 |
| Gloves | 30 | -3 |
| Boots | 40 | -4 |
| Cape | 40 | -4 |
| **Full Set** | **260** | **-23 (+ 30% restore bonus)** |

Graceful can be recolored for additional marks at various rooftop course vendors. Dark graceful recolor costs 300 hallowed marks per piece from the Hallowed Sepulchre.

### 1.3 Rooftop Courses

The primary Agility training method. Players complete laps by running through a sequence of obstacles. XP is awarded per obstacle and a bonus on lap completion. Marks of grace spawn on the course as the player completes laps.

#### Course Table

| Course | Level Req | Lap XP | XP/hr (approx) | Marks/hr (approx) |
|---|---|---|---|---|
| Draynor Village | 1 | 120 | 10,000 | 12 |
| Al Kharid | 20 | 180 | 12,000 | 10 |
| Varrock | 30 | 238 | 14,000 | 10 |
| Canifis | 40 | 240 | 19,700 | 17 |
| Falador | 50 | 440 | 35,000 | 11 |
| Seers' Village | 60 | 570 | 46,000-59,000 | 12-17 |
| Pollnivneach | 70 | 890 | 52,000-60,000 | 14-18 |
| Rellekka | 80 | 780 | 55,000-65,000 | 14-20 |
| Ardougne | 90 | 793 | 70,000-164,000 | 16-22 |

Higher XP rates at some courses are available after diary completion (configurable per world).

#### Mark of Grace Mechanics

| Property | Value |
|---|---|
| Spawn trigger | Random chance per obstacle completion |
| Despawn timer | 10 minutes |
| Level penalty | Marks spawn at 20% normal rate when player level is 20+ above course requirement |
| Trade value | Not directly tradeable; spent at grace vendors |

#### Obstacle Types

| Obstacle | Description | Failure? |
|---|---|---|
| Balance beam | Walk across a narrow beam | Yes, fall for damage |
| Tightrope | Cross a rope between buildings | No |
| Gap jump | Leap across a gap between rooftops | Yes, fall for damage |
| Wall climb | Scale a wall using handholds | No |
| Zip line | Slide down a rope | No |
| Stepping stones | Hop across a series of stones | Yes, fall into water |
| Log balance | Walk across a fallen log | Yes, slip for damage |
| Pipe squeeze | Crawl through a narrow pipe | No |
| Ladder/Stairs | Climb up or down | No |

Failure chance decreases with higher Agility level. Most failures deal 1-6 damage and return the player to the obstacle start.

### 1.4 Shortcuts

Permanent world traversal shortcuts unlocked at specific Agility levels. Over 150 shortcuts exist in OSRS across all level ranges.

#### Shortcut Categories

| Category | Examples | Typical Levels |
|---|---|---|
| Wall climb | Crumbling walls, rock scrambles | 1-30 |
| Pipe/Crevice squeeze | Underground pipes, cave crevices | 20-50 |
| Stepping stones | River crossings, cave hops | 25-60 |
| Grapple | Requires crossbow + mithril grapple | 30-70 |
| Log balance | Fallen tree traversal | 30-60 |
| Cliff descent | Climb down steep terrain | 40-80 |
| Advanced | Complex traversal near high-level content | 70-92 |

#### Shortcut Data Model

| Field | Type | Description |
|---|---|---|
| shortcut_id | string | Unique identifier |
| location | string | Map region and tile coordinates |
| agility_req | integer | Minimum Agility level |
| quest_req | string or null | Quest completion required |
| other_reqs | JSON or null | Other requirements (e.g., ranged level for grapple) |
| xp_award | float | XP granted per use (usually 0-20) |
| fail_possible | boolean | Whether the shortcut can fail |
| fail_damage | integer range | Damage on failure (e.g., 1-6) |
| obstacle_type | enum | Category from the table above |

### 1.5 Hallowed Sepulchre

High-level Agility minigame with five floors, each requiring a higher Agility level. Players navigate timed obstacle gauntlets, dodge traps, and optionally loot coffins for hallowed marks.

#### Floor Table

| Floor | Agility Req | Timer Added | XP/hr (approx) |
|---|---|---|---|
| 1 | 52 | 2:00 | 30,000 |
| 2 | 62 | 2:00 | 40,000 |
| 3 | 72 | 2:00 | 63,000 |
| 4 | 82 | 2:00 | 73,000 |
| 5 | 92 | 2:00 | 90,000 |

Timer carries between floors. Unused time from floor 1 adds to floor 2, etc. Hallowed tokens (found in-dungeon) extend the timer by 1 minute each.

#### Trap Types

| Trap | Description |
|---|---|
| Wizard statue | Fires a projectile in a line. Dodge by timing movement |
| Knight statue | Swings weapon in an arc. Time passage between swings |
| Crossbowman statue | Shoots bolts in a pattern. Navigate between shots |
| Priest statue | Casts area denial. Move through gaps in the pattern |

#### Coffin Loot

| Item | Source | Rarity |
|---|---|---|
| Hallowed marks | All coffins | Common |
| Ring of endurance | Grand hallowed coffin (floor 5) | Rare |
| Strange old lockpick | Floor 4-5 coffins | Uncommon |

#### Hallowed Marks Shop

| Item | Cost (marks) |
|---|---|
| Hallowed grapple | 100 |
| Hallowed focus | 100 |
| Hallowed symbol | 100 |
| Hallowed hammer | 100 |
| Hallowed ring | 250 |
| Dark dye (per piece) | 300 |
| Dark acorn (pet recolor) | 3,000 |

### 1.6 Brimhaven Agility Arena

25 pillars connected by agility obstacles. A random ticket dispenser activates every 60 seconds. Players navigate to the active dispenser to earn tickets. Tickets are exchanged for XP or items.

| Property | Value |
|---|---|
| Pillar count | 25 |
| Dispenser cycle | 60 seconds |
| XP/hr at 99 | ~67,000 |
| Reward currency | Agility arena tickets |

### 1.7 Agility Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| courses_enabled | boolean | true | Enable rooftop courses |
| shortcuts_enabled | boolean | true | Enable agility shortcuts in the world |
| hallowed_sepulchre_enabled | boolean | true | Enable the Hallowed Sepulchre |
| brimhaven_arena_enabled | boolean | true | Enable the Brimhaven Agility Arena |
| graceful_enabled | boolean | true | Enable the graceful outfit and marks of grace |
| mark_spawn_rate | float | 1.0 | Multiplier on mark of grace spawn chance |
| mark_level_penalty_threshold | integer | 20 | Levels above course req before mark rate drops |
| mark_level_penalty_rate | float | 0.2 | Mark spawn rate multiplier when over threshold |
| course_fail_damage_multiplier | float | 1.0 | Scale obstacle failure damage |
| shortcut_fail_enabled | boolean | true | Whether shortcuts can fail |
| run_energy_system | enum | `osrs` | Run energy formula (`osrs`, `rs3`, `custom`) |
| custom_drain_formula | string or null | null | Custom drain formula (if run_energy_system = custom) |
| custom_restore_formula | string or null | null | Custom restore formula |
| graceful_restore_bonus | float | 0.3 | Full-set restore bonus (30%) |
| sepulchre_timer_per_floor | integer | 120 | Seconds added per Sepulchre floor |

---

## 2. Prayer (Training Mechanics)

Source: OSRS. The combat-side of Prayer (protection prayers, stat boosts) is documented in the combat skills doc. This section covers how Prayer XP is gained -- bones, altars, and ensouled heads.

### 2.1 Bone Types

Bones are dropped by monsters and offered at altars for Prayer XP.

| Bone | Bury XP | Gilded Altar (3.5x) | Chaos Altar (3.5x + 50% save) | Ectofuntus (4x) |
|---|---|---|---|---|
| Bones | 4.5 | 15.75 | 15.75 (eff. 31.5) | 18 |
| Burnt bones | 4.5 | 15.75 | 15.75 (eff. 31.5) | 18 |
| Wolf bones | 4.5 | 15.75 | 15.75 (eff. 31.5) | 18 |
| Monkey bones | 5 | 17.5 | 17.5 (eff. 35) | 20 |
| Bat bones | 5.3 | 18.5 | 18.5 (eff. 37) | 21.2 |
| Big bones | 15 | 52.5 | 52.5 (eff. 105) | 60 |
| Zogre bones | 22.5 | 78.75 | 78.75 (eff. 157.5) | 90 |
| Babydragon bones | 30 | 105 | 105 (eff. 210) | 120 |
| Wyrm bones | 50 | 175 | 175 (eff. 350) | 200 |
| Wyvern bones | 72 | 252 | 252 (eff. 504) | 288 |
| Dragon bones | 72 | 252 | 252 (eff. 504) | 288 |
| Drake bones | 80 | 280 | 280 (eff. 560) | 320 |
| Fayrg bones | 84 | 294 | 294 (eff. 588) | 336 |
| Lava dragon bones | 85 | 297.5 | 297.5 (eff. 595) | 340 |
| Raurg bones | 96 | 336 | 336 (eff. 672) | 384 |
| Hydra bones | 110 | 385 | 385 (eff. 770) | 440 |
| Dagannoth bones | 125 | 437.5 | 437.5 (eff. 875) | 500 |
| Ourg bones | 140 | 490 | 490 (eff. 980) | 560 |
| Superior dragon bones | 150 | 525 | 525 (eff. 1050) | 600 |

"Eff." values for Chaos Altar reflect the effective XP per bone accounting for the 50% chance the bone is not consumed.

### 2.2 Altar Types

| Altar | XP Multiplier | Location | Requirements |
|---|---|---|---|
| Regular altar | 1x (same as burying) | Churches, various locations | None |
| Gilded altar | 3.5x (both burners lit) | Player-owned house | 75 Construction (to build) |
| Gilded altar (1 burner) | 3.0x | Player-owned house | 75 Construction |
| Gilded altar (no burners) | 2.5x | Player-owned house | 75 Construction |
| Chaos altar | 3.5x + 50% save | Wilderness (level 38) | None (PvP risk) |
| Ectofuntus | 4x | Port Phasmatys | Ghosts Ahoy (partial) |

#### Gilded Altar Mechanics

- Built in the Chapel room of a player-owned house.
- Two incense burners flank the altar. Each must be lit with a marrentill herb.
- Burners stay lit for ~2 minutes before needing to be relit.
- With both burners lit: 3.5x XP. One burner: 3.0x. No burners: 2.5x.
- Other players can use your altar if your house is set to public.

#### Chaos Altar Mechanics

- Located in the Wilderness at level 38 (PvP zone).
- Provides 3.5x XP per bone offered.
- 50% chance each bone is not consumed when offered, effectively doubling throughput.
- Risk: other players can attack you. Bones in inventory are lost on death.
- Unnoting bones: an NPC near the altar un-notes bones for a small fee.

#### Ectofuntus Mechanics

- Requires grinding bones into bonemeal using the bone grinder.
- Requires filling a bucket with ectoplasm from the slime pool below.
- Each worship action consumes 1 bonemeal + 1 bucket of slime = 4x bury XP.
- Slower than gilded/chaos altar but higher per-bone XP.

### 2.3 Ensouled Heads

Dropped by monsters. Players use the Arceuus spellbook to reanimate creatures, then kill them for Prayer XP.

| Head | Magic Req | Prayer XP |
|---|---|---|
| Ensouled goblin head | 3 | 130 |
| Ensouled monkey head | 7 | 182 |
| Ensouled imp head | 12 | 286 |
| Ensouled minotaur head | 17 | 364 |
| Ensouled scorpion head | 19 | 454 |
| Ensouled bear head | 21 | 480 |
| Ensouled unicorn head | 22 | 494 |
| Ensouled dog head | 26 | 520 |
| Ensouled chaos druid head | 30 | 584 |
| Ensouled giant head | 37 | 650 |
| Ensouled ogre head | 40 | 716 |
| Ensouled elf head | 43 | 754 |
| Ensouled troll head | 46 | 780 |
| Ensouled horror head | 52 | 832 |
| Ensouled kalphite head | 57 | 884 |
| Ensouled dagannoth head | 62 | 936 |
| Ensouled bloodveld head | 65 | 1,040 |
| Ensouled tzhaar head | 69 | 1,104 |
| Ensouled demon head | 72 | 1,170 |
| Ensouled aviansie head | 78 | 1,234 |
| Ensouled abyssal head | 85 | 1,300 |
| Ensouled dragon head | 93 | 1,560 |

### 2.4 Prayer Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| bone_burying_enabled | boolean | true | Allow burying bones for XP |
| gilded_altar_enabled | boolean | true | Enable gilded altar in POH |
| gilded_altar_multiplier | float | 3.5 | XP multiplier with both burners lit |
| chaos_altar_enabled | boolean | true | Enable wilderness chaos altar |
| chaos_altar_save_chance | float | 0.5 | Chance bone is not consumed at chaos altar |
| chaos_altar_pvp_zone | boolean | true | Whether chaos altar is in a PvP area |
| ectofuntus_enabled | boolean | true | Enable Ectofuntus worship |
| ectofuntus_multiplier | float | 4.0 | XP multiplier for Ectofuntus |
| ensouled_heads_enabled | boolean | true | Enable ensouled head reanimation |
| bone_xp_global_multiplier | float | 1.0 | Global multiplier on all bone XP |
| custom_bone_types | JSON or null | null | Define additional bone types with custom XP |

---

## 3. Dungeoneering

Source: RS3. Procedurally generated dungeon floors cleared solo or in teams. Uses all other skills within the dungeon. Awards tokens spent at a reward shop.

### 3.1 Floor System

Daemonheim contains 60 floors organized into themed tiers. Players unlock floors sequentially by completing the one before.

| Theme | Floors | Level Range | Description |
|---|---|---|---|
| Frozen | 1-11 | 1-22 | Ice caverns, frost creatures |
| Abandoned I | 12-17 | 23-34 | Ruined corridors, undead |
| Furnished | 18-29 | 35-58 | Decorated rooms, magical creatures |
| Abandoned II | 30-35 | 59-70 | Deeper ruins, stronger undead |
| Occult | 36-47 | 71-94 | Dark magic, demonic creatures |
| Warped | 48-60 | 95-120 | Reality-bent, strongest creatures |

Revisiting a completed floor incurs a 50% XP penalty.

### 3.2 Dungeon Size

| Size | Grid | Max Rooms | XP Modifier | Complexity Req |
|---|---|---|---|---|
| Small | 4x4 | 16 | +0% | Any |
| Medium | 4x8 | 32 | +7% | 6 |
| Large | 8x8 | 64 | +15% | 6 |

### 3.3 Complexity Levels

Complexity determines which skills are available inside the dungeon.

| Complexity | Skills Available | XP Modifier |
|---|---|---|
| 1 | Combat only | Large penalty |
| 2 | Combat + a few support | Moderate penalty |
| 3 | Most combat + gathering | Small penalty |
| 4 | All combat + most gathering + basic processing | Minor penalty |
| 5 | Nearly all skills | Very small penalty |
| 6 | All skills | No penalty |

Medium and large dungeons require complexity 6.

### 3.4 Team Size

| Team Size | Scaling |
|---|---|
| 1 (solo) | Baseline difficulty and XP |
| 2 | Increased monster HP, more rooms, more XP |
| 3 | Further scaling |
| 4 | Further scaling |
| 5 | Maximum difficulty and XP |

Difficulty scales with team size: monster HP, puzzle complexity, and boss mechanics adjust. Players can manually set difficulty to accommodate members leaving mid-dungeon.

### 3.5 Prestige System

Prestige tracks how many unique floors a player has completed since their last reset.

- Completing all available floors then resetting prestige maximizes XP.
- Resetting converts current floor completion into "previous progress," which is the base for XP calculation.
- Optimal strategy: complete all unlocked floors, reset, repeat.
- Doing the same floor repeatedly without resetting yields diminishing returns.

#### XP Formula (Simplified)

```
base_xp = floor_base * (1 + prestige_bonus)
modifiers = size_modifier + complexity_modifier - death_penalty
final_xp = base_xp * modifiers
```

- `floor_base` scales with floor number.
- `prestige_bonus` scales with number of unique floors completed before reset.
- `death_penalty` reduces XP per death during the floor.

### 3.6 Room Types

#### Critical Path Rooms

Rooms that must be cleared to reach the boss. The dungeon always has a path from entrance to boss through critical rooms.

#### Bonus Rooms

Optional rooms off the critical path. Clearing them adds bonus XP to the floor completion reward. Often contain skill doors or resource nodes.

#### Puzzle Rooms

Rooms with environmental puzzles that must be solved before the door unlocks. Examples:

| Puzzle | Description |
|---|---|
| Ferret herding | Chase ferrets onto pressure plates |
| Colored lights | Activate pillars in the correct color sequence |
| Sliding tiles | Rearrange tiles to form a pattern |
| Lever sequence | Pull levers in the correct order |
| Barrel push | Push barrels onto marked positions |
| Fishing/cooking | Catch and cook specific fish to feed an NPC |
| Statue alignment | Rotate statues to face the correct direction |

#### Skill Doors

Doors requiring a specific skill level to open. The required skill and level depend on the floor and complexity. Opening a high-level skill door on the critical path is a strong indicator of the boss direction.

| Property | Description |
|---|---|
| Required skill | Any non-combat skill (Mining, Woodcutting, Fishing, etc.) |
| Required level | Scales with floor number |
| XP award | Significant XP in the required skill |
| Team sharing | Nearby teammates receive 45% of the XP if they meet the level |

### 3.7 Boss Encounters

Each floor theme has a pool of bosses. One is randomly selected per floor.

| Theme | Example Bosses |
|---|---|
| Frozen | Gluttonous Behemoth, Astea Frostweb, Icy Bones, Luminescent Icefiend |
| Abandoned | Skeletal trio, Unholy Cursebearer |
| Furnished | Sagittare, Night-gazer Khighorahk |
| Occult | Shadow-forger Ihlakhizan, Bal'lak the Pummeller |
| Warped | Kal'Ger the Warmonger, Hope Devourer |

Bosses drop tier-appropriate equipment. Tier 11 (level 99 requirement) gear only comes from boss drops.

### 3.8 Binding System

Players bind items to carry between dungeon runs.

| Level | Active Binds |
|---|---|
| 1 | 1 (+1 if off-hand) |
| 20 | 2 |
| 50 | 3 |
| 90 | 4 |
| 120 | 5 |

Total bind pool: 10 items (12 with hard diary/task completion). Ammunition and potion binds use separate slots.

### 3.9 Token System

Players earn Dungeoneering tokens at a rate of 1 token per 10 XP earned.

#### Reward Shop (Key Items)

| Item | Token Cost | Description |
|---|---|---|
| Chaotic rapier | 200,000 | Best-in-slot melee stab weapon |
| Chaotic longsword | 200,000 | Best-in-slot melee slash weapon |
| Chaotic maul | 200,000 | Best-in-slot melee crush weapon |
| Chaotic crossbow | 200,000 | Best-in-slot ranged weapon |
| Chaotic staff | 200,000 | Best-in-slot magic weapon |
| Scroll of efficiency | 20,000 | Chance to save a bar when smithing |
| Scroll of cleansing | 20,000 | Chance to save a secondary when making potions |
| Scroll of augury | 20,000 | Unlocks the Augury prayer |
| Scroll of rigour | 20,000 | Unlocks the Rigour prayer |
| Bonecrusher | 34,000 | Automatically buries bones from kills |
| Herbicide | 34,000 | Automatically cleans/destroys herb drops |
| Ring of vigour | 50,000 | Saves 10% special attack energy |
| Bone-saving necklace | 80,000 | Chance to save bones when offering |

### 3.10 Resource Management

Items created inside a dungeon cannot leave Daemonheim. All weapons, armor, food, potions, and materials are dungeon-exclusive. Exceptions: Ring of Kinship, capes, cosmetics, and boss-exclusive reward drops.

### 3.11 Dungeoneering Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| max_floor | integer | 60 | Total available floors |
| max_team_size | integer | 5 | Maximum party members |
| prestige_enabled | boolean | true | Enable prestige resets |
| complexity_levels | integer | 6 | Number of complexity tiers |
| guide_mode_enabled | boolean | true | Highlight critical path (5% XP penalty) |
| guide_mode_penalty | float | 0.05 | XP penalty for guide mode |
| tokens_per_xp | float | 0.1 | Token earn rate (1 per 10 XP) |
| revisit_penalty | float | 0.5 | XP penalty for repeating completed floors |
| death_xp_penalty | float | 0.05 | XP reduction per death during a floor |
| binding_slots | JSON | See table | Bind slot unlock levels |
| reward_shop_enabled | boolean | true | Enable the token reward shop |
| custom_rewards | JSON or null | null | Add custom items to the reward shop |
| skill_door_xp_sharing | float | 0.45 | XP share for nearby teammates |
| puzzle_rooms_enabled | boolean | true | Enable puzzle rooms |
| boss_tier_drops | boolean | true | Bosses drop tier equipment |
| procedural_seed | string or null | null | Fixed seed for deterministic layouts (null = random) |
| dungeon_max_level | integer | 120 | Max Dungeoneering level (RS3 uses 120) |

---

## 4. Sailing

Source: OSRS (released November 2025). Players build ships, hire crew, and navigate seas to discover islands, fight sea creatures, and complete port tasks.

### 4.1 Ship Types

| Ship | Level Req | Grid Size | Hotspots | Player Cap | Crew Slots | Cost |
|---|---|---|---|---|---|---|
| Raft | 1 | 1x3 | 1 | 2 | 0 | 1,000 gp |
| Skiff | 15 | 2x5 | 7 | 5 | 2 | 15,000 gp |
| Sloop | 30 | 3x10 | 11 | 10 | 5 | 200,000 gp |

Hotspots are building locations on the ship where facilities (cannons, nets, storage) are constructed using Sailing + Construction levels.

### 4.2 Ship Facilities

| Facility | Sailing Req | Construction Req | Purpose |
|---|---|---|---|
| Bronze cannon | 28 | 21 | Ship combat (sea monsters) |
| Iron cannon | 38 | 31 | Improved ship combat |
| Steel cannon | 48 | 41 | Further improved combat |
| Wind catcher | 53 | 47 | Store and release wind motes for speed |
| Teleport focus | 55 | 49 | Teleport boat to discovered locations |
| Rope trawling net | 56 | 45 | Deep sea trawling (Fishing hybrid) |
| Crystal extractor | 73 | 67 | Generate wind motes passively |

### 4.3 Navigation Mechanics

| Mechanic | Description |
|---|---|
| Helm control | Click helm to steer. White arrow indicates heading |
| Speed | Adjustable via interface arrows (slow, medium, fast) |
| Wind trimming | Wind gusts appear periodically. Trim sails for 12-second speed boost (100 Sailing XP) |
| Wind motes | Stored via wind catcher, released for on-demand speed boosts |
| Rapids | Unlocked by charting seas. Fast-travel lanes between discovered areas |

### 4.4 Training Methods

#### Port Tasks

NPC-assigned tasks at port towns. Deliver cargo between ports or hunt sea creatures.

| Task Type | Level Range | Description |
|---|---|---|
| Courier | 1+ | Deliver cargo between two ports |
| Bounty | 30+ | Hunt a specific sea creature |

Active task limits scale with level:

| Level | Max Active Tasks |
|---|---|
| 1 | 1 |
| 7 | 2 |
| 28 | 3 |
| 56 | 4 |
| 84 | 5 |

#### Sea Charting

One-time exploration activities. Players use specialized tools to chart unknown waters. Completing a sea unlocks rapids for fast travel.

| Property | Value |
|---|---|
| Intensity | Low |
| XP rate | Low (one-time) |
| Reward | Fast travel routes, collection log entries |

#### Shipwreck Salvaging

Deploy salvaging hooks near shipwrecks within 7 tiles. Collect salvage and process it at stations.

| Hook Type | Level Req |
|---|---|
| Bronze | 15 |
| Iron | 30 |
| Steel | 45 |
| Mithril | 60 |
| Adamant | 75 |

#### Barracuda Trials (Races)

Timed sailing races with three difficulty tiers per course.

| Race | Level Req | Difficulties |
|---|---|---|
| The Tempor Tantrum | 30 | Swordfish, Shark, Marlin |
| The Jubbly Jive | 55 | Swordfish, Shark, Marlin |
| The Gwenith Glide | 72 | Swordfish, Shark, Marlin |

Higher difficulty = faster required completion = more XP.

#### Deep Sea Trawling

Hybrid Fishing/Sailing activity. Deploy trawling nets at fish shoals and maintain boat alignment as shoals move.

| Property | Value |
|---|---|
| Level Req | 56 Sailing, varies Fishing |
| Intensity | High |
| Reward | High GP/hr, Fishing + Sailing XP |

### 4.5 Ship Combat

Sea monsters are engaged using ship-mounted cannons.

| Property | Value |
|---|---|
| Cannon range | Up to 15 tiles |
| Firing arc | 180 degrees from cannon facing |
| Boat HP | Replaces player HP during naval combat |
| Conventional damage | Melee/ranged/magic deal ~23.75% normal damage on sea creatures |

#### Notable Sea Creature Drops

| Creature | Drop |
|---|---|
| Orca | Echo pearl |
| Albatross | Swift albatross feather |
| Narwhal | Narwhal horn |
| Ray | Ray barbs |
| Sea dragon | Broken dragon hook |
| Storm elemental | Bottled storm |

### 4.6 Crew Management

Players hire crewmates at the Crew Registrar. Each crew member has stats in three areas:

| Stat | Effect |
|---|---|
| Helmsmanship | Navigation speed and handling |
| Privateering | Combat effectiveness of ship |
| Deckhandiness | Facility efficiency (trawling, salvaging) |

Crew members are assigned to specific facilities on the ship.

### 4.7 Oceans and Mooring Points

Nine oceans are navigable. Some remain inaccessible due to hazards.

| Ocean | Status | Notes |
|---|---|---|
| Ardent | Accessible | Starting waters |
| Unquiet | Accessible | Moderate difficulty |
| Shrouded | Accessible | Fog mechanics |
| Western | Accessible | Open waters |
| Northern | Accessible | Cold weather effects |
| Sunset | Partially accessible | High-level content |
| Forgotten | Inaccessible | Unpreventable hazards |
| Untamed | Inaccessible | Unpreventable hazards |
| Eastern | Inaccessible | Unpreventable hazards |

#### Notable Mooring Points

| Island | Level Req | Feature |
|---|---|---|
| The Great Conch | 45 | Coral farming |
| The Onyx Crest | 47 | Lantern harpooning |
| Deepfin Point | 67 | Deep sea fishing |
| Grimstone | 87 | Frost dragons |

### 4.8 Death and Consequences

| Scenario | Result |
|---|---|
| Player dies on ship | Gravestone at last departure gangplank |
| Ship capsizes | Player teleported to last gangplank, inventory kept |
| Cargo hold | Contents lost in both death and capsizing |

### 4.9 Sailing Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| ship_types | JSON | See table | Available ship types and their stats |
| max_crew_size | integer | 5 | Maximum crew members per ship |
| wind_trim_xp | integer | 100 | XP per successful wind trim |
| wind_trim_cooldown | integer | varies | Seconds between wind gust spawns |
| task_limits | JSON | See table | Max active tasks by level |
| barracuda_trials_enabled | boolean | true | Enable racing content |
| deep_sea_trawling_enabled | boolean | true | Enable hybrid fishing/sailing |
| ship_combat_enabled | boolean | true | Enable sea creature combat |
| cargo_loss_on_death | boolean | true | Lose cargo hold on death/capsize |
| accessible_oceans | JSON | 6 oceans | Which oceans are navigable |
| custom_mooring_points | JSON or null | null | Add custom discoverable islands |
| conventional_damage_modifier | float | 0.2375 | Player melee/ranged/magic damage on sea creatures |
| rapids_enabled | boolean | true | Charted seas unlock fast-travel |
| crew_stat_range | integer range | 1-10 | Min/max crew stat values when hiring |
| facility_construction_enabled | boolean | true | Require Construction for facilities |

---

## 5. Socializing

Source: Inspired (original design). XP from social interactions: chatting, voice communication, emotes, trading, and group activities. Designed to reward the social fabric of an MMO.

### 5.1 Design Philosophy

Most MMOs treat social interaction as a side effect. Socializing makes it a first-class skill. The goal is not to incentivize spam but to reward genuine engagement. Anti-abuse measures are baked in: diminishing returns, cooldowns, and diversity requirements prevent farming.

### 5.2 XP Sources

| Source | Base XP | Cooldown | Notes |
|---|---|---|---|
| Send a chat message (Public) | 1 | 5 seconds | Only in Public/Area/Clan channels |
| Send a chat message (Clan) | 2 | 5 seconds | Clan chat weighted higher |
| Receive a reply | 1 | Per unique reply | Someone responds to your message |
| Use an emote near another player | 3 | 30 seconds | Must be within 5 tiles of another player |
| Synchronized emote | 10 | 60 seconds | Two players perform the same emote within 1 tick |
| Complete a trade | 15 | Per trade | Both players receive XP. Minimum trade value applies |
| Voice chat (per minute active) | 5 | Per minute | Requires voice system enabled |
| Add a friend (first time) | 25 | Per unique friend | Only on initial friend add |
| Play a minigame with others | 20 | Per minigame completion | Must finish the activity |
| Group boss kill | 10 | Per kill | Scales with group size |
| Help a lower-level player | 15 | 5 minutes | Assist action or mentoring tag |
| Attend a player-hosted event | 50 | Per event | Events registered through clan/event system |
| Roleplay channel message | 2 | 5 seconds | RP channel participation |

### 5.3 Anti-Abuse Measures

| Measure | Description |
|---|---|
| Diminishing returns | After 50 chat XP in 10 minutes, XP per message drops to 0.25 |
| Unique interaction bonus | XP multiplied by 1.5x when interacting with a player you have not interacted with in the last hour |
| Self-trade prevention | Trading the same player within 10 minutes awards 0 XP on repeat |
| Minimum trade value | Trades worth less than 100 GP award no XP |
| AFK detection | No voice XP if no input detected for 2+ minutes |
| Bot pattern detection | Repetitive identical messages within 60 seconds award 0 XP |
| Daily XP cap (soft) | After 5,000 Socializing XP in a day, rates halve. After 10,000, rates quarter |
| Channel diversity | Must earn XP from at least 2 different sources per hour to maintain full rates |

### 5.4 Milestones

| Level | Unlock |
|---|---|
| 10 | Expanded emote set (3 new emotes) |
| 25 | Chat bubble color customization |
| 40 | Unique overhead icon ("Social Butterfly") |
| 55 | Access to the Social Hub area (exclusive gathering space) |
| 70 | Title: "The Socialite" |
| 85 | Ability to host registered events (double event XP for attendees) |
| 99 | Socializing skillcape, title: "The Life of the Party" |

### 5.5 Socializing Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| chat_xp_enabled | boolean | true | Award XP for chat messages |
| voice_xp_enabled | boolean | true | Award XP for voice participation |
| emote_xp_enabled | boolean | true | Award XP for emotes near players |
| trade_xp_enabled | boolean | true | Award XP for completing trades |
| trade_min_value | integer | 100 | Minimum trade GP value for XP |
| daily_soft_cap | integer | 5000 | XP threshold before rate reduction |
| daily_hard_cap | integer | 10000 | XP threshold before severe rate reduction |
| diminishing_window | integer | 600 | Seconds for diminishing return window |
| diminishing_threshold | integer | 50 | Messages before diminishing kicks in |
| unique_interaction_bonus | float | 1.5 | Multiplier for new interactions |
| unique_interaction_cooldown | integer | 3600 | Seconds before a player counts as "new" again |
| afk_timeout | integer | 120 | Seconds of no input before voice XP stops |
| synchronized_emote_window | integer | 1 | Ticks for synchronized emote detection |
| custom_milestones | JSON or null | null | Override or add milestone unlocks |

---

## 6. Bank Standing

Source: Inspired (original design). XP from spending time idle at a bank. A loving tribute to the universal MMO behavior of standing at the Grand Exchange or a bank booth doing absolutely nothing productive.

### 6.1 Design Philosophy

Bank standing is already a core MMO activity. Players socialize, show off gear, trade, and AFK at banks constantly. This skill acknowledges that behavior and gamifies it with a wink. It is intentionally the slowest skill in the game -- reaching 99 requires genuine commitment to doing nothing. The skill doubles as a fashion/cosmetic showcase since bank standers are the most visible players.

### 6.2 XP Mechanics

XP is earned passively while standing within the bank zone (defined per bank location).

| Condition | XP per Minute | Notes |
|---|---|---|
| Standing in bank zone (base) | 1 | Must be in a designated bank area |
| Wearing full matching outfit | 2 | Any complete armor/cosmetic set |
| Wearing a rare/unique item | 3 | Items flagged as "showcase worthy" |
| Performing an emote | +5 (one-time) | Per emote, 2-minute cooldown |
| Another player examines you | +3 (one-time) | Per unique examiner, 10-minute cooldown |
| AFK for 5+ minutes | 0.5 | Rate halves after 5 minutes of no input |
| AFK for 15+ minutes | 0 | No XP after 15 minutes of no input |

### 6.3 Bank Zone Tiers

Different banks provide different XP rates based on prestige.

| Tier | Examples | XP Multiplier |
|---|---|---|
| Tier 1 (Basic) | Lumbridge, Draynor, small town banks | 1.0x |
| Tier 2 (Popular) | Varrock West, Falador, Seers' Village | 1.25x |
| Tier 3 (Hub) | Grand Exchange, Prif, player-owned house bank | 1.5x |
| Tier 4 (Prestige) | Exclusive unlockable locations | 2.0x |

### 6.4 Bank Standing Activities

Optional micro-activities that award bonus XP while bank standing. These are not required but reward engaged players.

| Activity | XP Bonus | Description |
|---|---|---|
| Fashion show judging | 10 per vote | Vote on other players' outfits (1 vote per player per hour) |
| Gear showcase | 20 per showcase | Display a full equipment set for 30 seconds (5-minute cooldown) |
| Price check conversation | 5 per check | Use the price checker on items while in bank |
| Bank tab organization | 15 per reorganization | Rearrange bank tabs (once per 30 minutes) |
| Examine other players | 3 per examine | Examine someone else's equipment |

### 6.5 Milestones

| Level | Unlock |
|---|---|
| 10 | Custom idle animation (lean, sit on crate) |
| 25 | Bank standing title: "Regular" |
| 40 | Access to a VIP bank area (Tier 4 zone) |
| 55 | Unique overhead icon (coffee cup, newspaper) |
| 70 | Title: "Professional Idler" |
| 85 | Custom bank standing spot reservation (a tile is "yours") |
| 99 | Bank Standing skillcape (best idle animations in game), title: "The Immovable" |

### 6.6 XP Rate Analysis

At optimal engagement (Tier 3 bank, full outfit, active emotes and examines):

| Scenario | XP/hr | Time to 99 (13,034,431 XP) |
|---|---|---|
| Pure AFK (base, Tier 1) | 60 | ~217,240 hours (never realistic) |
| Active Tier 3 | ~300 | ~43,448 hours |
| Optimal with activities | ~500 | ~26,069 hours |

This is intentionally absurd. Bank Standing is a meme skill. Most players will never max it, and that is the point. It exists as a long-term flex and conversation piece. Worlds can adjust the XP rate dramatically via the multiplier config.

### 6.7 Bank Standing Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| base_xp_per_minute | float | 1.0 | Base XP earned per minute in bank zone |
| outfit_bonus_xp | float | 2.0 | XP per minute when wearing matching outfit |
| rare_item_bonus_xp | float | 3.0 | XP per minute when showcasing rare items |
| afk_half_rate_minutes | integer | 5 | Minutes before AFK rate halving |
| afk_zero_rate_minutes | integer | 15 | Minutes before AFK stops XP entirely |
| bank_zone_tiers | JSON | See table | Define bank zones and their tier multipliers |
| fashion_show_enabled | boolean | true | Enable fashion show judging activity |
| examine_xp_enabled | boolean | true | Award XP when examined by others |
| examine_cooldown | integer | 600 | Seconds before same player can give examine XP again |
| xp_rate_multiplier | float | 1.0 | Global multiplier (increase for faster leveling) |
| custom_idle_animations | JSON or null | null | Add custom idle animations at milestones |
| vip_zone_enabled | boolean | true | Enable Tier 4 VIP bank area |

---

## 7. Pet Raising

Source: Inspired (original design). XP from feeding, training, bonding with, and caring for pet companions. Ties directly into the Pet Companion plugin (see pet-companion.md).

### 7.1 Design Philosophy

Pet Raising turns the pet system from a cosmetic trophy into a nurturing relationship. Players invest time in their pets -- feeding them, teaching them tricks, playing with them, housing them -- and gain skill XP for that care. The skill also ties into Construction (pet housing), Cooking (pet food), and Farming (growing pet food).

### 7.2 Relationship to Pet Companion Plugin

Pet Raising is the skill layer on top of the Pet Companion plugin. The pet plugin handles acquisition, following behavior, transmog, and abilities. Pet Raising adds the growth/XP progression.

| System | Plugin | Skill |
|---|---|---|
| Pet acquisition | Pet Companion | -- |
| Pet following | Pet Companion | -- |
| Pet transmog | Pet Companion | -- |
| Pet growth stages | Pet Companion | Pet Raising XP drives growth |
| Pet happiness/hunger | Pet Companion | Pet Raising actions affect meters |
| Pet abilities | Pet Companion | Unlocked at growth milestones (driven by Raising XP) |
| Pet tricks | -- | Pet Raising (teach and perform) |
| Pet housing | Construction + Pet Companion | Pet Raising XP from housing interactions |
| Pet food crafting | Cooking/Farming | Pet Raising XP when feeding crafted food |

### 7.3 XP Sources

| Source | XP per Action | Cooldown | Notes |
|---|---|---|---|
| Feed (basic food) | 5 | None | Any valid food item |
| Feed (favorite food) | 15 | None | Each pet has a favorite food type |
| Feed (crafted pet food) | 25 | None | Food specifically cooked for pets (Cooking skill) |
| Pet (interact) | 10 | 5 minutes | Pet the companion |
| Play | 15 | 10 minutes | Play with the companion |
| Teach a trick | 50 | Per trick | One-time XP per trick learned |
| Perform a learned trick | 10 | 5 minutes | Command pet to perform a trick |
| Bonding (passive) | 1 per minute | -- | Pet is actively following the player |
| Groom | 20 | 15 minutes | Grooming interaction (requires grooming kit) |
| Heal (nurse back to health) | 30 | When pet is injured | Heal a pet after combat damage |
| House interaction | 25 | 30 minutes | Pet interacts with housing furniture (bed, toy, etc.) |
| Pet show entry | 100 | Per event | Enter a pet in a player-hosted pet show |
| Combat assist (per kill) | 5 | Per kill | Pet helped in combat (pet combat assist toggle) |

### 7.4 Pet Tricks

Tricks are learned by repeated training. Each trick requires multiple training sessions before the pet learns it.

| Trick | Training Sessions | Raising Level Req | Description |
|---|---|---|---|
| Sit | 3 | 1 | Pet sits on command |
| Roll over | 5 | 10 | Pet rolls over |
| Shake | 5 | 15 | Pet offers a paw/claw |
| Beg | 8 | 25 | Pet performs begging animation |
| Dance | 10 | 35 | Pet dances |
| Fetch | 12 | 45 | Pet retrieves a nearby ground item |
| Play dead | 10 | 55 | Pet plays dead |
| Speak | 8 | 40 | Pet makes its characteristic sound on command |
| Guard | 15 | 65 | Pet growls at nearby hostile NPCs |
| Backflip | 20 | 75 | Pet performs acrobatic flip |
| Treasure hunt | 25 | 85 | Pet sniffs out buried treasure nearby (cosmetic, or minor loot with toggle) |
| Bond | 30 | 99 | Special animation where pet and player perform a unique bonding emote |

### 7.5 Pet Food Crafting

Ties into the Cooking skill. Players can cook specialized pet food for higher Raising XP.

| Food | Cooking Level | Ingredients | Raising XP when Fed |
|---|---|---|---|
| Basic kibble | 1 | Grain + cooked meat | 10 |
| Fish biscuit | 15 | Cooked fish + flour | 15 |
| Herb-infused treat | 30 | Clean herb + cooked meat + spice | 25 |
| Gourmet pet meal | 50 | 2x cooked fish + herb + spice + egg | 40 |
| Elder feast | 75 | 3x rare ingredients (dragon meat, etc.) | 75 |

### 7.6 Pet Housing

Ties into Construction. Pet housing furniture built in the player-owned house provides Raising XP when the pet interacts with it.

| Furniture | Construction Level | Raising XP per Interaction | Interaction Cooldown |
|---|---|---|---|
| Pet bed | 10 | 10 | 30 minutes |
| Food bowl | 15 | 5 (when filled) | When empty |
| Toy mouse / ball | 20 | 15 | 30 minutes |
| Scratching post | 25 | 10 | 30 minutes |
| Pet pool / bath | 35 | 20 | 60 minutes |
| Obstacle course | 50 | 30 | 60 minutes |
| Training dummy | 60 | 25 | 60 minutes |
| Luxury den | 75 | 40 | 60 minutes |

### 7.7 Growth Stage Integration

Pet Raising XP directly feeds into the pet growth stages defined in the Pet Companion plugin.

| Growth Stage | Raising XP Threshold | Effect |
|---|---|---|
| Baby | 0 | Small model, limited animations, no tricks |
| Juvenile | 1,000 | Slightly larger, basic tricks available |
| Adult | 10,000 | Full-size, all tricks available, abilities unlock |
| Elder | 100,000 | Visual distinction, bonus animations, all abilities maxed |

The Raising skill level determines which tricks can be taught. The pet's accumulated XP determines its growth stage. These are separate progressions -- a high Raising level lets you train pets faster, but each pet must individually earn its growth XP.

### 7.8 Milestones

| Level | Unlock |
|---|---|
| 10 | Grooming kit (enables groom interaction) |
| 20 | Pet food recipes (enables crafted pet food) |
| 30 | Second pet can follow simultaneously |
| 45 | Pet trick: Fetch can pick up items |
| 60 | Pet mood indicator (see exact happiness value) |
| 75 | Pet show hosting (organize pet shows for others) |
| 85 | Title: "Beast Whisperer" |
| 99 | Pet Raising skillcape, pet gains a miniature skillcape cosmetic |

### 7.9 Pet Raising Configurable Properties

| Property | Type | Default | Description |
|---|---|---|---|
| passive_bonding_xp | float | 1.0 | XP per minute while pet follows |
| feed_cooldown | integer | 0 | Seconds between feeding actions (0 = no cooldown) |
| pet_cooldown | integer | 300 | Seconds between petting interactions |
| play_cooldown | integer | 600 | Seconds between play interactions |
| groom_cooldown | integer | 900 | Seconds between grooming |
| trick_training_enabled | boolean | true | Enable trick learning system |
| trick_sessions_multiplier | float | 1.0 | Scale required training sessions |
| pet_food_crafting_enabled | boolean | true | Enable specialized pet food recipes |
| pet_housing_xp_enabled | boolean | true | Award Raising XP from housing interactions |
| max_following_pets | integer | 1 | Pets that can follow simultaneously (2 at level 30) |
| pet_show_enabled | boolean | true | Enable pet show events |
| treasure_hunt_loot | boolean | false | Whether Treasure Hunt trick gives actual loot |
| growth_thresholds | JSON | See table | XP required per growth stage |
| combat_assist_xp | float | 5.0 | XP per kill with pet combat assist |
| custom_tricks | JSON or null | null | Define additional tricks |
| custom_pet_food | JSON or null | null | Define additional pet food recipes |

---

## Module Toggles (Master List)

Top-level toggles for the entire Activity and Social/Quirky skill categories.

| Toggle | Default | Description |
|---|---|---|
| Agility | On | Entire Agility skill |
| Agility: Run Energy | On | Run energy drain/restore system |
| Agility: Courses | On | Rooftop courses and marks of grace |
| Agility: Shortcuts | On | World shortcuts |
| Agility: Hallowed Sepulchre | On | Sepulchre minigame |
| Agility: Brimhaven Arena | On | Brimhaven Agility Arena |
| Agility: Graceful Outfit | On | Graceful outfit and marks of grace shop |
| Prayer: Bone Training | On | Bone offering for XP |
| Prayer: Gilded Altar | On | Player-owned house altar |
| Prayer: Chaos Altar | On | Wilderness altar |
| Prayer: Ectofuntus | On | Port Phasmatys ectofuntus |
| Prayer: Ensouled Heads | On | Arceuus reanimation |
| Dungeoneering | On | Entire Dungeoneering skill |
| Dungeoneering: Prestige | On | Prestige reset system |
| Dungeoneering: Puzzles | On | Puzzle rooms in dungeons |
| Dungeoneering: Reward Shop | On | Token reward shop |
| Dungeoneering: Guide Mode | On | Critical path highlighting |
| Sailing | On | Entire Sailing skill |
| Sailing: Ship Combat | On | Sea creature combat |
| Sailing: Deep Sea Trawling | On | Hybrid fishing/sailing |
| Sailing: Barracuda Trials | On | Racing content |
| Sailing: Crew System | On | Hiring and managing crew |
| Sailing: Sea Charting | On | Exploration and fast-travel unlock |
| Socializing | Off | Entire Socializing skill (off by default, opt-in) |
| Socializing: Voice XP | Off | Voice chat XP (requires voice system) |
| Socializing: Trade XP | On | XP from trading |
| Socializing: Emote XP | On | XP from emotes near players |
| Bank Standing | Off | Entire Bank Standing skill (off by default, meme skill) |
| Bank Standing: Fashion Show | Off | Fashion show judging activity |
| Bank Standing: VIP Zone | Off | Tier 4 prestige bank area |
| Pet Raising | On | Entire Pet Raising skill |
| Pet Raising: Tricks | On | Trick learning system |
| Pet Raising: Pet Food | On | Specialized pet food crafting |
| Pet Raising: Pet Housing XP | On | XP from housing furniture interactions |
| Pet Raising: Pet Shows | On | Player-hosted pet show events |
