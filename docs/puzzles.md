# Puzzles

Game design document for the puzzle system. Puzzles are self-contained challenges that can exist standalone or be embedded as quest steps.

## Module Toggles

Each puzzle instance can independently enable or disable the following modules:

| Module | Description |
|---|---|
| Timer | Beat-the-clock countdown or elapsed time tracking |
| Hints | Progressive hint system, optional per puzzle |
| Fail conditions | What happens on failure (reset, damage, death, kicked out) |
| Randomization | Randomize maze layout, object placement, or solution order |
| Leaderboards | Track completion times and attempts per puzzle |
| Practice mode | Attempt without consequences, no rewards granted |
| Spectating | Allow other players to watch an active attempt |
| Puzzle builder | Player-created puzzles, requires DM approval before publication |

## Data Model

Two linked spreadsheets define all puzzle content. The Puzzle List defines the puzzle itself (constraints, mechanics, steps, rewards). The Puzzle Components sheet defines every interactable object used across all puzzles. Objects are referenced by Object ID from puzzle step definitions.

---

### 1. Puzzle List

#### Core Fields

| Field | Type | Description |
|---|---|---|
| ID | string | Unique puzzle identifier |
| Name | string | Display name |
| Description | text | Flavor text and premise |
| Category | enum | See category table below |
| Difficulty | enum | Easy, Medium, Hard, Elite, Master |
| Estimated time | duration | Expected completion time |
| Solo/Group | enum | Solo, Group, Either |
| Min players | integer | Minimum players required (1 for solo) |
| Max players | integer | Maximum players allowed |

#### Categories

| Category | Description |
|---|---|
| Escape Room | Locked environment, find the way out |
| Logic | Deduction, pattern recognition, process of elimination |
| Mechanical | Physical interaction puzzles (levers, pressure plates, timing) |
| Maze | Navigation through generated or static mazes |
| Combat Puzzle | Combat with puzzle mechanics (order of kills, positioning, phase triggers) |
| Riddle | Text-based clues requiring lateral thinking |
| Skill Challenge | Requires specific skill levels or skill-based actions to solve |

#### Constraints

Constraints restrict what the player can do or use during the puzzle.

| Field | Type | Description |
|---|---|---|
| Restricted items | list | Items that cannot be used |
| Restricted equipment | list | Equipment that cannot be worn |
| Restricted skills | list | Skills that cannot be used |
| Restricted area | zone | Confines the player to a specific area |
| Inventory loadout | list | Pre-set items given on entry, replaces inventory |
| Equipment loadout | list | Pre-set gear given on entry, replaces equipment |
| Stat override | map | Override player stats for the duration |
| No banking | boolean | Banking disabled |
| No trading | boolean | Trading disabled |
| No teleporting | boolean | Teleportation disabled |
| No combat | boolean | Combat disabled (or combat-only if false and puzzle requires it) |
| Custom restrictions | text | Freeform restriction description for edge cases |

#### Mechanics

| Field | Type | Description |
|---|---|---|
| Interactable objects | list of Object IDs | Levers, doors, pressure plates, tiles, etc. |
| Object states | per object | On/off, open/closed, lit/unlit, raised/lowered |
| Trigger chains | list | Pull A -> opens B -> reveals C |
| Combination locks | list | Code entry sequences |
| Pattern matching | definition | Visual or logical patterns to reproduce |
| Maze generation | enum | Static (hand-built) or Randomized (per attempt) |
| Timer | config | Beat the clock or untimed |
| Fail condition | enum | Reset, Damage, Death, Kicked out |
| Retry | enum | Unlimited, Limited (N attempts), One shot |
| Hints system | enum | Progressive (reveal more over time), Optional (player requests), None |

#### Steps

Each puzzle is a sequence of steps. Steps can branch.

| Field | Type | Description |
|---|---|---|
| Step ID | string | Unique within the puzzle |
| Order | integer | Sequence position (branching may share order numbers) |
| Instruction/clue text | text | What the player sees or hears |
| Solution action | action | What the player must do to complete this step |
| Condition to unlock next step | condition | State required before next step activates |
| Branch paths | list of Step IDs | Alternative solutions leading to different next steps |

#### Rewards

| Field | Type | Description |
|---|---|---|
| XP | map | Skill -> amount |
| Items | list | Items granted on completion |
| Area unlock | zone ID | New area becomes accessible |
| Achievement trigger | achievement ID | Achievement granted |
| Collection log entry | entry ID | Added to collection log |
| Title/cosmetic | string | Cosmetic reward (title, outfit, color) |
| Leaderboard time | boolean | Whether completion time is recorded |

---

### 2. Puzzle Components

Every interactable object used in any puzzle is defined here. Puzzles reference these by Object ID.

| Field | Type | Description |
|---|---|---|
| Object ID | string | Unique object identifier |
| Object name | string | Display name |
| Object type | enum | See type table below |
| Location | coordinates | Position in the world |
| States | list of strings | All possible states (e.g., on, off, open, closed) |
| Interactions | list | Actions the player can perform on this object |
| Triggers | list | Effects that fire when the object is interacted with |
| Condition to be interactable | condition | Prerequisites before the object responds to interaction |

#### Object Types

| Type | Description |
|---|---|
| Lever | Pull to change state, often triggers other objects |
| Door | Opens/closes, may require key or condition |
| Tile | Floor tile that reacts to being stepped on |
| Chest | Contains items, may be locked or trapped |
| NPC | Non-player character that gives clues or reacts to actions |
| Light | Light source that can be lit/extinguished, may reveal hidden elements |
| Pressure plate | Activates when weight is applied, deactivates when removed |

---

## Integration

Puzzles can be embedded in quests as a step type. The quest step references the puzzle by Puzzle ID. When the player reaches that quest step, the puzzle instance is created with the defined constraints and modules. Completing the puzzle advances the quest.

Quest step definition:

| Field | Value |
|---|---|
| Step type | puzzle |
| Puzzle ID | Reference to Puzzle List |
| Override modules | Optional overrides to timer, hints, fail conditions for quest context |

---

## Views

Puzzle data can be filtered and displayed through the following views:

| View | Groups by |
|---|---|
| All | No grouping, full list |
| By Category | Escape Room, Logic, Mechanical, Maze, Combat Puzzle, Riddle, Skill Challenge |
| By Difficulty | Easy, Medium, Hard, Elite, Master |
| By Player Count | Solo, Small group (2-3), Group (4-5), Large group (6+) |
| By Reward | XP, Items, Area unlock, Achievement, Cosmetic |
| By Creator | Player-created vs DM-created |
