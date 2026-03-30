# Crafting System

Game design document for the Crafting System plugin. This is the unified recipe engine that all combining and processing skills share. Individual skill docs (skills-combining.md, skills-processing.md) define their specific recipes -- this doc defines how the recipe system itself works.

---

## 1. Recipe Structure

Every craftable item follows the same pipeline: inputs consumed + tool or station required + skill check + action time = output produced + XP awarded.

### Recipe Properties

| Property | Type | Description |
|---|---|---|
| Recipe ID | Integer | Unique identifier for this recipe |
| Name | String | Display name of the recipe |
| Skill | Enum | Which skill this recipe belongs to |
| Level Requirement | Integer | Minimum skill level to attempt this recipe |
| Inputs | Array | Items consumed on craft (item ID + quantity per item) |
| Tool | Item ID | Required tool, not consumed -- hammer, knife, needle, etc. |
| Station | Object ID | Required world object -- furnace, anvil, range, loom, etc. |
| Output | Array | Items produced on success (item ID + quantity) |
| XP | Float per skill | XP awarded per successful craft |
| Ticks | Integer | Time per craft action in game ticks |
| Success Rate | Formula | Chance of success -- 100% for most recipes, variable for some |
| Fail Output | Item ID | Item produced on failure (burnt food, crushed gem) |
| Fail XP | Float | XP awarded on failure (usually 0) |
| Batch Size | Integer | How many items can be crafted per action cycle |
| Members Only | Boolean | Requires membership to craft |
| Quest Requirement | Quest ID | Required quest completion before recipe is available |
| Category | Enum | Recipe category for UI grouping and filtering |

---

## 2. Multi-Step Recipes

Some items require a chain of crafting steps across different skills. Each step is its own recipe, and chain recipes link them together.

### Example Chain: Ore to Finished Item

| Step | Skill | Input | Output |
|---|---|---|---|
| 1 | Mining | Rock (resource node) | Ore |
| 2 | Smithing | Ore + fuel | Bar |
| 3 | Smithing | Bar | Unfinished item |
| 4 | Crafting | Unfinished item + gem | Finished item |

### Chain Properties

| Property | Description |
|---|---|
| Step Order | Defined sequence of recipes from raw material to final product |
| Intermediate Products | Output of step N is the input of step N+1 |
| Cross-Skill Steps | Different skills can be required at each step |
| Total XP | Sum of XP across all steps in the chain |
| Bottleneck Identification | Highlights the slowest or highest-level step in the chain |

---

## 3. Tool System

Tools are items required to perform a recipe but not consumed in the process.

### Tool Types

| Tool | Used By |
|---|---|
| Knife | Fletching, Cooking, Crafting |
| Chisel | Crafting (gems, limestone) |
| Hammer | Smithing, Construction |
| Needle + Thread | Crafting (leather, dragonhide) |
| Pestle and Mortar | Herblore |
| Glassblowing Pipe | Crafting (molten glass) |
| Saw | Construction |
| Spade | Farming |
| Rake | Farming |
| Seed Dibber | Farming |

### Tool Features

| Feature | Description |
|---|---|
| Tool Tiers | Bronze through Dragon -- higher tiers affect speed or success rate, not just access |
| Tool Degradation | Optional -- tool loses durability per use and eventually breaks or requires repair |
| Tool Belt | RS3-style permanent tool storage -- tools stored in belt are always available without inventory space (toggle) |

---

## 4. Station System

Stations are world objects the player must stand near to use certain recipes.

### Station Types

| Station | Used By |
|---|---|
| Furnace | Smithing (smelting), Crafting (jewelry, glass) |
| Anvil | Smithing (forging) |
| Range / Stove | Cooking |
| Pottery Wheel | Crafting (unfired pottery) |
| Kiln | Crafting (firing pottery) |
| Spinning Wheel | Crafting (spinning flax, wool) |
| Loom | Crafting (weaving) |
| Workbench | Construction, Invention |
| Altar | Prayer, Runecrafting |
| Obelisk | Summoning |
| Well | Cooking (filling containers) |
| Sandpit | Crafting (collecting sand) |
| Tanning Rack | Crafting (tanning hides) |
| Sawmill | Construction (cutting planks) |

### Station Features

| Feature | Description |
|---|---|
| Public Stations | Open to all players, placed in towns and cities |
| Guild-Only Stations | Restricted to players with sufficient skill level to enter the guild |
| POH Stations | Built inside player-owned houses for private use |
| Station Bonuses | Some stations grant better rates -- Hosidius range reduces burn chance |
| Portable Stations | Items that temporarily place a station anywhere in the world (toggle) |

---

## 5. Batch Crafting

| Feature | Description |
|---|---|
| Make-X Interface | Choose quantity to craft -- 1, 5, 10, All, or Custom |
| Auto-Continue | Keeps crafting until materials run out or inventory is full |
| Interruptible | Moving or clicking cancels the batch |
| Progress Display | Shows X/Y items completed in the current batch |
| Speed Display | Estimated items per hour at current rate |

---

## 6. Discovery / Recipe Unlock

Two modes for how players learn recipes. The dungeon master selects one.

| Mode | Description |
|---|---|
| All Known (OSRS style) | Every recipe is available as soon as the player meets the level requirement |
| Discovery Required | Recipes must be unlocked before they can be crafted |

### Discovery Methods

| Method | Description |
|---|---|
| Reach Skill Level | Recipe unlocks automatically at a certain level |
| Recipe Scroll | Consume a dropped or purchased scroll to learn the recipe |
| Quest Completion | Recipe unlocked as a quest reward |
| Blueprint | Find a blueprint item in the world or from a drop table |
| NPC Teacher | Pay or complete a task for an NPC who teaches the recipe |
| Experimentation | Discover by attempting to combine items (see section 8) |

### Recipe Book

| Feature | Description |
|---|---|
| Searchable | Filter by name, keyword, or item |
| Filterable | Filter by skill, level range, or category |
| Unlock Status | Shows which recipes are known and which are undiscovered |
| Favorites | Pin frequently used recipes to the top |

---

## 7. Quality System

Optional tiered quality for crafted items.

| Tier | Description |
|---|---|
| Normal | Standard item with base stats |
| Fine | Slightly improved stats and value |
| Superior | Noticeably improved stats and value |
| Masterwork | Best possible stats and highest value |

### Quality Determination

| Factor | Description |
|---|---|
| Skill Level Above Requirement | Higher level increases chance of better quality |
| Tool Quality | Higher tier tools increase quality chance |
| Station Quality | Better stations improve quality outcomes |
| Random Roll | Final quality determined by weighted random roll using the above factors |

### Quality Effects

| Effect | Description |
|---|---|
| Stat Bonus | Higher quality grants better combat or skilling stats |
| Sell Price | Higher quality items sell for more GP |
| Prestige | Visual indicator (name color, particle effect) showing item quality |

---

## 8. Experimentation

Optional system for discovering recipes through trial and error.

| Feature | Description |
|---|---|
| Free Combining | Players can attempt to combine any items, even without a known recipe |
| Discovery Chance | Successful combination of valid ingredients unlocks the recipe |
| Material Loss | Failed experiments consume materials with no output |
| Discovery Log | New recipes discovered through experimentation are added to the recipe book |
| Hidden Recipes | Dungeon master can seed secret recipes that are only discoverable through experimentation |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `crafting.recipe_system` | true | Core recipe engine |
| `crafting.multi_step_chains` | true | Recipes that span multiple steps and skills |
| `crafting.tool_tiers` | true | Bronze through Dragon tool progression |
| `crafting.tool_degradation` | false | Tools lose durability and require repair |
| `crafting.tool_belt` | false | RS3-style permanent tool storage |
| `crafting.station_bonuses` | true | Certain stations provide better success or speed |
| `crafting.portable_stations` | false | Deployable temporary stations |
| `crafting.batch_crafting` | true | Make-X interface for bulk crafting |
| `crafting.recipe_discovery` | false | Recipes must be unlocked before use |
| `crafting.recipe_book` | true | UI for browsing and filtering all recipes |
| `crafting.quality_system` | false | Crafted items can roll quality tiers |
| `crafting.experimentation` | false | Free-form combining to discover recipes |
| `crafting.fail_mechanics` | true | Failed crafts produce fail items (burnt food, crushed gems) |
| `crafting.success_rate_formulas` | true | Variable success rate based on level and bonuses |
| `crafting.category_filtering` | true | Group recipes by category in the UI |
| `crafting.favorite_recipes` | true | Pin recipes to a favorites list |
| `crafting.recently_crafted` | true | Track and display recently crafted items |
| `crafting.material_calculator` | true | Calculate total materials needed for a target item and quantity |

---

## Views

| View | Description |
|---|---|
| Recipe Book | Searchable, filterable list of all recipes by skill, level, and category |
| Material Calculator | Input a target item and quantity to see all raw materials needed across all chain steps |
| Crafting Queue | Batch progress indicator showing current item count and estimated completion |
| Station Locator | Highlights the nearest required station on the world map |
| Recipe Chains | Visual flowchart from raw material to final product showing every intermediate step |
