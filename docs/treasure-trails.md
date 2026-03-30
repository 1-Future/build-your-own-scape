# Treasure Trails (Clue Scrolls)

Game design document for the Treasure Trails plugin. This system defines a multi-step treasure hunt where players receive clue scrolls from monster drops, skilling, or other sources. Each clue leads to the next until a final reward casket is earned. Every tier, clue type, and mechanic is independently toggleable.

---

## 1. Clue Tiers

Each tier represents a difficulty level. Higher tiers require more steps, harder puzzles, better stats, and reward from richer loot tables.

| Tier | Steps | Monster Drop Sources | Skill Recommendation | Puzzle Difficulty | Reward Quality |
|---|---|---|---|---|---|
| Beginner | 1--3 | Low-level monsters (goblins, cows, chickens) | None | Trivial | Basic cosmetics, small coin stacks |
| Easy | 3--5 | Low-mid monsters (guards, Al-Kharid warriors, imps) | 20+ combat or gathering | Simple | Low-tier unique items, moderate coin |
| Medium | 4--6 | Mid-level monsters (cockatrices, basilisks, ice warriors) | 40+ combat, 30+ skills | Moderate | Mid-tier uniques, decent resources |
| Hard | 5--7 | High-level monsters (hellhounds, black dragons, elves) | 70+ combat, 50+ skills | Challenging | High-tier uniques, rare resources |
| Elite | 5--8 | Boss monsters, high slayer creatures | 85+ combat, 70+ skills | Difficult | Elite uniques, valuable resources |
| Master | 6--10 | Endgame bosses, master-tier slayer creatures | 90+ all combat, 80+ skills | Expert | Rarest uniques, best-in-slot cosmetics |

---

## 2. Clue Types

The kinds of challenges a clue step can present. Not all types appear in all tiers.

| Clue Type | Tiers | Description |
|---|---|---|
| Coordinate | Easy, Medium, Hard, Elite, Master | Navigate to specific tile coordinates and dig. Coordinates given as degrees-minutes or raw x,y values |
| Map | Beginner, Easy, Medium | An image showing a small section of the world. Player identifies the location and digs there |
| Anagram | Easy, Medium, Hard | Scrambled NPC name. Unscramble it, find the NPC, and talk to them |
| Cryptic | Easy, Medium, Hard, Elite | A riddle describing a location, NPC, or action the player must perform |
| Emote | Medium, Hard, Elite, Master | Wear specific items and perform a specific emote at a specific location. May trigger a combat encounter |
| Hot/Cold | Medium, Hard, Elite, Master | A proximity indicator. Device or NPC tells the player warmer or colder as they move. Dig when "boiling" |
| Puzzle Box | Hard, Elite, Master | A slide puzzle. Rearrange tiles to form an image. Higher tiers use larger grids |
| Light Box | Hard, Elite | A grid of lights. Toggling one light affects neighbors. Turn all lights on or off to solve |
| Celtic Knot | Master | Three intertwined threads that must be rotated until the pattern aligns. Multiple solutions possible |
| Compass | Elite, Master | An arrow in the interface points toward the destination. Player walks in that direction until the arrow spins (arrived). Dig there |
| Fairy Ring | Medium, Hard | A fairy ring code is given. Use that fairy ring and dig at or near the destination |
| Skill Challenge | Medium, Hard, Elite, Master | Requires a specific skill level to complete the step. Examples: mine a specific rock, catch a specific fish, cook a specific item |
| Combat Challenge | Medium, Hard, Elite, Master | A specific NPC spawns and must be defeated. NPC difficulty scales with clue tier |
| NPC Request | Beginner, Easy, Medium | An NPC asks the player to bring a specific item. Item varies by tier and NPC |

---

## 3. Clue Step Properties

Data model for a single step within a clue trail.

| Property | Type | Description |
|---|---|---|
| Step ID | String | Unique identifier for this step |
| Clue Tier | Enum | Beginner, Easy, Medium, Hard, Elite, or Master |
| Clue Type | Enum | One of the clue types listed above |
| Location | Coordinates or Zone ID | Where the player must go. Tile coordinates for dig clues, zone reference for NPC clues |
| Required Items | Item ID list | Items the player must wear or bring (emote clues, NPC request clues) |
| Required Emote | Emote ID | The emote to perform (emote clues only) |
| NPC Target | NPC ID | The NPC to talk to or find (anagram, cryptic, NPC request clues) |
| Puzzle Data | JSON object | Image reference and solution data for puzzle box, light box, or celtic knot clues |
| Answer Text | String | Descriptive text for cryptic clues explaining the required action |
| Map Image | Image reference | The map fragment shown to the player (map clues only) |
| Compass Target | Coordinates | The destination the compass arrow points toward (compass clues only) |
| Hot/Cold Center | Coordinates | The target dig location for hot/cold proximity calculation |
| Fairy Ring Code | String (3 chars) | The fairy ring code to use (fairy ring clues only) |
| Skill Requirement | Skill ID + Level | Skill and minimum level needed (skill challenge clues only) |
| Combat NPC | NPC definition | The NPC that spawns for combat challenge steps. Includes stats, equipment, and attack style |
| Request Item | Item ID | The item the NPC requests (NPC request clues only) |
| Next Step ID | String or null | The next step in the chain. Null if this is the final step |
| Is Final Step | Boolean | If true, completing this step awards the reward casket |
| Hint Text | String | Optional hint the player can reveal after a configurable delay |

---

## 4. Reward System

Completing the final step of a clue trail awards a reward casket. Opening the casket rolls from the tier-specific reward table.

### Reward Caskets

| Tier | Casket Name | Roll Count | Description |
|---|---|---|---|
| Beginner | Reward casket (beginner) | 1--2 rolls | Small number of low-value rewards |
| Easy | Reward casket (easy) | 2--4 rolls | Minor resources, coins, low-tier uniques |
| Medium | Reward casket (medium) | 3--5 rolls | Moderate resources, coins, mid-tier uniques |
| Hard | Reward casket (hard) | 4--6 rolls | Good resources, coins, high-tier uniques |
| Elite | Reward casket (elite) | 4--6 rolls | Valuable resources, coins, elite uniques |
| Master | Reward casket (master) | 5--7 rolls | Best resources, coins, rarest uniques |

### Reward Categories

| Category | Description |
|---|---|
| Unique Cosmetics | Items obtainable only from clue scrolls. Tier-specific appearances. No combat stats |
| Equipment | Weapons and armor. Quality scales with tier |
| Resources | Skilling materials -- ores, logs, herbs, runes. Quantity scales with tier |
| Coins | Gold coin stacks. Amount scales with tier |
| Rare Items | Extremely rare drops with distinct visual appearance. 3rd age armor, gilded equipment, cosmetic overrides. Broadcast-worthy |
| Shared Table | Items that can appear in multiple tiers but with different quantities or probabilities |

### Reward Properties

| Property | Description |
|---|---|
| Drop Rate | Probability of rolling each item. Configurable per item per tier |
| Mega-Rare Table | Separate ultra-rare table rolled independently. Contains the rarest items (3rd age, gilded) |
| Broadcast | Server-wide announcement when a player receives a mega-rare drop. Configurable threshold |
| Collection Log | Every unique clue reward tracked in the collection log. Completion percentage per tier |
| Duplicate Protection | Optional toggle to reduce odds of receiving items already logged. Off by default (OSRS-authentic) |
| Reward Value Display | Show estimated total value of casket contents on open |

---

## 5. STASH Units

Build Once, Always Stash. Storage units placed at emote clue locations to permanently store the required items for that clue step.

| Property | Description |
|---|---|
| Location | One STASH unit per emote clue location. Placed in the game world near the emote spot |
| Build Requirement | Construction level required to build the unit. Scales with clue tier |
| Build Materials | Planks, nails, or other construction materials. Defined per STASH unit |
| Storage | Holds exactly the items required for that specific emote clue step. Cannot store anything else |
| Retrieval | Items automatically equipped when the player interacts with the STASH unit at the clue location |
| Persistence | Items remain stored permanently until the player removes them |
| Visual | STASH unit is visible in the world as a small crate, bush, or hole depending on tier |
| Destroy | Player can destroy a built STASH unit to recover stored items |

### STASH Tiers

| Tier | Construction Level | Materials | Description |
|---|---|---|---|
| Beginner | 12 | 2 planks, 10 nails | Simple wooden crate |
| Easy | 27 | 2 oak planks, 10 nails | Oak storage box |
| Medium | 42 | 2 teak planks, 10 nails | Teak storage unit |
| Hard | 55 | 2 mahogany planks, 10 nails | Mahogany chest |
| Elite | 77 | 2 mahogany planks, gold leaf | Ornate chest |
| Master | 88 | 2 mahogany planks, gold leaf, marble block | Master vault |

---

## 6. Clue Juggling

Rules for holding and managing multiple active clue scrolls simultaneously.

| Rule | Description |
|---|---|
| One Per Tier | Default: a player can hold one active clue scroll per tier at a time. Receiving a duplicate tier clue while one is active results in no drop |
| Unlimited Mode | DM toggle: remove the one-per-tier restriction. Players can hold any number of clues |
| Drop Mechanics | Dropping a clue scroll places it on the ground. Clue retains its current progress |
| Ground Timer | Dropped clues despawn after a configurable number of ticks (default 300 ticks / 3 minutes) |
| Re-obtain | If a clue despawns or is destroyed, the player can receive a new one from the same sources |
| Bank Storage | Clue scrolls can be stored in the bank. They retain progress |
| Trade Restriction | Clue scrolls are untradeable by default. DM toggle to allow trading |
| Clue Bottle / Geode / Nest | Skilling-sourced clues arrive as containers (fishing = bottle, mining = geode, woodcutting = nest). Opening reveals the clue scroll |

---

## 7. DM Clue Builder

Tools for dungeon masters to create custom clue trails. Custom trails use the same engine as built-in trails but with DM-authored content.

| Tool | Description |
|---|---|
| Coordinate Dig Placer | Click a tile on the map to set it as a coordinate clue dig spot. Automatically generates the coordinate text |
| Map Image Painter | Select a region of the world map to generate a map clue image. Crop, rotate, and stylize the image |
| Cryptic Riddle Writer | Text editor for writing cryptic riddles. Linked to a target location or NPC. Preview how the riddle appears in-game |
| Anagram Generator | Select an NPC and auto-generate an anagram of their name. Manual override available |
| Emote Step Builder | Select a location, required equipment, and required emote. Preview the step requirements |
| Puzzle Box Designer | Upload or generate an image for a puzzle box. Set grid size (3x3, 4x4, 5x5). Preview solvability |
| Light Box Designer | Set grid dimensions and define which lights are linked. Verify the puzzle is solvable before saving |
| Combat Encounter Builder | Define the NPC that spawns during a combat challenge step. Set stats, equipment, attack style, and special mechanics |
| Skill Challenge Builder | Select a skill, minimum level, and the action the player must perform |
| NPC Request Builder | Select an NPC and the item they request. Set dialogue text |
| Step Chainer | Visual interface for linking steps into a full trail. Drag and drop steps into order. Set which step is final |
| Reward Table Editor | Define custom reward tables for custom clue tiers. Set items, quantities, and drop rates. Preview expected loot distribution |
| Clue Source Assigner | Assign custom clue trails to specific monster drop tables, skilling actions, or other sources |
| Test Mode | Run through a custom trail as a player to verify all steps are completable before publishing |

---

## 8. Clue Scroll Sources

Where clue scrolls come from. Every source has a configurable drop rate per clue tier.

### Monster Drop Sources

| Tier | Example Sources | Base Drop Rate |
|---|---|---|
| Beginner | Goblins, cows, chickens, men/women | 1/50 |
| Easy | Guards, Al-Kharid warriors, H.A.M. members, thugs | 1/64 |
| Medium | Ice warriors, cockatrices, basilisks, pyrefiends | 1/128 |
| Hard | Hellhounds, black dragons, elves, skeletal wyverns | 1/64 (hellhounds), 1/128 (others) |
| Elite | Boss monsters, abyssal demons, dark beasts, thermonuclear smoke devil | 1/200--1/500 |
| Master | Endgame bosses only (raids, high-level bosses) | 1/500--1/1000 |

### Skilling Sources

| Skill | Container | Base Drop Rate | Description |
|---|---|---|---|
| Fishing | Clue bottle | 1/1000--1/2000 | Found while fishing. Tier scales with fish level |
| Mining | Clue geode | 1/1000--1/2000 | Found while mining. Tier scales with rock level |
| Woodcutting | Clue nest | 1/1000--1/2000 | Found while cutting trees. Tier scales with tree level |
| Thieving | Direct clue | 1/200--1/500 | Pickpocketed from NPCs. Tier based on NPC level |
| Farming | Clue nest | 1/500--1/1000 | Found in bird nests from farming contracts |

### Other Sources

| Source | Description |
|---|---|
| Implings | Catching specific impling types yields clue scrolls of corresponding tiers |
| Treasure Chest | World-placed chests that occasionally contain clue scrolls |
| Event Rewards | Seasonal or special events may award clue scrolls |
| Achievement Rewards | Completing specific achievements grants a clue scroll of appropriate tier |
| Purchase | Optional NPC or shop that sells clue scrolls for GP or tokens. DM toggle |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `clues.beginner` | true | Beginner tier clue scrolls |
| `clues.easy` | true | Easy tier clue scrolls |
| `clues.medium` | true | Medium tier clue scrolls |
| `clues.hard` | true | Hard tier clue scrolls |
| `clues.elite` | true | Elite tier clue scrolls |
| `clues.master` | true | Master tier clue scrolls |
| `clues.type_coordinate` | true | Coordinate dig clues |
| `clues.type_map` | true | Map image clues |
| `clues.type_anagram` | true | Anagram NPC clues |
| `clues.type_cryptic` | true | Cryptic riddle clues |
| `clues.type_emote` | true | Emote and equipment clues |
| `clues.type_hotcold` | true | Hot/cold proximity clues |
| `clues.type_puzzlebox` | true | Slide puzzle clues |
| `clues.type_lightbox` | true | Light toggle puzzle clues |
| `clues.type_celticknot` | true | Celtic knot puzzle clues |
| `clues.type_compass` | true | Compass direction clues |
| `clues.type_fairyring` | true | Fairy ring code clues |
| `clues.type_skillchallenge` | true | Skill level requirement clues |
| `clues.type_combatchallenge` | true | Combat encounter clues |
| `clues.type_npcrequest` | true | NPC item request clues |
| `clues.stash_units` | true | STASH unit building and storage |
| `clues.juggling_one_per_tier` | true | Limit one active clue per tier |
| `clues.juggling_unlimited` | false | Allow unlimited active clues |
| `clues.tradeable` | false | Allow clue scroll trading |
| `clues.broadcast_megarare` | true | Server broadcast on mega-rare drops |
| `clues.duplicate_protection` | false | Reduce odds of already-logged rewards |
| `clues.skilling_sources` | true | Clue bottles, geodes, and nests from skilling |
| `clues.monster_sources` | true | Clue scrolls from monster drop tables |
| `clues.custom_clues` | true | DM-created custom clue trails |
| `clues.hint_system` | true | Optional hints for stuck players |
| `clues.collection_log` | true | Track clue rewards in collection log |
| `clues.reward_value_display` | true | Show estimated value when opening caskets |

---

## Views

| View | Description |
|---|---|
| Active Clue Panel | Shows the current clue step text, type, and tier. Displays puzzle interfaces when applicable |
| Clue Log | History of all completed clue trails per tier with step count and completion time |
| Reward Tracker | Summary of all rewards received from clue scrolls. Sorted by tier and rarity |
| Collection Log (Clues) | Checklist of all unique clue rewards with obtained/unobtained status per tier |
| STASH Location Map | World map overlay showing all STASH unit locations with build status (built, unbuilt, filled, empty) |
| Clue Counter | Running total of clues completed per tier. Displayed in the clue interface |
| DM Clue Builder | Full clue trail creation interface with step editor, chainer, reward table, and test mode |
| DM Clue Source Manager | Interface for assigning clue trails to drop tables and skilling sources with rate configuration |
| DM Reward Table Editor | Define and preview reward tables with item lists, drop rates, and mega-rare thresholds |
| Puzzle Interface (Puzzle Box) | Interactive slide puzzle grid rendered in-game |
| Puzzle Interface (Light Box) | Interactive light toggle grid rendered in-game |
| Puzzle Interface (Celtic Knot) | Interactive knot rotation interface rendered in-game |
| Compass Interface | Directional arrow overlay that points toward the target location |
| Hot/Cold Interface | Temperature indicator showing proximity to the target dig spot |
