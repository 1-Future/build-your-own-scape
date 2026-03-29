# Player Housing

Game design document for the Player Housing plugin. This system covers house configuration, room types, furniture crafting, portal teleportation, trophy displays, storage, servants, visitor interaction, and Construction skill integration.

---

## Module Toggles

Each toggle enables or disables a subsystem within the player housing module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Player Housing | Master toggle for the entire housing system |
| Room System | Players can add, remove, and rearrange rooms |
| Furniture Tiers | Furniture quality scales through material tiers (basic to ornate) |
| Portal Room | Configurable teleport destinations within the house |
| Trophy Room | Display boss trophies, rare items, and achievements |
| Storage Rooms | Costume room, tool storage, and other dedicated storage |
| Butler and Servant | Hire NPC servants to assist with house tasks |
| Visitor System | Other players can visit, rate, and interact with the house |
| House Rating | Visitors can rate houses, contributing to a leaderboard |
| PvP Combat Room | Visitors can fight the owner or each other in a combat room |
| Dungeon Builder | Owner builds a dungeon challenge for visitors to attempt |
| Flatpack Crafting | Build furniture as tradeable flatpack items |
| Construction Contracts | NPC-assigned build tasks for bonus XP (Mahogany Homes style) |
| House Themes | Multiple visual themes available per house style |
| Pet Room | Dedicated room for displaying and storing pets |
| Party Room | Drop parties, balloon events, and social gatherings |
| Music Room | Jukebox placement and music playback for visitors |

---

## 1. House Settings

Global properties for a player's house.

### House Data Model

| Field | Type | Description |
|---|---|---|
| house_id | string | Unique house identifier |
| owner_id | string | Player who owns the house |
| location_zone_id | integer | Zone ID where the house portal is located |
| theme | string | Visual theme applied to the house (DM-defined) |
| max_rooms | integer | Maximum number of rooms allowed (scales with Construction level) |
| max_floors | integer | Maximum floor levels (basement, ground, upper floors) |
| build_mode | boolean | Whether the owner is currently in build mode |
| visitor_permission | enum | `anyone`, `friends`, `clan`, `locked` |
| pvp_enabled | boolean | Whether the combat room allows PvP |
| dungeon_enabled | boolean | Whether the dungeon challenge is active for visitors |
| created_at | datetime | When the house was first created |
| last_modified | datetime | When the house layout was last changed |

### Available Themes

Themes are defined by the dungeon master and control the visual appearance of walls, floors, and ambient lighting.

| Theme | Description |
|---|---|
| Medieval | Stone walls, wood floors, torchlight |
| Desert | Sandstone walls, tile floors, warm light |
| Cave | Rough stone walls, uneven floors, dim lighting |
| Modern | Clean walls, polished floors, bright lighting |
| Gothic | Dark stone, stained glass, candlelight |
| Elven | Carved wood, leaf motifs, natural light |
| Underwater | Coral walls, shell floors, blue-green light |
| Infernal | Obsidian walls, lava accents, red glow |

### House Portal Location

| Field | Type | Description |
|---|---|---|
| zone_id | integer | Zone where the portal to this house exists |
| portal_position | JSON | Coordinates of the portal within the zone (e.g., `{"x": 100, "y": 200}`) |
| portal_style | string | Visual appearance of the portal entrance |
| enter_cost | integer | GP cost for visitors to enter (0 = free, owner always free) |

---

## 2. Room System

Houses are built on a grid. Each cell on the grid holds one room. Rooms span a single grid cell and are stacked across floor levels.

### Room Types

| Room Type | Description | Default Furniture Hotspots |
|---|---|---|
| Parlour | Entry room, seating, basic decoration | Chairs, rugs, curtains, fireplace |
| Kitchen | Cooking range, sink, shelves | Range, sink, shelves, table, larder |
| Dining Room | Table, chairs, wall decorations | Table, chairs, bell-pull, wall decoration |
| Bedroom | Bed, wardrobe, dresser | Bed, wardrobe, dresser, clock, rug |
| Workshop | Crafting benches, repair stand | Workbench, repair stand, tool rack, whetstone |
| Study / Library | Bookshelf, lectern, globe | Bookshelf, lectern, globe, telescope, writing desk |
| Chapel / Altar | Prayer altar, icon, incense | Altar, icon, musical instrument, torch, rug |
| Combat Room | Training dummies, fighting ring | Combat dummy, weapon rack, ring corner, score board |
| Garden | Plants, pond, trees, statue | Tree patch, flower patch, pond, statue, bench |
| Portal Room | Teleport portals to configured destinations | Portal frame (x3 default, scales with level) |
| Treasure Room | Display cases, pedestals, gold pile | Display case, pedestal, gold pile, chest, painting |
| Trophy Room | Boss heads, achievement plaques, cape racks | Wall mount, display case, plaque, cape rack, pedestal |
| Pet Room | Pet house, display pedestals, feeding bowl | Pet house, pedestal, feeding bowl, toy, bed |
| Aquarium | Fish tanks, coral, treasure chest | Large tank, small tank, coral, treasure chest, diver decoration |
| Costume Room | Outfit storage, mirror, mannequin | Costume box, mirror, mannequin, wardrobe, chest |
| Storage Room | Extra bank-like storage chests | Storage chest (x4), shelving, crate, barrel |
| Throne Room | Throne, guards, lever, trapdoor | Throne, guard (x2), lever, trapdoor, banner |
| Dungeon | Traps, monsters, obstacles for visitors | Trap tile, monster spawn, obstacle, door, reward chest |
| Party Room | Drop party chest, balloon spawner, dance floor | Party chest, balloon machine, dance floor, DJ booth, banner |
| Music Room | Jukebox, instruments, speakers | Jukebox, piano, drum set, speaker (x2), rug |

### Room Data Model

| Field | Type | Description |
|---|---|---|
| room_id | string | Unique room identifier |
| house_id | string | House this room belongs to |
| room_type | string | One of the room types above |
| floor_level | integer | Floor level (-1 = basement, 0 = ground, 1+ = upper floors) |
| grid_x | integer | Horizontal position on the house grid |
| grid_y | integer | Vertical position on the house grid |
| rotation | integer (0-3) | Room rotation in 90-degree increments |
| wall_style | string | Wall appearance (from theme or custom) |
| floor_style | string | Floor appearance (from theme or custom) |
| lighting | enum | `bright`, `normal`, `dim`, `dark`, `custom` |
| furniture | array[object] | List of placed furniture with hotspot assignments |

---

## 3. Furniture System

Furniture is built using the Construction skill and placed on hotspots within rooms.

### Furniture Data Model

| Field | Type | Description |
|---|---|---|
| furniture_id | string | Unique furniture identifier |
| name | string | Display name |
| room_types | list[string] | Which room types this furniture can be placed in |
| hotspot | string | Specific placement slot within the room |
| construction_level | integer | Minimum Construction level to build |
| materials | JSON | Items consumed to build (e.g., `{"oak_plank": 4, "steel_nails": 4, "bolt_of_cloth": 2}`) |
| functionality | enum | `decorative`, `storage`, `teleport`, `altar`, `combat_dummy`, `cooking_range`, `crafting_bench`, `repair_stand`, `display`, `interactive` |
| tier | integer | Quality tier (1 = basic, 2 = oak, 3 = teak, 4 = mahogany, 5 = marble, 6 = gold) |
| xp_awarded | integer | Construction XP awarded on building |
| flatpackable | boolean | Whether this furniture can be built as a tradeable flatpack |
| examine_text | string | Text shown when examining the furniture |

### Material Tiers

Each tier uses progressively better materials and awards more XP.

| Tier | Name | Primary Material | Typical Level Range | XP Multiplier |
|---|---|---|---|---|
| 1 | Basic | Regular planks, bronze nails | 1 - 19 | 1.0x |
| 2 | Oak | Oak planks, steel nails | 20 - 39 | 1.5x |
| 3 | Teak | Teak planks, mithril nails | 40 - 59 | 2.0x |
| 4 | Mahogany | Mahogany planks, adamant nails | 60 - 79 | 3.0x |
| 5 | Marble | Marble blocks, gold leaf | 80 - 89 | 4.0x |
| 6 | Gold | Gold leaf, magic stone | 90 - 99 | 5.0x |

### Upgrade Path

Furniture placed on a hotspot can be upgraded in place by using higher-tier materials on the same hotspot. The old furniture is destroyed and replaced.

| Field | Type | Description |
|---|---|---|
| source_furniture_id | string | The current furniture |
| target_furniture_id | string | The upgraded furniture |
| additional_materials | JSON | Extra materials required beyond the target's base recipe |
| level_requirement | integer | Construction level required for the upgrade |
| preserves_contents | boolean | Whether stored items are preserved during upgrade (storage furniture) |

---

## 4. Portal Room

A dedicated room containing configurable teleport portals.

### Portal Configuration

| Field | Type | Description |
|---|---|---|
| portal_index | integer | Which portal slot (1, 2, 3, etc.) |
| destination_name | string | Display name of the teleport destination |
| destination_zone_id | integer | Zone ID of the destination |
| destination_coordinates | JSON | Exact coordinates within the zone (e.g., `{"x": 50, "y": 75}`) |
| attunement_cost | JSON | Cost to attune the portal (e.g., `{"law_rune": 100, "coins": 10000}`) |
| construction_level | integer | Minimum Construction level to attune this destination |
| quest_requirement | integer or null | Quest ID required to attune (null = none) |
| favorite | boolean | Marked as favorite for quick access |
| label | string | Player-assigned label for the portal |

### Portal Limits

| Field | Type | Description |
|---|---|---|
| Max Portals (base) | integer | Number of portals at minimum Construction level (default 3) |
| Max Portals (scaled) | integer | Maximum portals at 99 Construction (default 8) |
| Scaling Formula | string | How portal count scales with level (e.g., `3 + floor(level / 20)`) |

### Available Destinations

Dungeon masters define the pool of available destinations. Players choose from this pool when attuning portals.

| Field | Type | Description |
|---|---|---|
| destination_id | integer | Unique destination identifier |
| name | string | Display name |
| zone_id | integer | Target zone |
| coordinates | JSON | Target coordinates |
| level_required | integer | Minimum Construction level |
| quest_required | integer or null | Quest completion requirement |
| attunement_cost | JSON | Materials or currency to attune |
| category | enum | `city`, `dungeon`, `boss`, `skilling`, `minigame`, `custom` |

---

## 5. Trophy and Display Room

Rooms dedicated to showing off accomplishments, rare items, and collectibles.

### Display Types

| Display | Description | Capacity |
|---|---|---|
| Wall Mount | Mounted boss heads and trophies | 1 per mount, multiple mounts per wall |
| Display Case | Glass case showing rare items | 1 - 6 items per case |
| Achievement Plaque | Wall-mounted plaque with achievement name and date | 1 per plaque |
| Kill Count Board | Board displaying boss kill counts | 1 boss per board |
| Cape Rack | Rack holding skill capes and achievement capes | 1 - 10 capes per rack |
| Pet Pedestal | Pedestal displaying a pet (links to pet system) | 1 pet per pedestal |
| Collection Log Book | Open book on lectern showing collection log highlights | N/A (interactive) |
| Clan Trophy | Display of clan achievements and tournament wins | 1 per display |
| Painting | Framed artwork or screenshot | 1 per frame |
| Statue | Full-body statue of the player or notable NPC | 1 per base |

### Display Item Data Model

| Field | Type | Description |
|---|---|---|
| display_id | string | Unique display instance identifier |
| display_type | string | One of the display types above |
| room_id | string | Room where this display is placed |
| hotspot | string | Which hotspot in the room |
| item_id | integer or null | ID of the item being displayed (null for plaques, boards) |
| boss_id | integer or null | Boss ID for wall mounts and kill count boards |
| achievement_id | string or null | Achievement ID for plaques |
| label | string | Player-assigned label or caption |
| obtained_date | datetime or null | Date the displayed item or trophy was obtained |
| kill_count | integer or null | KC at time of obtaining (boss trophies) |

---

## 6. Storage

Dedicated storage rooms that do not consume bank space.

### Storage Types

| Storage | Description | Capacity | Item Restrictions |
|---|---|---|---|
| Costume Box | Store complete outfits | 40 - 100 outfit slots (scales with tier) | Wearable items only |
| Tool Storage | Axes, pickaxes, harpoons, etc. | 1 per tool type | Tools only |
| Armour Case | Full armour sets | 20 - 50 sets (scales with tier) | Armour only |
| Magic Wardrobe | Robes, mystic gear, magical clothing | 20 - 50 sets (scales with tier) | Magic-related wearables only |
| Treasure Chest | Clue scroll reward items | 50 - 200 items (scales with tier) | Clue scroll rewards only |
| Cape Rack | All capes | 1 per cape type | Capes only |
| Toy Box | Fun items, holiday items, emote unlocks | 50 items | Fun and holiday items only |
| Pet House | Pets not currently following the player | 20 - 50 pets (scales with tier) | Pets only |
| Seed Vault | Seeds and saplings | 100 - 500 stacks (scales with tier) | Seeds and saplings only |
| Spice Rack | Spices and cooking ingredients | 50 stacks | Spices only |

### Storage Data Model

| Field | Type | Description |
|---|---|---|
| storage_id | string | Unique storage instance identifier |
| storage_type | string | One of the storage types above |
| room_id | string | Room where this storage is located |
| tier | integer | Quality tier (determines capacity) |
| capacity | integer | Maximum items or slots |
| contents | array[object] | List of stored items with IDs and quantities |
| searchable | boolean | Whether the player can search by name (always true) |
| sort_options | list[string] | Available sort orders: `name`, `value`, `date_stored`, `quantity` |

---

## 7. Butler and Servant System

Hire NPC servants to perform house tasks. Higher-tier servants are faster and more capable.

### Servant Tiers

| Tier | Name | Construction Level | Salary (GP per use) | Speed | Tasks |
|---|---|---|---|---|---|
| 1 | Rick | 20 | 500 | Slow | Fetch from bank, basic cooking |
| 2 | Maid | 25 | 1,000 | Moderate | Fetch from bank, cook food, serve guests |
| 3 | Cook | 30 | 3,000 | Moderate | All above + unnote items |
| 4 | Butler | 40 | 5,000 | Fast | All above + faster bank runs |
| 5 | Demon Butler | 50 | 10,000 | Very fast | All above + multiple item types per trip |

### Servant Tasks

| Task | Description |
|---|---|
| Fetch from Bank | Retrieve items from the player's bank and deliver to the house |
| Cook Food | Cook raw food on the house kitchen range |
| Serve Guests | Offer food to visiting players |
| Unnote Items | Convert noted items to unnoted form |
| Greet Visitors | Welcome visitors at the entrance |
| Clean House | Cosmetic animation (no gameplay effect) |

### Servant Data Model

| Field | Type | Description |
|---|---|---|
| servant_id | string | Unique servant instance identifier |
| house_id | string | House the servant is assigned to |
| tier | integer | Servant tier (1 - 5) |
| name | string | Servant display name |
| salary_per_use | integer | GP cost per task performed |
| items_per_trip | integer | Number of item stacks fetched per bank trip |
| trip_time | integer (ticks) | Time to complete a bank trip |
| last_paid | datetime | When the servant was last paid |

---

## 8. Visitor System

Other players can visit a house, interact with its features, and participate in activities.

### Visitor Features

| Feature | Description |
|---|---|
| Open House Mode | House is open to anyone, no invitation required |
| Party Mode | Drop parties, balloon events, group activities |
| Guest Book | Visitors sign in with a message (viewable by owner) |
| House Rating | Visitors rate the house (1 - 5 stars). Average displayed on profile |
| House Tour | Owner guides visitors through rooms with waypoints and descriptions |
| PvP Room | Visitors can challenge each other or the owner in the combat room |
| Dungeon Challenge | Visitors attempt to complete the owner-built dungeon |
| Altar Access | Visitors can use the chapel altar for prayer training |
| Portal Access | Visitors can use house portals (if owner permits) |
| Cooking Access | Visitors can use the kitchen range (if owner permits) |

### Visitor Permissions

| Permission | Options | Description |
|---|---|---|
| Entry | `anyone`, `friends`, `clan`, `locked` | Who can enter the house |
| Portal Use | `owner_only`, `friends`, `clan`, `anyone` | Who can use teleport portals |
| Altar Use | `owner_only`, `friends`, `clan`, `anyone` | Who can use the chapel altar |
| Kitchen Use | `owner_only`, `friends`, `clan`, `anyone` | Who can use cooking facilities |
| PvP Challenge | `on`, `off` | Whether PvP is allowed in the combat room |
| Dungeon Entry | `on`, `off` | Whether visitors can attempt the dungeon |

### Guest Book Data Model

| Field | Type | Description |
|---|---|---|
| entry_id | string | Unique guest book entry identifier |
| house_id | string | House visited |
| visitor_id | string | Player who signed the guest book |
| visitor_name | string | Display name at time of visit |
| message | string | Message left by the visitor (max 200 characters) |
| rating | integer (1-5) | Star rating given to the house |
| visited_at | datetime | When the visit occurred |

### House Rating System

| Field | Type | Description |
|---|---|---|
| house_id | string | House being rated |
| total_ratings | integer | Number of ratings received |
| average_rating | float (1.0 - 5.0) | Average star rating |
| last_rated | datetime | When the last rating was received |
| rating_cooldown | integer (hours) | Minimum time before the same visitor can re-rate (default 24h) |

---

## 9. Construction Skill Integration

Player housing is built and improved through the Construction skill. All building actions award Construction XP.

### Building Process

| Step | Description |
|---|---|
| 1. Enter Build Mode | Toggle build mode in house settings |
| 2. Select Hotspot | Click on an empty or occupied hotspot in a room |
| 3. Choose Furniture | Select from available furniture (filtered by level and materials) |
| 4. Consume Materials | Materials are removed from inventory |
| 5. Award XP | Construction XP is granted based on furniture tier |
| 6. Place Furniture | Furniture appears on the hotspot |

### XP Rates

| Action | Base XP | Notes |
|---|---|---|
| Build Furniture | Varies by item | See furniture data model for per-item XP |
| Remove Furniture | 0 | No XP for removal |
| Build Room | 100 - 500 | Flat XP for adding a new room (scales with floor level) |
| Remove Room | 0 | No XP for removal |
| Upgrade Furniture | Target item XP only | No bonus for upgrading vs building fresh |

### Flatpack System

Furniture can be built as a tradeable flatpack item instead of being placed directly.

| Field | Type | Description |
|---|---|---|
| flatpack_item_id | integer | Item ID of the flatpack version |
| furniture_id | string | Furniture this flatpack represents |
| construction_level | integer | Level required to build the flatpack |
| materials | JSON | Same materials as direct build |
| xp_awarded | integer | Same XP as direct build |
| tradeable | boolean | Always true for flatpacks |
| place_level | integer | Construction level required to place the flatpack (same as build level) |

### Construction Contracts (Mahogany Homes Style)

NPC-assigned build tasks that award bonus Construction XP.

| Field | Type | Description |
|---|---|---|
| contract_id | string | Unique contract identifier |
| npc_id | integer | NPC who assigns the contract |
| tier | enum | `beginner`, `intermediate`, `advanced`, `expert` |
| target_furniture | list[string] | Furniture items to build or upgrade |
| target_room | string | Room type where the work must be done |
| location_npc_name | string | Name of the NPC whose house needs work |
| base_xp_reward | integer | Bonus XP awarded on completion |
| point_reward | integer | Contract points earned (spent at reward shop) |
| time_limit | integer (minutes) or null | Optional time limit for bonus rewards |

### Contract Tiers

| Tier | Construction Level | Bonus XP | Points | Description |
|---|---|---|---|---|
| Beginner | 1 - 19 | 500 | 1 | Build basic furniture |
| Intermediate | 20 - 49 | 2,000 | 2 | Build oak and teak furniture |
| Advanced | 50 - 79 | 5,000 | 3 | Build mahogany and marble furniture |
| Expert | 80 - 99 | 10,000 | 5 | Build gold and magic stone furniture |

### Contract Reward Shop

Points earned from contracts are spent at a dedicated reward shop.

| Reward | Cost (points) | Description |
|---|---|---|
| Carpenter's Outfit (piece) | 20 | Construction XP boost set (2.5% per piece, 10% full set) |
| Supply Crate | 5 | Contains random construction materials |
| Blueprint | 10 | Unlocks a cosmetic furniture variant |
| House Teleport Scroll (x10) | 3 | Teleport directly to your house |
| Plank Sack | 30 | Carry extra planks in a dedicated container |
| Golden Hammer | 50 | Cosmetic hammer with a small XP boost (1%) |

---

## Views

Predefined views for the player housing interface.

| View | Description |
|---|---|
| House Layout | Top-down grid view of all rooms across all floor levels. Drag rooms to reposition |
| Room Editor | Interior view of a single room with hotspots highlighted. Build, upgrade, or remove furniture |
| Furniture Catalog | Browsable list of all furniture, filtered by room type, tier, level, and materials. Shows locked and unlocked items |
| Portal Configuration | List of portal slots with current destinations, attunement status, and cost to change |
| Storage Browser | Unified view of all storage rooms with contents, search, and sort |
| Visitor Log | Guest book entries with visitor name, message, rating, and timestamp |
| House Rating Leaderboard | Server-wide ranking of houses by average visitor rating |
| Contract Board | Available construction contracts with tier, target, and rewards |
| Build Mode HUD | Overlay showing available hotspots, materials in inventory, and level requirements |

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---|---|
| Diagonal Wall Half-Paint | Tiles split diagonally by NE or NW walls render as two triangles with different textures/colors. Enables complex visual patterns, natural terrain transitions, and decorative floor designs within a single tile |
