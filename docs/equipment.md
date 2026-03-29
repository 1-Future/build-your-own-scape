# Equipment

Game design document for the Equipment plugin. This system covers all wearable items, their combat and skilling properties, special attacks, set bonuses, upgrade chains, and scaling rules.

---

## Module: Worn Equipment

### Module Toggles

Each toggle enables or disables a subsystem within the equipment module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Weight | Items contribute to carry weight, affecting run energy drain |
| Degradation | Items lose durability through use and require repair |
| Quest Requirements | Items require quest completion to equip |
| Stat Requirements | Items require minimum skill levels to equip |
| Combat Bonuses | Items provide offensive and defensive stat modifiers |
| Untradeables | Certain items cannot be traded between players |
| Set Bonuses | Wearing multiple items from a set grants additional effects |
| Enchanting | Items can be enchanted using skills and materials |
| Imbuing | Items can be imbued through minigames, achievements, or other sources |
| Weapon Scaling | Weapon damage scales with player progression or conditions |
| Special Attacks | Weapons have unique special attack abilities |
| Fashion | Items support cosmetic color customization |

---

### Slot Categories

15 equipment slots. The interface supports a dropdown filter to view items by slot.

| # | Slot | Notes |
|---|---|---|
| 1 | Head | Helmets, hats, hoods, crowns |
| 2 | Cape | Cloaks, capes, wings |
| 3 | Neck | Amulets, necklaces, scarves |
| 4 | Ammo | Configurable number of sub-slots. Supports physical ammo (arrows, bolts, javelins) and magic ammo (runes for mages). Rune ammo occupies the same slot system. |
| 5 | Weapon | One-handed weapons (swords, wands, daggers) |
| 6 | Shield | Shields, defenders, books |
| 7 | Two-handed | Occupies both weapon and shield slots |
| 8 | Body | Platebodies, robes, chainmail |
| 9 | Legs | Platelegs, skirts, chaps |
| 10 | Hands | Gloves, vambraces, gauntlets |
| 11 | Feet | Boots, shoes, sandals |
| 12 | Ring | Rings |
| 13 | Set | A virtual slot representing a matched set (see Set Bonuses) |
| 14 | Aura | Passive effect slot, typically time-limited or earned |
| 15 | Prayer | Prayer-specific equipment (god books, relics, prayer scrolls) |

---

### Combat Styles

Equipment is categorized by combat style. Hybrid items span multiple styles.

| Style | Description |
|---|---|
| Melee | Close-range physical combat |
| Ranged | Projectile-based physical combat |
| Magic | Spell-based combat using runes or catalysts |
| Prayer | Faith-based combat and support |
| Hybrid | Effective across multiple styles with no penalty |
| Summoning | Familiar-enhanced combat |
| Necromancy | Undead-themed combat with unique mechanics |

---

## Linked Spreadsheets

Five spreadsheets define the full equipment data model. All are linked by item ID or set ID.

---

### 1. Equipment (Main Table)

The primary spreadsheet. Every equippable item has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique item identifier |
| Name | String | Display name |
| Slot | Enum | One of the 15 slot categories |
| Combat Style | Enum | Melee, Ranged, Magic, Prayer, Hybrid, Summoning, Necromancy |

#### Core Properties

| Column | Type | Description |
|---|---|---|
| Weight | Float | Weight in kg, affects run energy drain |
| Tradeable | Boolean | Whether the item can be traded |
| Fashion | Boolean | Whether the item supports color customization |
| No Properties | Boolean | Purely cosmetic item with no stats (overrides all combat/skilling fields) |
| Fashion JSON | JSON | Defines color slots per item (e.g., `{"primary": "#FF0000", "trim": "#GOLD"}`) |

#### Stat Requirements

One column per skill. Value is the minimum level required to equip.

| Column | Type |
|---|---|
| Attack Req | Integer |
| Strength Req | Integer |
| Defence Req | Integer |
| Ranged Req | Integer |
| Magic Req | Integer |
| Prayer Req | Integer |
| Hitpoints Req | Integer |
| Summoning Req | Integer |
| Necromancy Req | Integer |

#### Quest Requirements

| Column | Type | Description |
|---|---|---|
| Quest IDs | List[Integer] | Quest IDs that must be completed to equip |

#### Combat Bonuses

Attack bonuses, strength bonuses, and defence bonuses per combat style.

| Column | Type | Description |
|---|---|---|
| Attack Stab | Integer | Stab attack bonus |
| Attack Slash | Integer | Slash attack bonus |
| Attack Crush | Integer | Crush attack bonus |
| Attack Ranged | Integer | Ranged attack bonus |
| Attack Magic | Integer | Magic attack bonus |
| Strength Melee | Integer | Melee strength bonus |
| Strength Ranged | Integer | Ranged strength bonus |
| Strength Magic | Integer | Magic damage bonus |
| Defence Stab | Integer | Stab defence bonus |
| Defence Slash | Integer | Slash defence bonus |
| Defence Crush | Integer | Crush defence bonus |
| Defence Ranged | Integer | Ranged defence bonus |
| Defence Magic | Integer | Magic defence bonus |
| Prayer Bonus | Integer | Prayer bonus |

#### Max HP Modifier

| Column | Type | Description |
|---|---|---|
| Max HP Flat | Integer | Flat increase/decrease to max HP |
| Max HP Percent | Float | Percentage increase/decrease to max HP |

---

#### PvM On-Hit Effects

These effects trigger on successful hits against monsters. An identical copy of every column exists prefixed with `PvP` for player-versus-player combat.

| Column | Type | Description |
|---|---|---|
| Chance to Poison | Float (%) | Probability of poisoning the target on hit |
| Chance to Heal | Float (%) | Probability of healing self on hit |
| Heal Amount | Integer | HP restored when heal triggers |
| Chance to Freeze | Float (%) | Probability of freezing the target in place |
| Chance to Slow | Float (%) | Probability of slowing the target |
| Slow Duration | Integer (ticks) | Duration of slow effect |
| Chance to Burn | Float (%) | Probability of applying a burn DoT |
| Chance to Bleed | Float (%) | Probability of applying a bleed DoT |
| Chance to Stun | Float (%) | Probability of stunning the target |
| Chance to Disarm | Float (%) | Probability of disarming the target (removes weapon temporarily) |
| Chance to Silence | Float (%) | Probability of silencing (prevents magic/prayer) |
| Multi-hit Count | Integer | Number of additional hits per attack |
| Lifesteal | Float (%) | Percentage of damage dealt restored as HP |
| Drain Max HP | Integer | Reduces target's maximum HP on hit |
| Damage Reflection | Float (%) | Percentage of incoming damage reflected back |

> PvP columns follow the same schema prefixed with `PvP_` (e.g., `PvP_Chance_to_Poison`).

---

#### Damage Modifiers

Separate PvM and PvP columns for each.

| Column | Type | Description |
|---|---|---|
| Damage Bonus per Beast Type | JSON | Map of beast type to bonus (% or flat). Example: `{"dragon": "+15%", "demon": "+10"}` |
| Damage Penalty per Beast Type | JSON | Map of beast type to penalty (% or flat) |
| Monster Type Allowlist | List[String] | Only effective against these monster types |
| Monster Individual Allowlist | List[Integer] | Only effective against these specific monster IDs |
| Bypass Prayer | Boolean | Damage ignores target prayer protection |

---

#### Leeching

Drains stats from the target and transfers them to the wielder. Separate PvM and PvP columns.

| Column | Type | Description |
|---|---|---|
| Leech Attack | Float | Amount leeched (flat or %) |
| Leech Strength | Float | Amount leeched (flat or %) |
| Leech Defence | Float | Amount leeched (flat or %) |
| Leech Ranged | Float | Amount leeched (flat or %) |
| Leech Magic | Float | Amount leeched (flat or %) |
| Leech Prayer | Float | Amount leeched (flat or %) |
| Leech Special Energy | Float | Amount leeched (flat or %) |
| Leech Run Energy | Float | Amount leeched (flat or %) |
| Leech Amount Type | Enum | `flat` or `percent` |
| Leech Duration | Variant | Duration in ticks, or `until_death` |

---

#### Scaling

| Column | Type | Description |
|---|---|---|
| Scales with Missing HP | Float (%) | Damage increases as player HP decreases |
| Scales with Full HP | Float (%) | Damage increases as player HP approaches max |
| Scales with Target Defence | Float (%) | Damage increases against higher-defence targets |
| Scales with Prayer Level | Float (%) | Damage increases with prayer level |
| Consecutive Hit Bonus | Float (%) | Damage increases per consecutive hit on same target |
| First Hit Bonus | Float (%) | Bonus damage on the first hit of combat |
| Low HP Bonus | Float (%) | Bonus damage when player HP is below a threshold |

---

#### Resource Manipulation

| Column | Type | Description |
|---|---|---|
| Drain Run Energy | Float | Drains target run energy on hit |
| Drain Prayer Points | Float | Drains target prayer points on hit |
| Drain Special Energy | Float | Drains target special attack energy on hit |
| Restore Prayer on Hit | Float | Restores player prayer points per hit |
| Restore Special on Hit | Float | Restores player special energy per hit |

---

#### Positioning

| Column | Type | Description |
|---|---|---|
| Knockback | Integer (tiles) | Pushes target away from player |
| Pull | Integer (tiles) | Drags target toward player |
| Teleblock | Boolean | Prevents target from teleporting |

---

#### Environmental

| Column | Type | Description |
|---|---|---|
| Extra Damage Wilderness | Float (%) | Bonus damage while in the wilderness |
| Biome Bonus | JSON | Map of biome ID to damage bonus |
| Weather Bonus | JSON | Map of weather type to damage bonus |
| Reduced Underground | Float (%) | Damage reduction when underground |

---

#### Conditional States

| Column | Type | Description |
|---|---|---|
| Full Inventory Penalty | Float (%) | Damage penalty when inventory is full |
| HP Threshold Activation | Integer | Effect only activates below this HP value |

---

#### Survivability

| Column | Type | Description |
|---|---|---|
| Damage Taken Cap | Integer | Maximum damage receivable per hit |
| Death Protection | Boolean | Prevents death once (consumed on trigger) |
| Auto-heal Threshold | Integer | Automatically heals when HP drops below this value |
| Auto-heal Amount | Integer | Amount healed by auto-heal |
| Damage Reflection | Float (%) | Percentage of incoming damage reflected to attacker |

---

#### Tempo

| Column | Type | Description |
|---|---|---|
| Attack Speed Bonus | Integer (ticks) | Reduces attack interval |
| Cooldown Reduction | Float (%) | Reduces special attack cooldown |
| Run Energy Regen Rate | Float (multiplier) | Multiplier on run energy regeneration |
| Prayer Regen Rate | Float (multiplier) | Multiplier on prayer point regeneration |

---

#### Skilling Properties

| Column | Type | Description |
|---|---|---|
| XP Boost Percent | Float (%) | Bonus XP per action (per piece) |
| XP Boost Skill | Enum | Which skill receives the XP boost |
| XP Boost Condition | String | Condition for the boost (e.g., "while in guild", "during night") |
| XP Stacking | Boolean | Whether this XP boost stacks with other equipped pieces |
| XP Stack Cap | Float (%) | Maximum total XP boost from stacking |
| Success Rate Bonus | Float (%) | Increases success chance for skilling actions |
| Resource Depletion Prevention | Float (%) | Chance the resource node does not deplete |
| Burn/Fail Chance Reduction | Float (%) | Reduces chance of burning food, failing craft, etc. |
| Yield Bonus | Float (%) | Increases base yield of gathering actions |
| Chance for Double Resource | Float (%) | Chance to receive double the gathered resource |
| Chance to Save Materials | Float (%) | Chance to not consume materials on craft |
| Auto-process | Boolean | Gathered resources are automatically processed (e.g., ore to bar) |
| Invisible Skill Boost | Integer | Hidden skill level increase (not visible to player) |
| Storage Capacity | Integer | Extra storage slots granted (e.g., coal bag, gem bag) |
| Environmental Protection | String | Protection type granted (e.g., "heat", "cold", "poison_gas") |

---

#### Luck and Loot

| Column | Type | Description |
|---|---|---|
| Drop Rate Bonus | Float (%) | Increases drop rate of items from monsters |
| Double Loot Chance | Float (%) | Chance to double a loot drop |
| Rare Drop Table Access | Boolean | Grants access to the rare drop table |
| Bonus Loot Spawns | Integer | Additional loot rolls per kill |

---

#### Drain Resistance

| Column | Type | Description |
|---|---|---|
| Prayer Drain Reduction | Float (%) | Reduces prayer point drain rate |
| Run Energy Drain Reduction | Float (%) | Reduces run energy drain rate |
| Stat Decay Prevention | Boolean | Prevents boosted stats from decaying over time |
| Boosted Stat Persistence | Integer (ticks) | Duration before boosted stats begin to decay |

---

#### Degradation

| Column | Type | Description |
|---|---|---|
| Degradation Type | Enum | `charges`, `combat_damage`, `time_in_combat`, `hits_taken` |
| Max Charges | Integer | Maximum charges or durability points |
| Max Durability | Integer | Maximum durability (if separate from charges) |
| Repair Method | String | How the item is repaired (e.g., "NPC", "anvil", "POH") |
| Repair Cost | JSON | Cost to repair (e.g., `{"coins": 50000}` or `{"bars": 5}`) |
| On Depletion | Enum | `destroys` (item is lost) or `stops_working` (unequips/goes inert) |

---

#### Resource Cost Removal

| Column | Type | Description |
|---|---|---|
| Skip Material Requirements | Boolean | Skilling actions do not consume materials |
| Trade-off | String | Penalty for skipping materials (e.g., "no_xp", "reduced_damage", "half_result") |

---

#### AoE Properties

Separate PvM and PvP columns for each.

| Column | Type | Description |
|---|---|---|
| AoE Enabled | Boolean | Whether the item hits multiple targets |
| AoE Shape | Enum | `square`, `radius`, `cone`, `line` |
| AoE Size | Integer | Radius, length, or width in tiles |
| AoE Max Targets | Integer | Maximum number of targets hit |
| Damage Falloff | Float (%) | Damage reduction per additional target |
| Friendly Fire | Boolean | Whether AoE can hit friendly players/NPCs |
| Hits Self | Boolean | Whether AoE damages the wielder |

---

#### Self-Inflicting Modifiers

Costs and penalties applied to the wielder. Separate PvM and PvP columns.

| Column | Type | Description |
|---|---|---|
| HP Cost per Attack | Variant | Flat HP or percentage of current/max HP per attack |
| HP Cost per Special | Variant | Flat HP or percentage per special attack |
| Prayer Drain per Attack | Float | Prayer points consumed per attack |
| Run Energy Drain per Attack | Float | Run energy consumed per attack |
| Stat Reduction on Use | JSON | Stats reduced per use (e.g., `{"attack": -5}`) |
| Recoil Damage | Integer | Flat damage dealt to self per attack |
| Max HP Reduction While Equipped | Integer | Max HP is reduced while item is worn |
| Damage to Self on Miss | Integer | Damage dealt to self on a missed attack |
| Can Self-Inflict Kill | Boolean | Whether self-damage can reduce HP to 0 |

---

#### Location Access

| Column | Type | Description |
|---|---|---|
| Grants Access to Location | List[Integer] | Location IDs the item grants access to |
| Required to Enter | Boolean | Whether the item must be equipped to enter |
| Per Piece or Full Set | Enum | `per_piece` (any piece works) or `full_set` (all set pieces required) |

---

#### Teleportation

| Column | Type | Description |
|---|---|---|
| Teleport Locations | List[JSON] | List of teleport destinations with names and coordinates |
| Teleport Charges | Integer | Number of teleport charges |
| Rechargeable | Boolean | Whether charges can be replenished |
| Recharge Method | String | How to recharge (e.g., "fountain_of_rune", "coins", "quest_item") |
| Wilderness Level Cap | Integer | Maximum wilderness level from which teleport works |

---

#### Auto-Trigger

| Column | Type | Description |
|---|---|---|
| Auto-Teleport HP Threshold | Integer | Automatically teleports player when HP drops below value |
| Auto-Heal HP Threshold | Integer | Automatically heals when HP drops below value |
| Auto-Restore Prayer Threshold | Integer | Automatically restores prayer when points drop below value |
| Damage Reflection Passive | Float (%) | Passively reflects damage without requiring an on-hit event |

---

#### Charges System

| Column | Type | Description |
|---|---|---|
| Charge Capacity | Integer | Maximum charges the item can hold |
| Charge Type | Enum | `uses` (per action), `damage_absorbed` (total damage soaked), `items_processed` (per item) |
| On Depletion | Enum | `destroys` or `stops_working` |
| Rechargeable | Boolean | Whether the item can be recharged |
| Recharge Method | String | Method to restore charges |

---

### 2. Special Attacks (Linked by Item ID)

Each row defines a special attack available to a specific weapon.

#### Core

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | The weapon this special attack belongs to |
| Name | String | Display name of the special attack |
| Energy Cost | Float (%) | Percentage of special attack energy consumed (0-100) |
| Category | Enum | `Melee`, `Ranged`, `Magic`, `Prayer` |

#### Overrides

| Column | Type | Description |
|---|---|---|
| Bypass Weapon Speed | Boolean | Special ignores the weapon's normal attack speed |
| Bypass Prayer Protection | Boolean | Special ignores target's prayer protection |
| Bypass Defence Roll | Boolean | Special ignores target's defence calculation |
| Bypass Distance | Boolean | Special ignores weapon range limitations |

#### Effects

| Column | Type | Description |
|---|---|---|
| Effect 1 | Enum (dropdown) | Primary effect from the effects library |
| Effect 2 | Enum (dropdown) | Secondary effect |
| Effect 3 | Enum (dropdown) | Tertiary effect |

#### Restrictions

| Column | Type | Description |
|---|---|---|
| Monster Allowlist | List[Integer] | Only usable against these monster IDs |
| Monster Blocklist | List[Integer] | Cannot be used against these monster IDs |
| Monster Type Bonus | JSON | Bonus damage vs specific monster types (e.g., `{"dragon": "+25%"}`) |
| Monster Type Penalty | JSON | Reduced damage vs specific monster types |
| Location Locked | List[Integer] | Only usable in these location IDs |
| HP Threshold Activation | Integer | Only usable below this HP value |
| PvP Only | Boolean | Only usable against other players |
| PvE Only | Boolean | Only usable against monsters |
| PvP Version | Integer | Links to a separate special attack ID used in PvP |
| PvM Version | Integer | Links to a separate special attack ID used in PvM |

#### Cooldown

| Column | Type | Description |
|---|---|---|
| Cooldown Ticks | Integer | Cooldown in ticks (alternative or additional to energy cost) |
| Buff Generation | Boolean | Special generates a buff applied to the next normal attack |

#### Stacking and Build Mechanics

| Column | Type | Description |
|---|---|---|
| Generates Stacks | Boolean | Each hit generates a stack |
| Max Stacks | Integer | Maximum stacks achievable |
| Damage Bonus per Stack | Float (%) | Damage increase per stack held |
| Consume Stacks for Burst | Boolean | All stacks can be consumed for a single amplified attack |
| Stack Decay | Enum | `over_time` (stacks decay after duration), `weapon_swap` (stacks lost on swap), `none` |
| Stack Decay Duration | Integer (ticks) | Time before stacks begin to decay (if `over_time`) |

#### Enemy Marking

| Column | Type | Description |
|---|---|---|
| Mark Target on Special | Boolean | Applies a mark to the target |
| Bonus Damage vs Marked | Float (%) | Extra damage dealt to marked targets |
| Mark Duration | Integer (ticks) | How long the mark persists |
| Max Marks | Integer | Maximum marks on a single target |
| Mark Consumed on Hit | Boolean | Mark is removed when hit (`true`) or persists (`false`) |

---

### 3. Set Bonuses (Linked by Set ID)

Each row defines a set bonus. Items reference a set ID to indicate membership.

| Column | Type | Description |
|---|---|---|
| Set ID | Integer | Unique identifier for the set |
| Set Name | String | Display name (e.g., "Barrows - Dharok's") |
| Required Pieces | List[Integer] | Item IDs that belong to this set |
| Partial Bonus | Boolean | Whether wearing fewer than all pieces grants a partial bonus |
| Partial Bonus Values | JSON | Bonus granted per piece count (e.g., `{"2": "+5% accuracy", "3": "+10% damage"}`) |
| Full Set Bonus | String | Description of the bonus when all pieces are worn |
| Bonus Type | Enum | `flat`, `percent`, `chance_on_hit`, `scaling` |
| Weapon Restriction | List[Integer] | Weapon IDs required for the set bonus to activate (empty = any) |
| Effect 1 | Enum (dropdown) | Primary set effect from the effects library |
| Effect 2 | Enum (dropdown) | Secondary set effect |
| Effect 3 | Enum (dropdown) | Tertiary set effect |
| PvP Version | JSON | Overrides for PvP (any field can be overridden) |
| PvM Version | JSON | Overrides for PvM (any field can be overridden) |
| Condition: Equipped | Boolean | Bonus only active while all pieces are equipped (not just in inventory) |
| Condition: Inventory | Boolean | Bonus active if pieces are in inventory (not necessarily equipped) |
| Condition: Stat | JSON | Stat requirements for the set bonus to activate (e.g., `{"prayer": 70}`) |
| Condition: Quest | List[Integer] | Quest IDs required for the set bonus |

---

### 4. Upgrade Chains (Linked by Item ID)

Each row defines one upgrade step. Multiple rows per item create branching or sequential upgrade paths.

#### Core

| Column | Type | Description |
|---|---|---|
| Source Item ID | Integer | The item being upgraded |
| Result Item ID | Integer | The item produced after upgrade |
| Upgrade Type | Enum | `enchant` or `imbue` |

#### Enchanting (when Upgrade Type = enchant)

| Column | Type | Description |
|---|---|---|
| Skill | Enum | Skill required (e.g., Magic, Crafting, Smithing) |
| Skill Level | Integer | Minimum skill level required |
| Materials | JSON | Materials consumed (e.g., `{"cosmic_rune": 1, "diamond": 1}`) |

#### Imbuing (when Upgrade Type = imbue)

| Column | Type | Description |
|---|---|---|
| Minigame Points | JSON | Minigame and point cost (e.g., `{"nightmare_zone": 650000}`) |
| Quest | Integer | Quest ID required to imbue |
| Combat Achievement | Integer | Combat achievement ID required |
| Boss KC | JSON | Boss kill count required (e.g., `{"vorkath": 50}`) |
| Skilling Milestone | JSON | Skilling milestone required (e.g., `{"mining_level": 92}`) |
| Currency | JSON | Currency cost (e.g., `{"coins": 500000, "tokkul": 50000}`) |

#### Branching

| Column | Type | Description |
|---|---|---|
| Branch Group | String | Groups upgrade options that share the same source item (player picks one path) |

#### Upgrade Styles

| Column | Type | Description |
|---|---|---|
| Upgrade Style | Enum | `transformation`, `modular`, `scaling` |

- **Transformation**: The source item becomes an entirely new item. Irreversible unless a separate downgrade row exists.
- **Modular**: The item gains a modification slot. Mods (like gems) can be slotted on and off freely. Multiple rows define available mods.
- **Scaling**: The item grows through use. No materials required. Progression is tracked per item instance.

---

### 5. Scaling Rules (Linked by Item ID)

Each row defines how an item grows in power through use.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | The item that scales |
| Scales with Player Stats | JSON | Which player stats influence scaling and their weight (e.g., `{"attack": 0.5, "strength": 1.0}`) |
| Scales with Kills (Total) | Integer | Total kills required per scaling tier |
| Scales with Kills (Per Monster Type) | JSON | Kills per monster type required (e.g., `{"dragon": 500}`) |
| Scales with XP Earned | Integer | Total XP earned while equipped to advance |
| Scales with Time Equipped | Integer (ticks) | Total time worn to advance |
| Soul/Charge Feeding | JSON | Items or souls consumed to advance (e.g., `{"dark_essence": 100}`) |
| Cap per Type | JSON | Maximum scaling achievable per scaling source |

---

## Views

Predefined filtered views of the Equipment spreadsheet for common workflows.

| View | Filter |
|---|---|
| All | No filter, all items |
| PvP | Items with any PvP-specific column populated |
| PvM | Items with any PvM-specific column populated |
| By Slot | Grouped by slot category (Head, Cape, Neck, etc.) |
| By Combat Style | Grouped by combat style (Melee, Ranged, Magic, etc.) |
| By Stat Requirements | Sorted/grouped by stat level requirements |
| By Set | Grouped by set membership |
| By Fashion | Items with Fashion = true |
| By Skill | Items with any skilling property populated, grouped by XP Boost Skill |
