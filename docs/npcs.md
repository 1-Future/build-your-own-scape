# NPCs

Game design document for the NPCs plugin. This system covers all non-combat entities that players interact with --- shopkeepers, bankers, quest givers, service NPCs, transport operators, and ambient townsfolk. Monsters (combat NPCs) are covered in monsters.md. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the NPCs module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| NPC Schedules | NPCs follow day/night cycles and time-of-day appearance rules |
| NPC Wander | NPCs move within a defined radius from their spawn point |
| NPC Services | Service NPCs provide tanning, sawmill, decanting, and other utilities |
| NPC Faction/Reputation | NPCs belong to factions and require minimum reputation to interact |
| Random Event NPCs | NPCs appear randomly to interrupt AFK players |
| NPC Walk-Through | Players can walk through NPCs instead of being blocked |
| NPC Block Reset | NPCs teleport to spawn if stuck by players for too long |
| Transport NPCs | NPCs operate boats, carts, balloons, canoes, and fairy rings |
| Guard System | Guard NPCs enforce safe zones and attack skulled players |
| Musician Rest | Musician NPCs restore run energy to nearby players |
| Custom NPC Types | Dungeon masters can define new NPC types beyond the built-in list |

---

## Linked Spreadsheets

Three spreadsheets define the full NPC data model. All are linked by NPC ID.

---

### 1. NPC List (Main Table)

The primary spreadsheet. Every NPC has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique NPC identifier |
| Name | String | Display name |
| Title | String | Descriptive title (e.g. "Bob the Axe Seller") |
| Examine | String | Examine text shown on right-click |
| Model | String | Model or appearance reference |
| Size | Integer | Tile footprint (e.g., 1 = 1x1, 2 = 2x2) |
| Minimap Icon Color | Color | Icon color on minimap (default: yellow) |

#### NPC Types

Each NPC is assigned one primary type. 14 built-in types plus custom types the dungeon master creates.

| Type | Description |
|---|---|
| Shopkeeper | Runs a shop (links to shops.md) |
| Banker | Provides bank access (links to inventory-bank.md) |
| Quest Giver | Starts or progresses quests (links to quests.md) |
| Skill Trainer | Teaches skills, sells skill capes (links to skills.md) |
| Task Master | Assigns slayer tasks, farming contracts, and other repeatable assignments |
| Guard | Enforces safe zones, attacks skulled players |
| Service NPC | Provides a utility service (tanner, sawmill, decanter, make-over mage) |
| Transport | Operates boats, carts, balloons, canoes, or fairy rings |
| Musician | Plays music, restores run energy to nearby players |
| Townsfolk | Flavor NPCs with dialogue but no mechanical function |
| Pet/Follower | Companion NPCs (links to pet-companion.md) |
| Random Event | Appears randomly to interrupt AFK players |
| Herald/Crier | Announces game updates, events, or server news |
| Tutorial | Guides new players through the tutorial |
| Custom | Dungeon master defines a new NPC type |

#### Location

| Column | Type | Description |
|---|---|---|
| Spawn Location | String | Zone ID and coordinates where NPC spawns |
| Wander Radius | Integer | Maximum distance in tiles NPC can walk from spawn |
| Wander Speed | Integer (ticks) | Time between wander movements |
| Respawn If Killed | Boolean | Whether NPC respawns after being killed (for attackable NPCs that also serve NPC functions) |
| Multi-Location | Boolean | Same NPC exists at multiple locations, toggled per instance |

#### Schedule

Optional fields for time-based NPC behavior.

| Column | Type | Description |
|---|---|---|
| Day/Night Presence | Enum | NPC only appears during day, night, or both |
| Time-of-Day Dialogue | Boolean | Dialogue changes based on time of day |
| Weekly Rotation | String | Different days map to different spawn locations |
| Event Trigger | String | NPC appears only during a quest, after an event, or during a holiday |

#### Interaction

| Column | Type | Description |
|---|---|---|
| Right-Click Options | List | Available options (Talk-to, Trade, Bank, Pickpocket, Attack, Examine, Travel, Custom) |
| Default Left-Click | String | Action performed on left-click (configurable per NPC) |
| Dialogue ID | Integer | Links to dialogue tree (links to dialogue.md) |
| Shop ID | Integer | Links to shop inventory (links to shops.md) |
| Service Type | String | Service this NPC provides, if any |

#### Behavior

| Column | Type | Description |
|---|---|---|
| Walks Through Players | Boolean | Whether the NPC can walk through players |
| Blocked By Players | Boolean | Whether players physically block the NPC |
| Reset On Block | Boolean | NPC teleports to spawn if stuck too long |
| Facing Direction | Enum | Faces player on interact, or holds a fixed direction |
| Idle Animation | String | Animation played when NPC is idle |
| Interaction Animation | String | Animation played when NPC is interacted with |

#### Faction/Reputation

| Column | Type | Description |
|---|---|---|
| Faction ID | Integer | Which faction this NPC belongs to |
| Reputation Threshold | Integer | Minimum reputation required to interact |
| Reputation Change | Integer | Amount reputation changes on interaction (positive or negative) |

---

### 2. NPC Services (Linked Table)

One row per service offering. An NPC can provide multiple services.

#### Service Types

| Service | Description |
|---|---|
| Tanning | Converts hides to leather (input item, output item, cost) |
| Sawmill | Converts logs to planks (input, output, cost per type) |
| Decanting | Combines potion doses (4-dose into 3+1, etc.) |
| Unnoting | Converts noted items to unnoted (cost per item) |
| Dying | Colors items (input item + dye = colored item) |
| Repairing | Fixes degraded equipment (cost based on item + smithing level) |
| Enchanting | Enchants items (cost, requirements) |
| Make-Over | Changes character appearance (free or cost) |
| Transportation | Moves player to a destination (cost, destination list) |
| Teleportation | Teleports player (cost, destination, requirements) |
| Healing | Restores HP, prayer, or stats (free or cost, cooldown) |
| Storage | Provides specific item storage (tool leprechaun, Zulrah priestess) |
| Exchange | Swaps items (trade X for Y, currency exchange) |
| Naming | Names pets or items |
| Insurance | Insures pets or items (cost, reclaim fee) |
| Custom | Dungeon master defines service with configurable input, output, and cost |

#### Service Fields

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | Links to NPC list |
| Service Type | Enum | One of the service types above |
| Input Item ID | Integer | Item consumed by the service |
| Output Item ID | Integer | Item produced by the service |
| Cost | Integer | Currency cost to use the service |
| Currency Type | String | Currency used (coins, tokens, custom) |
| Skill Requirement | String | Skill and level required to use the service (optional) |
| Skill Discount | Boolean | Higher skill level reduces cost |
| Cooldown | Integer (ticks) | Time before service can be used again (0 = no cooldown) |

---

### 3. NPC Spawns (Linked Table)

One row per spawn instance. An NPC with multiple locations has multiple rows.

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | Links to NPC list |
| Location ID | String | Zone or region identifier |
| X | Integer | X coordinate |
| Y | Integer | Y coordinate |
| Layer | Integer | Layer/plane (-1000 to 1000) |
| Spawn Count | Integer | How many of this NPC spawn at this location |
| Wander Area | String | Rectangular bounds or radius defining wander zone |
| Active Hours | String | Time range when this spawn is active (if schedule enabled) |
| Instance Specific | Boolean | NPC has different behavior in instanced areas |

---

## Views

| View | Description |
|---|---|
| All NPCs | Every NPC in the database |
| By Type | Filter by NPC type (shopkeeper, banker, guard, etc.) |
| By Location | Filter by zone or region |
| By Service | Filter by service type provided |
| By Faction | Filter by faction membership |
| By Quest | NPCs involved in a specific quest (links to quests.md) |
| By Shop | NPCs that run a specific shop (links to shops.md) |
