# Quests

Game design document for the Quests plugin. This system covers quest definitions, requirements, step-by-step walkthrough data, rewards, zone instancing, and dungeon master authoring tools for building custom quest content.

---

## Module Toggles

Each toggle enables or disables a subsystem within the quests module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Quest Points | Quests award quest points used as progression currency and gate content |
| Boostable Requirements | Skill requirements can be met using temporary boosts (potions, prayers, equipment) |
| Repeatable Quests | Certain quests can be completed more than once for reduced or alternative rewards |
| Subquests | Quests can contain nested subquests that must be completed as part of the parent |
| Holiday Quests | Seasonal or event-locked quests available only during specific calendar windows |
| Quest Series/Chains | Quests belong to narrative series where each entry continues the story |
| Step-by-step Helper | In-game overlay walks the player through each quest step with instructions |
| Map Markers | Quest-relevant locations are marked on the world map and minimap |
| Item Highlighting | Required and recommended items are highlighted in the player inventory and bank |
| Dialogue Helper | Correct dialogue choices are highlighted during NPC conversations |
| Puzzle Solver | Puzzle solutions are displayed or hinted at during puzzle steps |
| Shortest Path Integration | Travel steps suggest optimal routes using the pathfinding system |
| Item Lookup Integration | Required items link to drop sources, shop locations, and crafting recipes |

---

## Linked Spreadsheets

Five spreadsheets define the full quest data model. All are linked by quest ID, step ID, or zone ID.

---

### 1. Quest List

The primary spreadsheet. Every quest has one row.

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Unique identifier |
| Name | String | Display name of the quest |
| Description | Text | Short summary shown in the quest journal |
| Difficulty | Enum | Novice, Intermediate, Experienced, Master, Grandmaster |
| Length | Enum | Very Short, Short, Medium, Long, Very Long |
| Quest Points | Integer | Points awarded on completion |
| Series/Chain | String | Name of the quest series this belongs to, if any |
| Repeatable | Boolean | Whether the quest can be completed more than once |
| Holiday | String | Holiday or event window required, if any (e.g. Halloween, Christmas) |
| Subquests | ID List | List of child quest IDs that must be completed as part of this quest |
| Age/Era | String | Timeline tag defined by the dungeon master (e.g. First Age, Fourth Age) |
| Timeline Order | Integer | Canonical position in the world timeline, used for chronological sorting |
| Combat Scaling | String | Fixed level, or a min-max range that scales with the player's combat level |

---

### 2. Quest Requirements

One row per requirement entry. A quest can have many rows. Linked by Quest ID.

#### Skill Requirements

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Skill | String | Skill name (e.g. Mining, Agility) |
| Level | Integer | Minimum level required |
| Boostable | Boolean | Whether the requirement can be met with a temporary boost |

#### Quest Prerequisites

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Required Quest ID | Integer | Quest that must be completed first |
| QP Threshold | Integer | Minimum total quest points the player must have |

#### Combat Requirements

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Combat Level | Integer | Minimum combat level required |
| Enemy Name | String | Name of the combat encounter |
| Enemy Level | Integer | Level of the enemy |
| Enemy Style | String | Combat style of the enemy (Melee, Ranged, Magic, etc.) |

#### Item Requirements

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Item ID | Integer | Required item |
| Quantity | Integer | Number needed |
| Alternatives | ID List | Substitute item IDs that satisfy the same requirement |
| Check Bank | Boolean | Whether the system checks the player bank in addition to inventory |

#### Recommended Items

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Item ID | Integer | Recommended item |
| Reason | String | Why this item is recommended (e.g. "stamina restoration", "antipoison") |

#### Equipment Requirements

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Item ID | Integer | Item that must be worn during specific steps |
| Slot | String | Equipment slot the item occupies |

#### Follower Requirements

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Follower ID | Integer | Follower or familiar required |
| Reason | String | Why this follower is needed |

---

### 3. Quest Steps

The core walkthrough table. One row per step. A quest can have dozens of steps. Linked by Quest ID.

| Column | Type | Description |
|---|---|---|
| Step ID | Integer | Unique identifier for this step |
| Quest ID | Integer | Foreign key to Quest List |
| Step Order | Integer | Position in the step sequence |
| Step Type | Enum | NPC Talk, Object Interact, Dig, Emote, Tile/Travel, Item Pickup, Combat, Conditional, Puzzle |
| Instruction Text | Text | Human-readable instruction shown to the player |
| NPC/Object ID | Integer | Primary NPC or object involved in this step |
| Alternate IDs | ID List | Alternative NPC or object IDs that also satisfy this step |
| Location | String | Coordinates or zone ID where this step takes place |
| Zone | String | Rectangular bounds defined as min X, min Y, max X, max Y |
| Required Items | ID List | Items the player must have in inventory for this step |
| Highlighted Items | ID List | Items to visually highlight in the inventory during this step |
| Must Be Worn | ID List | Items that must be equipped (not just carried) for this step |
| Dialogue Choices | JSON | Ordered list of correct dialogue options for NPC conversations |
| Widget Highlights | ID List | Interface widget IDs to highlight during this step |
| Conditions | JSON | Conditions that must be true for this step to appear (see Condition Types below) |
| Logic Operator | Enum | AND, OR, NAND, NOR, XOR -- how multiple conditions are combined |
| Locking Condition | JSON | Condition that, if true, blocks progress until resolved |
| Substeps | ID List | Child step IDs nested under this step |
| Teleport Suggestion | String | Suggested teleport method to reach this step location |
| Map Markers | JSON | Coordinates and labels for world map and minimap markers |
| NPC Highlights | ID List | NPCs to highlight in the game world during this step |
| Object Highlights | ID List | Objects to highlight in the game world during this step |
| Tile Highlights | JSON | Tile coordinates to highlight on the ground during this step |
| Puzzle Solution | JSON | Solution data for puzzle-type steps (layout, sequence, answer) |

#### Condition Types

Conditions control when a step is shown or available. Multiple conditions can be combined using the logic operator.

| Condition Type | Description |
|---|---|
| Varbit | A server variable bit equals a specific value |
| Item Check | Player has (or does not have) a specific item |
| Zone Check | Player is (or is not) inside a defined zone |
| Quest State | Another quest or this quest is at a specific state |
| Prayer Active | A specific prayer is currently active |

---

### 4. Quest Rewards

One row per reward entry. A quest can have many reward rows. Linked by Quest ID.

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to Quest List |
| Reward Type | Enum | XP, Quest Points, Item, Equipment Unlock, Area Unlock, Skill Unlock, Ability Unlock, Spell Unlock, Prayer Unlock, NPC Unlock, Imbue Unlock, Title, Cosmetic, Collection Log |
| Skill | String | Skill name, if Reward Type is XP |
| XP Amount | Integer | Experience points awarded, if Reward Type is XP |
| Player Choice | Boolean | Whether the player chooses which skill receives the XP |
| Item ID | Integer | Item awarded, if Reward Type is Item |
| Quantity | Integer | Number of items awarded |
| Unlock ID | String | ID of the area, skill, ability, spell, prayer, NPC, or imbue unlocked |
| Unlock Description | Text | Human-readable description of what is unlocked |
| Title | String | Title or cosmetic name awarded |
| Collection Log Entry | String | Entry added to the collection log |

---

### 5. Quest Zones

Defines instanced or quest-specific areas. Linked by Zone ID, referenced from Quest Steps.

| Column | Type | Description |
|---|---|---|
| Zone ID | Integer | Unique identifier for this zone |
| Zone Name | String | Display name of the zone |
| Min X | Integer | Western boundary coordinate |
| Min Y | Integer | Southern boundary coordinate |
| Max X | Integer | Eastern boundary coordinate |
| Max Y | Integer | Northern boundary coordinate |
| Quest ID | Integer | Foreign key to Quest List, if this zone is quest-specific |
| Instanced | Boolean | Whether this zone is a private instance per player or group |

---

## Dungeon Master Authoring Tools

Tools available to dungeon masters (server operators and content creators) for building and testing quests.

---

### Dialogue Trees

Full NPC conversation authoring with branching responses.

| Feature | Description |
|---|---|
| Branching Responses | Each NPC dialogue node supports multiple player response options |
| Conditional Branches | Dialogue options appear or hide based on quest state, items held, skills, or varbits |
| Looping Dialogue | Conversations can loop back to earlier nodes for repeated interactions |
| Farewell Handling | Configurable behavior when the player ends conversation early |

---

### Cutscenes and Narrative

| Feature | Description |
|---|---|
| Cutscene Triggers | Steps can trigger camera-controlled cutscenes at specific quest states |
| Voice Acting Flags | Dialogue nodes can be flagged for voice acting with audio file references |
| Lore Entries | Books, journals, and scrolls with readable content authored per quest |
| Multiple Endings | Quests can branch to different conclusion steps based on player choices |
| Branching Paths | Player decisions at key steps route to different step sequences |

---

### World State Changes

Quests can permanently alter the game world for the player who completed them.

| Change Type | Description |
|---|---|
| NPC Appear | NPCs spawn in the world after quest completion |
| NPC Disappear | NPCs are removed from the world after quest completion |
| Object Change | Objects transform, appear, or disappear (e.g. a bridge is built, a door opens) |
| Area Transform | Zones change appearance or layout post-quest (e.g. a town is rebuilt) |
| New Shops | NPC shops become available or stock new items |
| Dialogue Change | NPC dialogue updates to reflect quest completion |

---

### Fail and Death Mechanics

| Setting | Description |
|---|---|
| Fail Conditions | Conditions that cause the quest to fail and require a restart or rollback |
| Time Limits | Optional countdown timer for timed quest segments |
| Safe Death | Whether dying during the quest is safe (no item loss) or punished normally |
| Lose Items | Whether items are lost on death during the quest |
| Checkpoint | Step to return to on failure, rather than restarting the entire quest |

---

### Instance Settings

| Setting | Description |
|---|---|
| Solo | Instance is restricted to a single player |
| Group | Instance allows a party of players |
| Max Players | Maximum number of players in a group instance |
| Scaling | Whether enemy difficulty scales with group size |

---

### Debug and Testing

| Tool | Description |
|---|---|
| Quest State Flags | View and manually set internal quest state variables |
| Reset Capability | Reset quest progress to any step or to the beginning |
| Debug Mode | Skip to a specific step in the quest for testing |
| Step Inspector | View all conditions, items, and state for any step in real time |

---

## Views

The quest journal and management interfaces support the following filter views.

| View | Description |
|---|---|
| All | Every quest in the database, unfiltered |
| By Difficulty | Grouped by Novice, Intermediate, Experienced, Master, Grandmaster |
| By Series | Grouped by quest series or chain |
| By Skill Requirement | Filtered to quests requiring a specific skill at a specific level |
| By Reward Type | Filtered to quests granting a specific reward type (XP, items, unlocks) |
| By Area Unlock | Filtered to quests that unlock access to a specific area |
| By Step Count | Sorted by total number of steps (shortest to longest or reverse) |
| By Age/Era | Grouped by dungeon master defined timeline tag |
