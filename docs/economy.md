# Economy

Game economy system covering currency, trading, shops, wealth monitoring, and self-correcting balance mechanics.

---

## 1. Currency System

### Primary Currency

| Property | Description |
|---|---|
| Name | Configurable (e.g. coins, gold, GP) |
| Max stack | Integer limit or custom cap |
| Display format | Shortened display (e.g. 1.2M, 45K) |

### Secondary Currencies

Optional token-style currencies that exist alongside the primary currency.

| Property | Description |
|---|---|
| ID | Unique identifier |
| Name | Display name (e.g. Slayer Points, Marks of Grace) |
| Conversion rate | Exchange rate to primary currency (0 = non-convertible) |
| Sources | How the currency is earned |
| Tradeable | Whether it can be traded between players |

### Gold Sources

| Source | Description |
|---|---|
| Monster drops | Coins dropped by NPCs on death |
| Alchemy | High/low alch spell converts items to gold |
| NPC shop selling | Players sell items to NPC shops |
| Thieving | Pickpocketing NPCs or looting stalls |
| Skilling products | Gold earned through gathering/production skills |
| Quest rewards | One-time gold payouts from quest completion |
| Minigame rewards | Gold from minigame participation or completion |
| Custom | Server-defined gold sources |

### Gold Sinks

| Sink | Description |
|---|---|
| GE tax | Percentage taken on Grand Exchange sales |
| NPC shop buying | Players buy items from NPC shops |
| Construction costs | Building and furnishing player-owned content |
| Instance fees | Cost to enter instanced areas |
| Death reclaim fees | Cost to recover items on death |
| Travel tolls | Fees for transportation (charter ships, toll gates) |
| Repair costs | Repairing degraded equipment |
| Skill training costs | Tutors, supplies, unlocks |
| Clan coffer deposits | Contributions to clan treasury |
| Custom | Server-defined gold sinks |

### Item Sinks

| Sink | Description |
|---|---|
| Consumables | Food, potions, runes, ammunition consumed on use |
| Degradation to dust | Equipment that breaks permanently after use |
| GE item sink | Automated purchasing and removal of items |
| Death mechanics | Items lost permanently on death (configurable) |
| Sacrifice mechanics | Offering items for XP, favour, or rewards |
| Disassembly | Breaking items into components (destroys original) |
| Custom | Server-defined item sinks |

---

## 2. Grand Exchange

Linked spreadsheet: central marketplace for asynchronous item trading.

### Order Matching

| Rule | Description |
|---|---|
| Price matching | Lowest sell offer matches highest buy offer |
| Tie-breaking | Older offers prioritized at same price |
| Partial fills | Orders can fill partially over time |

### Slots

| Property | Description |
|---|---|
| Slots per player | Configurable (default 8) |
| Members bonus | Additional slots for members (optional) |

### Buy Limits

| Property | Description |
|---|---|
| Limit per item | Maximum quantity purchasable per time window |
| Time window | Hours per buy limit cycle (e.g. 4 hours) |
| No sell limits | Players can sell any quantity |

### Pricing

| Property | Description |
|---|---|
| Guide price | Auto-adjusting reference price |
| Update frequency | How often guide prices recalculate |
| Max change per update | Percentage cap on price movement per cycle |
| Price floor | Optional minimum price (prevents crashing to 0) |
| Price ceiling | Optional maximum price |
| Price history | Historical price graphs per item |

### Tax

| Property | Default | Description |
|---|---|---|
| Tax rate | 2% | Percentage taken from completed sales |
| Tax cap | Configurable | Maximum tax per individual sale |
| Exemptions | Configurable | Items under X value are tax-free |
| Tax destination | Gold sink | Where tax revenue goes: gold sink, item sink fund, clan coffers, or custom |

### Item Sink (Tax-Funded)

| Property | Description |
|---|---|
| Automated purchasing | System buys items using accumulated tax revenue |
| Target items list | Which items the system targets for removal |
| Sink rate per item | How aggressively each item is purchased |
| Spending limit | Maximum tax revenue spent per cycle |

### Restrictions

| Restriction | Description |
|---|---|
| New accounts | Minimum hours played, quest points, or total level required |
| Ironman | Bonds only (no regular trading) |
| Item restrictions | Certain items excluded from GE |
| F2P restrictions | Limited item pool or slot count |

---

## 3. Player Trading

Linked spreadsheet: direct player-to-player item exchange.

### Direct Trade

| Feature | Description |
|---|---|
| Trade interface | Both players add items, confirm, exchange |
| Value warning | Alert when trade is lopsided beyond threshold |
| Warning threshold | Configurable value difference to trigger alert |
| Scam protection | Second confirmation screen after both accept |
| Trade restrictions | Level, quest, or time-based restrictions |
| Trade logging | All trades recorded with timestamps and values |

### Lending

| Feature | Description |
|---|---|
| Lend items | Temporarily give items to another player |
| Auto-return timer | Item returns after configurable duration |
| Lender recall | Original owner can recall at any time |
| Restrictions per item | Not all items are lendable |
| Insurance | Optional deposit required from borrower |
| History | Full lending log per player |

### Drop Trading

| Feature | Description |
|---|---|
| Mechanic | Items dropped on ground for another player to pick up |
| Prevention toggle | Enable or disable drop trading entirely |
| Visibility timer | Delay before dropped items become visible to others |

---

## 4. Shop System

Linked spreadsheet: NPC-operated stores.

### Shop Definition

| Property | Description |
|---|---|
| ID | Unique shop identifier |
| Name | Display name |
| NPC owner | NPC who operates the shop |
| Location | World coordinates or area |
| Currency accepted | Which currency the shop uses |
| Stock list | Items with quantity, price, and restock rate |

### Stock Types

| Type | Description |
|---|---|
| Fixed | Infinite supply, price never changes |
| Limited | Finite stock that restocks over time |
| Player-fed | Players sell items into stock, other players buy them |

### Pricing Mechanics

| Mechanic | Description |
|---|---|
| Restock rate | Time for limited stock to replenish |
| Overstocking | Prices drop when stock exceeds default quantity |
| Understocking | Prices rise when stock is below default quantity |
| Buy-back price | What the shop pays when players sell items to it |
| Specialty shops | Shops that only deal in specific item categories |

### Ironman Rules

| Rule | Description |
|---|---|
| Separate stock | Ironman players see independent stock from normal players |
| No player-fed | Ironman players cannot buy from player-fed stock |

---

## 5. Wealth Distribution

DM dashboard for monitoring and controlling the economy.

### Monitoring

| Metric | Description |
|---|---|
| Total money supply | All gold in circulation (player inventories, banks, GE offers) |
| Money supply over time | Historical graph of total gold |
| Average player wealth | Mean gold per active account |
| Wealth distribution | Breakdown by percentile (top 1%, median, bottom 50%) |
| Item price tracking | Price trends for key items |
| Most traded items | Highest volume items on GE |
| Biggest gold sources | Which sources inject the most gold |
| Biggest gold sinks | Which sinks remove the most gold |
| Net flow | Inflation rate (sources minus sinks over time) |

### Controls

| Control | Description |
|---|---|
| Emergency tax adjustment | Temporarily change GE tax rate |
| Price manipulation detection | Flag suspicious price movements |
| Item value alerts | Notifications when items spike or crash |
| Economy health score | Aggregate indicator of economy stability |
| Manual price intervention | Override guide prices (use sparingly) |

---

## 6. Economy Auto-Balancer

Self-correcting systems that adjust the economy without manual intervention.

### Dynamic Adjustments

| System | Description |
|---|---|
| Dynamic tax rate | Adjusts based on inflation/deflation indicators |
| Dynamic drop rates | Item price crashes reduce drop rate; overly rare items get slight increases |
| Dynamic shop prices | NPC shop prices adjust based on GE market prices |
| Dynamic gold drops | Gold from monsters scales inversely with total money supply |
| Auto-rebalance interval | How often the system recalculates adjustments |
| Soft vs hard caps | Soft caps warn DM; hard caps enforce limits automatically |

### Vulnerability Scanner

| Detection | Description |
|---|---|
| Abnormal trading patterns | Unusual volume, timing, or counterparties |
| Price manipulation | Coordinated buy/sell to inflate or crash prices |
| Gold farming patterns | Repetitive gold-generating behaviour from single accounts |
| RWT signals | Real-world trading indicators (suspicious trade values) |
| Dupe exploit detection | Sudden spike in quantity of specific items |
| Arbitrage loops | Exploitable price differences between shops and GE |
| Bot patterns | Inhuman input timing, pathing, or session length |

### Alert Severity

| Tier | Response |
|---|---|
| Info | Logged, no action |
| Warning | DM notified |
| Critical | DM notified, flagged for review |
| Emergency | Auto-response triggered |

### Auto-Response

| Action | Description |
|---|---|
| Flag account | Mark for DM review |
| Freeze trades | Temporarily suspend trading for flagged account |
| Auto-adjust prices | Correct manipulated prices |
| Rate limit | Throttle suspicious activity |
| Quarantine items | Hold items (not deleted) until DM reviews |
| Notify DM | Alert with full context |
| Auto-ban | Automatically ban confirmed violators (toggle) |

### Stress Testing

| Feature | Description |
|---|---|
| What-if scenarios | Simulate economic changes before deploying |
| Inflation/deflation prediction | Model future economy state |
| New content impact | Estimate effect of new drops, shops, or sinks |
| Test new gold sinks | Simulate sink effectiveness |
| Historical comparison | Compare current state against past data |

### Bounty System

| Feature | Description |
|---|---|
| Player reports | Players report suspected exploits for rewards |
| DM bounties | DM posts specific economy investigations |
| Rewards | Titles, GP, cosmetics for confirmed vulnerabilities |
| Bug bounty board | Public listing of active bounties |
| Hall of fame | Recognition for contributors |

### Transparency Dashboard (Optional)

Public-facing economy information available to all players.

| Widget | Description |
|---|---|
| Money supply graph | Total gold over time |
| Item price history | Per-item price charts |
| Tax revenue | Gold removed via GE tax |
| Items sunk | Count of items removed from game |
| Inflation indicator | Green / yellow / red status |
| Most traded | Top items by volume |
| Economy health score | Letter grade A through F |

---

## Module Toggles

Every economy subsystem can be independently enabled or disabled.

| Module | Default | Description |
|---|---|---|
| Grand Exchange | On | Central marketplace |
| GE Tax | On | Tax on GE sales |
| GE Item Sink | On | Automated item removal via tax revenue |
| Player Trading | On | Direct player-to-player trades |
| Drop Trading | On | Dropping items for others |
| Lending | Off | Item lending system |
| NPC Shops | On | NPC-operated stores |
| Alchemy | On | Convert items to gold via spells |
| Buy Limits | On | Per-item purchase caps on GE |
| Price Guidance | On | Auto-adjusting guide prices |
| Trade Logging | On | Record all trades |
| Wealth Dashboard | On | DM economy monitoring |
| Price Intervention | Off | Manual price overrides |
| Multiple Currencies | Off | Secondary currency support |
| New Account Restrictions | On | GE/trade restrictions for new accounts |
| Self-Correcting Economy | Off | Dynamic adjustment systems |
| Dynamic Tax Rate | Off | Tax adjusts with inflation |
| Dynamic Drop Rates | Off | Drop rates adjust with prices |
| Dynamic Shop Prices | Off | Shop prices follow GE |
| Vulnerability Scanner | Off | Automated exploit detection |
| Auto-Response | Off | Automated enforcement actions |
| Stress Testing | Off | Economy simulation tools |
| Bounty System | Off | Player-reported exploit rewards |
| Transparency Dashboard | Off | Public economy stats |

---

## Views

| View | Description |
|---|---|
| Price History | Per-item historical price charts |
| Most Traded | Items ranked by trade volume |
| Biggest Movers | Items with largest recent price changes |
| Gold Sources vs Sinks | Net gold flow breakdown |
| Wealth Distribution | Player wealth percentile breakdown |
| Shop Directory | All NPC shops with stock and location |
| Trade Log | Searchable history of all trades |
| Economy Health | Aggregate dashboard with key indicators |
