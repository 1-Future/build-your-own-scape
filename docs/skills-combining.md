# Skills: Combining (Production / Artisan)

All combining skills share a universal pattern: **Inputs + Tool/Station = Product + XP**. The player gathers or purchases raw materials, uses them at a workstation or with a tool, and receives a finished product and experience. This document covers the five primary combining skills from OSRS and notes RS3-exclusive additions.

---

## Table of Contents

1. [Shared Combining Pattern](#shared-combining-pattern)
2. [Herblore](#herblore)
3. [Crafting](#crafting)
4. [Fletching](#fletching)
5. [Smithing](#smithing)
6. [Construction](#construction)
7. [RS3 Additions](#rs3-additions)
8. [Configurable Properties](#configurable-properties)
9. [Module Toggles](#module-toggles)

---

## Shared Combining Pattern

Every combining skill follows this core loop:

```
[Primary Input] + [Secondary Input(s)] + [Tool/Station] --> [Product] + [XP]
```

### Universal Properties

| Property | Description |
|----------|-------------|
| `skill_id` | Which skill governs this recipe |
| `level_required` | Minimum skill level to attempt |
| `xp_reward` | Experience granted on success |
| `inputs[]` | Array of `{ item_id, quantity }` consumed |
| `output` | `{ item_id, quantity }` produced |
| `tool` | Required tool (not consumed) |
| `station` | Required workstation (anvil, furnace, etc.) |
| `ticks` | Game ticks per craft action |
| `members_only` | Boolean flag |
| `quest_required` | Optional quest gate |
| `fail_rate` | Probability of failure (0.0 for most recipes) |
| `fail_output` | Item produced on failure (e.g., crushed gem) |

### Multi-Step Recipes

Some products require sequential crafting steps. Each step is an independent recipe whose output is another recipe's input.

```
Step 1: Clean herb + Vial of water --> Unfinished potion
Step 2: Unfinished potion + Secondary --> Finished potion
```

```
Step 1: Knife + Log --> Unstrung bow
Step 2: Unstrung bow + Bowstring --> Strung bow
```

```
Step 1: Furnace + Ore(s) --> Bar
Step 2: Hammer + Bar + Anvil --> Equipment
```

### Batch Crafting

Most combining actions support batch processing: the player selects a quantity (1, 5, 10, X, All) and the character repeats the action automatically until interrupted or materials run out. Each action consumes a fixed number of game ticks.

---

## Herblore

**Theme:** Alchemy and potion brewing. Cleaning raw herbs, then combining herbs with secondary ingredients in vials to produce potions with temporary stat effects.

**Unlock:** Requires completion of a starter quest (OSRS: Druidic Ritual).

### Tools and Stations

| Tool/Station | Consumed | Purpose |
|-------------|----------|---------|
| Vial of water | Yes | Base container for unfinished potions |
| Pestle and mortar | No | Crushing secondaries (e.g., unicorn horn, dragon scales) |
| Swamp tar | Yes | Tar recipe base (15 per batch) |

### Core Mechanics

#### 1. Herb Cleaning

Grimy (unidentified) herbs are cleaned by clicking them. Each herb type has a level requirement. Cleaning is fast (1 tick) and grants small XP.

| Herb | Level | Clean XP |
|------|-------|----------|
| Guam leaf | 3 | 2.5 |
| Marrentill | 5 | 3.8 |
| Tarromin | 11 | 5.0 |
| Harralander | 20 | 6.3 |
| Ranarr weed | 25 | 7.5 |
| Toadflax | 30 | 8.0 |
| Irit leaf | 40 | 8.8 |
| Avantoe | 48 | 10.0 |
| Kwuarm | 54 | 11.3 |
| Snapdragon | 59 | 11.8 |
| Cadantine | 65 | 12.5 |
| Lantadyme | 67 | 13.1 |
| Dwarf weed | 70 | 13.8 |
| Torstol | 75 | 15.0 |

**NPC service:** An NPC can clean herbs for a fee (200 coins each).

#### 2. Potion Making (Two-Step)

**Step 1 -- Unfinished Potion:** Clean herb + Vial of water = Unfinished potion (grants XP).

**Step 2 -- Finished Potion:** Unfinished potion + Secondary ingredient = Finished potion (grants XP). A finished potion contains 3 doses by default.

| Potion | Level | Herb | Secondary | XP (total) | Effect |
|--------|-------|------|-----------|------------|--------|
| Attack potion | 3 | Guam leaf | Eye of newt | 25 | +10% + 3 Attack |
| Antipoison | 5 | Marrentill | Unicorn horn dust | 37.5 | Cure poison, 90s immunity |
| Strength potion | 12 | Tarromin | Limpwurt root | 50 | +10% + 3 Strength |
| Stat restore potion | 22 | Harralander | Red spiders' eggs | 62.5 | Restores lowered stats |
| Energy potion | 26 | Harralander | Chocolate dust | 67.5 | Restores run energy |
| Defence potion | 30 | Ranarr weed | White berries | 75 | +10% + 3 Defence |
| Agility potion | 34 | Toadflax | Toad's legs | 80 | +3 Agility |
| Prayer potion | 38 | Ranarr weed | Snape grass | 87.5 | Restores prayer points |
| Super attack | 45 | Irit leaf | Eye of newt | 100 | +15% + 5 Attack |
| Super antipoison | 48 | Irit leaf | Kerp | 106.3 | Extended poison immunity |
| Fishing potion | 50 | Avantoe | Snape grass | 112.5 | +3 Fishing |
| Super energy | 52 | Avantoe | Mort myre fungus | 117.5 | Restores more run energy |
| Super strength | 55 | Kwuarm | Limpwurt root | 125 | +15% + 5 Strength |
| Super restore | 63 | Snapdragon | Red spiders' eggs | 142.5 | Restores all stats + prayer |
| Super defence | 66 | Cadantine | White berries | 150 | +15% + 5 Defence |
| Antifire potion | 69 | Lantadyme | Dragon scale dust | 157.5 | Partial dragonfire resist (~6 min) |
| Ranging potion | 72 | Dwarf weed | Wine of Zamorak | 162.5 | +10% + 4 Ranged |
| Magic potion | 76 | Lantadyme | Potato cactus | 172.5 | +4 Magic |
| Saradomin brew | 81 | Toadflax | Crushed nest | 180 | Heals HP, boosts Defence, lowers other stats |
| Super combat potion | 90 | Torstol | Super attack + Super strength + Super defence | 150 | Combines all three super potions |

#### 3. Dose System

Potions come in 1-dose, 2-dose, 3-dose, and 4-dose vials. Newly made potions are 3-dose. Each dose is one sip. An NPC decanting service converts potions between dose sizes for free.

#### 4. Barbarian Mixes

Adding roe or caviar (from fishing) to a 2-dose potion creates a "mix" that heals 3-6 HP per dose in addition to the potion's normal effect. Requires Barbarian Training.

#### 5. Divine Potions

Created by adding crystal dust to a normal potion. The divine version locks the stat boost for 5 minutes without degradation. Requires Song of the Elves quest.

#### 6. Extended Potions

Certain potions have extended variants that double the duration (e.g., Extended antifire lasts 12 minutes instead of 6).

#### 7. Combination Potions

High-level potions that merge multiple effects into one dose:
- Super combat potion (90): Super attack + Super strength + Super defence + Torstol
- Stamina potion (77): Super energy + Amylase crystal x4

#### 8. Tar Recipes

Herbs combined with swamp tar using a pestle and mortar create ammunition for salamander weapons.

| Tar | Level | Herb | XP |
|-----|-------|------|----|
| Guam tar | 19 | Guam leaf | 30 |
| Marrentill tar | 31 | Marrentill | 42.5 |
| Tarromin tar | 39 | Tarromin | 55 |
| Harralander tar | 44 | Harralander | 72.5 |
| Irit tar | 55 | Irit leaf | 85 |

#### 9. Special Mechanics

| Mechanic | Effect |
|----------|--------|
| Amulet of Chemistry | 5% chance to create a 4-dose potion instead of 3-dose |
| Prescription goggles | 10% chance to not consume the secondary ingredient |
| Herblore cape (99) | Grimy herbs can be used directly to make unfinished potions |
| Mastering Mixology | Minigame (level 60+) for slower but more herb-efficient training |

---

## Crafting

**Theme:** General artisan skill covering leather, gems, glass, pottery, jewelry, spinning, weaving, battlestaves, and more.

### Tools and Stations

| Tool/Station | Consumed | Purpose |
|-------------|----------|---------|
| Needle | No | Leather and hide armor |
| Thread | Yes (1 per 5 items) | Used with needle |
| Chisel | No | Gem cutting, amethyst tips, snelms |
| Glassblowing pipe | No | Glass items |
| Spinning wheel | Station | Wool, flax, sinew |
| Loom | Station | Weaving cloth, baskets |
| Potter's wheel | Station | Shaping pottery |
| Pottery oven | Station | Firing pottery |
| Furnace | Station | Jewelry, molten glass, tanning |
| Moulds | No | Ring, necklace, amulet, bracelet, tiara, holy symbol moulds |
| Hammer | No | Shield construction |
| Knife | No | Birdhouses |

### Core Mechanics

#### 1. Tanning (Preparation)

Raw hides must be tanned at a tanner NPC before use.

| Hide | Product | Cost |
|------|---------|------|
| Cowhide | Leather | 1 gp |
| Cowhide | Hard leather | 3 gp |
| Green dragonhide | Green dragon leather | 20 gp |
| Blue dragonhide | Blue dragon leather | 20 gp |
| Red dragonhide | Red dragon leather | 20 gp |
| Black dragonhide | Black dragon leather | 20 gp |
| Snakeskin | Tanned snakeskin | 15 gp |

#### 2. Leather Armor (Needle + Thread)

| Item | Level | Material | XP |
|------|-------|----------|-----|
| Leather gloves | 1 | 1 Leather | 13.8 |
| Leather boots | 7 | 1 Leather | 16.3 |
| Leather cowl | 9 | 1 Leather | 18.5 |
| Leather vambraces | 11 | 1 Leather | 22 |
| Leather body | 14 | 1 Leather | 25 |
| Leather chaps | 18 | 1 Leather | 27 |
| Hardleather body | 28 | 1 Hard leather | 35 |
| Coif | 38 | 1 Leather | 37 |
| Studded body | 41 | Leather body + Steel studs | 40 |
| Studded chaps | 44 | Leather chaps + Steel studs | 42 |

#### 3. Dragonhide Armor (Needle + Thread)

| Tier | Vamb Lvl | Chaps Lvl | Body Lvl | Vamb XP | Chaps XP | Body XP | Hides (V/C/B) |
|------|----------|-----------|----------|---------|----------|---------|---------------|
| Green | 57 | 60 | 63 | 62 | 124 | 186 | 1 / 2 / 3 |
| Blue | 66 | 68 | 71 | 70 | 140 | 210 | 1 / 2 / 3 |
| Red | 73 | 75 | 77 | 78 | 156 | 234 | 1 / 2 / 3 |
| Black | 79 | 82 | 84 | 86 | 172 | 258 | 1 / 2 / 3 |

#### 4. Gem Cutting (Chisel)

Semi-precious gems can fail (producing a crushed gem). Precious gems never fail.

| Gem | Level | XP | Can Fail |
|-----|-------|----|----------|
| Opal | 1 | 15 | Yes |
| Jade | 13 | 20 | Yes |
| Red topaz | 16 | 25 | Yes |
| Sapphire | 20 | 50 | No |
| Emerald | 27 | 67.5 | No |
| Ruby | 34 | 85 | No |
| Diamond | 43 | 107.5 | No |
| Dragonstone | 55 | 137.5 | No |
| Onyx | 67 | 167.5 | No |
| Zenyte | 89 | 200 | No |

#### 5. Gold Jewelry (Furnace + Gold Bar + Gem + Mould)

Each gem tier produces 4 jewelry types: ring, necklace, bracelet, amulet (unstrung).

| Gem | Ring Lvl/XP | Necklace Lvl/XP | Bracelet Lvl/XP | Amulet(u) Lvl/XP |
|-----|-------------|-----------------|-----------------|-------------------|
| None | 5 / 15 | 6 / 20 | 7 / 25 | 8 / 30 |
| Sapphire | 20 / 40 | 22 / 55 | 23 / 60 | 24 / 65 |
| Emerald | 27 / 55 | 29 / 60 | 30 / 65 | 31 / 70 |
| Ruby | 34 / 70 | 40 / 75 | 42 / 80 | 50 / 85 |
| Diamond | 43 / 85 | 56 / 90 | 58 / 95 | 70 / 100 |
| Dragonstone | 55 / 100 | 72 / 105 | 74 / 110 | 80 / 150 |
| Onyx | 67 / 115 | 82 / 120 | 84 / 125 | 90 / 165 |
| Zenyte | 89 / 150 | 92 / 165 | 95 / 180 | 98 / 200 |

#### 6. Silver Jewelry (Furnace + Silver Bar + Gem + Mould)

Silver jewelry uses semi-precious gems (opal, jade, topaz) and produces the same 4 types. Also includes holy symbols, unholy symbols, and tiaras.

| Item | Level | XP |
|------|-------|----|
| Unstrung holy symbol | 16 | 50 |
| Unstrung unholy symbol | 17 | 50 |
| Tiara | 23 | 52.5 |

#### 7. Glass Blowing (Glassblowing Pipe + Molten Glass)

Molten glass is made at a furnace from bucket of sand + soda ash (or via Superglass Make spell).

| Item | Level | XP |
|------|-------|----|
| Beer glass | 1 | 17.5 |
| Empty candle lantern | 4 | 19 |
| Empty oil lamp | 12 | 25 |
| Vial | 33 | 35 |
| Fishbowl | 42 | 42.5 |
| Unpowered orb | 46 | 52.5 |
| Lantern lens | 49 | 55 |
| Empty light orb | 87 | 70 |

#### 8. Pottery (Potter's Wheel then Pottery Oven)

Two-step: shape soft clay at wheel (XP), then fire in oven (more XP).

| Item | Level | Shape XP | Fire XP | Total XP |
|------|-------|----------|---------|----------|
| Pot | 1 | 6.3 | 6.3 | 12.6 |
| Empty cup | 3 | 8.5 | 8.5 | 17 |
| Pie dish | 7 | 15 | 10 | 25 |
| Bowl | 8 | 18 | 15 | 33 |
| Plant pot | 19 | 20 | 17.5 | 37.5 |
| Pot lid | 25 | 20 | 20 | 40 |

#### 9. Spinning (Spinning Wheel)

| Product | Level | Material | XP |
|---------|-------|----------|-----|
| Ball of wool | 1 | Wool | 2.5 |
| Bowstring | 10 | Flax | 15 |
| Crossbow string | 10 | Sinew / Roots | 15 |
| Magic string | 19 | Magic roots | 30 |
| Rope | 30 | Hair | 25 |

#### 10. Weaving (Loom)

| Product | Level | Materials | XP |
|---------|-------|-----------|-----|
| Strip of cloth | 10 | 4 Ball of wool | 12 |
| Empty sack | 21 | 4 Jute fibre | 38 |
| Drift net | 26 | 2 Jute fibre | 55 |
| Basket | 36 | 6 Willow branch | 56 |

#### 11. Battlestaves (Battlestaff + Charged Orb)

| Staff | Level | Orb | XP |
|-------|-------|-----|----|
| Water battlestaff | 54 | Water orb | 100 |
| Earth battlestaff | 58 | Earth orb | 112.5 |
| Fire battlestaff | 62 | Fire orb | 125 |
| Air battlestaff | 66 | Air orb | 137.5 |

#### 12. Amethyst (Chisel + Amethyst)

| Product | Level | XP |
|---------|-------|----|
| Amethyst bolt tips | 83 | 60 |
| Amethyst arrowtips | 85 | 60 |
| Amethyst javelin tips | 87 | 60 |
| Amethyst dart tips | 89 | 60 |

#### 13. Birdhouses (Logs + Clockwork + Chisel + Hammer)

Scales with log tier from level 5 (normal logs, 15 XP) to level 90 (redwood logs, 50 XP).

---

## Fletching

**Theme:** Creating ranged weapons and ammunition from wood, feathers, and metal tips.

### Tools and Stations

| Tool | Consumed | Purpose |
|------|----------|---------|
| Knife | No | Cutting logs into bows, stocks, shafts, shields |
| Chisel | No | Cutting gems into bolt tips |
| Hammer | No | Attaching crossbow limbs |

No workstation required -- all fletching is done from inventory.

### Core Mechanics

#### 1. Bow Making (Two-Step)

**Step 1:** Knife + Logs = Unstrung bow (shortbow or longbow)
**Step 2:** Unstrung bow + Bowstring = Strung bow

| Log | Short Lvl/XP | Long Lvl/XP |
|-----|-------------|-------------|
| Normal | 5 / 5 | 10 / 10 |
| Oak | 20 / 16.5 | 25 / 25 |
| Willow | 35 / 33.3 | 40 / 41.5 |
| Maple | 50 / 50 | 55 / 58.3 |
| Yew | 65 / 67.5 | 70 / 75 |
| Magic | 80 / 83.3 | 85 / 91.5 |

Stringing grants XP equal to the cutting step (approximately).

#### 2. Shield Making (Knife + 2 Logs)

| Shield | Level | XP |
|--------|-------|----|
| Oak shield | 27 | 50 |
| Willow shield | 42 | 83 |
| Maple shield | 57 | 116.5 |
| Yew shield | 72 | 150 |
| Magic shield | 87 | 183 |
| Redwood shield | 92 | 216 |

#### 3. Arrow Making (Three-Step)

```
Step 1: Knife + Logs --> Arrow shafts (15 per normal log, +15 per tier)
Step 2: Arrow shafts + Feathers --> Headless arrows (1 XP each)
Step 3: Headless arrows + Arrowheads --> Finished arrows
```

| Arrow Type | Level | XP per arrow |
|-----------|-------|-------------|
| Bronze | 1 | 1.3 |
| Iron | 15 | 2.5 |
| Steel | 30 | 5 |
| Mithril | 45 | 7.5 |
| Adamant | 60 | 10 |
| Rune | 75 | 12.5 |
| Amethyst | 82 | 13.5 |
| Dragon | 90 | 15 |

Special: Broad arrows (level 52, requires Slayer reward unlock).

#### 4. Bolt Making (Two-Step)

```
Step 1: Unfinished bolts (smithed) + Feathers --> Bolts
Step 2 (optional): Bolts + Gem bolt tips --> Tipped bolts
```

| Bolt Type | Level | XP per bolt |
|-----------|-------|-------------|
| Bronze | 9 | 0.5 |
| Blurite | 24 | 1 |
| Iron | 39 | 1.5 |
| Steel | 46 | 3.5 |
| Mithril | 54 | 5 |
| Adamant | 61 | 7 |
| Rune | 69 | 10 |
| Dragon | 84 | 12 |

Special: Broad bolts (level 55, requires Slayer reward unlock).

#### 5. Gem Bolt Tips (Chisel + Gem)

Cutting gems produces bolt tips (typically 12 per gem). Level ranges from 11 (opal) to 73 (onyx).

| Gem | Level | Tips per gem |
|-----|-------|-------------|
| Opal | 11 | 12 |
| Jade | 26 | 12 |
| Red topaz | 48 | 12 |
| Sapphire | 56 | 12 |
| Emerald | 58 | 12 |
| Ruby | 63 | 12 |
| Diamond | 65 | 12 |
| Dragonstone | 71 | 12 |
| Onyx | 73 | 24 |

#### 6. Dart Making (Dart Tip + Feather)

Requires completion of The Tourist Trap quest.

| Dart Type | Level | XP per dart |
|-----------|-------|-------------|
| Bronze | 10 | 1.8 |
| Iron | 22 | 3.8 |
| Steel | 37 | 7.5 |
| Mithril | 52 | 11.2 |
| Adamant | 67 | 15 |
| Rune | 81 | 18.8 |
| Amethyst | 90 | 21 |
| Dragon | 95 | 25 |

#### 7. Crossbow Making (Three-Step)

```
Step 1: Knife + Logs --> Crossbow stock
Step 2: Stock + Limbs (smithed) --> Unstrung crossbow (requires hammer)
Step 3: Unstrung crossbow + Crossbow string --> Crossbow
```

| Log | Stock Level | XP |
|-----|------------|-----|
| Normal | 9 | 6 |
| Oak | 19 | 16 |
| Willow | 39 | 22 |
| Teak | 46 | 27 |
| Maple | 54 | 32 |
| Mahogany | 61 | 41 |
| Yew | 72 | 50 |
| Magic | 80 | 70 |

#### 8. Javelin Making (Javelin Shaft + Javelin Head)

Javelin shafts are cut from logs. Javelin heads are smithed.

| Javelin Type | Level | XP |
|-------------|-------|----|
| Bronze | 3 | 1 |
| Iron | 17 | 2 |
| Steel | 32 | 5 |
| Mithril | 47 | 8 |
| Adamant | 62 | 10 |
| Rune | 77 | 12.4 |
| Amethyst | 84 | 13.5 |
| Dragon | 92 | 15 |

#### 9. Special Items

- **Toxic blowpipe** (78): Chisel on tanzanite fang, 120 XP
- **Ballistae** (47/72): Multi-component assembly (frame, limbs, spring, monkey tail)

---

## Smithing

**Theme:** Smelting ores into bars at a furnace, then hammering bars into equipment at an anvil.

### Tools and Stations

| Tool/Station | Consumed | Purpose |
|-------------|----------|---------|
| Hammer | No | Required for anvil smithing |
| Anvil | Station | Shaping bars into items |
| Furnace | Station | Smelting ores into bars |
| Moulds | No | Cannonball mould, etc. |

### Core Mechanics

#### 1. Smelting (Furnace)

| Bar | Level | Ores Required | XP | Notes |
|-----|-------|--------------|-----|-------|
| Bronze | 1 | 1 Copper + 1 Tin | 6.2 | Always succeeds |
| Iron | 15 | 1 Iron ore | 12.5 | 50% success rate normally |
| Silver | 20 | 1 Silver ore | 13.7 | Always succeeds |
| Steel | 30 | 1 Iron ore + 2 Coal | 17.5 | Always succeeds |
| Gold | 40 | 1 Gold ore | 22.5 | Always succeeds (56.2 with goldsmith gauntlets) |
| Mithril | 50 | 1 Mithril ore + 4 Coal | 30 | Always succeeds |
| Adamant | 70 | 1 Adamant ore + 6 Coal | 37.5 | Always succeeds |
| Rune | 85 | 1 Runite ore + 8 Coal | 50 | Always succeeds |

**Iron special case:** 50% chance to fail (ore consumed, no bar produced). Countered by: Ring of Forging (100% success for 140 uses), Superheat Item spell (100%), or Blast Furnace (100%).

**Superheat Item:** Magic spell (43 Magic) that smelts without a furnace, giving 100% iron success and granting both Smithing and Magic XP.

**Goldsmith gauntlets:** Increases gold bar smelting XP from 22.5 to 56.2.

#### 2. Blast Furnace (Minigame)

A bulk-smelting facility that requires only **half the normal coal**. Key features:
- Conveyor belt system: deposit ore, collect bars from dispenser
- Requires ice gloves or bucket of water to collect hot bars
- Official worlds have NPC operators (72,000 gp/hr fee)
- Coal bag lets players carry extra coal in one inventory slot
- XP rates: up to 370,900 XP/hr (gold bars with gauntlets)

| Bar | Normal Coal | Blast Furnace Coal |
|-----|------------|-------------------|
| Steel | 2 | 1 |
| Mithril | 4 | 2 |
| Adamant | 6 | 3 |
| Rune | 8 | 4 |

#### 3. Anvil Smithing

All anvil recipes follow the pattern: bars consumed at a rate of 5 ticks per action. The Smiths' Uniform (from Giants' Foundry) can reduce this to 4 ticks.

**Bars Required Per Item Type:**

| Bars | Item Types |
|------|-----------|
| 1 | Dagger, axe, mace, med helm, sword, arrowtips (x15), dart tips (x10), nails (x15), knife, wire, bolts (unf), javelin tips (x5), crossbow limbs |
| 2 | Scimitar, longsword, full helm, sq shield, claws, throwing knives |
| 3 | Warhammer, battleaxe, chainbody, kiteshield, 2h sword |
| 4 | Platelegs, plateskirt |
| 5 | Platebody |

**XP Per Bar:** XP scales by metal tier. The formula is approximately `12.5 * tier_multiplier * bars_used` where tier multiplier increases with metal tier.

| Metal | Base XP per bar | Level range |
|-------|----------------|-------------|
| Bronze | 12.5 | 1-18 |
| Iron | 25 | 15-33 |
| Steel | 37.5 | 30-48 |
| Mithril | 50 | 50-68 |
| Adamant | 62.5 | 70-88 |
| Rune | 75 | 85-99 |

**Level Progression Within a Tier:**

Each item type within a metal tier has its own level requirement. Pattern for each tier (using base level B):

| Offset | Items |
|--------|-------|
| B + 0 | Dagger |
| B + 1 | Axe, mace |
| B + 2 | Med helm |
| B + 3 | Sword, bolts, nails, dart tips |
| B + 4 | Scimitar, arrowtips, limbs |
| B + 5 | Longsword |
| B + 6 | Full helm, throwing knives |
| B + 7 | Sq shield, javelin tips |
| B + 8 | Warhammer |
| B + 9 | Battleaxe |
| B + 10 | Chainbody |
| B + 11 | Kiteshield |
| B + 12 | Claws |
| B + 13 | 2h sword |
| B + 14 | Platelegs, plateskirt |
| B + 16 | Platebody |

#### 4. Cannonballs

Requires Dwarf Cannon quest. 1 Steel bar = 4 cannonballs, 25.6 XP. Very slow (takes ~6 ticks per bar).

#### 5. Giants' Foundry (Minigame)

An alternative Smithing training method where players forge giant weapons for an NPC.

**Process:**
1. Fill crucible with bars or smithable equipment (up to 28 bars)
2. Select mould pieces (forte + blade + tip) to match commission requirements
3. Pour the alloy into the mould
4. Refine using 3 tools in temperature-dependent phases:
   - **Trip hammer** (hot range): +2% progress, cools metal
   - **Grindstone** (medium range): +1% progress, heats metal
   - **Polishing wheel** (cold range): +1% progress, cools metal
5. Manage temperature via lava pool (heat) and waterfall (cool)
6. Complete at 100% progress; quality determines XP and rewards

**Key features:**
- Metal score based on bar tier and alloy mixing (combining different metals = bonus)
- Mould score from matching commission descriptors
- Sweet spots: 6-second windows for bonus progress
- Wrong tool or wrong temperature damages quality
- Rewards: Smiths' Uniform (speed boost set), Colossal blade, moulds
- XP rates: 85,000-276,000/hr depending on materials

### Smithing Recipe Summary Table

| Metal | Dagger | Scimitar | Platebody | Level Start |
|-------|--------|----------|-----------|-------------|
| Bronze | 1 bar, 12.5 XP, Lv1 | 2 bars, 25 XP, Lv4 | 5 bars, 62.5 XP, Lv18 |1 |
| Iron | 1 bar, 25 XP, Lv15 | 2 bars, 50 XP, Lv19 | 5 bars, 125 XP, Lv33 | 15 |
| Steel | 1 bar, 37.5 XP, Lv30 | 2 bars, 75 XP, Lv34 | 5 bars, 187.5 XP, Lv48 | 30 |
| Mithril | 1 bar, 50 XP, Lv50 | 2 bars, 100 XP, Lv54 | 5 bars, 250 XP, Lv68 | 50 |
| Adamant | 1 bar, 62.5 XP, Lv70 | 2 bars, 125 XP, Lv74 | 5 bars, 312.5 XP, Lv88 | 70 |
| Rune | 1 bar, 75 XP, Lv85 | 2 bars, 150 XP, Lv89 | 5 bars, 375 XP, Lv99 | 85 |

---

## Construction

**Theme:** Building and furnishing a player-owned house (POH). Unique among combining skills because the products are placed in the world rather than carried in inventory.

### Tools and Stations

| Tool | Consumed | Purpose |
|------|----------|---------|
| Saw | No | Required for most furniture (Amy's Saw is wieldable) |
| Hammer | No | Required for most furniture (Imcando Hammer is wieldable) |
| Watering can | No | Required for planting in gardens |
| Hotspots | Station | Fixed build locations within rooms |

### Core Mechanics

#### 1. Player-Owned House System

- Purchase starter house for 1,000 coins
- Enter via house portal, teleport spell (40 Magic), or house tablet
- Toggle "Building mode" to see hotspots and build/remove furniture
- House has a grid layout; rooms snap to a grid
- Rooms can be moved and rotated
- Multiple floors (ground, upper, dungeon)

#### 2. Room System

Each room costs coins to add and has a level requirement.

| Room | Level | Cost | Key Hotspots |
|------|-------|------|-------------|
| Parlour | 1 | Free | Chairs, bookcase, fireplace |
| Garden | 1 | Free | Trees, plants, centrepiece |
| Bedroom | 1 | 25,000 | Beds (required for servants) |
| Kitchen | 5 | 30,000 | Stove, larder, shelves |
| Dining Room | 10 | 40,000 | Tables, benches, wall decorations |
| Workshop | 15 | 50,000 | Workbench, repair stand, tool store |
| Chapel | 45 | 50,000 | Altar, burners, icon, musical instrument |
| Portal Chamber | 50 | 100,000 | 3 portal hotspots, centrepiece |
| Superior Garden | 65 | 75,000 | Spirit tree, fairy ring, pool |
| Portal Nexus | 72 | 200,000 | Multi-destination portal |
| Achievement Gallery | 80 | 200,000 | Jewellery box, spellbook altar, boss jars |
| Dungeon | 70 | 50,000 | Monster spawns, traps |
| Menagerie | 37 | 50,000 | Pet storage |
| Costume Room | 42 | 50,000 | Armor, cape, treasure chest storage |
| Throne Room | 60 | 150,000 | Throne, lever, trapdoor |

#### 3. Hotspot and Furniture System

Each room contains fixed hotspot positions. Each hotspot accepts one of several furniture tiers. Higher-tier furniture requires higher Construction level and more expensive materials. Building furniture consumes materials and grants XP. Removing furniture returns nothing.

#### 4. Materials

| Material | Base XP Contribution | Source | Cost |
|----------|---------------------|--------|------|
| Plank (regular) | 29 XP per plank | Sawmill (100 gp from logs) | ~500 gp |
| Oak plank | 60 XP per plank | Sawmill (250 gp from oak logs) | ~400 gp |
| Teak plank | 90 XP per plank | Sawmill (500 gp from teak logs) | ~650 gp |
| Mahogany plank | 140 XP per plank | Sawmill (1,500 gp from mahogany logs) | ~1,400 gp |
| Nails | Varies | Smithed or purchased | cheap |
| Bolt of cloth | 15 XP | Purchased (650 gp) | 650 gp |
| Limestone brick | 20 XP | Crafted or purchased | ~25 gp |
| Steel bar | 20 XP | Smelted | ~400 gp |
| Gold leaf | 300 XP | Purchased (130,000 gp) | 130,000 gp |
| Marble block | 500 XP | Purchased (325,000 gp) | 325,000 gp |
| Magic stone | 1,000 XP | Purchased (975,000 gp) | 975,000 gp |

#### 5. Plank Making

Logs are converted to planks at the Sawmill NPC northeast of Varrock.

| Log Type | Plank Type | Sawmill Cost |
|----------|-----------|-------------|
| Logs | Plank | 100 gp |
| Oak logs | Oak plank | 250 gp |
| Teak logs | Teak plank | 500 gp |
| Mahogany logs | Mahogany plank | 1,500 gp |

The Plank Make spell (Lunar spellbook, 86 Magic) converts logs to planks from inventory.

#### 6. Representative Furniture XP

| Furniture | Level | Materials | XP |
|-----------|-------|-----------|-----|
| Crude wooden chair | 1 | 2 planks, 2 nails | 58 |
| Wooden bookcase | 4 | 4 planks, 4 nails | 115 |
| Oak chair | 19 | 2 oak planks | 120 |
| Oak dining table | 22 | 4 oak planks | 240 |
| Oak larder | 33 | 8 oak planks | 480 |
| Teak table | 38 | 4 teak planks | 360 |
| Mahogany table | 52 | 6 mahogany planks | 840 |
| Gilded altar | 75 | 4 marble blocks, 2 gold leaves | 2,230 |
| Ornate rejuvenation pool | 90 | Varies (high-end materials) | High |
| Occult altar | 90 | Limestone, marble, magic stone | High |

#### 7. Butler / Servant System

Servants fetch items from the bank or sawmill, allowing continuous building without leaving the house.

| Servant | Level | Wage | Trip Time | Capacity |
|---------|-------|------|-----------|----------|
| Rick | 20 | 500 gp | 60s | 6 items |
| Maid | 25 | 1,000 gp | 30s | 10 items |
| Cook | 30 | 3,000 gp | 17s | 16 items |
| Butler | 40 | 5,000 gp | 12s | 20 items |
| Demon Butler | 50 | 10,000 gp | 7s | 26 items |

Requirements: 2 bedrooms with beds. Payment every 8 trips.

#### 8. POH Locations

Players can move their house to different cities as they level up.

| Location | Level | Move Cost |
|----------|-------|-----------|
| Rimmington | 1 | 5,000 |
| Taverley | 10 | 5,000 |
| Pollnivneach | 20 | 7,500 |
| Hosidius | 25 | 8,750 |
| Rellekka | 30 | 10,000 |
| Brimhaven | 40 | 15,000 |
| Yanille | 50 | 25,000 |
| Prifddinas | 70 | 50,000 |

#### 9. Key Utility Furniture

These are the most impactful unlocks in the POH:

| Furniture | Room | Level | Effect |
|-----------|------|-------|--------|
| Gilded altar | Chapel | 75 | 350% Prayer XP when lit with burners |
| Ornate rejuvenation pool | Superior Garden | 90 | Full HP, prayer, run, spec, cure poison/venom |
| Fairy ring | Superior Garden | 85 | Teleport network access |
| Spirit tree | Superior Garden | 75 | Spirit tree teleport network |
| Ornate jewellery box | Achievement Gallery | 91 | All jewelry teleports in one object |
| Portal nexus | Portal Nexus Room | 72+ | Multiple city teleports in one portal |
| Occult altar | Achievement Gallery | 90 | Switch between all 4 spellbooks |

#### 10. Mahogany Homes (Minigame)

An alternative training method. Players receive contracts to build furniture for NPCs across the world.
- 4 tiers: Beginner, Novice, Adept, Expert
- Uses planks (type depends on tier)
- Rewards Mahogany Homes points
- Points buy Carpenter's Outfit (2.5% XP boost when complete), Amy's Saw (wieldable)
- Less XP/hr than traditional POH training but cheaper

#### 11. Construction Boosts

| Boost | Source | Amount |
|-------|--------|--------|
| Crystal saw | Quest reward | +3 (invisible, saw items only) |
| Tea (trimmed) | POH kitchen | +3 |
| Spicy stew (orange) | Random | +/-5 |
| Carpenter's outfit | Mahogany Homes | +2.5% XP |
| Construction cape | Level 99 | +1 |

---

## RS3 Additions

RS3 has reworked several combining skills and added new ones. These notes are based on general knowledge of RS3 mechanics.

### Invention (Elite Skill, Level 80 Crafting + Divination + Smithing)

RS3's most unique combining skill. Core mechanics:

| Mechanic | Description |
|----------|-------------|
| Discovery | Research blueprints at an invention workbench to unlock new devices and perks |
| Augmentation | Attach an augmentor to weapons/armor, enabling perk slots |
| Perks | Created by combining materials in a gizmo shell. Materials are obtained by disassembling items |
| Disassembly | Break down any item into component materials (e.g., simple parts, precise components, subtle components) |
| Device creation | Build machines, tools, and utility devices (auto-alchers, spring cleaners, etc.) |
| Item leveling | Augmented equipment gains item XP through combat/skilling, which can be siphoned for Invention XP or disassembled for more XP |
| Tech trees | Human and cave goblin tech trees unlock specialized devices |
| Ancient Invention | Unlocked via archaeology, adds powerful new perks |

**Material Categories:**
- Common: Simple, base, flexible, swift, dextrous, direct, subtle, precise, strong, smooth, heavy, protective, sharp, crafted, organic, delicate, head, light, living, tensile, clear, padded, plated, deflecting, healthy, pious, stave, cover, spiritual, crystal, metallic, spiked, knightly, undead, fungal, dragonfire, pestiferous, culinary, oceanic, magic, ethereal, enhancing, powerful, variable
- Rare: Vintage, stunning, fortunate, faceted, harnessed, saradomin, zamorak, guthix, armadyl, bandos, ancient, explosive, seren, zaros, third-age, shadow, timeworn, classic, historic, ascended

**Perk System:**
- Gizmo shells: weapon gizmo, armor gizmo, tool gizmo
- Each gizmo has 5 material slots
- Materials determine which perks can appear and their rank
- Examples: Precise, Equilibrium, Biting, Crackling, Aftershock, Planted Feet, Devoted, Enhanced Devoted

### Divination Crafting

Divination is a gathering skill, but its products feed into combining:

| Product | Description |
|---------|-------------|
| Signs | Consumed automatically on trigger (e.g., Sign of Life auto-revives on death) |
| Portents | Triggered automatically (e.g., Portent of Restoration auto-heals below threshold) |
| Divine locations | Temporary gathering nodes placed in the world for group use |
| Transmutation | Convert lower-tier resources into higher-tier ones (e.g., iron ore to coal) |
| Charges | Divine charges power augmented equipment (made from energy + simple parts) |

**Energy Types by Tier:** Pale, flickering, bright, glowing, sparkling, gleaming, vibrant, lustrous, brilliant, radiant, luminous, incandescent, elder

### RS3 Smithing Rework

RS3 completely reworked Smithing in 2019:

| Change | Description |
|--------|-------------|
| Heat mechanic | Items start hot and cool over time; reheating at forge restores speed |
| Progress bar | Each item requires multiple strikes to complete (not instant) |
| Burial items | Masterwork-tier items can be "buried" at an anvil for bonus XP (item destroyed) |
| Masterwork armor | T90 power armor crafted through a multi-step process requiring hundreds of bars |
| Elder rune | New top-tier smithable metal (level 90) |
| +1 to +5 upgrades | Smithed items can be upgraded through additional bars and smithing |
| Ore box | Holds multiple ore types while mining, feeds into smelting |

### RS3 Crafting Additions

| Addition | Description |
|----------|-------------|
| Urns | Clay urns that auto-collect XP during skilling (e.g., Mining urn, Fishing urn) |
| Attuned crystal equipment | High-tier crystal armor and weapons from crystal-flecked sandstone |
| Mechanised chinchompas | High-tier thrown explosive from chinchompa + parts |

### RS3 Herblore Additions

| Addition | Description |
|----------|-------------|
| Combination potions | Holy overload, Supreme overload salve (6+ potions combined) |
| Overloads (96) | Extreme attack + strength + defence + ranging + magic in one potion |
| Extreme potions | Tier above super potions (e.g., Extreme attack at 88) |
| Adrenaline potions | Restore adrenaline (special attack energy equivalent) |
| Bomb vials | AoE thrown potions |
| Primal extract | Secondary ingredient replacement |

---

## Configurable Properties

These properties should be exposed in the game engine for world builders to customize combining skills.

### Global Configuration

| Property | Type | Description |
|----------|------|-------------|
| `tick_rate_ms` | number | Milliseconds per game tick (default 600) |
| `batch_sizes` | number[] | Available batch quantities (default [1, 5, 10, 28]) |
| `xp_multiplier` | float | Global XP scaling factor |
| `fail_enabled` | boolean | Whether recipes can fail |
| `level_cap` | number | Maximum skill level (default 99, can set to 120 for RS3-style) |
| `xp_curve` | string | XP-to-level formula identifier |
| `members_gating` | boolean | Whether to enforce members-only flags |

### Per-Skill Configuration

| Property | Type | Description |
|----------|------|-------------|
| `enabled` | boolean | Whether this skill exists in the game |
| `unlock_quest` | string | Quest required before training (null for none) |
| `recipes[]` | Recipe[] | Full recipe list (see recipe schema below) |
| `tools[]` | Tool[] | Tools required for this skill |
| `stations[]` | Station[] | Workstations required |
| `boosts[]` | Boost[] | Items/effects that modify XP or success rate |

### Recipe Schema

```json
{
  "recipe_id": "string",
  "skill_id": "string",
  "level_required": "number",
  "xp_reward": "number",
  "inputs": [
    { "item_id": "string", "quantity": "number" }
  ],
  "output": { "item_id": "string", "quantity": "number" },
  "tool_required": "string | null",
  "station_required": "string | null",
  "ticks_per_action": "number",
  "members_only": "boolean",
  "quest_required": "string | null",
  "fail_rate": "number (0.0 - 1.0)",
  "fail_output": "{ item_id, quantity } | null",
  "category": "string",
  "subcategory": "string",
  "steps": "number (1 = single step, 2+ = multi-step)",
  "next_recipe_id": "string | null (for multi-step chains)"
}
```

### Multi-Step Recipe Chain Schema

```json
{
  "chain_id": "string",
  "name": "string",
  "steps": [
    {
      "step_number": 1,
      "recipe_id": "string",
      "description": "Clean herb + vial of water"
    },
    {
      "step_number": 2,
      "recipe_id": "string",
      "description": "Unfinished potion + secondary ingredient"
    }
  ]
}
```

### Boost Schema

```json
{
  "boost_id": "string",
  "name": "string",
  "type": "xp_bonus | success_rate | speed | resource_saving | output_bonus",
  "value": "number",
  "source": "item | equipment | set_bonus | spell | cape",
  "skill_id": "string",
  "description": "string"
}
```

---

## Module Toggles

World builders can enable or disable entire subsystems. Each toggle is independent.

### Skill-Level Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `herblore.enabled` | true | Entire Herblore skill |
| `crafting.enabled` | true | Entire Crafting skill |
| `fletching.enabled` | true | Entire Fletching skill |
| `smithing.enabled` | true | Entire Smithing skill |
| `construction.enabled` | true | Entire Construction skill |
| `invention.enabled` | false | RS3-style Invention (elite skill) |
| `divination_crafting.enabled` | false | RS3-style sign/portent/transmutation crafting |

### Subsystem Toggles (Herblore)

| Toggle | Default | Description |
|--------|---------|-------------|
| `herblore.herb_cleaning` | true | Grimy-to-clean herb step |
| `herblore.unfinished_potions` | true | Two-step potion making |
| `herblore.barbarian_mixes` | true | Barbarian Training mix potions |
| `herblore.divine_potions` | false | RS3/OSRS divine potion system |
| `herblore.combination_potions` | true | Multi-potion combinations (super combat, etc.) |
| `herblore.tar_recipes` | true | Salamander ammunition |
| `herblore.dose_system` | true | 1-4 dose vials and decanting |
| `herblore.overloads` | false | RS3-style overload potions |
| `herblore.extreme_potions` | false | RS3-style extreme tier |
| `herblore.npc_cleaning` | true | NPC herb cleaning service |
| `herblore.mastering_mixology` | false | Minigame training alternative |

### Subsystem Toggles (Crafting)

| Toggle | Default | Description |
|--------|---------|-------------|
| `crafting.leather` | true | Leather and studded armor |
| `crafting.dragonhide` | true | Dragonhide armor tiers |
| `crafting.gem_cutting` | true | Uncut to cut gems |
| `crafting.gem_fail` | true | Semi-precious gems can fail |
| `crafting.gold_jewelry` | true | Rings, necklaces, bracelets, amulets |
| `crafting.silver_jewelry` | true | Silver + semi-precious gems |
| `crafting.glass_blowing` | true | Molten glass items |
| `crafting.pottery` | true | Soft clay shaping and firing |
| `crafting.spinning` | true | Spinning wheel products |
| `crafting.weaving` | true | Loom products |
| `crafting.battlestaves` | true | Battlestaff + orb attachment |
| `crafting.tanning` | true | Hide tanning at NPC |
| `crafting.amethyst` | true | Amethyst tip cutting |
| `crafting.birdhouses` | true | Birdhouse crafting |
| `crafting.urns` | false | RS3-style skilling urns |

### Subsystem Toggles (Fletching)

| Toggle | Default | Description |
|--------|---------|-------------|
| `fletching.bows` | true | Shortbow and longbow making |
| `fletching.shields` | true | Wooden shield making |
| `fletching.arrows` | true | Arrow shaft + head + feather |
| `fletching.bolts` | true | Bolt + feather + optional gem tip |
| `fletching.darts` | true | Dart tip + feather |
| `fletching.crossbows` | true | Stock + limbs + string |
| `fletching.javelins` | true | Javelin shaft + head |
| `fletching.broad_ammo` | true | Broad arrows/bolts (Slayer gated) |
| `fletching.special_weapons` | true | Toxic blowpipe, ballistae |

### Subsystem Toggles (Smithing)

| Toggle | Default | Description |
|--------|---------|-------------|
| `smithing.smelting` | true | Ore to bar at furnace |
| `smithing.anvil` | true | Bar to equipment at anvil |
| `smithing.cannonballs` | true | Steel bar to cannonballs |
| `smithing.blast_furnace` | true | Half-coal smelting minigame |
| `smithing.giants_foundry` | true | Giant weapon forging minigame |
| `smithing.iron_fail_rate` | true | 50% iron smelting failure |
| `smithing.superheat` | true | Magic-based smelting spell |
| `smithing.rs3_rework` | false | RS3-style progress bar + heat system |
| `smithing.masterwork` | false | RS3-style masterwork armor chain |

### Subsystem Toggles (Construction)

| Toggle | Default | Description |
|--------|---------|-------------|
| `construction.poh` | true | Player-owned house system |
| `construction.rooms` | true | Room building on grid |
| `construction.furniture` | true | Hotspot-based furniture |
| `construction.servants` | true | Butler/servant banking system |
| `construction.mahogany_homes` | true | Contract-based minigame |
| `construction.plank_make_spell` | true | Lunar spell for plank conversion |
| `construction.utility_furniture` | true | Pools, portals, altars, fairy rings |
| `construction.poh_locations` | true | Multiple house location options |
| `construction.stash_units` | true | Clue scroll storage units |

### Subsystem Toggles (RS3 Invention)

| Toggle | Default | Description |
|--------|---------|-------------|
| `invention.augmentation` | false | Augmenting equipment with perks |
| `invention.disassembly` | false | Breaking items into components |
| `invention.perks` | false | Gizmo + material perk system |
| `invention.discovery` | false | Blueprint research mechanic |
| `invention.devices` | false | Utility device creation |
| `invention.ancient_invention` | false | Archaeology-gated advanced perks |
| `invention.tech_trees` | false | Human and goblin tech tree progression |
