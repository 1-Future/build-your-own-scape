# Bosses and Raids

Game design document for the Bosses and Raids plugin. This system covers world bosses, instanced encounters, multi-phase fights, raid dungeons, difficulty scaling, loot distribution, and a reusable behavior library that all boss content draws from.

---

## Module: Bosses and Raids

### Module Toggles

Each toggle enables or disables a subsystem within the bosses and raids module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Player Scaling | Boss stats scale with the number of players in the fight |
| Phase System | Bosses transition between distinct phases with different mechanics |
| Difficulty Modes | Bosses support Normal, Hard, and Awakened difficulty tiers |
| Enrage Timer | Boss grows stronger the longer the fight lasts |
| MVP Loot Bonus | Top damage contributor receives bonus loot rolls |
| Damage Tracking | Per-player damage contribution is tracked throughout the fight |
| Kill Count Hiscores | Boss kill counts are recorded and ranked on hiscores |
| Instance System | Players can create private boss instances for a fee |
| Skilling Bosses | Boss encounters include phases that require skilling instead of combat |
| Death Mechanics | Custom death penalties per boss (item loss, fees, HC status) |
| Respawn Timer | Bosses have a configurable delay before respawning in the world |

---

## Linked Spreadsheets

Eight spreadsheets define the full bosses and raids data model. All are linked by boss ID, raid ID, room ID, or behavior ID.

---

### 1. Boss List (Main Table)

The primary spreadsheet. Every boss has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique boss identifier |
| Name | String | Display name |
| Description | String | Lore or flavor text shown in the boss log |
| Combat Level | Integer | Displayed combat level |
| Category | Enum | World, Wilderness, Instanced, Slayer, Quest, Raid, Skilling, Sporadic |

#### Group Settings

| Column | Type | Description |
|---|---|---|
| Participation | Enum | Solo, Group, Both |
| Min Players | Integer | Minimum players required to start the fight |
| Max Players | Integer | Maximum players allowed in the instance |
| Scaling | Boolean | Whether stats scale with player count |

#### Access Requirements

| Column | Type | Description |
|---|---|---|
| Location | String | World coordinates or area name where the boss is found |
| Quest IDs | List[Integer] | Quest IDs that must be completed to access |
| Skill Requirements | JSON | Minimum skill levels required (e.g., `{"agility": 70, "prayer": 43}`) |
| Slayer Level | Integer | Minimum Slayer level required |
| Slayer Task Required | Boolean | Whether the boss can only be fought on an active Slayer task |
| Spawn Item | Integer | Item ID required to spawn the boss (consumed on use) |
| KC Requirement | Integer | Kill count of a related boss required to enter |
| KC Boss ID | Integer | The boss whose kill count is checked for KC Requirement |
| Instance Cost | Integer | Gold cost to create a private instance |
| Cooldown | Integer (ticks) | Minimum time between consecutive fights |

#### Base Combat Stats

Same column structure as the monsters spreadsheet. Repeated here for completeness.

| Column | Type | Description |
|---|---|---|
| Hitpoints | Integer | Total HP pool |
| Attack Level | Integer | Attack level |
| Strength Level | Integer | Strength level |
| Defence Level | Integer | Defence level |
| Ranged Level | Integer | Ranged level |
| Magic Level | Integer | Magic level |
| Attack Speed | Integer (ticks) | Base attack speed |
| Max Hit Melee | Integer | Maximum melee hit |
| Max Hit Ranged | Integer | Maximum ranged hit |
| Max Hit Magic | Integer | Maximum magic hit |
| Attack Style | Enum | Primary attack style (Melee, Ranged, Magic, Multi) |

#### Defence Bonuses

| Column | Type | Description |
|---|---|---|
| Defence Stab | Integer | Stab defence bonus |
| Defence Slash | Integer | Slash defence bonus |
| Defence Crush | Integer | Crush defence bonus |
| Defence Ranged | Integer | Ranged defence bonus |
| Defence Magic | Integer | Magic defence bonus |

#### Immunities

| Column | Type | Description |
|---|---|---|
| Poison Immune | Boolean | Immune to poison |
| Venom Immune | Boolean | Immune to venom |
| Stun Immune | Boolean | Immune to stuns |
| Freeze Immune | Boolean | Immune to freezes |
| Stat Drain Immune | Boolean | Immune to stat drain |
| Cannon Immune | Boolean | Immune to dwarf cannon damage |
| Thrall Immune | Boolean | Immune to thrall damage |

---

### 2. Boss Phases

Each row defines one phase of a boss fight. Bosses with no phases have a single implicit phase.

#### Phase Identity

| Column | Type | Description |
|---|---|---|
| Boss ID | Integer | Links to Boss List |
| Phase Number | Integer | Ordering within the fight (1, 2, 3...) |
| Phase Name | String | Display name (e.g., "Enraged Form", "Shadow Phase") |

#### Transition Trigger

| Column | Type | Description |
|---|---|---|
| Trigger Type | Enum | HP Threshold, Timer, Mechanic Completion, Totem Charged |
| HP Threshold | Float (%) | Boss HP percentage that triggers this phase |
| Timer | Integer (ticks) | Time after fight start or previous phase that triggers transition |
| Mechanic ID | Integer | Mechanic that must be completed to trigger transition |

#### Form Change

| Column | Type | Description |
|---|---|---|
| New Model | String | Model or appearance ID for this phase (null = no change) |
| New Attack Style | Enum | Primary attack style override for this phase |
| New Movement | Enum | Movement behavior override (Stationary, Chase, Teleport, etc.) |

#### Stat Changes

| Column | Type | Description |
|---|---|---|
| New HP Pool | Integer | Fresh HP pool for this phase (null = carry over remaining HP) |
| Attack Speed Override | Integer (ticks) | New attack speed for this phase |
| Max Hit Override | Integer | New maximum hit for this phase |
| Immunities Override | JSON | Immunities that change in this phase (e.g., `{"poison": true, "stun": false}`) |

#### Arena Changes

| Column | Type | Description |
|---|---|---|
| Pillars Collapse | Boolean | Pillars in the arena are destroyed |
| Area Shrinks | Boolean | The safe fighting area gets smaller |
| New Zones | JSON | Zones added to the arena (e.g., `[{"type": "lava", "tiles": [[3,4],[3,5]]}]`) |
| Safe Spots Appear | List[String] | Safe spot tile coordinates that become available |
| Safe Spots Disappear | List[String] | Safe spot tile coordinates that are removed |

#### Adds and Summons

| Column | Type | Description |
|---|---|---|
| Spawn Monster IDs | List[Integer] | Monster IDs summoned during this phase |
| Spawn Count | Integer | Number of each monster spawned |
| Spawn Location | Enum | Random, Fixed, Near Boss, Near Players |
| Spawn Trigger | Enum | Phase Start, Timed, HP Threshold, On Attack |

#### Required Action

| Column | Type | Description |
|---|---|---|
| Required Item | Integer | Item ID that must be used during this phase |
| Required Tile | String | Tile coordinates the player must stand on |
| Required Skill | String | Skill that must be used instead of combat (e.g., Mining, Smithing) |
| Totem Charge | Boolean | Whether a totem or object must be charged during this phase |

---

### 3. Boss Attacks

Each row defines one attack a boss can perform.

#### Identity

| Column | Type | Description |
|---|---|---|
| Attack ID | Integer | Unique attack identifier |
| Boss ID | Integer | Links to Boss List |
| Attack Name | String | Display name |
| Phase(s) | List[Integer] | Phase numbers during which this attack is available |

#### Damage

| Column | Type | Description |
|---|---|---|
| Attack Style | Enum | Melee, Ranged, Magic, Typeless, Dragonfire, Special |
| Damage Min | Integer | Minimum damage dealt |
| Damage Max | Integer | Maximum damage dealt |

#### Area of Effect

| Column | Type | Description |
|---|---|---|
| AoE Shape | Enum | None, Circle, Line, Cone, Cross, Square, Custom |
| AoE Size | Integer | Radius or length in tiles |
| Max Targets | Integer | Maximum number of players hit (0 = unlimited) |

#### Telegraph and Avoidance

| Column | Type | Description |
|---|---|---|
| Telegraphed | Boolean | Whether the attack has a visible warning before landing |
| Telegraph Animation | String | Animation or visual cue shown before the attack |
| Telegraph Sound | String | Sound cue played before the attack |
| Avoidable | Boolean | Whether the attack can be dodged |
| Avoid Method | Enum | Move Off Tile, Safe Spot, Prayer Switch, Hide Behind Pillar, None |

#### Frequency

| Column | Type | Description |
|---|---|---|
| Frequency Type | Enum | Every X Attacks, Random, HP Threshold, Timed |
| Frequency Value | Integer | The X in "every X attacks", the tick interval, or the HP percentage |

#### Effects

| Column | Type | Description |
|---|---|---|
| Debuff Applied | Enum | Poison, Venom, Prayer Drain, Stat Drain, Bind, Stun, None |
| Debuff Strength | Integer | Magnitude of the debuff (damage per tick, drain amount, bind duration) |
| Boss Heals | Boolean | Whether the boss heals from this attack |
| Heal Amount | Integer | HP restored to the boss when this attack lands |

#### Player Interaction

| Column | Type | Description |
|---|---|---|
| Interaction Type | String | Special player requirement (e.g., "bounce projectile", "everyone must be hit") |
| Failure Punishment | Enum | Damage, Instant Kill, Boss Heals, Enrage, None |
| Failure Value | Integer | Damage dealt or HP healed on failure |

---

### 4. Boss Mechanics

Each row defines a unique mechanic used in a boss encounter. Mechanics are the strategic layer on top of attacks.

| Column | Type | Description |
|---|---|---|
| Mechanic ID | Integer | Unique mechanic identifier |
| Boss ID | Integer | Links to Boss List |
| Mechanic Name | String | Display name |
| Category | Enum | Prayer Switching, Team Coordination, Totem/Pillar, Minion Management, DPS Check, Skilling Phase, Safe Spot Rotation, Enrage Timer, Damage Tracking, Required Item |
| Description | String | Full description of what the mechanic does and how players interact with it |

#### Prayer Switching

| Column | Type | Description |
|---|---|---|
| Prayer Cycle | List[Enum] | Ordered list of prayer styles to switch between |
| Switch Frequency | Integer (ticks) | How often the correct prayer changes |
| Failure Damage | Integer | Damage dealt when the wrong prayer is active |

#### Team Coordination

| Column | Type | Description |
|---|---|---|
| Coordination Type | Enum | Calling, Stack, Spread, Role Assignment, Pass Object |
| Required Players | Integer | Minimum players needed for the mechanic |
| Failure Wipe | Boolean | Whether failing this mechanic wipes the team |

#### Totem and Pillar Systems

| Column | Type | Description |
|---|---|---|
| Object Count | Integer | Number of totems or pillars in the arena |
| Charge Method | Enum | Attack, Skill, Item, Stand On |
| Charge Threshold | Integer | Amount of charge needed to activate |
| Effect on Boss | String | What happens when charged (e.g., "removes shield", "stuns for 5 ticks") |

#### Minion Management

| Column | Type | Description |
|---|---|---|
| Minion Monster IDs | List[Integer] | Monster IDs of the minions |
| Minion Behavior | Enum | Heal Boss, Explode, Shield Boss, Attack Players |
| Heal Amount | Integer | HP restored to boss per minion tick (if Heal Boss) |
| Explode Damage | Integer | Damage dealt if minion is not killed in time (if Explode) |

#### DPS Check

| Column | Type | Description |
|---|---|---|
| Damage Required | Integer | Total damage that must be dealt |
| Time Limit | Integer (ticks) | Time window to deal the required damage |
| Failure Result | Enum | Wipe, Heavy Damage, Boss Heals, Enrage |

#### Skilling Phase

| Column | Type | Description |
|---|---|---|
| Skill Required | Enum | Mining, Smithing, Fishing, Herblore, Woodcutting, Fletching, etc. |
| Skill Level Required | Integer | Minimum level in the skill |
| Action Description | String | What the player must do (e.g., "mine the weakened pillar", "brew the antidote") |
| Stations | Integer | Number of skilling stations in the arena |

#### Enrage Timer

| Column | Type | Description |
|---|---|---|
| Enrage Start | Integer (ticks) | Time after fight start when enrage begins |
| Enrage Interval | Integer (ticks) | Time between each enrage stack |
| Enrage Max Hit Increase | Float (%) | Max hit increase per enrage stack |
| Enrage Attack Speed Increase | Integer (ticks) | Attack speed increase per enrage stack |
| Enrage Cap | Integer | Maximum enrage stacks |

#### Required Item

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Item required to interact with the mechanic |
| Item Consumed | Boolean | Whether the item is consumed on use |
| Item Source | Enum | Bring Your Own, Dropped by Boss, Found in Arena |

---

### 5. Boss Loot

Each row defines one drop from a boss. Follows the same structure as monster drop tables with additional boss-specific columns.

#### Standard Drop Columns

| Column | Type | Description |
|---|---|---|
| Drop ID | Integer | Unique drop entry identifier |
| Boss ID | Integer | Links to Boss List |
| Item ID | Integer | Item dropped |
| Quantity Min | Integer | Minimum quantity |
| Quantity Max | Integer | Maximum quantity |
| Drop Rate | Float | Base drop rate (e.g., 1/128) |
| Table | Enum | Always, Common, Uncommon, Rare, Very Rare, Tertiary |

#### Boss-Specific Loot Columns

| Column | Type | Description |
|---|---|---|
| MVP Bonus | Float | Drop rate multiplier applied to the top damage contributor |
| Damage Weighted | Boolean | Whether the unique roll chance scales with damage contribution |
| Minimum Damage Threshold | Float (%) | Minimum percentage of total damage required to be eligible for this drop |
| Split Method | Enum | FFA (free-for-all), Damage Based, Round Robin, Coinshare, Lootshare |
| KC Milestone Pet Chance | JSON | Kill count thresholds with pet drop rate (e.g., `{"500": "1/1000", "1000": "1/500"}`) |
| Hard Mode Exclusive | Boolean | Only drops in Hard or Awakened difficulty |
| Awakened Exclusive | Boolean | Only drops in Awakened difficulty |
| Collection Log | Boolean | Whether this drop appears in the collection log |

---

### 6. Boss Difficulty Modes

Each row defines the stat and mechanic modifications for a difficulty tier of a boss.

| Column | Type | Description |
|---|---|---|
| Boss ID | Integer | Links to Boss List |
| Difficulty | Enum | Normal, Hard, Awakened |
| HP Multiplier | Float | Multiplier applied to base HP (e.g., 1.0, 1.5, 2.5) |
| Max Hit Multiplier | Float | Multiplier applied to all max hits |
| Attack Speed Multiplier | Float | Multiplier applied to attack speed (lower = faster) |
| Defence Multiplier | Float | Multiplier applied to all defence bonuses |
| Additional Mechanics | List[Integer] | Mechanic IDs that are only active in this difficulty |
| Removed Safe Spots | Boolean | Whether safe spots are disabled in this difficulty |
| Exclusive Drop IDs | List[Integer] | Drop IDs only available in this difficulty |
| Lockout Timer | Integer (hours) | Cooldown before the boss can be fought again in this difficulty |
| Enrage Multiplier | Float | Multiplier on enrage timer speed (lower = enrages faster) |

---

### 7. Raids

Each row defines one raid instance. Sub-tables handle rooms, parties, difficulty, death, supply, scoring, and loot.

#### Raid Identity

| Column | Type | Description |
|---|---|---|
| Raid ID | Integer | Unique raid identifier |
| Name | String | Display name |
| Description | String | Lore or flavor text |

#### Room Sequence

| Column | Type | Description |
|---|---|---|
| Room IDs | List[Integer] | Ordered list of room IDs in the raid |
| Randomization | Enum | Fixed, Randomized, Player-Chosen Path |
| Path System | Boolean | Whether the raid uses multiple paths |
| Path Count | Integer | Number of available paths |
| Path Order | Enum | Linear, Any Order, Paths Affect Each Other |
| Floor Count | Integer | Number of floors or levels |
| Scouting | Boolean | Whether players can preview rooms before committing |

#### Party

| Column | Type | Description |
|---|---|---|
| Min Players | Integer | Minimum party size |
| Max Players | Integer | Maximum party size |
| Role System | Enum | None, Tank/DPS/Healer/Base, Custom Roles |
| Custom Roles | JSON | Role definitions if Custom (e.g., `[{"name": "Bomb", "description": "Handles explosives"}]`) |
| Party Leader Mechanics | Boolean | Whether the party leader has special responsibilities |
| Join Restrictions | JSON | Requirements to join (e.g., `{"combat_level": 100, "quest_ids": [45]}`) |
| Iron Rules | Enum | Normal, Solo Only, Group Iron Only, Any |

#### Difficulty

| Column | Type | Description |
|---|---|---|
| Invocation System | Boolean | Whether the raid uses toggleable invocations |
| Invocations | JSON | List of invocations with descriptions and point values (e.g., `[{"name": "Walk the Path", "points": 50, "description": "Floor hazards deal more damage"}]`) |
| Raid Level | Integer | Cumulative score from active invocations |
| Stat Scaling Per Level | Float (%) | Percentage stat increase per raid level point |
| Boss-Specific Invocations | JSON | Invocations that only modify specific bosses within the raid |
| Difficulty Tiers | JSON | Named tiers with minimum raid level (e.g., `{"Entry": 0, "Normal": 150, "Expert": 300}`) |
| Hard Mode | Boolean | Whether a distinct hard mode exists separate from invocations |
| Challenge Mode | Boolean | Whether a challenge mode exists with unique rewards |
| Path Leveling | Boolean | Whether path difficulty increases with repeated completions |

#### Death

| Column | Type | Description |
|---|---|---|
| Lives System | Enum | Shared Pool, Individual, Unlimited |
| Total Lives | Integer | Number of lives (if Shared Pool or Individual) |
| Death Penalty | Enum | Lose Points, Lose Items, HC Status Lost, Team Wipe, None |
| Spectate While Dead | Boolean | Whether dead players can spectate |
| Wipe Condition | String | What triggers a full team wipe (e.g., "all lives lost", "boss enrage max") |
| Checkpoint System | Boolean | Whether checkpoints exist within the raid |
| Checkpoint Room IDs | List[Integer] | Rooms that serve as checkpoints |
| Disconnect Handling | Enum | Hold Spot (timed), Kick After Timeout, Rejoin Anytime |
| Disconnect Timeout | Integer (seconds) | Time before a disconnected player is removed |

#### Supply

| Column | Type | Description |
|---|---|---|
| Supply Mode | Enum | Bring Your Own, Raid Provided, Craft In-Raid, Hybrid |
| Supply Chests | Boolean | Whether supply chests exist in the raid |
| Spirit Helpers | Boolean | Whether NPCs provide supplies |
| Craftable Skills | List[Enum] | Skills available for in-raid crafting (Farming, Fishing, Herblore, etc.) |
| Supply Scaling | Boolean | Whether supplies are reduced at higher difficulty |
| Supply Reduction Per Level | Float (%) | Percentage reduction in supplies per raid level point |
| Storage Units | Boolean | Whether shared or personal storage exists in the raid |
| Storage Type | Enum | Shared, Personal, Tiered by Skill |
| Resupply Points | Integer | Number of resupply opportunities during the raid |
| No Resupply Option | Boolean | Whether a no-resupply invocation or mode exists |

#### Scoring

| Column | Type | Description |
|---|---|---|
| Point Sources | JSON | Actions that earn points (e.g., `{"damage": 1, "skilling": 2, "puzzle": 5, "supply_creation": 1, "objectives": 10}`) |
| Point Penalties | JSON | Actions that lose points (e.g., `{"death": -10, "wrong_action": -5, "overtime_per_tick": -1}`) |
| Point Cap | Integer | Maximum points earnable in a single raid (0 = no cap) |
| Minimum Threshold | Integer | Minimum points required to receive any rewards |
| MVP Tracking | Boolean | Whether MVP is tracked and rewarded |
| Score Type | Enum | Team Total, Individual, Both |
| Performance Titles | JSON | Titles earned at score thresholds (e.g., `{"S": 90, "A": 75, "B": 50}`) |
| Time Tracking | Boolean | Whether completion time is recorded for speedrun rankings |

#### Loot

| Column | Type | Description |
|---|---|---|
| Loot Type | Enum | Individual Chest, Shared Chest |
| Points Scaling | Boolean | Whether drop rates scale with points earned |
| Points Rate Formula | String | Formula mapping points to drop rate (e.g., "base_rate * (points / cap)") |
| Damage Weighted Uniques | Boolean | Whether unique drop chance scales with damage contribution |
| MVP Loot Bonus | Float | Drop rate multiplier for MVP |
| Raid Level Drop Bonus | Float (%) | Drop rate increase per raid level point |
| Unique Limit | Integer | Maximum number of uniques that can drop per raid (0 = no limit) |
| Consolation Loot | Boolean | Whether non-unique guaranteed loot exists |
| Lockout Timer | Integer (hours) | Cooldown before loot can be earned again |
| Reroll Tokens | Boolean | Whether tokens can be spent to reroll the loot chest |
| Forfeit Option | Boolean | Whether players can forfeit loot for bonus points or tokens |
| Role-Based Priority | Boolean | Whether certain drops prioritize specific roles |
| Chest Visual Indicators | Boolean | Whether the chest appearance hints at loot quality |

---

#### 7a. Raid Rooms (Sub-Spreadsheet)

Each row defines one room within a raid. Linked to the parent raid by Raid ID.

| Column | Type | Description |
|---|---|---|
| Room ID | Integer | Unique room identifier |
| Raid ID | Integer | Links to Raids table |
| Room Name | String | Display name |
| Room Type | Enum | Boss, Puzzle, Skilling, Supply, Scavenger, Rest, Transition |
| Boss ID | Integer | Links to Boss List (if Boss room) |
| Puzzle ID | Integer | Links to puzzle definitions (if Puzzle room) |
| Skills Available | List[Enum] | Skills usable in this room |
| Resources Available | JSON | Gatherable resources and quantities |
| Timer | Integer (ticks) | Time limit for the room (0 = no limit) |
| Scaling | Boolean | Whether room difficulty scales with raid level |
| Required | Boolean | Whether the room must be completed to progress |
| Room-Specific Drops | List[Integer] | Drop IDs exclusive to this room |

---

### 8. Boss Behaviors Library

Master library of reusable behaviors. Bosses reference these by behavior ID rather than redefining patterns. Each behavior can be shared across multiple bosses.

#### Behavior Identity

| Column | Type | Description |
|---|---|---|
| Behavior ID | Integer | Unique behavior identifier |
| Behavior Name | String | Display name |
| Category | Enum | Attack Pattern, Movement, Spawning, Defensive, Environmental, Player Interaction, Punishment, Transition |

---

#### Attack Patterns

| Behavior | Description |
|---|---|
| Cycle Styles | Rotates through attack styles in a fixed order |
| Random Style | Chooses attack style randomly each attack |
| Distance Based | Uses melee at close range, ranged or magic at distance |
| Prayer Reactive | Switches to the style the player is not praying against |
| Fixed Rotation | Follows a set sequence of specific named attacks |
| Escalating | Each successive attack deals more damage or adds effects |

#### Movement

| Behavior | Description |
|---|---|
| Stationary | Does not move from spawn position |
| Chase | Follows the player with highest threat |
| Kite | Maintains distance from the nearest player |
| Teleport | Teleports to a random or specific tile periodically |
| Phase Reposition | Moves to a set position when transitioning phases |
| Patrol | Walks a predefined path when not engaged |

#### Spawning

| Behavior | Description |
|---|---|
| Summon Adds (Heal) | Spawns minions that heal the boss each tick they are alive |
| Summon Adds (Explode) | Spawns minions that explode for AoE damage if not killed in time |
| Summon Adds (Shield) | Spawns minions that grant the boss a damage-absorbing shield |
| Timed Spawns | Minions spawn on a fixed timer regardless of boss state |
| Threshold Spawns | Minions spawn when the boss reaches an HP threshold |

#### Defensive

| Behavior | Description |
|---|---|
| Protection Prayers (Cycles) | Boss cycles overhead prayers on a timer |
| Protection Prayers (Reactive) | Boss activates prayer matching the player's attack style |
| Protection Prayers (Permanent) | Boss has a permanent overhead prayer, must be bypassed |
| Shield Phase | Boss becomes invulnerable until a condition is met |
| Damage Cap | Boss cannot take more than X damage per hit |
| Damage Reflection | Percentage of damage dealt to the boss is reflected to the attacker |
| Style Immunity | Boss is immune to one or more attack styles |
| Enrage | Boss gains stacking stat buffs over time |

#### Environmental

| Behavior | Description |
|---|---|
| Arena Shrinks | The safe area of the arena gets smaller over time |
| Floor Tiles Rotate | Tiles cycle between safe and dangerous states |
| Pillars Spawn | Pillars appear that can be used as cover |
| Pillars Collapse | Existing pillars are destroyed, removing cover |
| Falling Debris | Random tiles are targeted by falling objects |
| Poison Zones | Areas of the floor deal poison damage when stood on |
| Fire Zones | Areas of the floor deal fire damage when stood on |
| Ice Zones | Areas of the floor apply freeze or slow when stood on |
| Darkness | Visibility is reduced, limiting the player's view radius |
| Water Rising | Water level rises over time, restricting safe tiles |

#### Player Interaction

| Behavior | Description |
|---|---|
| Prayer Switch | Player must switch prayers to match the boss's attack style |
| Move to Safe Tile | Player must stand on a specific highlighted tile |
| Hide Behind Obstacle | Player must line-of-sight the boss using a pillar or wall |
| Stand on Specific Tile | Player must stand on an exact tile for a mechanic to resolve |
| Group Up | All players must stand near each other to split damage |
| Spread Out | All players must be apart to avoid stacking AoE damage |
| Pass Object | Players must pass an item between each other before a timer expires |
| Use Specific Item | Player must use a specific item from inventory on the boss or arena |
| Use Specific Weapon | Player must equip and attack with a designated weapon |
| Use Specific Skill | Player must use a non-combat skill on an arena object |
| Coordinate Roles | Players must fulfill assigned roles simultaneously |
| Bounce Projectile | Player must redirect a projectile back at the boss |
| Interrupt / Stun | Player must stun or interrupt the boss to cancel a channeled attack |

#### Punishment

| Behavior | Description |
|---|---|
| Instant Kill | Player is killed immediately for failing a mechanic |
| Heavy Damage | Player takes a large unavoidable hit for failing a mechanic |
| Stat Drain | Player's combat stats are drained for failing a mechanic |
| Prayer Drain | Player's prayer points are drained for failing a mechanic |
| Teleport Player | Player is teleported to a dangerous position for failing a mechanic |
| Confuse Controls | Player's movement inputs are reversed or randomized temporarily |
| Heal Boss | Boss recovers HP as punishment for player failure |
| Enrage Boss | Boss gains permanent enrage stacks as punishment |
| Spawn More Adds | Additional minions are summoned as punishment |

#### Transitions

| Behavior | Description |
|---|---|
| HP Threshold | Phase changes when boss reaches a specific HP percentage |
| Timer | Phase changes after a set duration |
| Mechanic Completion | Phase changes when players complete a required mechanic |
| Player Action | Phase changes when a player performs a specific action |
| Damage Threshold | Phase changes when a total damage threshold is reached within a window |

---

#### Additional Mechanics Reference

Advanced mechanics drawn from OSRS and RS3 boss design. Each can be composed from the behaviors above or implemented as standalone mechanic rows.

| Mechanic | Description |
|---|---|
| Multi-Body Parts | Boss has separate hittable parts (head, hands, tail) with independent HP and damage reduction. Disabling a part removes an attack or buff. |
| Form / Color System | Boss cycles between forms or colors, each with a different weakness. Rotation may be predictable or random. |
| Environmental Vent / Pool | Boss gains a buff that can only be removed by luring it to a specific arena location (vent, pool, pillar). |
| Modifier Selection Between Waves | Between combat waves, players choose a modifier that increases difficulty and rewards (Colosseum style). |
| Customizable Mechanics Toggle | Players toggle individual mechanics on or off before the fight. More mechanics active means higher HP and better rewards (Arch-Glacor style). |
| Resource Bar System | Boss has a secondary resource bar (energy, shadow, blight) alongside HP that drives special attacks or phase changes. |
| Resurrection / Revival | Boss or its minions revive dead enemies. Dead adds must be permanently killed through a specific method. |
| Moving Shield / Cover | A mobile object in the arena provides cover. Players must stay behind it as it moves to avoid lethal damage. |
| Life Siphon | Damage dealt to players heals the boss at a multiplied rate. Players must minimize hits taken during the siphon window. |
| Skilling Boss Roles | Arena has multiple stations, each requiring a different skill. Players split across stations to progress the fight. |
| Prayer Counter | Boss detects which prayer is active and uses the counter-style, punishing predictable prayer usage. |
| Kill Count Gate | A set number of minions must be killed before the boss room opens. |
| Bodyguard System | Boss has permanent companions that must be managed alongside the main target. |
| Weekly / Daily Rotation | Boss mechanics or forms rotate on a real-world schedule. |
| Team Roles (Tank / Base / Bomb) | Players are assigned roles with distinct responsibilities. Role performance affects loot eligibility. |
| Damage Contribution Loot | Solo HP is assigned per player. MVP receives a bonus roll. Players below a minimum damage threshold receive no unique chance. |
| Dimension Split | Part of the team is sent to an alternate realm and must complete a task while the rest hold the main arena. |
| Triple / Multi-Hit Attacks | Boss fires three or more projectiles in rapid succession, requiring multiple prayer switches or movements. |
| Pad / Rune Charging | Players must charge pads or runes scattered across the arena by standing on them or using items. |
| Arena Destruction | The arena is progressively destroyed during the fight, reducing safe space permanently. |
| Enrage / Streak System | Boss enrage scales from 0 to 4000%+. Streaking consecutive kills increases loot multiplier but death forfeits the streak. |
| Path / Route System | Raid has 2-3 paths that rotate daily. Each path has different rooms and bosses. |
| Story / Practice / Hard Modes | Story mode has reduced stats for learning. Practice mode has no loot but no death cost. Hard mode has exclusive rewards. |
| Trash Mob Runs | Waves of regular monsters between boss rooms that must be cleared to progress. |
| Checkpoint System | Specific rooms act as checkpoints. On death, players respawn at the last checkpoint instead of restarting. |
| Death Cost | Gold fee charged on death, scaling with equipped item value. |
| Aggro / Tank System | Boss targets the player with highest threat. Tank gear and actions generate threat. Losing aggro redirects the boss to other players. |
| Web / Reflect Mechanics | Boss traps a player in a web or reflects all damage back. Teammates must free the trapped player or stop attacking during reflect. |
| Acid / Absorption | Boss applies an acid debuff that deals damage when the player moves, or an absorption buff that heals the boss from incoming damage. |
| Daily / Weekly Rotation / Spotlight | A featured boss receives boosted drop rates or reduced instance costs on a rotating schedule. |

---

## Views

Predefined filtered views of the Boss List spreadsheet for common workflows.

| View | Filter |
|---|---|
| All | No filter, all bosses |
| By Category | Grouped by category (World, Wilderness, Instanced, Slayer, Quest, Raid, Skilling, Sporadic) |
| By Combat Level | Sorted by boss combat level ascending |
| By Player Count | Grouped by participation type (Solo, Group, Both) and sorted by max players |
| By Difficulty | Grouped by available difficulty modes |
| By Drop | Filtered to bosses that drop a specific item |
| By Phase Count | Sorted by number of phases descending |
| By Skill Required | Filtered to bosses that require specific skill levels to access or fight |
