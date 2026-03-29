# Shops

Game design document for the Shops plugin. This system covers NPC-run trading --- item sinks where players sell to shops, item sources where players buy from shops, and price floors and ceilings that anchor the economy. Separate from player-to-player trading and the Grand Exchange (see economy.md). Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the shops module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| General Stores | Generic shops that buy almost anything and sell basics |
| Specialty Stores | Shops that only buy and sell specific item categories |
| Guild Stores | Shops that require guild membership or skill level to access |
| Quest-Locked Stores | Shops that unlock after quest completion |
| Minigame Currency Stores | Shops that accept minigame tokens or points instead of coins |
| Traveling Merchant | A merchant that spawns periodically at random locations with rare stock |
| Black Market | Hidden shops selling illicit or rare items at premium prices |
| Dynamic Pricing | Prices adjust based on supply and demand over time |
| Per-World Stock | Stock levels are tracked per world rather than globally |
| Per-Account-Type Stock | Separate stock pools for mains, ironmen, and group ironmen |
| Ironman Separate Stock | Ironman accounts see isolated stock that other players cannot affect |
| Buy-Back System | Recently sold items appear in a buy-back tab for repurchase |
| Shop Runs | Limited daily or weekly stock that resets on a timer |
| Bulk Discount | Buying in large quantities applies a configurable discount |
| Price Comparison | Interface shows Grand Exchange price alongside shop price |
| Infinite Stock Items | Specific items never deplete regardless of purchases |
| Restock On Hop Prevention | Stock does not reset when a player hops worlds |
| Player-Fed Stock | Players sell items to a shop and other players can buy them |
| Clan Stores | Internal clan marketplace using clan currency |
| Custom Shop Types | Dungeon masters can define new shop types beyond the built-in list |

---

## Linked Spreadsheets

Three spreadsheets define the full shop data model. All are linked by shop ID.

---

### 1. Shop List (Main Table)

The primary spreadsheet. Every shop has one row.

#### Identity

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique shop identifier |
| Name | String | Display name (e.g. "Varrock Sword Shop") |
| Owner NPC ID | Integer | NPC that runs this shop (links to npcs.md) |
| Location | String | Zone ID where the shop is located (links to locations.md) |
| Shop Type | Enum | One of the shop types below |

#### Shop Types

| Type | Description |
|---|---|
| General Store | Buys almost anything, sells basics. Low prices. Every town has one |
| Specialty Store | Only buys and sells specific item categories (weapons, runes, food, etc.) |
| Guild Store | Requires guild membership or skill level to access |
| Quest Store | Unlocked by quest completion (links to quests.md) |
| Minigame Store | Uses minigame currency such as tokens or points (links to minigames.md) |
| Clan Store | Internal clan marketplace (links to clan-system.md) |
| Ironman Store | Separate stock per ironman account for isolated economy |
| Traveling Merchant | Appears periodically at random locations with rare stock |
| Black Market | Sells illicit or rare items at premium prices from a hidden location |
| Custom | Dungeon master defines a new shop type |

#### Access Requirements

| Column | Type | Description |
|---|---|---|
| Quest Requirement | Integer | Quest ID that must be completed to access (0 = none) |
| Skill Requirement | String | Skill and minimum level required (e.g. "Smithing 50") |
| Faction Requirement | Integer | Minimum faction reputation required (0 = none) |
| Account Type | Enum | Restricted to specific account types (all, ironman, group ironman) |

---

### 2. Shop Stock (Linked Table)

One row per item per shop. Defines what the shop sells and buys.

#### Item Entry

| Column | Type | Description |
|---|---|---|
| Shop ID | Integer | Links to shop list |
| Item ID | Integer | Item sold or accepted (links to items.md) |
| Item Name | String | Display name for reference |
| Default Stock | Integer | Base quantity the shop starts with |
| Max Stock | Integer | Overstocking cap when players sell to the shop |
| Restock Rate | Integer | Items per minute returning to default stock level |
| Overstock Decay | Integer | Items per minute draining when stock exceeds default |
| Infinite Stock | Boolean | Item never depletes regardless of purchases |
| Members Only | Boolean | Item only available on members worlds |

#### Pricing

| Column | Type | Description |
|---|---|---|
| Base Price | Integer | Price at default stock level |
| Currency Type | String | Currency used (coins, slayer points, tokkul, dungeoneering tokens, custom) |
| Price Increase Per Stock Decrease | Float | Percentage price rises as stock drops below default |
| Price Decrease Per Stock Increase | Float | Percentage price drops as stock exceeds default |
| Minimum Buy Price | Integer | Floor price --- shop never sells below this |
| Sell Price Ratio | Float | Percentage of base value the shop pays when buying from a player (e.g. 0.60 for specialty, 0.30 for general) |
| Minimum Sell Price | Float | Floor percentage --- shop always pays at least this fraction of item value (default: 0.10) |
| Buy Limit | Integer | Maximum quantity a player can buy per visit or per time period (0 = unlimited) |

#### Stock Behavior

| Column | Type | Description |
|---|---|---|
| Stock Scope | Enum | Per-world (default), global, or per-player |
| Account Type Isolation | Boolean | Separate stock for mains vs ironmen vs group ironmen |
| Player-Fed | Boolean | Players can sell items to the shop for other players to buy |
| Restock On Hop | Boolean | Whether stock resets when a player hops worlds |

---

### 3. Shop Interface (Configuration Table)

One row per shop. Defines how the shop UI is presented.

| Column | Type | Description |
|---|---|---|
| Shop ID | Integer | Links to shop list |
| Grid Columns | Integer | Number of columns in the shop grid |
| Grid Rows | Integer | Number of rows in the shop grid |
| Search Enabled | Boolean | Whether the shop has a search and filter function |
| Category Tabs | Boolean | Whether large shops display category tabs |
| Price Display | Boolean | Show price before the player confirms a purchase |
| Quantity Options | String | Available quantity presets (e.g. "1, 5, 10, 50, All, Custom") |
| Value Check | Boolean | Right-click to see what the shop will pay before selling |
| GE Price Comparison | Boolean | Show Grand Exchange price alongside shop price |

---

## Buy-Back System

Recently sold items appear in a buy-back tab within the shop interface.

| Property | Description |
|---|---|
| Repurchase Price | Player can buy back at the price they sold for |
| Expiry | Buy-back clears when the shop window is closed or after a configurable number of minutes |
| Slot Limit | Maximum number of buy-back slots before oldest entries are removed |

---

## Dynamic Pricing

Optional system that adjusts prices based on supply and demand over time.

| Property | Description |
|---|---|
| Demand Tracking | Items bought frequently cost more over time |
| Supply Tracking | Items sold to the shop frequently cost less over time |
| Price History | Track price changes over configurable time windows |
| Update Frequency | How often prices recalculate (per tick, per hour, per day) |
| Ties To Economy | Feeds into the economy self-correcting system (see economy.md) |

---

## Traveling Merchant

A special shop type that appears periodically at random locations.

| Property | Description |
|---|---|
| Spawn Schedule | Configurable frequency and duration (e.g. every 4 hours for 30 minutes) |
| Spawn Locations | Pool of possible locations the merchant can appear at |
| Stock Pool | Master list of items the merchant can carry |
| Stock Selection | Random from pool or rotating on a fixed schedule |
| Stock Quantity | Limited --- once sold, gone until next spawn |
| Server Broadcast | Announce merchant arrival to all players on the server |
| Despawn Timer | Merchant disappears after configurable duration |

---

## Shop Runs

Limited stock that resets on a timer, creating daily or weekly profit opportunities. Ties into dailies.md.

| Property | Description |
|---|---|
| Reset Timer | Daily, weekly, or custom interval |
| Stock Quantity | Amount available per reset cycle |
| Diary Scaling | Stock quantity increases with diary or achievement tier (links to achievements.md) |
| Estimated Profit | Calculated profit per run based on buy price vs GE price |

---

## Views

| View | Description |
|---|---|
| All Shops | Every shop in the database |
| By Type | Filter by shop type (general, specialty, guild, etc.) |
| By Location | Filter by zone or region (links to locations.md) |
| By Currency | Filter by currency type accepted |
| By Item | Search which shops sell a specific item |
| Stock Status | Current stock levels across all shops |
| Price Comparison | Shop price vs Grand Exchange price for stocked items |
| Shop Run Tracker | Daily profit opportunities with reset timers |
