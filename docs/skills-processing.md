# Processing Skills

## Overview

Processing skills transform raw inputs into finished products. All five processing skills share a common pattern but each introduces unique mechanics that a dungeon master can toggle and configure.

The five processing skills are:

| Skill | Pattern | Source |
|-------|---------|--------|
| Cooking | Raw food + Heat source -> Cooked food | OSRS |
| Firemaking | Logs + Tinderbox -> Fire / Light | OSRS |
| Smithing (Smelting) | Ore + Furnace -> Bars | OSRS |
| Runecrafting | Essence + Altar -> Runes | OSRS |
| Thieving | NPC/Stall/Chest + Skill -> Loot | OSRS |

---

## Shared Processing Pattern

All processing skills follow the same core loop:

```
Tool + Input + Station = Product + XP (with chance of failure)
```

### Shared Configurable Properties

| Property | Description | Default |
|----------|-------------|---------|
| `baseSuccessLow` | Success chance at level 1 (out of 255) | Per-recipe |
| `baseSuccessHigh` | Success chance at level 99 (out of 255) | Per-recipe |
| `xpPerAction` | Experience granted on success | Per-recipe |
| `levelRequired` | Minimum skill level to attempt | Per-recipe |
| `stopFailLevel` | Level at which failure rate hits 0% | Per-recipe |
| `ticksPerAction` | Game ticks per processing action | Per-skill |
| `toolRequired` | Item needed in inventory or equipped | Per-skill |
| `stationRequired` | World object the player must be near | Per-recipe |

### Universal Success Formula

OSRS uses a single linear interpolation formula across all skilling success checks:

```
successChance = floor( lowChance * (99 - level) / 98 + highChance * (level - 1) / 98 + 0.5 ) + 1
successRate = successChance / 256
```

Where:
- `level` = player's current skill level (1-99)
- `lowChance` = the success value at level 1 (out of 255)
- `highChance` = the success value at level 99 (out of 255)
- Result is clamped between 0 and 1

This formula applies to cooking burn rates, firemaking light success, iron ore smelting, and thieving pickpocket rates. Each recipe/action defines its own `lowChance` and `highChance` pair.

---

## 1. Cooking

### Core Mechanic

Raw food + Heat source (fire or range) -> Cooked food or Burnt food.

On failure, the raw food is destroyed and replaced with a burnt version. No XP is granted on failure.

### Heat Sources

| Station | Burn Bonus | Access Requirement | Notes |
|---------|------------|-------------------|-------|
| Fire (any logs) | None (baseline) | None | Disappears after time |
| Range (standard) | Lower burn rate than fire | None | Permanent world object |
| Lumbridge Range | Lower burn than standard range | Cook's Assistant quest | Special range |
| Hosidius Kitchen | +5% success (+10% with Elite diary) | Easy Kourend & Kebos Diary | Near bank chest |
| Rogues' Den fire | Same as fire | Members | Permanent, next to banker |

### Cooking Methods

| Method | Station | Notes |
|--------|---------|-------|
| Meat/Fish | Fire or Range | Standard processing |
| Bread | Range only | Cannot cook on fire |
| Pies | Range only | Multi-step (shell + filling + cooking) |
| Pizzas | Range only | Multi-step (base + topping + cooking) |
| Cakes | Range only | Multi-step |
| Wine | No station | Combine grapes + jug of water, ferments over time |
| Gnome cooking | Range | Complex multi-ingredient recipes |
| Brewing | Brewery vat | Long fermentation (3 x 640 minute cycles) |

### Food Table (Key Entries)

| Food | Level | XP | Heals | Stop-Burn (Fire) | Stop-Burn (Range) | Stop-Burn (Gauntlets) |
|------|-------|----|-------|-------------------|--------------------|-----------------------|
| Shrimps | 1 | 30 | 3 | 34 | 34 | -- |
| Trout | 15 | 70 | 7 | 50 | 49 | -- |
| Salmon | 25 | 90 | 9 | 58 | 55 | -- |
| Lobster | 40 | 120 | 12 | 74 | 74 | 64 |
| Swordfish | 45 | 140 | 14 | 86 | 81 | 81 |
| Monkfish | 62 | 150 | 16 | 92 | 90 | 82 |
| Shark | 80 | 210 | 20 | 99+ | 99+ | 94 |
| Anglerfish | 84 | 230 | 3-22 | 99+ | 99+ | 98 |
| Dark crab | 90 | 215 | 22 | 99+ | 99+ | 99+ |

### Wine Fermentation

| Property | Value |
|----------|-------|
| Level required | 35 |
| XP per wine | 200 |
| Heals | 11 HP (Jug of wine) |
| Fermentation time | 12 seconds (20 ticks) |
| Batch reset | Making new wine resets the 12-second timer for all pending wines |
| Failure | Produces jug of bad wine (no XP) |
| Max XP/hr | 400,000+ with optimal batching |

### Equipment Bonuses

| Item | Effect | Source |
|------|--------|--------|
| Cooking gauntlets | Lowers stop-burn level for specific foods | Family Crest quest |
| Cooking cape (99) | Never burn any food | Skill mastery |
| Hosidius range | +5% or +10% success chance | Kourend diary |

### Tick Manipulation (2-Tick Cooking)

Drop raw food on ground near range, alternate between picking up and clicking range. Skips the cooking interface when only one raw item is in inventory. Yields 30-40% faster cooking (approximately 40 seconds per inventory instead of 65).

### Configurable Properties

| Property | Description |
|----------|-------------|
| `burnFormulaEnabled` | Whether food can burn at all |
| `burnRatePerFood` | Low/high chance pair per food item |
| `stopBurnLevels` | Level thresholds per food per station type |
| `gauntletsReduction` | How much gauntlets lower the stop-burn level |
| `rangeBonus` | Percentage bonus for range vs fire |
| `specialRangeBonus` | Hosidius/Lumbridge bonus percentage |
| `wineFermentationTicks` | Ticks before wine completes |
| `wineBatchReset` | Whether new wines reset pending fermentation |
| `tickManipulationEnabled` | Allow 2-tick cooking method |
| `brewingCycleMinutes` | Duration of each brewing cycle |

---

## 2. Firemaking

### Core Mechanic

Logs + Tinderbox -> Fire on the ground. Player steps west (or east/south/north if blocked).

### Log Types

| Log | Level | XP | Notes |
|-----|-------|----|-------|
| Normal | 1 | 40 | F2P |
| Achey | 1 | 40 | Special conditions |
| Oak | 15 | 60 | |
| Willow | 30 | 90 | |
| Teak | 35 | 105 | |
| Arctic pine | 42 | 125 | |
| Maple | 45 | 135 | |
| Mahogany | 50 | 157.5 | |
| Yew | 60 | 202.5 | |
| Blisterwood | 62 | 96 | Quest-locked |
| Magic | 75 | 303.8 | |
| Redwood | 90 | 350 | |

### Fire Lighting Success Formula

Uses the universal skilling success formula:
- Level 1: 65/256 chance (~25%)
- Level 43: 256/256 (100% for normal logs)
- Level 99: 513/256 (capped at 100%)
- Coloured logs: always 100% success

### Fire Placement Rules

| Rule | Detail |
|------|--------|
| Requires | Empty ground tile, no plants/ferns/doorways |
| Movement after lighting | West > East > South > North (first available) |
| Closed doors | Block fire placement |
| Open doors | Allow fire placement |
| Fire duration | Random, unpredictable regardless of level or log type |

### Light Sources

| Level | Item | Type |
|-------|------|------|
| 1 | Torch | Purchasable |
| 1 | Candle | Purchasable |
| 4 | Candle lantern | Craftable |
| 12 | Oil lamp | Craftable/purchasable |
| 26 | Oil lantern | Craftable |
| 49 | Bullseye lantern | Craftable |
| 65 | Mining helmet | Monster drop |
| 99 | Firemaking cape | Inextinguishable light source |

### Wintertodt (Boss Skilling)

A group boss that is subdued by maintaining braziers rather than dealing combat damage.

| Mechanic | Detail |
|----------|--------|
| Minimum points for reward | 500 |
| Lighting brazier | 25 points, 6x Firemaking level XP |
| Feeding bruma root | 10 points, 3x Firemaking level XP |
| Feeding kindling | 25 points, 3.8x Firemaking level XP |
| Fletching root to kindling | 0 points, 0.6x Fletching level XP |
| Repairing brazier | 25 points, 4x Construction level XP |
| Healing pyromancer | 75 points |
| Subduing boss (500+ pts) | 100x Firemaking level XP |
| Warm clothing | 4 pieces for max damage reduction |
| Damage scaling | Based on Hitpoints level; lower HP = less damage taken |

### Pyre Logs (Shade Burning)

Logs anointed with sacred oil, burned at funeral pyres with shade remains to earn shade keys.

| Log | Level | Sacred Oil Doses | XP |
|-----|-------|-----------------|-----|
| Normal | 5 | 2 | 50.5 |
| Oak | 20 | 2 | 70 |
| Willow | 35 | 3 | 100 |
| Maple | 45 | 3 | 175 |
| Yew | 60 | 4 | 255 |
| Magic | 75 | 4 | 404.5 |
| Redwood | 95 | 4 | 500 |

### Barbarian Firemaking

| Method | Requirement | Detail |
|--------|------------|--------|
| Bow firemaking | 20 levels above tinderbox method | Use a player-crafted bow to light logs |
| Pyre ships | Firemaking + Crafting 11+ | Burn chewed/mangled bones for prayer bonus (up to 300% multiplier) |

### Equipment Bonuses

| Item | Effect |
|------|--------|
| Pyromancer outfit | XP bonus (Wintertodt reward) |
| Firemaking cape | Inextinguishable light source, +1 boost |

### Configurable Properties

| Property | Description |
|----------|-------------|
| `logTypes[]` | Available log tiers with level/XP |
| `lightSuccessFormula` | Low/high pair for fire lighting success |
| `fireDurationMode` | Fixed ticks, random, or level-scaled |
| `placementRules` | Which tiles allow fires |
| `movementPriority` | Direction order after lighting (W/E/S/N) |
| `wintertodtEnabled` | Toggle the Wintertodt boss |
| `wintertodtPointsForReward` | Minimum points threshold |
| `wintertodtXPMultipliers` | Per-action XP scaling factors |
| `warmClothingSlots` | Number of warm items for max reduction |
| `pyreLogsEnabled` | Toggle shade burning system |
| `sacredOilDoses` | Oil cost per pyre log tier |
| `barbarianFiremakingEnabled` | Toggle bow/pyre ship methods |

---

## 3. Smithing (Smelting + Anvil)

### Core Mechanic (Smelting)

Ore + Coal + Furnace -> Metal bar. Some bars have a failure chance.

### Core Mechanic (Anvil)

Bar + Hammer + Anvil -> Equipment. Always succeeds. Fixed tick rate.

### Bar Smelting Table

| Bar | Level | Primary Ore | Coal | XP | Success Rate |
|-----|-------|-------------|------|----|-------------|
| Bronze | 1 | Tin (1) + Copper (1) | 0 | 6.2 | 100% |
| Iron | 15 | Iron (1) | 0 | 12.5 | 50% (furnace) |
| Silver | 20 | Silver (1) | 0 | 13.7 | 100% |
| Steel | 30 | Iron (1) | 2 | 17.5 | 100% |
| Gold | 40 | Gold (1) | 0 | 22.5 | 100% |
| Mithril | 50 | Mithril (1) | 4 | 30 | 100% |
| Adamantite | 70 | Adamantite (1) | 6 | 37.5 | 100% |
| Runite | 85 | Runite (1) | 8 | 50 | 100% |

### Iron Smelting Success

Iron ore has a flat 50% success rate at a standard furnace, regardless of Smithing level. Failed attempts consume the ore. This can be bypassed by:

| Method | Effect |
|--------|--------|
| Ring of forging | 100% success (140 charges) |
| Superheat Item spell | 100% success + Magic XP |
| Blast Furnace | 100% success + half coal |

### Blast Furnace

A members-only smelting facility that halves coal requirements and guarantees 100% success on all bars.

| Property | Detail |
|----------|--------|
| Coal requirement | Half of normal (e.g., Steel = 1 coal, Runite = 4 coal) |
| Success rate | 100% for all bars including iron |
| Cost (official worlds) | 72,000 coins/hour (1,200/minute) via coffer |
| Cost (non-official) | 2,500 coins per 10 minutes, free at 60+ Smithing |
| Ring of Charos discount | 1,250 coins per 10 minutes |
| NPC workers | 5 dwarves operate on official worlds |
| Player operation | Manual pedals, pumps, repairs on non-official worlds |
| Ore storage limit | 254 coal, 254 tin, 28 primary ore |
| Bar dispenser capacity | 28 bars |
| Ice gloves | Skip water-cooling step when collecting bars |
| Smiths gloves (i) | Stack with ice gloves |
| Throughput | 1,150-6,850 bars/hour depending on ore type |

### Superheat Item Spell

| Property | Detail |
|----------|--------|
| Magic level required | 43 |
| Runes | 4 Fire + 1 Nature |
| Effect | Smelt one bar anywhere, no furnace needed |
| Iron success | 100% guaranteed |
| XP | Normal Smithing XP + 53 Magic XP |

### Goldsmith Gauntlets

| Property | Detail |
|----------|--------|
| Normal gold bar XP | 22.5 |
| With gauntlets | 56.2 XP (2.5x multiplier) |
| Source | Family Crest quest |

### Anvil Smithing

All anvil smithing takes 5 ticks per item (approximately 3 seconds), regardless of item type or bars used.

**Smiths' Uniform:** Each piece gives 20% chance to speed up anvil actions by 1 tick (5 -> 4 ticks). Full set = 100% chance = always 4 ticks.

**XP Formula:** `XP = baseXPperBar * barsUsed`

| Metal | XP per Bar | Items Range | Bars per Item |
|-------|-----------|-------------|---------------|
| Bronze | 12.5 | Dagger (1) to Platebody (5) | 1-5 |
| Iron | 25 | Dagger (1) to Platebody (5) | 1-5 |
| Steel | 37.5 | Dagger (1) to Platebody (5) | 1-5 |
| Mithril | 50 | Dagger (1) to Platebody (5) | 1-5 |
| Adamant | 62.5 | Dagger (1) to Platebody (5) | 1-5 |
| Rune | 75 | Dagger (1) to Platebody (5) | 1-5 |

**Standard item progression per metal tier:** Dagger (1 bar) -> Axe (1) -> Mace (1) -> Med helm (1) -> Bolts (1) -> Sword (1) -> Dart tips (1) -> Nails (1) -> Scimitar (2) -> Arrowtips (1) -> Longsword (2) -> Full helm (2) -> Knife (1) -> Sq shield (2) -> Warhammer (3) -> Battleaxe (3) -> Chainbody (3) -> Kiteshield (3) -> Claws (2) -> 2h sword (3) -> Platelegs (3) -> Plateskirt (3) -> Platebody (5).

### Configurable Properties

| Property | Description |
|----------|-------------|
| `barRecipes[]` | Ore + coal combinations per bar |
| `ironFailureRate` | Chance of failing iron smelt (default 50%) |
| `blastFurnaceEnabled` | Toggle Blast Furnace |
| `blastFurnaceCoalReduction` | Percentage coal reduction (default 50%) |
| `blastFurnaceCostPerHour` | Operating cost in coins |
| `blastFurnaceFreeAtLevel` | Level at which BF becomes free |
| `superheatEnabled` | Toggle Superheat Item spell |
| `goldsmithGauntletsMultiplier` | XP multiplier for gold smelting |
| `anvilTickRate` | Ticks per smithing action (default 5) |
| `smithsUniformSpeedup` | Tick reduction per piece (default 1 tick at full set) |
| `itemsPerTier[]` | Which items can be made per metal tier |
| `barsPerItem{}` | Bar cost per item type |

---

## 4. Runecrafting

### Core Mechanic

Essence + Altar -> Runes. Higher levels yield multiple runes per essence.

### Rune Types

| Rune | Level | XP | Essence Type | Members | Category |
|------|-------|----|-------------|---------|----------|
| Air | 1 | 5 | Normal/Pure | F2P | Elemental |
| Mind | 2 | 5.5 | Normal/Pure | F2P | Catalytic |
| Water | 5 | 6 | Normal/Pure | F2P | Elemental |
| Earth | 9 | 6.5 | Normal/Pure | F2P | Elemental |
| Fire | 14 | 7 | Normal/Pure | F2P | Elemental |
| Body | 20 | 7.5 | Normal/Pure | F2P | Catalytic |
| Cosmic | 27 | 8 | Pure only | P2P | Catalytic |
| Chaos | 35 | 8.5 | Pure only | P2P | Catalytic |
| Nature | 44 | 9 | Pure only | P2P | Catalytic |
| Law | 54 | 9.5 | Pure only | P2P | Catalytic |
| Death | 65 | 10 | Pure only | P2P | Catalytic |
| Blood | 77 | 10.5 / 23.8 | Pure / Dark | P2P | Catalytic |
| Soul | 90 | 29.7 | Dark only | P2P | Catalytic |

### Multiple Runes Per Essence

At certain level thresholds, each essence produces additional runes (but XP stays the same per essence).

| Rune | 2x | 3x | 4x | 5x | 6x | 7x | 8x | 9x | 10x |
|------|----|----|----|----|----|----|----|----|-----|
| Air | 11 | 22 | 33 | 44 | 55 | 66 | 77 | 88 | 99 |
| Mind | 14 | 28 | 42 | 56 | 70 | 84 | 98 | -- | -- |
| Water | 19 | 38 | 57 | 76 | 95 | -- | -- | -- | -- |
| Earth | 26 | 52 | 78 | -- | -- | -- | -- | -- | -- |
| Fire | 35 | 70 | -- | -- | -- | -- | -- | -- | -- |
| Body | 46 | 92 | -- | -- | -- | -- | -- | -- | -- |
| Cosmic | 59 | -- | -- | -- | -- | -- | -- | -- | -- |
| Chaos | 74 | -- | -- | -- | -- | -- | -- | -- | -- |
| Nature | 91 | -- | -- | -- | -- | -- | -- | -- | -- |
| Law | 95 | -- | -- | -- | -- | -- | -- | -- | -- |
| Death | 99 | -- | -- | -- | -- | -- | -- | -- | -- |

### Essence Types

| Essence | Mining Level | Usable For | Notes |
|---------|-------------|-----------|-------|
| Rune essence | 1 | Air, Mind, Water, Earth, Fire, Body | F2P, mined from essence mine |
| Pure essence | 30 | All runes except Blood/Soul | Members, same mine but higher level |
| Daeyalt essence | -- | All standard runes | +50% XP bonus, requires Darkmeyer access |
| Dark essence fragments | 38 Mining + 38 Crafting | Blood, Soul | Mined at dense runestones, infused at Dark Altar, chiseled into fragments |

### Combination Runes

Crafted by bringing existing runes + pure essence + talisman to the opposite altar.

| Combo Rune | Components | Level | XP | Base Success |
|------------|-----------|-------|----|-------------|
| Mist | Air + Water | 6 | 8-8.5 | 50% |
| Dust | Air + Earth | 10 | 8.3-9 | 50% |
| Mud | Water + Earth | 13 | 9.3-9.5 | 50% |
| Smoke | Air + Fire | 15 | 8.5-9.5 | 50% |
| Steam | Water + Fire | 19 | 9.3-10 | 50% |
| Lava | Earth + Fire | 23 | 10-10.5 | 50% |

**Binding necklace:** Guarantees 100% success rate for combination runes (16 charges).

### Pouch System

Pouches hold extra essence beyond inventory space.

| Pouch | Level | Capacity | Degrades | Uses Before Decay |
|-------|-------|----------|----------|-------------------|
| Small | 1 | 3 | No | Infinite |
| Medium | 25 | 6 | Yes | ~45 |
| Large | 50 | 9 | Yes | ~29 |
| Giant | 75 | 12 | Yes | ~10 |
| Colossal | 85 | 40 | Yes | ~8 |

Degraded pouches must be repaired by the Dark Mage in the Abyss. Colossal pouch is crafted via abyssal needle (Guardians of the Rift reward).

### Altar Access

| Method | Detail |
|--------|--------|
| Talisman | Use on mysterious ruins to enter |
| Tiara | Wear to left-click enter ruins |
| Abyss | Teleport to Abyss, enter rift (no talisman needed, drains prayer, skulls player) |
| Abyssal bracelet | Prevents skull in Abyss |

### Blood and Soul Runecrafting (Zeah)

| Step | Detail |
|------|--------|
| 1 | Mine dense runestone (38 Mining) |
| 2 | Infuse at Dark Altar -> dark essence block |
| 3 | Chisel block -> dark essence fragments |
| 4 | Use fragments at Blood altar (77 RC) or Soul altar (90 RC) |
| No talisman needed | Altars are in the overworld |

### Guardians of the Rift

A minigame that provides an alternative Runecrafting training method.

| Mechanic | Detail |
|----------|--------|
| Requirement | Temple of the Eye quest (10 Runecrafting) |
| Loop | Mine fragments -> craft at workbench -> imbue at active portal -> create guardians |
| Active portals | 2 at a time (1 elemental, 1 catalytic) |
| Portal duration | ~25 seconds every ~2 minutes |
| Max active guardians | 10 |
| Energy cap | 1,000 per type (1,320 with abyssal lantern) |
| Reward threshold | 300 total energy minimum |
| Scaling | +250 guardian stones per player, +20% penalty beyond 20 players |

**Cells and Barriers:**

| Cell Type | Placement Energy | Strengthen Energy | Health Restored |
|-----------|-----------------|-------------------|-----------------|
| Weak | 2 | -- | 10 |
| Medium | 2 | 7 | 25 |
| Strong | 2 | 13 | 50 |
| Overcharged | 2 | 22 | 100 |

**Points per action:**

| Action | Elemental Energy | Catalytic Energy |
|--------|-----------------|------------------|
| Guardian stone | 2 | 2 |
| Polyelemental stone | 3 | -- |
| Weak cell placement | 17 | 17 |
| Medium cell placement | 20 | 20 |
| Strong cell placement | 24 | 24 |
| Overcharged placement | 30 | 30 |

**Rewards:**
- Abyssal pearls (currency)
- Raiments of the Eye outfit (+10% runes per piece, +20% set bonus = 60% total)
- Ring of the Elements (400 pearls)
- Abyssal lantern (various log-specific bonuses)
- Abyssal protector pet

### Configurable Properties

| Property | Description |
|----------|-------------|
| `runeTypes[]` | Available runes with level/XP/multiplier thresholds |
| `multipleRuneLevels{}` | Level thresholds for 2x, 3x, etc. per rune |
| `essenceTypes[]` | Which essence types exist and what they can craft |
| `daeyaltXPBonus` | Percentage XP bonus for daeyalt (default 50%) |
| `comboRuneBaseSuccess` | Base success rate for combination runes (default 50%) |
| `bindingNecklaceCharges` | Charges per necklace (default 16) |
| `pouches[]` | Capacity and decay rate per pouch tier |
| `pouchRepairMethod` | How pouches are repaired (NPC, spell, etc.) |
| `abyssEnabled` | Toggle Abyss access |
| `abyssPrayerDrain` | Whether Abyss drains prayer |
| `abyssSkullEnabled` | Whether Abyss skulls the player |
| `zeahRCEnabled` | Toggle Blood/Soul via dark essence |
| `gotrEnabled` | Toggle Guardians of the Rift minigame |
| `gotrScalingPerPlayer` | Energy requirement per additional player |
| `gotrMaxGuardians` | Maximum active rift guardians |
| `raimentsBonusPerPiece` | Percentage rune bonus per outfit piece |
| `raimentsSetBonus` | Additional set bonus percentage |

---

## 5. Thieving (Processing NPCs and Stalls into Loot)

Thieving follows the processing pattern: the "input" is an NPC, stall, or chest, and the "product" is loot. The "tool" is the player's skill level itself (no physical tool required).

### Core Mechanic

Attempt to pickpocket/steal -> Success (loot + XP) or Failure (stun + damage).

### Pickpocketing

Every pickpocket attempt takes 2 ticks (1.2 seconds). On failure, the player is stunned.

| NPC | Level | XP | Stun Damage | Notable Loot |
|-----|-------|----|-------------|-------------|
| Man/Woman | 1 | 8 | 1 | 3 gp |
| Farmer | 10 | 14.5 | 1 | 9 gp, seeds |
| H.A.M. Member | 15 | 22.2 | 1-3 | Clue scrolls, jewelry |
| Warrior | 25 | 26 | 2 | 18 gp |
| Master Farmer | 38 | 43 | 3 | Seeds (scales with Farming) |
| Guard | 40 | 46.8 | 2 | 30 gp |
| Knight of Ardougne | 55 | 84.3 | 3 | 50 gp |
| Paladin | 70 | 131.8 | 3 | 80 gp, chaos runes |
| Hero | 80 | 163.3 | 3 | Rare elite clues |
| Vyre | 82 | 306.9 | 5 | Blood shards (rare) |
| Elf | 85 | 353.3 | 5 | Enhanced crystal teleport seeds |
| TzHaar-Hur | 90 | 103.4 | 4 | Tokkul, uncut gems |

### Success Rate Formula

Pickpocketing uses the universal skilling success formula (linear interpolation). Each NPC has its own `lowChance` and `highChance` values that define success probability at level 1 and 99.

The success rate scales linearly between these endpoints. Equipment bonuses are applied as additive percentage modifiers after the base calculation.

### Stun Mechanics

| Property | Value |
|----------|-------|
| Stun duration | 9 ticks (5.4 seconds) |
| Pickpocket cooldown after stun | 8 ticks (4.8 seconds) |
| Stun damage | 1-5 HP depending on NPC |
| Eating during stun | Extends cooldown by food's attack delay |

**Average time per attempt:** `10 - 8p` ticks, where `p` = success probability.

**Pickpockets per N ticks:** `(N * p) / (10 - 8p)`

### Stall Thieving

Stalls have a 100% success rate but require the player to avoid line of sight of the stall owner and guards.

| Stall | Level | XP | Respawn Time | Notes |
|-------|-------|----|-------------|-------|
| Vegetable | 2 | 10 | 2.4s | |
| Baker's | 5 | 16 | 2.4s | |
| Tea | 5 | 16 | 4.8s | |
| Silk | 20 | 24 | 6s | |
| Wine | 22 | 27 | 7.2s | |
| Fur | 35 | 36 | 19.2s | |
| Silver | 50 | 54 | 60s | |
| Spice (Port Roberts) | 65 | 110 | 2.4s | 165k XP/hr |
| Gem | 75 | 160 | 105s | |
| Ore (Port Roberts) | 82 | 191 | 2.4s | 286k XP/hr |

**Stall owner cooldown:** After stealing, the owner refuses to interact for 10 in-game minutes.

### Chest Thieving

| Property | Detail |
|----------|--------|
| Trap detection | Must "Search for traps" before opening |
| Trap failure | Player takes damage scaled to current HP (with minimum) |
| Respawn range | 0 to 300 seconds depending on chest |
| Loot | Varies by chest location and quest status |

### Blackjacking

A special pickpocketing method for Pollnivnian NPCs (requires The Feud quest progress).

| Step | Detail |
|------|--------|
| 1 | Knock out NPC with blackjack weapon |
| 2 | Pickpocket while unconscious (100% success) |
| 3 | Can pickpocket twice per knockout |
| 4 | Failed knockout attempt aggros the NPC |

### Wall Safes

| Property | Detail |
|----------|--------|
| Level | 50 |
| XP | 70 per crack |
| Location | Rogues' Den |
| Failure | 2-6 damage, 2-second stun |
| Loot | Coins, uncut gems |

### Equipment and Bonuses

| Item/Bonus | Effect | Source |
|------------|--------|--------|
| Dodgy necklace | 25% chance to avoid stun on failure (10 charges) | Crafting |
| Gloves of silence | +5% pickpocket success | 54 Hunter |
| Rogue outfit (full set) | Guarantees double loot on pickpocket | Rogues' Den minigame |
| Ardougne diary (medium) | +10% success in Ardougne | Achievement diary |
| Ardougne diary (hard) | +10% success globally | Achievement diary |
| Thieving cape (99) | +10% pickpocket success | Skill mastery |
| Shadow Veil spell | 15% stun avoidance | 47 Magic |

### Configurable Properties

| Property | Description |
|----------|-------------|
| `pickpocketNPCs[]` | NPCs with level, XP, loot table, stun damage |
| `successFormula` | Low/high chance pair per NPC |
| `stunDurationTicks` | How long stun lasts (default 9) |
| `stunCooldownTicks` | Pickpocket cooldown after stun (default 8) |
| `stallThievingEnabled` | Toggle stall theft |
| `stallLineOfSight` | Whether guards/owners detect theft |
| `stallRespawnTicks` | Respawn time per stall |
| `stallOwnerCooldownMinutes` | Minutes before owner interacts again |
| `chestTrapsEnabled` | Whether chests have traps |
| `trapDamageScaling` | How trap damage scales with HP |
| `blackjackingEnabled` | Toggle knockout pickpocketing |
| `blackjackPickpocketsPerKO` | Successful pickpockets per knockout |
| `dodgyNecklaceCharges` | Charges per necklace (default 10) |
| `dodgyNecklaceAvoidChance` | Stun avoidance percentage (default 25%) |
| `rogueOutfitDoubleChance` | Double loot per piece, or full set only |
| `ardougneDiaryBonus` | Percentage bonus per diary tier |
| `glovesOfSilenceBonus` | Percentage success boost (default 5%) |
| `wallSafeEnabled` | Toggle wall safe cracking |

---

## Module Toggles (Dungeon Master Configuration)

Each processing skill's subsystems can be toggled independently.

### Cooking Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `cooking.enabled` | true | Master toggle for the skill |
| `cooking.burnRate` | true | Whether food can burn |
| `cooking.winemaking` | true | Wine fermentation system |
| `cooking.brewing` | true | Ale brewing at brewery vats |
| `cooking.pies` | true | Multi-step pie system |
| `cooking.gnomeCooking` | true | Complex gnome recipes |
| `cooking.2tickCooking` | true | Tick manipulation method |
| `cooking.specialRanges` | true | Hosidius/Lumbridge bonus ranges |
| `cooking.gauntlets` | true | Cooking gauntlets item |

### Firemaking Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `firemaking.enabled` | true | Master toggle |
| `firemaking.groundFires` | true | Traditional log burning |
| `firemaking.lightSources` | true | Lanterns, candles, torches |
| `firemaking.wintertodt` | true | Boss skilling minigame |
| `firemaking.shadeBurning` | true | Pyre logs and shade cremation |
| `firemaking.barbarianFiremaking` | true | Bow firemaking and pyre ships |
| `firemaking.pyromancerOutfit` | true | XP bonus outfit from Wintertodt |
| `firemaking.forestCampfire` | true | Slower communal firemaking |

### Smithing Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `smithing.enabled` | true | Master toggle |
| `smithing.smelting` | true | Furnace bar smelting |
| `smithing.anvilSmithing` | true | Shaping bars into items |
| `smithing.blastFurnace` | true | Half-coal fast smelting |
| `smithing.superheatItem` | true | Magic-based smelting |
| `smithing.ironFailure` | true | 50% iron ore failure rate |
| `smithing.goldsmithGauntlets` | true | Gold XP multiplier |
| `smithing.smithsUniform` | true | Anvil speed bonus |
| `smithing.crystalSinging` | true | Crystal equipment crafting |

### Runecrafting Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `runecrafting.enabled` | true | Master toggle |
| `runecrafting.multipleRunes` | true | Extra runes at higher levels |
| `runecrafting.combinationRunes` | true | Mist, dust, mud, etc. |
| `runecrafting.pouches` | true | Essence pouch system |
| `runecrafting.pouchDegradation` | true | Pouches break with use |
| `runecrafting.abyss` | true | Shortcut to all altars |
| `runecrafting.zeahRC` | true | Blood/Soul via dark essence |
| `runecrafting.gotr` | true | Guardians of the Rift minigame |
| `runecrafting.daeyaltEssence` | true | +50% XP essence variant |
| `runecrafting.raiments` | true | Rune bonus outfit |

### Thieving Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `thieving.enabled` | true | Master toggle |
| `thieving.pickpocketing` | true | NPC pickpocketing |
| `thieving.stallTheft` | true | Stall stealing |
| `thieving.chestTheft` | true | Trapped chest looting |
| `thieving.blackjacking` | true | Knockout pickpocketing |
| `thieving.wallSafes` | true | Safe cracking |
| `thieving.stunOnFailure` | true | Whether failure stuns |
| `thieving.dodgyNecklace` | true | Stun avoidance item |
| `thieving.rogueOutfit` | true | Double loot outfit |
| `thieving.ardougneBonus` | true | Diary-based success bonus |

---

## Summary: Processing as a Pattern

All five skills reduce to the same configurable framework:

| Component | Cooking | Firemaking | Smithing | Runecrafting | Thieving |
|-----------|---------|------------|----------|-------------|----------|
| Input | Raw food | Logs | Ore + Coal | Essence | NPC/Stall/Chest |
| Tool | None | Tinderbox | None (furnace) / Hammer (anvil) | Talisman/Tiara | None (skill only) |
| Station | Fire / Range | Ground tile | Furnace / Anvil | Altar | NPC location |
| Product | Cooked food | Fire / Light | Bars / Equipment | Runes | Loot (coins, items) |
| Failure | Burnt food (destroyed) | Failed light (retry) | Lost ore (iron only) | N/A (always succeeds) | Stun + damage |
| Success formula | Universal linear interp | Universal linear interp | Flat 50% (iron) or 100% | Always 100% | Universal linear interp |
| Level scaling | Burn rate decreases | Light chance increases | N/A (flat rates) | More runes per essence | Success rate increases |
| XP on failure | None | None | None | N/A | None |
| Equipment bonuses | Gauntlets, cape | Pyromancer outfit | Goldsmith gauntlets, uniform | Raiments, pouches | Dodgy, rogues, gloves |
| Minigame variant | None | Wintertodt | Blast Furnace | Guardians of the Rift | Rogues' Den |

A dungeon master configures each skill by:
1. Toggling subsystems on/off
2. Adjusting the universal success formula parameters per recipe/NPC
3. Setting XP rates, level requirements, and product outputs
4. Enabling or disabling equipment bonuses and minigame alternatives
5. Tuning tick rates for action speed
