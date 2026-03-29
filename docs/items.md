# Items

Game design document for the Items plugin. This system covers all in-game items, their properties, consumption mechanics, crafting materials, containers, teleportation items, currencies, and the augmentation/invention subsystem.

---

## Module Toggles

Each toggle enables or disables a subsystem within the items module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Stackability | Items can stack in a single inventory slot up to a configurable limit |
| Noting | Items can be converted to banknote form for compact trading and storage |
| Grand Exchange | Items can be listed on a centralized player marketplace |
| Alchemy (High/Low) | Items can be converted to gold using high or low alchemy spells |
| Weight | Items contribute to carry weight, affecting run energy drain |
| Food Combo Eating | Multiple food types (regular, combo, potion) can be consumed in the same tick |
| Potion Dose System | Potions hold multiple doses and produce lower-dose containers when consumed |
| Potion Decay | Stat boosts from potions degrade over time rather than expiring instantly |
| Barbarian Mixes | Potions can be combined with fish to create combo heal-and-boost items |
| Divine Potions | Potion variants that drain HP on drink but do not decay for a fixed duration |
| Item Despawn Timers | Dropped items disappear after a configurable delay |
| Disassembly/Invention | Items can be broken down into invention components |
| Augmentation | Equipment can be augmented with gizmos and perks via the invention system |
| Container System | Items that hold other items (quivers, rune pouches, gem bags) |
| Teleportation Charges | Teleportation items consume charges and can be recharged |
| Multiple Currencies | The economy supports more than one currency type (coins, tokens, points) |

---

## Item Categories

40 categories. The interface supports a dropdown filter to view items by category.

| # | Category | Description |
|---|---|---|
| 1 | Food | Consumable items that restore HP |
| 2 | Potions | Consumable items that apply stat boosts, cures, or effects |
| 3 | Runes | Catalysts for magic spells |
| 4 | Ammunition | Projectiles consumed by ranged weapons |
| 5 | Logs | Wood harvested from trees, used in Firemaking and Fletching |
| 6 | Ores | Raw minerals mined from rocks |
| 7 | Bars | Smelted metals used in Smithing |
| 8 | Fish | Raw or cooked aquatic catches |
| 9 | Herbs | Plants used in Herblore |
| 10 | Seeds | Plantable items for Farming |
| 11 | Gems | Cut or uncut stones used in Crafting |
| 12 | Bones/Ashes | Remains from defeated creatures, used in Prayer |
| 13 | Hides/Leather | Animal skins used in Crafting |
| 14 | Keys | Items that unlock chests, doors, or areas |
| 15 | Currency | Coins, tokens, points, and other monetary items |
| 16 | Quest Items | Items tied to specific quest progression |
| 17 | Teleportation | Items that transport the player to a destination |
| 18 | Tools | Axes, pickaxes, harpoons, and other skill tools |
| 19 | Materials | Crafting and production ingredients |
| 20 | Cosmetics | Items that change appearance only |
| 21 | Texts/Tomes | Books, scrolls, and readable items |
| 22 | Light Sources | Lanterns, candles, and other items that illuminate dark areas |
| 23 | Containers | Items that hold other items |
| 24 | Upgrade Components | Items consumed to enhance other items |
| 25 | Clue Scrolls | Treasure trail clues and caskets |
| 26 | Pet Items | Items related to pet acquisition or feeding |
| 27 | Construction | Flatpacks, furniture materials, and house items |
| 28 | Farming Supplies | Compost, plant cures, and farming tools |
| 29 | Summoning | Pouches, scrolls, and shards |
| 30 | Invention | Components, gizmos, and augmentors |
| 31 | Summoning/Companion | Items consumed by familiars or companions |
| 32 | Ensouled/Reanimation | Heads and remains used in reanimation spells |
| 33 | Divination/Energy | Energies and memories from Divination |
| 34 | Fossils/Artifacts | Collectible items from archaeology or exploration |
| 35 | Nests/Loot Containers | Openable containers that yield random loot |
| 36 | Ornament Kits | Cosmetic attachments applied to existing items |
| 37 | Item Packs | Bundled stacks of items purchasable from shops |
| 38 | Charms/Reagents | Non-tradeable drops used in Summoning or other skills |
| 39 | Emote Items | Items required to perform specific emotes |
| 40 | Walk Modifiers | Items that change the player walk or run animation |

---

## Linked Spreadsheets

Eight spreadsheets define the full item data model. All are linked by item ID.

---

### 1. Item List (Main Table)

The primary spreadsheet. Every item in the game has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique item identifier |
| Name | String | Display name |
| Examine | String | Text shown when the player examines the item |
| Icon | Asset | Sprite or icon reference |
| Category | Enum | One of the 40 item categories |
| Subcategory | String | Freeform grouping within a category (e.g., "Herb Tea" within Potions) |

#### Core Properties

| Column | Type | Description |
|---|---|---|
| Stackable | Boolean | Whether the item stacks in a single inventory slot |
| Stack Limit | Integer | Maximum stack size (default unlimited if stackable) |
| Tradeable | Boolean | Whether the item can be traded between players |
| GE Tradeable | Boolean | Whether the item can be listed on the Grand Exchange |
| Bankable | Boolean | Whether the item can be stored in a bank |
| Noteable | Boolean | Whether the item can be converted to banknote form |
| Droppable | Boolean | Whether the item can be dropped on the ground |
| Destroy on Drop | Boolean | Item is permanently destroyed instead of appearing on the ground |
| Despawn Timer | Integer (ticks) | Time before a dropped item disappears (0 = never) |
| Weight | Float | Weight in kg, contributes to carry weight |
| Members Only | Boolean | Whether the item is restricted to members-only servers |

#### Alchemy

| Column | Type | Description |
|---|---|---|
| Alchemisable | Boolean | Whether the item can be alchemised |
| High Alch Value | Integer | Gold received from high-level alchemy |
| Low Alch Value | Integer | Gold received from low-level alchemy |

#### Cross-Plugin Links

| Column | Type | Description |
|---|---|---|
| Equippable | Boolean | Links to the Equipment plugin for stat and slot data |
| Equipment ID | Integer | Foreign key to the Equipment spreadsheet (if equippable) |

---

### 2. Food Properties

One row per food item. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Heal Amount | Integer or Formula | HP restored on eat (flat value or level-scaled formula) |
| Overheal | Boolean | Whether healing can exceed max HP (creating temporary bonus HP) |
| Eat Delay | Integer (ticks) | Cooldown before the player can eat again |
| Combo Food | Boolean | Whether this food can be eaten in the same tick as regular food |
| Cooking Level | Integer | Cooking level required to cook this food |
| Burn Chance | Float (%) | Base probability of burning during cooking |
| Raw ID | Integer | Item ID of the raw version |
| Cooked ID | Integer | Item ID of the cooked version |
| Burnt ID | Integer | Item ID of the burnt version |
| Location Restricted | Boolean | Whether this food can only be eaten in specific areas |
| Stat Effects | JSON | Additional stat changes on consumption (e.g., `{"attack": -5, "strength": +5}`) |

---

### 3. Potion Properties

One row per potion. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Doses | Integer | Number of doses (typically 1--4, configurable) |
| Effect Type | Enum | Stat boost, Cure, Restore, Protect, Transform |
| Stat Affected | String | Which stat(s) the potion modifies |
| Boost Formula | String | Formula for the boost amount (e.g., `3 + floor(level * 0.10)`) |
| Duration | Integer (ticks) | How long the effect lasts |
| Decay | Boolean | Whether the boost degrades over time |
| Decay Rate | Integer (ticks) | Ticks between each step of decay |
| Drink Delay | Integer (ticks) | Cooldown before the player can drink again |
| Stacks with Food Timer | Boolean | Whether drinking resets or shares the food eat timer |
| Herblore Level | Integer | Herblore level required to create this potion |
| Primary Ingredient | Integer | Item ID of the primary herb or base |
| Secondary Ingredient | Integer | Item ID of the secondary ingredient |
| Barbarian Mix Variant | Integer | Item ID of the barbarian mix version (if applicable) |
| Divine Variant | Integer | Item ID of the divine version (no decay, drains HP on drink) |
| Dose Refill Method | String | How to restore doses (decant, well, NPC, other) |

---

### 4. Material Properties

One row per crafting material. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Material Tier | Integer | Tier level of the material (affects product quality) |
| Used in Skills | List[String] | Skills that consume this material |
| Product List | List[Integer] | Item IDs of products this material creates |
| Disassembly | Boolean | Whether this material can be disassembled (Invention) |
| Disassembly Components | JSON | Components yielded on disassembly (e.g., `{"sharp": 4, "metallic": 2}`) |
| Junk Chance | Float (%) | Probability of receiving junk instead of useful components |

---

### 5. Container Properties

One row per container item. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Capacity | Integer | Number of item slots the container holds |
| Restricted Contents | List[Integer] | Item IDs allowed in this container (empty = unrestricted) |
| Weight Reduction | Float (%) | Percentage reduction applied to stored item weight |
| Auto-Collect | Boolean | Whether the container automatically picks up matching drops |
| Degradable | Boolean | Whether the container degrades with use and requires repair |

---

### 6. Teleportation Properties

One row per teleportation item. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Destinations | List[Integer] | Location IDs the item can teleport to |
| Charges | Integer | Number of uses before the item is consumed or emptied |
| Rechargeable | Boolean | Whether the item can be recharged |
| Recharge Method | String | How the item is recharged (NPC, fountain, spell, other) |
| Wilderness Level Restriction | Integer | Maximum Wilderness level at which the teleport functions (0 = no restriction) |
| Cooldown | Integer (ticks) | Time before the item can be used again |
| Consumed on Use | Boolean | Whether the item is destroyed after its final charge |

---

### 7. Currency Properties

One row per currency type. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Earn Sources | List[String] | Activities or NPCs that award this currency |
| Spend Locations | List[String] | Shops or systems that accept this currency |
| Cap | Integer | Maximum amount a player can hold (0 = unlimited) |
| Tradeable Between Players | Boolean | Whether this currency can be given to other players |
| Conversion Rate | JSON | Exchange rates to other currencies (e.g., `{"coins": 100, "tokkul": 5}`) |

---

### 8. Augmentation/Invention Properties

One row per augmentable item. Linked to the main table by item ID.

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Foreign key to the Item List |
| Augmentable | Boolean | Whether the item can receive an augmentor |
| Gizmo Slots | Integer | Number of gizmo slots available when augmented |
| Perk List | List[String] | Perks that can roll on gizmos attached to this item |
| Item Level System | Boolean | Whether the augmented item gains XP and levels |
| XP to Level | JSON | XP thresholds for each item level (e.g., `{"1": 0, "2": 2000, "3": 5000}`) |
| Level Milestones | JSON | Bonuses at each milestone (e.g., `{"5": "reduced drain", "10": "perk activation boost"}`) |
| Charge Drain Rate | Float | Charge consumed per action or combat tick |
| Siphon vs Disassemble | Enum | Whether XP is extracted via siphon (keeps item) or disassemble (destroys item) |
| Disassembly Material Output | JSON | Components yielded on disassembly (e.g., `{"sharp": 8, "precise": 4}`) |

---

## Additional Item Properties

These properties extend the main item table for items with specialized behaviors. Each section represents a set of columns that apply only to items flagged for that behavior.

---

### Auto-Activate

Items that trigger automatically when a condition is met.

| Column | Type | Description |
|---|---|---|
| Auto-Activate | Boolean | Whether the item activates without player input |
| Trigger Condition | Enum | HP threshold, Death, Stat drain, Poison |
| Consumed on Trigger | Boolean | Whether the item is destroyed when it activates |
| Pocket Slot | Boolean | Whether the item must be in the pocket/sigil slot to function |

---

### Loot Container

Items that contain randomized loot when opened.

| Column | Type | Description |
|---|---|---|
| Loot Container | Boolean | Whether this item yields random loot on open |
| Loot Table ID | Integer | Reference to the loot table used for drops |
| Open Action | String | Action verb shown in the right-click menu (e.g., "Open", "Crack", "Search") |
| Loot Tier | Integer | Tier of the loot table (affects rarity weighting) |
| Stackable Container | Boolean | Whether multiple of this container can stack |
| Source Specific | Boolean | Whether the container yields different loot based on where it was obtained |

---

### Familiar/Companion Fuel

Items consumed by summoned familiars or companions.

| Column | Type | Description |
|---|---|---|
| Companion Fuel | Boolean | Whether this item fuels a familiar or companion |
| Summoning Points Drain Rate | Float | Points consumed per tick while the familiar is active |
| Duration | Integer (ticks) | How long the familiar lasts per unit of fuel |
| Special Move Cost | Integer | Points consumed when the familiar performs its special ability |
| Beast of Burden Capacity | Integer | Extra inventory slots the familiar provides |
| Companion Behavior | Enum | Combat, Forager, Healer, Teleporter, Scout, Skill Booster |

---

### Transmutation

Items that can be converted into upgraded versions through transmutation.

| Column | Type | Description |
|---|---|---|
| Transmutable | Boolean | Whether this item can be transmuted |
| Transmutation Input | JSON | Required items and materials (e.g., `{"item": 1234, "energy": 50}`) |
| Transmutation Output | Integer | Item ID of the upgraded result |
| Skill Requirement | JSON | Skill and level required (e.g., `{"divination": 45}`) |

---

### Reanimation

Items (ensouled heads, remains) that can be reanimated into monsters.

| Column | Type | Description |
|---|---|---|
| Reanimatable | Boolean | Whether this item can be reanimated |
| Spell Required | Integer | Spell ID required to reanimate |
| Rune Cost | JSON | Runes consumed (e.g., `{"nature": 2, "soul": 1, "blood": 1}`) |
| Monster Spawned | Integer | Monster ID of the creature summoned |
| XP Reward on Kill | Float | Prayer or combat XP awarded for killing the reanimated creature |
| Location Restriction | Boolean | Whether reanimation only works in specific areas |

---

### Crushable/Processable

Items that can be processed into another form using a tool.

| Column | Type | Description |
|---|---|---|
| Processable | Boolean | Whether this item can be crushed, ground, or otherwise processed |
| Output Item | Integer | Item ID of the resulting product |
| Tool Required | Integer | Item ID of the tool needed (pestle and mortar, chisel, etc.) |
| Used in Recipes | List[Integer] | Recipe IDs that consume the processed output |

---

### Placeable

Items that can be placed in the world as interactive objects (divine locations, etc.).

| Column | Type | Description |
|---|---|---|
| Placeable | Boolean | Whether this item can be placed in the game world |
| Divine Location | Boolean | Whether this is a divine location (shared resource node) |
| Max Uses | Integer | Number of times the placed item can be interacted with before despawning |
| Placement Cooldown | Integer (ticks) | Time before the player can place another of the same type |
| Skill to Place | JSON | Skill and level required to place (e.g., `{"divination": 60}`) |
| Skill to Use | JSON | Skill and level required to harvest from the placed item |
| Resources Provided | JSON | Items yielded per interaction (e.g., `{"item": 440, "quantity": 1}`) |
| Duration When Placed | Integer (ticks) | How long the placed item persists in the world |
| Area Buff | Boolean | Whether the placed item grants a buff to nearby players |
| Area Buff Radius | Integer (tiles) | Radius of the area buff effect |
| Area Buff Effect | JSON | Stat or rate modifiers applied within the radius |

---

### Cosmetic Modifier

Items applied to other items to change their appearance (ornament kits, recolors).

| Column | Type | Description |
|---|---|---|
| Cosmetic Modifier | Boolean | Whether this item modifies another item's appearance |
| Target Item ID | Integer | Item ID that this modifier can be applied to |
| Reversible | Boolean | Whether the modification can be undone to recover the kit |
| Changes Appearance Only | Boolean | Whether the modification is purely visual (no stat change) |
| Walk/Run Animation Override | String | Animation ID for custom walk or run animations |
| Emote Unlock | Integer | Emote ID unlocked when this item is equipped or applied |

---

### Bundle/Pack

Items that contain a fixed set of other items and can be unpacked.

| Column | Type | Description |
|---|---|---|
| Bundle | Boolean | Whether this item is a bundle or pack |
| Contains | JSON | Items and quantities inside (e.g., `[{"item": 314, "quantity": 100}]`) |
| Unbundle Action | String | Action verb shown in the right-click menu (e.g., "Unpack", "Open") |
| Shop Price | Integer | Cost to purchase the bundle from a shop |
| Rebundleable | Boolean | Whether the items can be repackaged into a bundle |

---

### Charm/Non-Tradeable Component

Items that drop from monsters and are consumed in skills but cannot be traded.

| Column | Type | Description |
|---|---|---|
| Charm | Boolean | Whether this item is a charm or non-tradeable component |
| Drop Source | List[Integer] | Monster IDs that drop this item |
| Used In | List[Integer] | Recipe or product IDs that consume this item |
| Charm Tier | Integer | Tier or quality level of the charm |
| Cannot Be Traded | Boolean | Enforced untradeable flag |
| Cannot Be Alched | Boolean | Enforced non-alchemisable flag |
| Cannot Be Dropped | Boolean | Whether dropping is blocked (destroy-only) |

---

### Fantasy/Futuristic Properties

Extended properties for servers running non-traditional rulesets or custom skills.

| Column | Type | Description |
|---|---|---|
| Throwable | Boolean | Whether the item can be thrown as a weapon |
| Throw Range | Integer (tiles) | Maximum throw distance |
| Throw AoE | Integer (tiles) | Splash radius on impact (0 = single target) |
| Recipe Item | Boolean | Whether combining multiple of this item produces a blueprint |
| Companion Fuel Drain Rate | Float | Rate at which the item is consumed to keep a summon alive |
| Reveals Hidden | Boolean | Whether holding this item reveals hidden objects, enemies, or paths |
| Transforms on Use | Boolean | Whether the item becomes a different item after use |
| Transform Result | Integer | Item ID of the post-use form |
| Volatile | Boolean | Whether the item explodes if dropped, damaged, or in inventory on death |
| Biome Restricted | List[String] | Biomes where the item can be used (empty = unrestricted) |
| Skill-Gated Use | JSON | Skill and level required to use (e.g., `{"invention": 40}`) |
| Combo Component | Boolean | Whether this item is one piece of a multi-item combination |

---

## Views

Predefined filters for browsing the item database.

| View | Description |
|---|---|
| All | Every item in the database, unfiltered |
| By Category | Grouped by one of the 40 item categories |
| By Tradeable/Untradeable | Split into tradeable and untradeable lists |
| By Skill | Filtered by the skill that creates, uses, or requires the item |
| By Source | Filtered by how the item is obtained (monster drop, shop, skilling, quest) |
| By Stackable | Stackable items vs. non-stackable items |
| By Weight | Sorted by weight, heaviest to lightest |
| By Value | Sorted by high alchemy value or shop price |
