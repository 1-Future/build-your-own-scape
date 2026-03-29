# Monsters

Game design document for the Monsters plugin. This system covers all NPCs that can be fought, their combat stats, aggression behavior, immunities, slayer assignments, drop tables, and shared loot pools.

---

## Module Toggles

Each toggle enables or disables a subsystem within the monsters module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Monster Types/Attributes | Monsters carry type tags (Demon, Undead, Draconic, etc.) that interact with equipment bonuses and slayer categories |
| Immunities | Monsters can be immune to specific effects (poison, venom, cannon, freeze, etc.) |
| Slayer System | Monsters require slayer levels, grant slayer XP, and are assignable by slayer masters |
| Aggression Mechanics | Monsters can initiate combat based on proximity, combat level comparison, or custom conditions |
| Elemental Weaknesses | Monsters have weaknesses to specific elements with configurable severity |
| Shared Drop Tables | Reusable loot pools (herb table, gem table, rare drop table, custom) referenced by multiple monsters |
| Drop Rate Broadcasting | Rare drops are announced to nearby or server-wide players |
| Kill Count Tracking | Per-player kill counts are recorded per monster and viewable in the adventure log |

---

## Linked Spreadsheets

Three spreadsheets define the full monster data model. All are linked by monster ID or table name.

---

### 1. Monster List (Main Table)

The primary spreadsheet. Every monster has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique monster identifier |
| Name | String | Display name |
| Examine | String | Examine text shown on right-click |
| Combat Level | Integer | Calculated combat level displayed to players |
| Size | Integer | Tile footprint (e.g., 1 = 1x1, 2 = 2x2, 3 = 3x3) |

#### Monster Types

Multi-tag field. A monster can carry multiple types simultaneously. 15 built-in types plus custom types the dungeon master creates.

| Type | Description |
|---|---|
| Demon | Fiends from the infernal planes |
| Draconic | Dragons and dragonkin |
| Fiery | Fire-based creatures, lava dwellers |
| Flying | Airborne enemies, may require ranged/magic |
| Golem | Animated stone, clay, or metal constructs |
| Kalphite | Insectoid hive creatures |
| Leafy | Plant-based monsters requiring leaf-bladed weapons |
| Penance | Creatures from the Penance realm |
| Rat | Vermin, rodents, and swarm creatures |
| Shade | Shadow-dwelling spirits requiring specific cremation |
| Spectral | Ghosts, spirits, and ethereal entities |
| Undead | Reanimated corpses, skeletons, zombies |
| Vampyre | Blood-feeding creatures requiring special weapons |
| Xerician | Creatures native to Xeric's domain |
| Werewolf | Lycanthropes requiring wolfbane or silver |
| Custom | Dungeon master creates new types as needed |

#### Combat Stats

| Column | Type | Description |
|---|---|---|
| Hitpoints | Integer | Total health points |
| Attack Level | Integer | Attack skill level |
| Strength Level | Integer | Strength skill level |
| Defence Level | Integer | Defence skill level |
| Magic Level | Integer | Magic skill level |
| Ranged Level | Integer | Ranged skill level |
| Attack Speed | Integer (ticks) | Time between attacks |

#### Max Hits

One column per attack style the monster can use.

| Column | Type | Description |
|---|---|---|
| Max Hit Melee | Integer | Maximum melee damage per hit |
| Max Hit Ranged | Integer | Maximum ranged damage per hit |
| Max Hit Magic | Integer | Maximum magic damage per hit |
| Max Hit Dragonfire | Integer | Maximum dragonfire damage per hit |
| Max Hit Special | Integer | Maximum damage from special/unique attacks |

#### Attack Styles Used

Multi-select field. Determines which combat styles the monster uses during combat.

| Style | Description |
|---|---|
| Stab | Melee stab attacks |
| Slash | Melee slash attacks |
| Crush | Melee crush attacks |
| Magic | Spell-based attacks |
| Ranged | Projectile attacks |
| Dragonfire | Dragon breath attacks, blockable by shields and prayers |
| Typeless | Unblockable damage that ignores protection prayers |

#### Elemental Weakness

| Column | Type | Description |
|---|---|---|
| Weakness Element | Enum | Element the monster is weak to (Fire, Water, Earth, Air, Light, Shadow, or None) |
| Weakness Percent | Float (%) | Severity of the weakness (damage increase from that element) |

#### Combat Bonuses (Offensive)

| Column | Type | Description |
|---|---|---|
| Attack Bonus | Integer | General attack accuracy bonus |
| Strength Bonus | Integer | Melee strength bonus |
| Magic Attack Bonus | Integer | Magic accuracy bonus |
| Magic Damage Bonus | Float (%) | Magic damage percentage bonus |
| Ranged Attack Bonus | Integer | Ranged accuracy bonus |
| Ranged Strength Bonus | Integer | Ranged strength bonus |

#### Defence Bonuses

| Column | Type | Description |
|---|---|---|
| Defence Stab | Integer | Defence against stab attacks |
| Defence Slash | Integer | Defence against slash attacks |
| Defence Crush | Integer | Defence against crush attacks |
| Defence Magic | Integer | Defence against magic attacks |
| Defence Light Ranged | Integer | Defence against light ranged attacks (shortbows, darts) |
| Defence Standard Ranged | Integer | Defence against standard ranged attacks (crossbows) |
| Defence Heavy Ranged | Integer | Defence against heavy ranged attacks (ballistas) |

#### Immunities

Each field is a boolean toggle. When enabled, the monster is fully immune to that effect.

| Column | Type | Description |
|---|---|---|
| Immune Poison | Boolean | Cannot be poisoned |
| Immune Venom | Boolean | Cannot be envenomed |
| Immune Cannon | Boolean | Cannot be damaged by a dwarf multicannon |
| Immune Burn | Boolean | Cannot be affected by burn damage over time |
| Immune Freeze | Boolean | Cannot be frozen in place |
| Immune Stun | Boolean | Cannot be stunned |

#### Behavior

| Column | Type | Description |
|---|---|---|
| Aggressive | Boolean | Whether the monster attacks players on sight |
| Aggro Range | Integer (tiles) | Distance at which the monster will initiate combat |
| Aggro Condition | Enum | `double_combat_level` (ignores players with combat level more than double the monster's), `always`, `never` |
| Poisonous | Boolean | Whether the monster can poison players |
| Poison Strength | Integer | Starting poison damage when the monster poisons a player |
| Retreat at Low HP | Boolean | Monster attempts to flee when health is low |
| Respawn Time | Integer (ticks) | Time after death before the monster reappears |
| Patrol Range | Integer (tiles) | Maximum distance the monster wanders from its spawn point |

#### Slayer

| Column | Type | Description |
|---|---|---|
| Slayer Level Required | Integer | Minimum slayer level to deal damage to this monster |
| Slayer XP | Float | Slayer XP granted on kill |
| Slayer Category | String | Grouping for slayer assignments (e.g., "Abyssal demons", "Black dragons") |
| Assigned By | List[String] | Which slayer masters can assign this monster (e.g., "Duradel", "Konar") |
| Special Kill Method | String | Required weapon or method to deal damage (e.g., "leaf-bladed", "silver", "ice_cooler", "rock_hammer") |

#### Locations

List field. Each entry defines a spawn location for this monster.

| Column | Type | Description |
|---|---|---|
| Location ID | Integer | Reference to the area/region where this monster spawns |
| Spawn Count | Integer | Number of this monster that spawn at this location |
| Access Requirement Quest | List[Integer] | Quest IDs required to access this location |
| Access Requirement Skill | JSON | Skill requirements to access (e.g., `{"agility": 70}`) |
| Access Requirement Item | List[Integer] | Item IDs required for access (e.g., dusty key, rope) |

---

### 2. Drop Tables (Linked by Monster ID)

Each row defines one item drop. A monster has many rows across this table.

| Column | Type | Description |
|---|---|---|
| Monster ID | Integer | The monster this drop belongs to |
| Item ID | Integer | The item dropped |
| Item Name | String | Display name of the dropped item |
| Quantity Min | Integer | Minimum quantity dropped |
| Quantity Max | Integer | Maximum quantity dropped |
| Drop Rate | String (1/X) | Drop rate expressed as a fraction (e.g., "1/128", "1/5000", "Always") |
| Always Drop | Boolean | Whether this item is a guaranteed drop on every kill (e.g., bones, ashes) |
| Members Only | Boolean | Whether this drop only appears on members worlds |
| Condition | String | Special condition for this drop (e.g., "Konar task only", "Wilderness only", "Location: Catacombs") |
| Subtable Reference | String | Name of a shared drop table this entry rolls from instead of dropping directly (e.g., "Herb table", "Gem table", "Rare drop table") |

---

### 3. Shared Drop Tables (Linked by Table Name)

Reusable loot pools referenced by multiple monsters. The dungeon master can create custom shared tables beyond the defaults.

#### Table Definition

| Column | Type | Description |
|---|---|---|
| Table Name | String | Unique name for the shared table (e.g., "Herb table", "Gem table", "Rare drop table", "Seed table") |
| Roll Chance | String | Chance to access this table per kill (e.g., "15/128", "1/16") |

#### Table Contents

Each row within a shared table defines one possible item.

| Column | Type | Description |
|---|---|---|
| Table Name | String | The shared table this item belongs to |
| Item ID | Integer | The item in this table |
| Item Name | String | Display name |
| Quantity Min | Integer | Minimum quantity |
| Quantity Max | Integer | Maximum quantity |
| Weight | Integer | Relative weight for selection within the table (higher = more common) |

---

## Views

Predefined filtered views of the Monster List spreadsheet for common workflows.

| View | Filter |
|---|---|
| All | No filter, all monsters |
| By Combat Level | Sorted and grouped by combat level brackets |
| By Monster Type | Grouped by type tag (Demon, Undead, Draconic, etc.) |
| By Slayer Category | Grouped by slayer assignment category |
| By Location | Grouped by spawn location |
| By Drop | Search for a specific item to see all monsters that drop it |
| By Weakness | Grouped by elemental weakness |
