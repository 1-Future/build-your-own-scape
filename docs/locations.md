# Locations

Game design document for the Locations plugin. This system covers all areas in the game world, their environmental effects, access requirements, combat rules, available resources, and dungeon master overrides.

---

## Module Toggles

Each toggle enables or disables a subsystem within the locations module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Nested Locations | Locations can contain sub-areas, forming a hierarchy (continent > region > area > room) |
| Environmental Hazards | Locations can deal passive damage over time from heat, cold, poison, or other sources |
| Environmental Mitigation | Items and equipment can reduce or negate environmental damage |
| Weather System | Dynamic weather events (rain, snow, sandstorm) that affect combat and skilling in the area |
| Darkness Mechanic | Some locations require a light source to see; without one, the player is blind and takes random damage |
| Underwater Mechanic | Breath timer and equipment restrictions for submerged locations |
| Run Energy Modifiers | Locations can increase or decrease run energy drain rate |
| Prayer Drain Modifiers | Locations can increase prayer point drain rate |
| Location Music | Each location triggers specific music tracks and ambient sounds |
| Teleport Restrictions | Locations can block incoming or outgoing teleports |
| Custom Rules | Dungeon master can apply arbitrary restrictions and modifiers to any location |

---

## Linked Spreadsheets

Three spreadsheets define the full location data model. All are linked by location ID.

---

### 1. Location List (Main Table)

The primary spreadsheet. Every location has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique location identifier |
| Name | String | Display name shown on the world map and minimap |
| Description | String | Flavor text shown when entering the area or examining it |
| Region | String | Top-level geographic grouping (continent or kingdom) |
| Sub-area Of | Integer (Location ID) | Parent location ID for nested areas (null if top-level) |
| Biome | Enum | Primary biome classification |

#### Biome Types

A location is assigned one primary biome. The biome determines default ambient visuals and sound.

| Biome | Description |
|---|---|
| Forest | Temperate woodlands, dense canopy |
| Desert | Arid sand or rock, extreme heat |
| Cave | Underground caverns, darkness possible |
| Underwater | Submerged areas requiring breath management |
| Volcanic | Lava fields, extreme heat, fire hazards |
| Arctic | Snow and ice, extreme cold |
| Swamp | Marshland, poison fog, waterlogged terrain |
| Mountain | High altitude, steep terrain, wind exposure |
| Plains | Open grassland, few obstacles |
| Jungle | Dense tropical vegetation, high humidity |
| Coastal | Shoreline, tidal zones |
| Urban | Towns, cities, player hubs |
| Dungeon | Constructed underground areas, corridors and chambers |
| Ethereal | Magical or otherworldly planes |
| Custom | Dungeon master creates new biome types as needed |

#### Combat Rules

| Column | Type | Description |
|---|---|---|
| PvP Enabled | Boolean | Whether players can attack each other in this location |
| Combat Type | Enum | `single` (1v1 only) or `multi` (multiple combatants) |
| Aggressive NPCs | Boolean | Whether NPCs in this location initiate combat on sight |
| Aggro Range Modifier | Float | Multiplier applied to NPC aggro ranges in this location (1.0 = default) |
| Safe Zone | Boolean | No combat of any kind is allowed (overrides PvP and NPC aggression) |
| Combat Level Bracket Min | Integer | Minimum combat level to enter (null = no minimum) |
| Combat Level Bracket Max | Integer | Maximum combat level to enter (null = no maximum) |

#### Access Requirements

Each field defines a prerequisite for entering the location. All specified requirements must be met.

| Column | Type | Description |
|---|---|---|
| Quest Required | List[Integer] | Quest IDs that must be completed before entry |
| Skill Required | JSON | Skill level requirements for access (e.g., `{"agility": 70, "mining": 50}`) |
| Item Required | List[Integer] | Item IDs that must be in the player's inventory or equipment (e.g., key, rope, light source) |
| Equipment Required | List[Integer] | Item IDs that must be equipped (e.g., heat-resistant gear, diving apparatus) |
| Achievement Required | List[String] | Achievement names that must be completed |
| Reputation Required | JSON | Faction reputation thresholds (e.g., `{"elves": 500, "dwarves": 200}`) |

#### Environmental Effects

| Column | Type | Description |
|---|---|---|
| Damage Over Time | Integer | Passive damage dealt per tick while in this location (0 = none) |
| Damage Type | Enum | Source of environmental damage: `Heat`, `Cold`, `Poison`, `Acid`, `Drowning`, `Typeless`, `None` |
| Damage Interval | Integer (ticks) | How often environmental damage is applied |
| Mitigation Items | List[Integer] | Item IDs that reduce or negate environmental damage (e.g., waterskins, warm clothing) |
| Mitigation Reduction | Float (%) | Percentage of environmental damage negated per mitigation item equipped or carried |
| Run Energy Drain Modifier | Float | Multiplier on run energy consumption (1.0 = normal, 2.0 = double drain, 0.5 = half drain) |
| Prayer Drain Modifier | Float | Multiplier on prayer point drain (1.0 = normal) |

#### Weather

| Column | Type | Description |
|---|---|---|
| Weather Enabled | Boolean | Whether this location has dynamic weather |
| Weather Types | List[Enum] | Possible weather events: `Rain`, `Snow`, `Sandstorm`, `Fog`, `Thunderstorm`, `Hail`, `Ashfall`, `Clear` |
| Weather Combat Modifier | JSON | Per-weather stat modifiers (e.g., `{"sandstorm": {"ranged_accuracy": -15}}`) |
| Weather Skilling Modifier | JSON | Per-weather skilling modifiers (e.g., `{"rain": {"fishing_speed": 1.1}}`) |
| Weather Cycle Duration | Integer (ticks) | How long each weather event lasts before rolling a new one |

#### Darkness and Underwater

| Column | Type | Description |
|---|---|---|
| Light Required | Boolean | Whether a light source is needed to see in this location |
| Darkness Damage | Integer | Damage dealt per tick when no light source is present |
| Darkness Attack | Boolean | Whether hidden creatures attack the player in darkness |
| Underwater | Boolean | Whether this location is submerged |
| Breath Timer | Integer (ticks) | Time before the player starts drowning (null = no limit) |
| Restricted Equipment Underwater | List[String] | Equipment slots or types disabled underwater (e.g., "cape", "2h_melee") |

#### Music and Ambience

| Column | Type | Description |
|---|---|---|
| Music Track IDs | List[Integer] | Music tracks that play when entering this location |
| Ambient Sound IDs | List[Integer] | Ambient loops that play alongside music (wind, water, cave drips) |
| Music Unlock | Boolean | Whether entering this location unlocks the track in the music player |

---

### 2. Location Resources (Linked by Location ID)

Each row defines one resource, NPC, or service available at a location. A location has many rows across this table.

#### Skilling Nodes

| Column | Type | Description |
|---|---|---|
| Location ID | Integer | The location this resource belongs to |
| Skill | String | The skill associated with this node (e.g., "Mining", "Woodcutting", "Fishing") |
| Node Type | String | Specific resource type (e.g., "Iron ore", "Yew tree", "Shark spot") |
| Node Count | Integer | Number of this node available at this location |
| Respawn Time | Integer (ticks) | Time for the node to reappear after depletion |
| Level Required | Integer | Minimum skill level to harvest this node |

#### Monster Spawns

| Column | Type | Description |
|---|---|---|
| Location ID | Integer | The location this spawn belongs to |
| Monster ID | Integer | Reference to the monster list |
| Spawn Count | Integer | Number of this monster present at this location |
| Respawn Time Override | Integer (ticks) | Custom respawn time for this location (null = use monster default) |

#### NPCs and Services

| Column | Type | Description |
|---|---|---|
| Location ID | Integer | The location this NPC belongs to |
| NPC ID | Integer | Unique NPC identifier |
| NPC Name | String | Display name |
| NPC Role | Enum | `Shop`, `Quest Giver`, `Banker`, `Slayer Master`, `Transport`, `Guard`, `Wandering`, `Custom` |
| Shop ID | Integer | Reference to shop inventory table (null if not a shop) |
| Quest ID | Integer | Quest this NPC is associated with (null if none) |

#### Facilities

| Column | Type | Description |
|---|---|---|
| Location ID | Integer | The location this facility belongs to |
| Facility Type | Enum | `Bank`, `Grand Exchange`, `Altar`, `Furnace`, `Anvil`, `Range`, `Spinning Wheel`, `Pottery Wheel`, `Loom`, `Tanning Rack`, `Sawmill`, `Farming Patch`, `Fairy Ring`, `Obelisk`, `Spirit Tree`, `Custom` |
| Facility Count | Integer | Number of this facility available |
| Members Only | Boolean | Whether this facility requires members access |

---

### 3. Location Rules (Linked by Location ID)

Custom rules applied by the dungeon master. Each row defines one rule or restriction. Multiple rules can stack on a single location.

| Column | Type | Description |
|---|---|---|
| Location ID | Integer | The location this rule applies to |
| Rule Type | Enum | Category of restriction (see rule types below) |
| Rule Value | JSON | Configuration for the rule (varies by type) |
| Rule Description | String | Human-readable explanation of what this rule does |
| Active | Boolean | Whether this rule is currently enforced |

#### Rule Types

| Rule Type | Value Format | Description |
|---|---|---|
| Equipment Restriction | `{"blocked_slots": ["weapon", "shield"]}` or `{"allowed_items": [1234, 5678]}` | Restricts what equipment can be worn |
| Skill Restriction | `{"blocked_skills": ["prayer", "magic"]}` | Disables specific skills in this location |
| Item Restriction | `{"blocked_items": [1234], "drop_on_entry": false}` | Prevents bringing certain items; optionally forces drop |
| Teleport Blocked | `{"incoming": true, "outgoing": true}` | Blocks teleportation into, out of, or both |
| Level Cap | `{"attack": 20, "strength": 20, "defence": 20}` | Caps effective stats to the specified levels |
| Ironman Only | `{"enabled": true}` | Only ironman accounts can enter |
| Time Locked | `{"start_hour": 18, "end_hour": 22, "timezone": "UTC"}` | Location is only accessible during the specified window |
| Custom Modifier | Any JSON | Dungeon master defines arbitrary modifiers |

---

## Views

Predefined filtered views of the Location List spreadsheet for common workflows.

| View | Filter |
|---|---|
| All | No filter, all locations |
| By Region | Grouped by top-level region or continent |
| By Biome | Grouped by biome classification |
| By Combat Type | Filtered by PvP enabled, single combat, or multi combat |
| By Access Requirement | Filtered to locations that require a quest, skill, item, or achievement |
| By Environmental Hazard | Filtered to locations with damage over time or special conditions |
| By Available Skill | Search for a skill to find all locations with nodes for it |
| By Monster | Search for a monster to find all locations where it spawns |
| By Facility | Search for a facility type to find all locations that have it |
| Safe Zones | Filtered to locations where combat is disabled |
| Dungeon Master Rules | Filtered to locations with custom rules applied |

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---|---|
| User-Placed Waypoints | Players can place custom waypoint markers on the world map with labels. Visible to self and optionally shared with party/clan. Persistent across sessions. |
| Distance Measurement Tool | Measure tile distance between two points. Shows walking path distance and straight-line distance. Useful for range calculations and planning. |
| Day/Night Cycle | Time-of-day system affecting lighting, NPC spawns, shop availability, monster aggression, and skilling rates. DM configures cycle length and effects per zone. |
| GPS Navigation | Pathfinding overlay showing optimal route to destination. Accounts for teleports, shortcuts, and agility obstacles. Turn-by-turn tile guidance. |
| Tile Marker Profiles | Save and load sets of tile markers as named profiles. Switch between profiles for different activities (bossing markers, skilling markers, PvP markers). |
| Tile Markers with Labels | Add custom text labels to tile markers. Shows text above the marked tile. Useful for boss safe spots, positioning notes, and team coordination. |
| Teleport Usage Analytics | Log all teleports used with timestamps, frequency, and destinations. Shows most-used teleport methods and suggests optimization. |
| Dynamic Weather System | Weather events beyond environmental damage. Rain, snow, fog, sandstorm, thunderstorm. Each affects visibility, movement speed, combat accuracy, and skilling success rates. DM configures per zone. |
| Instance Minimap | Minimap overlay for instanced/procedural areas showing room layout and player position. Auto-maps as you explore |
| Object Highlighting | Mark and color-highlight specific world objects (doors, shortcuts, interactables) persistently across sessions |
