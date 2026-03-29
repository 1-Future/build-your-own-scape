# Gathering Skills Design Document

This document covers the five core gathering skills (Mining, Fishing, Woodcutting, Farming, Hunter) plus two RS3-era additions (Divination, Archaeology). It defines the shared mechanical pattern, per-skill unique systems, configurable properties, formulas, and module toggles a world builder needs to configure these skills.

---

## Table of Contents

1. [Shared Gathering Pattern](#shared-gathering-pattern)
2. [Universal Success Formula](#universal-success-formula)
3. [Mining](#mining)
4. [Fishing](#fishing)
5. [Woodcutting](#woodcutting)
6. [Farming](#farming)
7. [Hunter](#hunter)
8. [RS3 Additions: Divination](#divination)
9. [RS3 Additions: Archaeology](#archaeology)
10. [Module Toggles](#module-toggles)

---

## Shared Gathering Pattern

All gathering skills follow the same core loop:

```
Player + Tool + Node --> Action Tick --> Success Roll --> Product + XP
```

### Universal Components

| Component | Description |
|-----------|-------------|
| **Skill Level** | Determines which nodes the player can interact with and affects success rate |
| **Tool** | Equipped or inventoried item that determines roll frequency (ticks between attempts) |
| **Node** | The world object being gathered from (rock, tree, fishing spot, patch, trap) |
| **Product** | The resource item received on success (ore, log, fish, crop, creature) |
| **Depletion** | Whether the node disappears or degrades after harvesting |
| **Respawn** | Time before a depleted node becomes available again |
| **Competition** | Whether multiple players can gather from the same node simultaneously |
| **Roll Interval** | Number of game ticks between success checks (determined by tool tier) |
| **XP Outfit** | Cosmetic set granting +2.5% XP when full set worn (0.4% hat, 0.8% top, 0.6% legs, 0.2% boots, +0.5% set bonus) |
| **Guild Bonus** | Invisible level boost (+7 typically) when gathering inside the skill's guild |
| **Secondary Drops** | Bonus rolls on secondary loot tables (gems, nests, seeds) |

### Configurable Global Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `game_tick_ms` | int | 600 | Length of one game tick in milliseconds |
| `xp_outfit_bonus` | float | 0.025 | Total XP bonus for full gathering outfit |
| `guild_invisible_boost` | int | 7 | Invisible level boost inside skill guilds |
| `guild_entry_level` | int | 60 | Minimum level to enter a skill guild |
| `enable_tick_manipulation` | bool | true | Whether advanced tick manipulation techniques work |
| `enable_secondary_drops` | bool | true | Whether gathering produces secondary loot |

---

## Universal Success Formula

All gathering skills use the same linear interpolation formula for success chance per roll:

```
chance = (1 + floor(low * (99 - level) / 98 + high * (level - 1) / 98 + 0.5)) / 256
```

Where:

| Variable | Description |
|----------|-------------|
| `level` | Player's effective skill level (base + visible boosts + invisible boosts) |
| `low` | Success numerator at level 1 (unique per node type) |
| `high` | Success numerator at level 99 (unique per node type) |

The result is clamped to [0, 1]. Higher `low`/`high` values mean easier gathering. Each node type defines its own `low` and `high` pair.

### Per-Node Configuration

Every gatherable node should define:

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier |
| `skill` | enum | Which gathering skill this belongs to |
| `level_required` | int | Minimum level to attempt |
| `xp_reward` | float | XP granted per successful gather |
| `success_low` | int | Success numerator at level 1 (0-255) |
| `success_high` | int | Success numerator at level 99 (0-255) |
| `depletion_chance` | float | Chance node depletes per successful gather (0.0-1.0) |
| `respawn_ticks` | int | Ticks until node respawns after depletion |
| `product_id` | string | Item produced on success |
| `members_only` | bool | Whether this node requires membership |

### Per-Tool Configuration

Every gathering tool should define:

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier |
| `skill` | enum | Which gathering skill this tool serves |
| `level_required` | int | Minimum skill level to use |
| `roll_interval_ticks` | int | Ticks between success rolls |
| `speed_bonus` | object | Optional: chance to reduce interval (e.g., `{chance: 1/6, reduced_ticks: 2}`) |
| `special_effect` | enum | Optional: `burn_product`, `extra_resource`, `none` |
| `special_chance` | float | Probability of special effect triggering |

---

## Mining

### Overview

Mining extracts ores and gems from rocks using pickaxes. Rocks deplete after a single successful mine and respawn on a timer. Multiple players compete for the same rock -- first to succeed claims the ore.

### Rock Types (Reference Data)

| Rock | Level | XP | Respawn (approx) | Members |
|------|-------|----|-------------------|---------|
| Rune/Pure Essence | 1 | 5 | Infinite (no depletion) | No |
| Clay | 1 | 5 | 1 second | No |
| Copper | 1 | 17.5 | 4 seconds | No |
| Tin | 1 | 17.5 | 4 seconds | No |
| Iron | 15 | 35 | 5 seconds | No |
| Silver | 20 | 40 | 60 seconds | No |
| Coal | 30 | 50 | 30 seconds | No |
| Sandstone | 35 | 30-60 | No depletion | Yes |
| Gold | 40 | 65 | 60 seconds | No |
| Gem Rock | 40 | 65 | Instant | Yes |
| Mithril | 55 | 80 | 120 seconds | No |
| Adamantite | 70 | 95 | 240 seconds | No |
| Runite | 85 | 125 | 720 seconds (12 min) | No |
| Amethyst | 92 | 240 | No depletion | Yes |

### Pickaxe Tiers

| Pickaxe | Skill Level | Roll Interval (ticks) | Speed Bonus |
|---------|-------------|-----------------------|-------------|
| Bronze | 1 | 8 | -- |
| Iron | 1 | 7 | -- |
| Steel | 6 | 6 | -- |
| Black | 11 | 5 | -- |
| Mithril | 21 | 5 | -- |
| Adamant | 31 | 4 | -- |
| Rune | 41 | 3 | -- |
| Dragon | 61 | 3 | 1/6 chance of 2-tick roll |
| Infernal | 61 | 3 | 1/6 chance of 2-tick roll |
| Crystal | 71 | 3 | 1/4 chance of 2-tick roll |

### Unique Mechanics

#### Gem Drop Table

Every mining attempt has a secondary roll on the gem table, independent of ore success:

| Condition | Gem Table Roll Chance |
|-----------|-----------------------|
| Base | 1/256 per attempt |
| With charged Glory amulet | 1/86 per attempt |

When the gem table is hit, the gem type is rolled:

| Gem | Weight (out of 128) |
|-----|---------------------|
| Nothing | 70 |
| Uncut Sapphire | 32 |
| Uncut Emerald | 16 |
| Uncut Ruby | 8 |
| Uncut Diamond | 2 |

A successful gem roll does NOT grant ore or XP for that attempt and does NOT deplete the rock.

#### Rock Competition

Rocks are single-claim: when one player succeeds, the rock depletes for everyone. Players racing the same rock creates natural competition. Higher-tier rocks (runite, adamantite) incentivize world-hopping over waiting.

#### Depletion-Free Nodes

Some node types never deplete (essence, sandstone, amethyst). These are configured with `depletion_chance: 0.0`.

### Equipment Bonuses

| Item | Effect |
|------|--------|
| Celestial ring (charged) | +4 invisible Mining boost (does not unlock higher nodes) + 10% chance for extra ore (up to adamantite) |
| Varrock armour 1-4 | 10% chance to mine 2 ores at once. Tier determines max ore affected (1=gold, 2=mithril, 3=adamantite, 4=runite) |
| Expert mining gloves | Prevents rock depletion 1-3 times (varies by glove tier and rock type) |
| Mining cape (99) | 5% chance for extra ore (up to adamantite) |
| Prospector outfit | +2.5% XP (standard gathering outfit) |

### Minigame Modules

#### Motherlode Mine

A separate mining area where players mine pay-dirt from generic veins instead of specific ores.

| Property | Value |
|----------|-------|
| Level Required | 30 |
| Product | Pay-dirt (converted to random ores at a washing machine) |
| Ore Output | Random ore based on current Mining level |
| Currency | Gold nuggets (exchanged for rewards) |
| Upper Level | Level 72 requirement, fewer vein spots but no competition |
| Rewards | Prospector outfit, coal bag, gem bag, soft clay packs |

Configurable: ore distribution table per level bracket, nugget drop rate, sack capacity.

#### Shooting Stars

A world event where a meteorite crashes every ~90 minutes per server. Players mine stardust and exchange it for rewards.

| Property | Value |
|----------|-------|
| Frequency | ~90 minutes per server |
| Star Tiers | 1-9 (higher tier = more stardust, higher level needed) |
| Currency | Stardust |
| Reward Shop | Extra ore chance buff, gem bag, soft clay packs |

Configurable: spawn frequency, tier distribution, reward shop contents.

#### Volcanic Mine

A group mining activity in a volcanic chamber.

| Property | Value |
|----------|-------|
| Level Required | 50 |
| Group Size | 1-50 (optimal 3-5) |
| Mechanic | Mine ore fragments while managing mine stability |
| XP Rate | High (best mining XP/hr in game) |
| Hazards | Lava beasts, collapsing stability |

Configurable: stability drain rate, fragment types, team size scaling.

### Mining Configuration Schema

```
mining:
  enabled: true
  gem_drop_enabled: true
  gem_base_chance: 1/256
  gem_glory_chance: 1/86
  guild_boost: 7
  guild_entry_level: 60
  guild_extra_respawn_speed: 2.0  # multiplier for respawn in guild
  competition_model: "first_success"  # or "individual_rolls"
  motherlode_mine:
    enabled: true
    entry_level: 30
    upper_level_requirement: 72
  shooting_stars:
    enabled: true
    spawn_interval_minutes: 90
  volcanic_mine:
    enabled: true
    entry_level: 50
```

---

## Fishing

### Overview

Fishing captures fish from fishing spots using various tools. Unlike mining, fishing spots do not deplete from player action but periodically move to nearby locations. Multiple players can fish the same spot simultaneously without competition.

### Fish Types (Reference Data)

| Fish | Level | XP | Method | Members |
|------|-------|----|--------|---------|
| Raw shrimps | 1 | 10 | Small net | No |
| Raw sardine | 5 | 20 | Bait | No |
| Raw karambwanji | 5 | 5 | Small net | Yes |
| Raw herring | 10 | 30 | Bait | No |
| Raw anchovies | 15 | 40 | Small net | No |
| Raw mackerel | 16 | 20 | Big net | Yes |
| Raw trout | 20 | 50 | Lure (fly) | No |
| Raw salmon | 30 | 70 | Lure (fly) | No |
| Raw tuna | 35 | 80 | Harpoon | No |
| Raw lobster | 40 | 90 | Cage | No |
| Raw swordfish | 50 | 100 | Harpoon | No |
| Raw monkfish | 62 | 120 | Small net | Yes |
| Raw shark | 76 | 110 | Harpoon | Yes |
| Raw anglerfish | 82 | 120 | Bait (sandworms) | Yes |
| Raw dark crab | 85 | 130 | Bait (dark bait) | Yes |
| Raw manta ray | 81 | 46 | Drift net / Trawler | Yes |
| Raw sea turtle | 79 | 38 | Drift net / Trawler | Yes |

### Fishing Tools

| Tool | Level | Method | Speed Bonus |
|------|-------|--------|-------------|
| Small fishing net | 1 | Net fishing | -- |
| Fishing rod | 5 | Bait fishing | -- |
| Fly fishing rod | 20 | Lure fishing | -- |
| Lobster pot | 40 | Cage fishing | -- |
| Harpoon | 35 | Harpoon fishing | -- |
| Big fishing net | 16 | Big net fishing | -- |
| Barbarian rod | 48 | Heavy rod fishing | -- |
| Dragon harpoon | 61 | Harpoon fishing | +20% catch speed, +3 level special attack |
| Infernal harpoon | 75 | Harpoon fishing | +20% catch speed, 1/3 fish cooked for FM XP |
| Crystal harpoon | 71 | Harpoon fishing | +35% catch speed, +3 level special attack |
| Cormorant's glove | 43 Fish / 35 Hunt | Aerial fishing | -- |

### Unique Mechanics

#### Fishing Spot Movement

Fishing spots are not static world objects. They periodically relocate within a defined area. This creates a natural pacing mechanism -- players must move to follow spots.

Configurable properties:
- `spot_move_chance_per_tick`: probability of spot moving each game tick
- `spot_move_radius`: maximum tiles a spot can relocate
- `static_spots`: list of fishing spot IDs that never move

Known static spots in OSRS: karambwan spots, certain quest-locked spots, Prifddinas fly fishing.

#### Shared Spots (No Competition)

Unlike mining, multiple players fish the same spot independently. Each player rolls separately. Spot movement affects all players at once.

#### Barehand Fishing

An advanced technique requiring both Fishing and Strength levels (20 levels higher than normal tool requirement). No tool needed. Grants both Fishing and Strength XP.

| Fish | Fishing Level | Strength Level |
|------|---------------|----------------|
| Tuna | 55 | 35 |
| Swordfish | 70 | 50 |
| Shark | 96 | 76 |

#### Tick Manipulation

Advanced technique where players use secondary actions (herb + tar, knife + log) to reset the gathering animation, achieving faster rolls:

| Method | Ticks Per Roll | Use Case |
|--------|---------------|----------|
| Normal fishing | 5 | Default |
| 3-tick fishing | 3 | Barbarian fishing, fly fishing |
| 2-tick swordfish | 2 | Harpoon spots with specific setups |

Configurable: `enable_tick_manipulation: true/false`

### Equipment Bonuses

| Item | Effect |
|------|--------|
| Angler outfit | +2.5% Fishing XP (standard gathering outfit) |
| Rada's blessing 1-4 | 2%/4%/6%/8% chance to catch an extra fish (no extra XP) |
| Spirit flakes | 50% chance to double catch (no extra XP), costs 1 flake per fish |
| Fish barrel | Stores up to 28 extra fish (extends trips) |

### Minigame Modules

#### Tempoross

A skilling boss fought with Fishing instead of combat.

| Property | Value |
|----------|-------|
| Level Required | 35 Fishing |
| Group Size | 1-40 |
| Mechanic | Fish harpoonfish, load cannons, manage hazards |
| Damage | Raw fish = 10 damage, Cooked fish = 15 damage |
| Reward Currency | Fishing permits (1 per 2,000 points, +1 per 700 after) |
| Unique Rewards | Dragon harpoon (1/8,000), Tome of water (1/1,600), Fish barrel (1/400), Spirit flakes, Tiny tempor pet (1/8,000) |
| XP Rate | ~65,000-95,000 Fishing XP/hr depending on level and method |

#### Drift Net Fishing

A hybrid Fishing + Hunter activity.

| Property | Value |
|----------|-------|
| Requirements | 47 Fishing, 44 Hunter |
| Mechanic | Set drift nets, chase fish shoals into them |
| Products | Assorted fish (tuna through manta ray based on level) |
| XP | Grants both Fishing and Hunter XP |

#### Aerial Fishing

A hybrid Fishing + Hunter activity at a specific lake.

| Property | Value |
|----------|-------|
| Requirements | 43 Fishing, 35 Hunter |
| Tool | Cormorant's glove + King worm bait |
| Products | Bluegill, Common tench, Moulten eel, Greater siren |
| XP | Grants both Fishing and Hunter XP |

#### Fishing Trawler

A team-based boat minigame.

| Property | Value |
|----------|-------|
| Level Required | 15 Fishing (to receive fish) |
| Duration | 5 minutes per trip |
| Mechanic | Repair boat leaks, bail water cooperatively |
| Rewards | Fish based on Fishing level, 1/12 chance for Angler outfit piece |

### Fishing Configuration Schema

```
fishing:
  enabled: true
  spot_movement:
    enabled: true
    move_chance_per_tick: 0.001
    move_radius_tiles: 5
  competition_model: "no_competition"
  barehand_fishing:
    enabled: true
    level_offset: 20
  tick_manipulation:
    enabled: true
  tempoross:
    enabled: true
    entry_level: 35
    max_group_size: 40
  drift_net:
    enabled: true
    fishing_level: 47
    hunter_level: 44
  aerial_fishing:
    enabled: true
    fishing_level: 43
    hunter_level: 35
  fishing_trawler:
    enabled: true
    entry_level: 15
    trip_duration_ticks: 500
```

---

## Woodcutting

### Overview

Woodcutting chops logs from trees using axes. Trees have a depletion timer that starts on first interaction -- once the timer expires and a successful chop occurs, the tree falls and must respawn. Multiple players can chop the same tree, and the depletion timer is shared.

### Tree Types (Reference Data)

| Tree | Level | XP | Chop Time (ticks) | Respawn Time | Members |
|------|-------|----|-------------------|--------------|---------|
| Regular tree | 1 | 25 | Single log | 8.4 seconds | No |
| Oak | 15 | 37.5 | 45 (27s) | 8.4 seconds | No |
| Willow | 30 | 67.5 | 50 (30s) | 8.4 seconds | No |
| Teak | 35 | 85 | -- | 9 seconds | Yes |
| Maple | 45 | 100 | 100 (60s) | 35.4 seconds | No |
| Hollow tree | 45 | 82.5 | -- | -- | Yes |
| Mahogany | 50 | 125 | 100 (60s) | 8.4 seconds | No |
| Yew | 60 | 175 | 190 (114s) | 59.4 seconds | No |
| Sulliuscep | 65 | 127 | 1/16 depletion per chop | Rotates to next | Yes |
| Blisterwood | 62 | 76 | 1/10 depletion per chop | Regrows | Yes |
| Magic | 75 | 250 | 390 (234s) | 119.4 seconds | Yes |
| Redwood | 90 | 380 | 440 (264s) | 119.4 seconds | Yes |

### Tree Depletion System

Trees use a despawn timer model, distinct from mining's single-success depletion:

1. Timer starts when first player begins chopping
2. Timer counts down in ticks while any player is chopping
3. When timer reaches zero, the NEXT successful chop depletes the tree
4. Tree stump appears for respawn duration
5. If all players stop chopping before depletion, timer resets

This creates a "shared resource with time pressure" dynamic.

**Exception trees** use per-chop depletion chance instead:
- Sulliuscep: 1/16 chance per successful chop
- Blisterwood: 1/10 chance per successful chop

### Axe Tiers

| Axe | Skill Level | Roll Interval | Speed Bonus |
|-----|-------------|---------------|-------------|
| Bronze | 1 | 4 ticks | -- |
| Iron | 6 | 4 ticks | Faster success rate |
| Steel | 11 | 4 ticks | Faster success rate |
| Black | 11 | 4 ticks | Faster success rate |
| Mithril | 21 | 4 ticks | Faster success rate |
| Adamant | 31 | 4 ticks | Faster success rate |
| Rune | 41 | 4 ticks | Faster success rate |
| Dragon | 61 | 4 ticks | +3 visible boost special attack |
| Infernal | 61 | 4 ticks | 1/3 logs burned for 1/2 Firemaking XP |
| Crystal | 71 | 4 ticks | Requires crystal shards to operate |

Note: All axes roll every 4 ticks. Unlike pickaxes (which change roll interval), axes affect the success probability within the shared formula. Higher-tier axes use better `low`/`high` intercepts in the success formula.

### Unique Mechanics

#### Bird Nest Drops

Every successful chop has a secondary chance to produce a bird nest:

| Condition | Nest Drop Chance |
|-----------|-----------------|
| Base rate | 1/256 per successful chop |
| Woodcutting cape (99) | +10% (effectively ~1/233) |
| Twitcher's gloves | +20% for specific nest types |

Nest contents:

| Nest Type | Contents |
|-----------|----------|
| Seed nest | Tree seeds (oak through magic, weighted by rarity) |
| Ring nest | Gold ring, sapphire ring, through diamond ring |
| Egg nest | Red, blue, or green bird egg (rare) |
| Clue nest | Clue scroll (easy through elite, rate varies by tree) |
| Empty nest | Crushed nest (herblore ingredient) |

#### Forestry Events

World events that spawn while players are woodcutting. Group-based, rewarding participation XP and unique items.

| Event | Duration | Mechanic |
|-------|----------|----------|
| Struggling Sapling | 3m 30s | Group Farming training |
| Rising Roots | 1m 33s | Root chopping |
| Friendly Ent | 2m | Fletching training, yields egg nest |
| Beehive | 2m 30s | Construction training |
| Flowering Tree | 3m | Yields strange fruit and seeds |
| Pheasant Control | 1m 36s | Thieving training |
| Poachers | 1m 50s | Hunter training |
| Enchantment Ritual | 2m | Yields petal garland |
| Leprechaun | 1m 12s | Solo event |

Configurable: event spawn rate, which events are enabled, reward tables.

#### Group Woodcutting Boost

Players chopping the same tree receive +1 invisible Woodcutting boost per player at the tree, up to +10. Does not apply to regular trees, guild trees, or farmed trees.

#### Special Tree Mechanics

- **Sulliuscep**: Six trees in a fixed rotation. When one depletes, the next in the cycle becomes active. Yields mushrooms and fossils as secondary drops.
- **Hollow trees**: Yield bark (crafting material) instead of logs.
- **Redwood**: Two-sided (north/south face). Only found in Woodcutting Guild and Farming Guild.

### Equipment Bonuses

| Item | Effect |
|------|--------|
| Lumberjack outfit | +2.5% Woodcutting XP (standard gathering outfit) |
| Infernal axe | Burns 1/3 of logs for 1/2 normal Firemaking XP |
| Crystal axe | Better success rate than dragon; consumes crystal shards |
| Kandarin headgear 1-4 | Chance to double logs from normal trees (diary-tiered) |
| Log basket | Doubles effective inventory (28 extra log capacity) |
| Nature offerings | 60-80% chance for additional log per action |
| Felling axe + Forester's ration | +10% XP but lose 20% of logs |

### Woodcutting Configuration Schema

```
woodcutting:
  enabled: true
  depletion_model: "timer"  # "timer" or "per_chop_chance"
  bird_nest_enabled: true
  bird_nest_base_chance: 1/256
  forestry_events:
    enabled: true
    spawn_rate_modifier: 1.0
  group_boost:
    enabled: true
    max_boost: 10
  guild_boost: 7
  guild_entry_level: 60
```

---

## Farming

### Overview

Farming is a time-gated gathering skill. Instead of actively gathering from nodes, players plant seeds in patches, wait real-world time for growth, then harvest. The core loop is: plant, wait, return, harvest. This makes it the only gathering skill that is primarily passive.

### Patch Types

| Patch Type | Seeds Per Plant | Growth Cycle | Stages | Yield Type |
|------------|-----------------|--------------|--------|------------|
| Allotment | 3 | 10 minutes | 4-8 | Variable (multi-harvest) |
| Herb | 1 | 20 minutes | 4 | Variable (multi-harvest) |
| Flower | 1 | 5 minutes | 4 | Single (1 item) |
| Hops | 3-4 | 10 minutes | 4-7 | Variable (multi-harvest) |
| Bush | 1 | 20 minutes | 5-8 | Regenerating (regrows berries) |
| Tree | 1 (sapling) | 40 minutes | 4-8 | Single (XP on health check) |
| Fruit Tree | 1 (sapling) | 160 minutes | 6 | Regenerating (6 fruit, regrows) |
| Spirit Tree | 1 (sapling) | 320 minutes | 4 | Teleport network node |
| Hardwood | 1 (sapling) | 640 minutes | 4 | Single (XP on health check) |
| Cactus | 1 | 80 minutes | 7 | Regenerating |
| Mushroom | 1 | 40 minutes | 6 | Variable (multi-harvest) |
| Belladonna | 1 | 80 minutes | 4 | Single |
| Seaweed | 1 | 10 minutes | 4 | Variable (multi-harvest) |
| Calquat | 1 | 160 minutes | 8 | Single (XP on health check) |
| Hespori | 1 | 640 minutes | 3 | Boss fight trigger |
| Crystal | 1 | 80 minutes | 6 | XP only (no item) |
| Anima | 1 | 640 minutes | 4 | Passive buff to other crops |
| Redwood | 1 | 640 minutes | 11 | Single (XP on health check) |

### Growth Tick System

Farming uses a separate tick system from the main game tick:

| Cycle Length | Applies To |
|--------------|------------|
| 5 minutes | Flowers, saplings, weeds |
| 10 minutes | Allotments, hops, seaweed |
| 20 minutes | Herbs, bushes |
| 40 minutes | Trees, mushrooms, coral |
| 80 minutes | Cactus, crystal, belladonna |
| 160 minutes | Fruit trees, calquat, celastrus |
| 320 minutes | Spirit trees |
| 640 minutes | Hardwoods, redwood, anima, hespori |

Critical implementation detail: **each player has a random offset of 0-30 minutes** that persists through logouts. This means two players planting at the same time will have their crops advance at different real-world times.

Every 640 minutes, all growth cycles align and every crop type advances simultaneously.

### Disease System

| Property | Description |
|----------|-------------|
| Timing | Disease check occurs once per growth cycle |
| Immunity | First growth stage and fully-grown crops cannot get diseased |
| Effect | Diseased crop cannot advance; if not cured within one cycle, the crop dies |
| Cure | Plant cure item, secateurs on trees/bushes, or Cure Plant lunar spell |

Disease probability is per-crop-type and modified by:

| Factor | Disease Reduction |
|--------|-------------------|
| Regular compost | -50% |
| Supercompost | -80% |
| Ultracompost | -90% |
| Iasor anima seed (alive in anima patch) | Additional -80% (multiplicative), minimum 1/128 per cycle |
| Adjacent flower (allotment only) | Full protection for specific crop pairing |
| Payment to gardener | 100% protection (gardener watches crop) |
| Disease-free patches (quest-locked) | 0% disease chance always |

### Compost System

| Compost Type | Disease Reduction | Harvest Lives Added | How to Obtain |
|--------------|-------------------|---------------------|---------------|
| Regular | 50% | +1 | Compost bin with weeds/crops |
| Supercompost | 80% | +2 | Compost bin with high-tier crops, or Fertile Soil spell |
| Ultracompost | 90% | +3 | Add 25 volcanic ashes to supercompost |

**Bottomless compost bucket**: Holds 10,000 compost of any type. Doubles any compost added to it. Obtained from Hespori boss.

### Yield Formula (Variable Harvest Crops)

For allotments, herbs, hops, and other multi-harvest patches:

**Harvest lives** determine how many items you can pick. Each pick has a chance to consume a harvest life:

```
lives = base_lives + compost_bonus
base_lives = 3
compost_bonus = 0 (none), 1 (regular), 2 (super), 3 (ultra)
```

**Chance to save a harvest life** (keep picking without consuming a life):

```
save_chance = (1 + floor(CTS_low * (99 - level) / 98 + CTS_high * (level - 1) / 98 + 0.5)) / 256
```

Where `CTS_low` and `CTS_high` are crop-specific constants.

**Multipliers applied to save chance:**

| Source | Multiplier |
|--------|------------|
| Magic secateurs (equipped or in inventory) | x1.10 |
| Farming cape (99, herbs only) | x1.05 |
| Attas anima seed (alive in anima patch) | x1.05 |
| Diary bonuses (patch-specific) | +10 to +25 flat added to numerator |

**Expected yield formula:**

```
expected_yield = harvest_lives / (1 - save_chance)
```

At level 99 with ultracompost + magic secateurs + attas + farming cape, average herb yield is approximately **9.5 herbs per patch**.

### Payment Protection System

Nearby NPC gardeners will protect a growing crop from disease in exchange for specific items. Payment can be given in noted form. Gardeners CANNOT protect herb patches or flower patches.

Configurable per crop:
- `protection_payment`: list of item requirements
- `protectable`: boolean

### Farming Runs

The standard training method is "runs" -- circuits of patch locations:

| Run Type | Patches | Frequency | Primary Reward |
|----------|---------|-----------|----------------|
| Herb run | 7-9 herb patches | Every 80 minutes | Herbs (profit + XP) |
| Tree run | 7+ tree patches | Every 4-8 hours | Large XP drops |
| Fruit tree run | 6+ fruit patches | Every 16+ hours | Large XP drops |
| Hardwood run | 3 hardwood patches | Every 3 days | Very large XP drops |
| Seaweed run | 2 underwater patches | Every 40 minutes | Giant seaweed |

### Farming Contracts

A task system in the Farming Guild:

| Tier | Level Required | Mechanic |
|------|----------------|----------|
| Easy | 45 | Grow a specific low-level crop in the guild |
| Medium | 65 | Grow a specific mid-level crop in the guild |
| Hard | 85 | Grow a specific high-level crop in the guild |

Reward: Seed packs containing random seeds (including rare seeds like hespori, celastrus, redwood).

### Farming Guild

Three tiers of access:

| Section | Level | Features |
|---------|-------|----------|
| Base | 45 | Allotment, flower, herb patches, seed vault, contract NPC |
| Intermediate | 65 | Tree, fruit tree, bush patches, Hespori cave |
| Advanced | 85 | Celastrus, redwood patches, anima patch, farming supplies shop |

### Hespori Boss

A solo demi-boss that grows from a Hespori seed (640-minute growth cycle). Cannot become diseased. Upon maturity, player enters an instanced fight.

| Property | Value |
|----------|-------|
| Level Required | 65 Farming (to plant) |
| Growth Time | 640 minutes (~32 hours of growth ticks) |
| Fight Type | Solo instanced boss |
| Key Reward | Bottomless compost bucket (unique) |
| Pet Chance | Tangleroot pet (scaling with level) |

### Special Mechanics

| Mechanic | Description |
|----------|-------------|
| Tool leprechaun | NPC at every patch; stores tools and converts harvested items to noted form |
| Magic secateurs | +10% yield on herbs, allotments, and hops. Quest reward item. |
| Auto-weed | Purchased perk preventing weed regrowth on cleared patches |
| Sapling creation | Plant seed in pot with soil, water it, wait ~5 min for sapling |
| Spirit tree limit | 1 at base, 2 at level 91, 3 at level 99 (teleport network) |
| Amulet of nature | Binds to one patch; alerts on disease/maturity/death remotely |

### Farming Configuration Schema

```
farming:
  enabled: true
  growth_tick_base_minutes: 5
  player_offset_range_minutes: 30
  disease:
    enabled: true
    base_rates: {}  # per crop type
    compost_reduction:
      regular: 0.50
      super: 0.80
      ultra: 0.90
  compost:
    regular_lives: 1
    super_lives: 2
    ultra_lives: 3
  yield:
    base_lives: 3
    magic_secateurs_multiplier: 1.10
    farming_cape_multiplier: 1.05
    attas_multiplier: 1.05
  payment_protection:
    enabled: true
  hespori:
    enabled: true
    entry_level: 65
  contracts:
    enabled: true
    easy_level: 45
    medium_level: 65
    hard_level: 85
  auto_weed:
    enabled: true
    unlock_cost: 50  # tithe farm points
```

---

## Hunter

### Overview

Hunter uses traps to catch creatures. Unlike other gathering skills, the player places trap objects in the world, then waits for creatures to approach. Success depends on Hunter level, trap placement, bait, and smoking. The number of simultaneous traps scales with level.

### Trap Types

| Trap | Description | Items Needed |
|------|-------------|--------------|
| Bird snare | Placed on ground, catches birds | Bird snare |
| Box trap | Placed on ground, catches mammals | Box trap |
| Deadfall | Set on a boulder, catches kebbits/monkeys | Logs + knife |
| Net trap | Set on young trees, catches salamanders | Rope + small fishing net |
| Pitfall | Set over pits, catches large creatures | Logs + knife |
| Falconry | Rented falcon catches kebbits (no player traps) | 500 gp rental fee |
| Butterfly net | Handheld, catches butterflies and implings | Butterfly net + jar |
| Magic box | Catches imps for banking utility | Magic box item |
| Rabbit snare | Catches rabbits with ferret assistance | Ferret + snare |
| Birdhouse | Passive traps on Fossil Island | Crafted birdhouse + seeds |

### Maximum Simultaneous Traps

| Hunter Level | Max Traps |
|--------------|-----------|
| 1-19 | 1 |
| 20-39 | 2 |
| 40-59 | 3 |
| 60-79 | 4 |
| 80+ | 5 |

Bonus: +1 extra trap when hunting in the Wilderness.

### Creature Reference Data

#### Bird Snare Creatures

| Creature | Level | XP | Location |
|----------|-------|----|----------|
| Crimson swift | 1 | 34 | Feldip Hunter area |
| Golden warbler | 5 | 47 | Uzer Hunter area |
| Copper longtail | 9 | 61 | Piscatoris Hunter area |
| Cerulean twitch | 11 | 64.6 | Rellekka Hunter area |
| Tropical wagtail | 19 | 95 | Feldip Hunter area |

#### Box Trap Creatures

| Creature | Level | XP | Bait |
|----------|-------|----|------|
| Ferret | 27 | 115 | None |
| Embertailed jerboa | 39 | 137 | None |
| Chinchompa (grey) | 53 | 198.4 | Spicy tomato |
| Carnivorous chinchompa (red) | 63 | 265 | Spicy minced meat |
| Black chinchompa | 73 | 315 | Spicy minced meat |

#### Net Trap Creatures (Salamanders)

| Creature | Level | XP |
|----------|-------|----|
| Swamp lizard | 29 | 152 |
| Orange salamander | 47 | 224 |
| Red salamander | 59 | 272 |
| Black salamander | 67 | 319.2 |
| Tecu salamander | 79 | 344 |

### Catch Rate

The exact Hunter catch formula has not been fully reverse-engineered, but the following is known:

- Base catch rate scales with Hunter level using the standard interpolation formula
- Each creature has its own `low` and `high` intercept values
- Two modifiers are confirmed:

| Modifier | Bonus |
|----------|-------|
| Bait (creature-specific) | +3% catch rate |
| Smoking trap (torch) | +2% catch rate |

These are additive bonuses to the base percentage.

### Unique Mechanics

#### Birdhouse Trapping (Passive Hunter)

A passive, time-gated system similar to Farming. Players craft birdhouses, fill them with seeds, and place them on Fossil Island. After ~50 minutes, they can be collected for XP and loot.

| Birdhouse | Crafting Level | Hunter Level | XP Per House |
|-----------|----------------|--------------|--------------|
| Regular | 5 | 5 | 280 |
| Oak | 15 | 14 | 420 |
| Willow | 25 | 24 | 560 |
| Teak | 35 | 34 | 700 |
| Maple | 45 | 44 | 820 |
| Mahogany | 50 | 49 | 960 |
| Yew | 60 | 59 | 1,020 |
| Magic | 75 | 74 | 1,140 |
| Redwood | 90 | 89 | 1,200 |

Seed cost: 10 low-level seeds or 5 high-level seeds per birdhouse. Four birdhouse slots on Fossil Island. Run takes ~70 seconds. Rewards include bird nests (seed, ring, egg types), feathers, and raw bird meat.

Nest drop rates scale with Hunter level. Seed nest chance ranges from 0.4% at level 1 to ~78.5% at level 99.

#### Herbiboar Tracking

A tracking-based hunt on Fossil Island.

| Property | Value |
|----------|-------|
| Level Required | 80 Hunter |
| Mechanic | Follow tracks through vegetation, inspect objects to reveal path |
| Reward | Grimy herbs (type and quantity scale with Herblore level) |
| Magic Secateurs | Increase herb quantity received |
| Pet Chance | Herbi pet at 1/6,500 per harvest |

#### Chinchompa Mechanics

Chinchompas are stackable ranged weapons, making them highly valuable. They are caught in box traps.

| Type | Level | XP | Value | Location |
|------|-------|----|-------|----------|
| Grey chinchompa | 53 | 198.4 | Low | Piscatoris |
| Red chinchompa | 63 | 265 | Medium | Feldip, Gwenith |
| Black chinchompa | 73 | 315 | High | Wilderness (PvP risk) |

Black chinchompas are in the Wilderness, adding PvP danger. This is a key risk/reward design.

#### Rumour System (Hunter Contracts)

The Hunter Guild (level 46+) assigns specific creatures to hunt. Completing rumours grants unique rewards:

| Property | Value |
|----------|-------|
| Entry Level | 46 Hunter |
| Mechanic | Guild hunter assigns target creature |
| Rewards | Quetzal whistle, guild hunter outfit, quetzal feed |
| XP Rate | 160,000-250,000 XP/hr at high levels |
| Outfit Bonus | +2.5% catch rate + 5% rare drops from rumours |

#### Implings

Flying creatures caught with butterfly nets. Higher-level implings contain better loot. Can be caught barehanded at +10 level requirement.

| Impling | Level | Notable Loot |
|---------|-------|-------------|
| Baby | 17 | Basic supplies |
| Young | 22 | Thread, steel nails |
| Gourmet | 28 | Food items |
| Earth | 36 | Mining supplies |
| Essence | 42 | Rune/pure essence |
| Eclectic | 50 | Medium clue scrolls |
| Nature | 58 | Herbs, seeds |
| Magpie | 65 | Jewelry, seeds |
| Ninja | 74 | Weapon poison, daggers |
| Dragon | 83 | Dragon items, dragonstone |
| Lucky | 89 | Clue scroll rewards directly |

### Hunter Configuration Schema

```
hunter:
  enabled: true
  max_traps:
    - { min_level: 1, max_level: 19, traps: 1 }
    - { min_level: 20, max_level: 39, traps: 2 }
    - { min_level: 40, max_level: 59, traps: 3 }
    - { min_level: 60, max_level: 79, traps: 4 }
    - { min_level: 80, max_level: 99, traps: 5 }
  wilderness_bonus_trap: true
  bait_catch_bonus: 0.03
  smoke_catch_bonus: 0.02
  birdhouse_trapping:
    enabled: true
    maturation_minutes: 50
    slots: 4
  herbiboar:
    enabled: true
    entry_level: 80
    pet_rate: 1/6500
  rumours:
    enabled: true
    entry_level: 46
  implings:
    enabled: true
    barehand_level_offset: 10
```

---

## Divination

### Overview (RS3 Addition)

Divination was added in RS3 as a gathering skill focused on harvesting divine energy from wisps -- ethereal entities that spawn at energy rifts scattered across the world. It fills the role of a "meta-gathering" skill: instead of physical resources, players harvest raw magical energy and memories.

### Core Mechanic

```
Player + No Tool Required --> Wisp --> Memory + Energy
```

1. **Wisps** spawn near energy rifts at specific locations
2. Player clicks a wisp to begin harvesting (no tool needed)
3. Each harvest produces a **memory** (primary product) and sometimes **divine energy** (secondary)
4. Memories are deposited at the nearby energy rift for XP
5. Player can choose: convert memory to XP only, or convert memory to energy + reduced XP

### Wisp/Energy Tiers

| Tier | Level | Location Theme | Energy Type |
|------|-------|----------------|-------------|
| Pale | 1 | Starting area | Pale energy |
| Flickering | 10 | Low-level area | Flickering energy |
| Bright | 20 | Mid-level area | Bright energy |
| Glowing | 30 | Mid-level area | Glowing energy |
| Sparkling | 40 | Mid-level area | Sparkling energy |
| Gleaming | 50 | Mid-level area | Gleaming energy |
| Vibrant | 60 | Mid-level area | Vibrant energy |
| Lustrous | 70 | High-level area | Lustrous energy |
| Brilliant | 80 | High-level area | Brilliant energy |
| Radiant | 85 | High-level area | Radiant energy |
| Luminous | 90 | High-level area | Luminous energy |
| Incandescent | 95 | Highest area | Incandescent energy |
| Elder | 97+ | Post-quest | Elder energy |

### Unique Systems

| System | Description |
|--------|-------------|
| **Divine Locations** | Craftable gathering nodes (divine trees, rocks, fishing spots) that anyone can use for daily-limited free resources |
| **Transmutation** | Convert lower resources into higher ones using energy (e.g., oak logs into willow logs) |
| **Signs and Portents** | Consumable items providing passive effects (auto-heal on death, bonus loot, etc.) |
| **Energy Rift Enrichment** | Periodically wisps become "enriched," yielding enhanced memories worth more XP |
| **Chronicle Fragments** | Rare spawns during training; collected and turned in for bonus XP and lore |

### Design Rationale for World Builders

Divination adds value as:
- A resource sink (energy consumed to create divine locations and transmutations)
- A gathering skill with zero tool requirements (accessible)
- A lore delivery mechanism (chronicle fragments)
- A daily-cap system (divine locations have per-player limits)

### Divination Configuration Schema

```
divination:
  enabled: false  # RS3 addition, off by default
  wisp_tiers: []  # define per world
  enriched_wisp_chance: 0.05
  chronicle_fragment_chance: 0.01
  divine_locations:
    enabled: true
    daily_limit_per_player: 50  # interactions per day
  transmutation:
    enabled: true
    energy_cost_multiplier: 1.0
```

---

## Archaeology

### Overview (RS3 Addition)

Archaeology was added in RS3 as a gathering skill focused on excavating artifacts from dig sites. It combines gathering mechanics with a restoration/collection system. Players excavate soil from dig spots, restore damaged artifacts at workbenches, and complete collections for rewards.

### Core Mechanic

```
Player + Mattock (tool) --> Excavation Spot --> Damaged Artifact + Soil + Materials
```

1. **Excavation spots** are located at dig sites across the world
2. Player uses a **mattock** (tiered tool, like pickaxe) to excavate
3. Each excavation can yield: damaged artifacts, soil, and material caches
4. Damaged artifacts are repaired at a workbench using gathered materials
5. Restored artifacts are added to collections or kept for passive relics

### Mattock Tiers

| Mattock | Level | Notes |
|---------|-------|-------|
| Bronze | 1 | Starting tool |
| Iron | 10 | -- |
| Steel | 20 | -- |
| Mithril | 30 | -- |
| Adamant | 40 | -- |
| Rune | 50 | -- |
| Dragon | 60 | -- |
| Crystal | 70 | Augmentable |
| Imcando | 76 | Special passive |
| Earth & Song | 76 | Best in slot (combination of crystal + imcando) |
| Time & Space | 99 | Master quest cape requirement |

### Dig Site Progression

| Dig Site | Level Range | Theme |
|----------|-------------|-------|
| Kharid-et | 1-25 | Zarosian fortress |
| Infernal Source | 20-45 | Zamorakian ritual site |
| Everlight | 42-68 | Saradominist lighthouse |
| Stormguard Citadel | 70-84 | Armadylean sky fortress |
| Warforge | 76-99 | Bandosian war factory |
| Orthen | 90-120 | Dragonkin laboratory |

### Unique Systems

| System | Description |
|--------|-------------|
| **Artifact Restoration** | Repair damaged artifacts using specific materials at a workbench. Each artifact has a recipe. |
| **Collections** | Turn in sets of related artifacts to collectors for XP, chronotes (currency), and lore |
| **Chronotes** | Currency earned from collections; spent on research, teleports, and unlocks |
| **Relics** | Passive powers unlocked by restoring specific artifacts. Equipped in relic slots for permanent bonuses (increased XP, auto-screening soil, bonus resources, etc.) |
| **Soil Screening** | Soil gathered during excavation can be screened at a screening station for bonus materials |
| **Mystery System** | Story-driven puzzles unlocked through artifact restoration and exploration |
| **Research** | Time-gated unlocks (real-time research missions) that open new dig site areas |
| **Material Storage** | Dedicated storage for excavation materials (separate from bank) |
| **Ancient Invention** | High-level artifacts unlock new Invention blueprints |
| **Qualifications** | Milestone levels that unlock access to deeper dig site areas and new excavation spots |

### Design Rationale for World Builders

Archaeology adds value as:
- A skill with strong narrative integration (every artifact tells a story)
- A collection/completion-driven progression system
- A source of powerful passive relics (equivalent to permanent buffs)
- A material sink connecting to other skills (Invention, Smithing)
- Content that scales to extremely high levels (up to 120)
- A model for "gather then process" skills where both steps grant XP

### Archaeology Configuration Schema

```
archaeology:
  enabled: false  # RS3 addition, off by default
  mattock_tiers: []  # define per world
  dig_sites: []  # define per world
  soil_screening:
    enabled: true
  collections:
    enabled: true
  relics:
    enabled: true
    max_relic_slots: 3
  research:
    enabled: true
    real_time_gated: true
  material_storage:
    capacity: 100  # per material type
  max_level: 120  # RS3 supports 120, OSRS caps at 99
```

---

## Module Toggles

Every gathering system is independently toggleable. This table summarizes all modules a world builder can enable or disable:

### Skill-Level Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `mining.enabled` | true | Enable/disable Mining skill entirely |
| `fishing.enabled` | true | Enable/disable Fishing skill entirely |
| `woodcutting.enabled` | true | Enable/disable Woodcutting skill entirely |
| `farming.enabled` | true | Enable/disable Farming skill entirely |
| `hunter.enabled` | true | Enable/disable Hunter skill entirely |
| `divination.enabled` | false | Enable/disable Divination (RS3) |
| `archaeology.enabled` | false | Enable/disable Archaeology (RS3) |

### Minigame/Activity Toggles

| Toggle | Default | Skill | Description |
|--------|---------|-------|-------------|
| `mining.motherlode_mine.enabled` | true | Mining | Motherlode Mine random-ore system |
| `mining.shooting_stars.enabled` | true | Mining | Shooting Stars world events |
| `mining.volcanic_mine.enabled` | true | Mining | Volcanic Mine group activity |
| `fishing.tempoross.enabled` | true | Fishing | Tempoross skilling boss |
| `fishing.drift_net.enabled` | true | Fishing | Drift net hybrid activity |
| `fishing.aerial_fishing.enabled` | true | Fishing | Aerial fishing hybrid activity |
| `fishing.fishing_trawler.enabled` | true | Fishing | Fishing Trawler team minigame |
| `woodcutting.forestry_events.enabled` | true | Woodcutting | Forestry world events |
| `farming.hespori.enabled` | true | Farming | Hespori boss encounter |
| `farming.contracts.enabled` | true | Farming | Farming Guild contract system |
| `hunter.birdhouse_trapping.enabled` | true | Hunter | Passive birdhouse runs |
| `hunter.herbiboar.enabled` | true | Hunter | Herbiboar tracking activity |
| `hunter.rumours.enabled` | true | Hunter | Hunter Guild contract system |
| `hunter.implings.enabled` | true | Hunter | Impling catching |

### Mechanic Toggles

| Toggle | Default | Description |
|--------|---------|-------------|
| `global.tick_manipulation` | true | Allow tick manipulation across all skills |
| `global.secondary_drops` | true | Enable secondary drop tables (gems, nests, etc.) |
| `global.xp_outfits` | true | Enable +2.5% XP gathering outfits |
| `global.guild_boosts` | true | Enable invisible level boosts in guilds |
| `mining.gem_drops` | true | Enable gem drop table while mining |
| `mining.competition_model` | "first_success" | "first_success" (competitive) or "individual" (solo) |
| `fishing.spot_movement` | true | Whether fishing spots relocate periodically |
| `fishing.barehand_fishing` | true | Allow barehand fishing technique |
| `woodcutting.bird_nests` | true | Enable bird nest drops from trees |
| `woodcutting.group_boost` | true | Enable group woodcutting level bonus |
| `farming.disease` | true | Enable crop disease system |
| `farming.payment_protection` | true | Enable NPC gardener crop protection |
| `farming.auto_weed` | true | Enable auto-weed purchasable perk |
| `farming.growth_offset` | true | Enable per-player growth tick offset |
| `hunter.wilderness_bonus_trap` | true | Enable +1 trap in Wilderness |
| `hunter.bait_bonus` | 0.03 | Catch rate bonus from using bait |
| `hunter.smoke_bonus` | 0.02 | Catch rate bonus from smoking traps |

### XP and Rate Multipliers

| Toggle | Default | Description |
|--------|---------|-------------|
| `global.xp_multiplier` | 1.0 | Global XP multiplier for all gathering |
| `global.respawn_speed_multiplier` | 1.0 | Global node respawn speed (higher = faster) |
| `global.success_rate_modifier` | 0 | Flat additive modifier to success formula numerator |
| `farming.growth_speed_multiplier` | 1.0 | Multiplier for farming growth tick intervals |
| `hunter.trap_check_interval_ticks` | 1 | How often traps check for catches |

---

## Summary of Gathering Skill Patterns

| Property | Mining | Fishing | Woodcutting | Farming | Hunter |
|----------|--------|---------|-------------|---------|--------|
| Active/Passive | Active | Active | Active | Passive (time-gated) | Semi-active (trap placement) |
| Tool Required | Yes (pickaxe) | Yes (varies by method) | Yes (axe) | Yes (dibber, rake, etc.) | Yes (trap items) |
| Node Depletes | Yes (single claim) | No (spots move instead) | Yes (timer-based) | N/A (harvested manually) | N/A (traps are player-placed) |
| Competition | Yes (first success) | No (independent rolls) | Yes (shared timer) | No (personal patches) | No (personal traps) |
| Ticks Between Rolls | Tool-dependent (2-8) | Method-dependent (5 default) | Fixed 4 ticks | N/A (growth ticks) | Creature-dependent |
| Secondary Drops | Gems (1/256) | Extra fish (equipment) | Bird nests (1/256) | Seeds from contracts | Nests from birdhouses |
| Hybrid Skill | No | Yes (Fishing+Hunter) | No | No | Yes (Hunter+Fishing) |
| Skilling Boss | Shooting Stars (event) | Tempoross | Forestry Events | Hespori | -- |
| Passive Method | No | No | No | Yes (entire skill) | Birdhouses |
| Guild Boost | +7 invisible | +7 invisible | +7 invisible | N/A (tiered access) | Contract system |
