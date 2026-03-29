# Plugin Registry

Master list of every individually toggleable plugin in the Build Your Own Scape system. Each plugin can be independently enabled or disabled by the dungeon master.

---

## Summary

| Category | Count | Description |
|---|---|---|
| Equipment | 20 | Wearable items, combat bonuses, upgrades, cosmetics |
| Skills -- Core | 20 | XP curves, level caps, milestones, calculators, analytics |
| Skills -- Combat | 20 | Combat triangle, prayers, slayer, summoning, abilities |
| Skills -- Gathering | 46 | Mining, fishing, woodcutting, farming, hunter, divination, archaeology |
| Skills -- Processing | 47 | Cooking, firemaking, smithing smelting, runecrafting, thieving |
| Skills -- Combining | 52 | Herblore, crafting, fletching, construction, invention |
| Skills -- Activity | 27 | Agility, prayer training, dungeoneering, sailing, socializing |
| Skills -- Inspired Fantasy | 75 | Original fantasy skill concepts across all disciplines |
| Skills -- Inspired Futuristic | 42 | Original sci-fi/futuristic skill concepts across all disciplines |
| Quests | 15 | Quest points, helpers, repeatable quests, quest series |
| Monsters | 16 | Types, immunities, drop tables, bestiary, tracking |
| Bosses and Raids | 18 | Scaling, phases, difficulty, loot, instance management |
| Items | 29 | Stacking, noting, alchemy, potions, loot filters, tracking |
| NPCs | 11 | Schedules, services, factions, random events, transport |
| Shops | 20 | Shop types, stock rules, pricing, bulk discount, custom types |
| Terrain | 20 | Tiles, elevation, biomes, procedural generation, speed modifiers |
| Buildings | 23 | Walls, doors, rooms, lighting, roofs, templates |
| Locations | 25 | Nested locations, weather, hazards, waypoints, day/night |
| Tick System | 19 | Tick manipulation, PID, freeze immunity, practice mode |
| Achievements | 12 | Points, tiers, feats, completionist, speedrun splitter |
| Collection Log | 9 | KC display, duplicates, milestones, luck calculator |
| Minigames | 17 | Deaths, rewards, matchmaking, spectating, prop hunt |
| Puzzles | 8 | Timer, hints, randomization, leaderboards, puzzle builder |
| Dailies | 16 | Reset cycles, streaks, D&D, profit tracking, notifications |
| Games of Chance | 56 | Master toggle, system toggles, individual games, drop simulator |
| Account Management | 42 | Profile, security, accessibility, save states, UI customization |
| Character Overview | 30 | Stats, logs, trackers, analytics, shareable profiles |
| Inventory and Bank | 25 | Weight, stacks, presets, tabs, search, tagging |
| Death System | 13 | Item loss, gravestones, death office, PvP loot, hardcore |
| Player Housing | 18 | Rooms, furniture, portals, trophies, servants, themes |
| Clan System | 31 | Ranks, citadel, wars, events, territory, analytics |
| Communication | 44 | Chat channels, voice, emojis, bots, threading, bridges |
| Friends and Social | 16 | Groups, activity feed, lending, LFG, spectating |
| Economy | 31 | Grand Exchange, trading, alchemy, price tools, anti-fraud |
| Monetization | 23 | Marketplace, cosmetics, battle pass, donations, safeguards |
| Rules and Moderation | 17 | Strikes, appeals, reports, mod tools, quarantine |
| Bot Detection | 12 | Random events, CAPTCHA, honeypots, ML, bot API |
| Modes | 20 | Multi-mode, seasonal, relics, randomizer, chunk-locked |
| Server Stats and Voting | 25 | Dashboard, hiscores, polls, bug reports, diagnostics |
| Dialogue | 18 | Scripted, branching, AI freeform, romance, NPC memory |
| External Integrations | 17 | Webhooks, API, data export, chat bridges, IoT |
| Music and Audio | 12 | Track unlocking, playlists, jukebox, custom uploads |
| Pet and Companion | 11 | Naming, transmog, raising, abilities, insurance |
| Engine Architecture | 14 | WebSocket, binary chunks, dual render, pathfinding |
| **Total (deduplicated)** | **~832** | |

---

## Equipment

| Plugin | Description |
|---|---|
| Weight System | Items contribute to carry weight, affecting run energy drain |
| Degradation | Items lose durability through use and require repair |
| Quest Requirements | Items require quest completion to equip |
| Stat Requirements | Items require minimum skill levels to equip |
| Combat Bonuses | Items provide offensive and defensive stat modifiers |
| Untradeables | Certain items cannot be traded between players |
| Set Bonuses | Wearing multiple items from a set grants additional effects |
| Enchanting | Items can be enchanted using skills and materials |
| Imbuing | Items can be imbued through minigames, achievements, or other sources |
| Weapon Scaling | Weapon damage scales with player progression or conditions |
| Special Attacks | Weapons have unique special attack abilities |
| Fashion/Transmog | Items support cosmetic appearance overrides and color customization |
| Slot Locking | DM can lock specific equipment slots to restrict gear choices |
| Equipment Screenshot | One-click screenshot of current equipment loadout for sharing |
| Empty Slot Warning | Warns players when entering combat with empty equipment slots |
| Equipment Rule Enforcement | Server-defined rules restricting what can be equipped where |
| Spellbook Presets | Save and switch between spellbook configurations |
| Gear Switch Trainer | Practice mode for learning fast gear switches with timing feedback |
| Gear Tier Restriction | Restrict accounts to specific gear tiers for challenge modes |
| Cosmetic Transmog | Override item appearance with unlocked cosmetic skins |

---

## Skills -- Core

| Plugin | Description |
|---|---|
| Per-Skill Enable/Disable | Toggle individual skills on or off for the server |
| XP Curve Config | Configure the experience curve formula per skill |
| Max Level Cap | Set the maximum achievable level per skill |
| Virtual Levels | Display levels beyond the functional cap for prestige |
| Skill Categories | Group skills into custom categories for UI organization |
| Skill Prerequisites | Require levels in one skill before training another |
| Skill Milestones/Capes | Award cosmetic capes and titles at milestone levels |
| Source Filter (OSRS/RS3/Inspired) | Filter available skills by their origin game or custom status |
| Success Rate Transparency | Display exact success/failure rates for skill actions |
| Per-Method XP Rate Tracking | Track and display XP rates for each training method |
| Resource Node Timers | Show respawn countdown timers on depleted resource nodes |
| Resource Node Map Tracking | Mark active resource node locations on the world map |
| XP Lock Per Skill | Lock a skill to prevent gaining XP in it |
| Prestige/Rebirth | Reset a skill to level 1 for prestige rewards and tracking |
| Task Assignment History | Track which skill tasks have been assigned and completed |
| Resting Mechanic | Players can rest to regenerate run energy or receive buffs |
| Banked XP Calculator | Calculate total XP stored as banked resources |
| XP Graphing/Analytics | Graph XP gains over time with session and historical views |
| Skill Calculator | Calculate resources and time needed to reach target levels |
| Time-Gated Activity Tracker | Track progress on activities with daily or weekly time gates |

---

## Skills -- Combat

| Plugin | Description |
|---|---|
| Combat Triangle | Melee beats ranged, ranged beats magic, magic beats melee |
| Attack/Strength Split | Separate accuracy (Attack) from damage (Strength) |
| Defence as Avoidance | Defence reduces damage taken or chance to be hit |
| Protection Prayers PvE | Overhead prayers reduce or negate damage from NPCs |
| Protection Prayers PvP | Overhead prayers reduce damage from other players |
| Prayer Flicking | Toggling prayers on the correct tick to conserve prayer points |
| Hitpoints Starting Level | Configure the starting hitpoints level for new characters |
| Natural HP Regen | Passive hitpoints regeneration over time |
| Overheal | Allow healing above maximum hitpoints temporarily |
| Slayer Task System | NPCs assigned as tasks that grant bonus XP on kill |
| Superior Monsters | Rare enhanced versions of slayer monsters with better drops |
| Splash Training | Allow zero-damage magic casts for magic XP |
| XP Per Damage Ratio | Configure how much combat XP is earned per damage dealt |
| Ammo Consumption | Ranged attacks consume ammunition from the ammo slot |
| Bolt Enchantments | Enchanted bolts trigger special effects on hit |
| Ability System | RS3-style abilities with cooldowns replacing auto-attacks |
| Adrenaline | Resource bar built through combat used to fuel special abilities |
| Dual Wielding | Equip weapons in both main-hand and off-hand slots |
| Summoning | Summon familiars that fight, carry items, or provide passive bonuses |
| Necromancy | Raise undead to fight alongside you, scale with skill level |

---

## Skills -- Gathering

### Global Gathering Toggles

| Plugin | Description |
|---|---|
| Tick Manipulation | Allow tick manipulation techniques for faster gathering |
| Secondary Drops | Gathering resources can yield secondary random items |
| XP Outfits | Equipping full skilling outfits grants bonus gathering XP |
| Guild Boosts | Gathering inside skill guilds provides invisible level boosts |
| Global XP Multiplier | Server-wide multiplier applied to all gathering XP |
| Respawn Speed Multiplier | Adjust how quickly depleted resource nodes respawn |
| Success Rate Modifier | Global modifier to gathering success roll calculations |

### Mining

| Plugin | Description |
|---|---|
| Mining Skill Toggle | Enable or disable the Mining skill entirely |
| Gem Drops | Mining rocks has a chance to yield uncut gems |
| Competition Model | Multiple players mining the same rock compete for resources |
| Motherlode Mine | Minigame-style mining with pay-dirt and golden nuggets |
| Shooting Stars | Random world event where fallen stars provide mining XP |
| Volcanic Mine | Team-based volcanic mining minigame |

### Fishing

| Plugin | Description |
|---|---|
| Fishing Skill Toggle | Enable or disable the Fishing skill entirely |
| Spot Movement | Fishing spots periodically move to nearby locations |
| Barehand Fishing | Catch fish without a tool using high skill levels |
| Fishing Tick Manipulation | Allow tick manipulation techniques specific to fishing |
| Tempoross | Skilling boss encounter for fishing |
| Drift Net Fishing | Underwater net fishing requiring hunter and fishing |
| Aerial Fishing | Bird-assisted fishing requiring hunter and fishing |
| Fishing Trawler | Cooperative boat-based fishing minigame |

### Woodcutting

| Plugin | Description |
|---|---|
| Woodcutting Skill Toggle | Enable or disable the Woodcutting skill entirely |
| Bird Nests | Chance to receive bird nests while chopping trees |
| Forestry Events | Random cooperative events while woodcutting |
| Group Boost | Nearby players woodcutting together gain a speed bonus |

### Farming

| Plugin | Description |
|---|---|
| Farming Skill Toggle | Enable or disable the Farming skill entirely |
| Disease | Crops can become diseased and die if untreated |
| Payment Protection | Pay an NPC to protect a crop patch from disease |
| Auto Weed | Automatically clear weeds from farming patches |
| Growth Offset | Randomize growth cycle start times to prevent sync farming |
| Growth Speed | Configure how fast crops grow through their stages |
| Hespori | Farming-themed boss encounter grown from special seeds |
| Contracts | Farming guild assigns specific crops to grow for bonus rewards |

### Hunter

| Plugin | Description |
|---|---|
| Hunter Skill Toggle | Enable or disable the Hunter skill entirely |
| Wilderness Trap | Place traps in the wilderness for rare hunter creatures |
| Bait Bonus | Using bait improves catch rate for specific creatures |
| Smoke Bonus | Smoking traps reduces creature detection of the trap |
| Birdhouses | Passive hunter XP through birdhouse runs on a timer |
| Herbiboar | Track and catch the herbiboar for herbs and hunter XP |
| Rumours | Hunter master assigns rumour tasks for bonus rewards |
| Implings | Catchable impling creatures that contain random loot |

### Divination

| Plugin | Description |
|---|---|
| Divination Skill Toggle | Enable or disable the Divination skill entirely |
| Divine Locations | Place temporary skilling nodes usable by all players |
| Transmutation | Convert lower-tier resources into higher-tier ones |
| Enriched Wisps | Rare wisps that yield enhanced energy and XP |
| Chronicles | Catch chronicle fragments for bonus divination XP |

### Archaeology

| Plugin | Description |
|---|---|
| Archaeology Skill Toggle | Enable or disable the Archaeology skill entirely |
| Soil Screening | Screen collected soil for bonus materials and artifacts |
| Collections | Turn in themed sets of artifacts for rewards |
| Relics | Equip ancient relics for passive gameplay-altering effects |
| Research | Unlock knowledge through research at the archaeology guild |
| Material Storage | Dedicated storage for archaeology materials |

---

## Skills -- Processing

### Shared Processing Toggle

| Plugin | Description |
|---|---|
| Processing Fail Chance | Configurable chance to fail when processing materials |

### Cooking

| Plugin | Description |
|---|---|
| Cooking Skill Toggle | Enable or disable the Cooking skill entirely |
| Burn Chance | Food can burn based on cooking level and heat source |
| Cooking Gauntlets | Equipment that reduces burn chance |
| Range vs Fire | Different heat sources provide different burn rates |
| Multi-Bite Food | Food heals in multiple bites over time |
| Combo Food | Eat multiple food types in one tick |
| Brewing | Brew ales and ciders with variable outcomes |
| Spice Stew | Boost or drain random skills using spiced stews |
| Cooking Guild | Access to a dedicated cooking area with bonuses |

### Firemaking

| Plugin | Description |
|---|---|
| Firemaking Skill Toggle | Enable or disable the Firemaking skill entirely |
| Line Firemaking | Fires must be lit in a line, walking west |
| Bonfire | Add logs to existing fires for bonus XP |
| Light Source | Fires and lanterns illuminate dark areas |
| Fire Spirits | Random chance for fire spirits to appear with bonus loot |
| Wintertodt | Skilling boss encounter for firemaking |
| Infernal Tools | Tools that auto-smelt or auto-cook gathered resources |
| Shade Burning | Burn shade remains on funeral pyres for rewards |

### Smithing Smelting

| Plugin | Description |
|---|---|
| Smithing Skill Toggle | Enable or disable the Smithing skill entirely |
| Smelting Bars | Combine ores at a furnace to produce metal bars |
| Superheat | Use magic to smelt ores without a furnace |
| Blast Furnace | Minigame with halved coal requirements for smelting |
| Cannonball Smithing | Smith steel bars into cannonballs for ranged combat |
| Artisan Workshop | Dedicated smithing area with burial armor and ceremonial swords |
| Heat Mechanic | RS3-style reheating system for smithing efficiency |
| Alloy Mixing | Combine multiple bar types to create alloy materials |
| Masterwork Smithing | Multi-step smithing process for endgame gear |

### Runecrafting

| Plugin | Description |
|---|---|
| Runecrafting Skill Toggle | Enable or disable the Runecrafting skill entirely |
| Multiple Runes | Higher levels yield multiple runes per essence |
| Combination Runes | Combine two rune types at specialized altars |
| Pure Essence | Require pure essence for higher-tier runes |
| Abyss | Dangerous shortcut to all runecrafting altars |
| Pouches | Carry extra essence using degradable pouches |
| Ourania Altar | Altar that crafts random rune types for bonus XP |
| Blood/Soul Runecrafting | Zeah-style runecrafting using dark essence |
| Runespan | RS3-style runecrafting minigame with siphoning |
| Rune Goldberg Machine | Daily puzzle for runecrafting rewards |

### Thieving

| Plugin | Description |
|---|---|
| Thieving Skill Toggle | Enable or disable the Thieving skill entirely |
| Stun on Fail | Failed pickpocket attempts stun the player |
| NPC Awareness | NPCs become harder to pickpocket after repeated attempts |
| Ardy Knight | Specific NPC with high pickpocket rates for training |
| Stalls | Steal from market stalls for items and XP |
| Chests | Locked chests requiring thieving level to open |
| Wall Safes | Crack wall safes for gems and rewards |
| Pyramid Plunder | Minigame with increasing risk and reward per room |
| Sorceress Garden | Stealth-based thieving minigame with herb rewards |
| Master Farmer | Pickpocket farmers for seeds |

---

## Skills -- Combining

### Herblore

| Plugin | Description |
|---|---|
| Herblore Skill Toggle | Enable or disable the Herblore skill entirely |
| Grimy Herbs | Herbs must be cleaned before use |
| Unfinished Potions | Two-step potion creation with unfinished intermediary |
| Secondary Ingredients | Potions require secondary ingredients beyond herbs |
| Potion Doses | Potions contain multiple doses per vial |
| Decanting | Combine partial potions into full-dose vials |
| Barbarian Mixes | Mix potions with fish for combo food-potion items |
| Divine Potions | Potions that boost stats with a resource cost over time |
| Overloads | Extreme potions that boost all combat stats simultaneously |
| Herblore Habitat | Dedicated area combining herblore with hunter and farming |
| Potion Recipes | Discovery-based recipe unlocking system |
| Adrenaline Potions | Potions that restore or boost the adrenaline bar |

### Crafting

| Plugin | Description |
|---|---|
| Crafting Skill Toggle | Enable or disable the Crafting skill entirely |
| Leather Working | Craft armor and items from tanned hides |
| Gem Cutting | Cut uncut gems into finished gems for jewelry |
| Jewelry Making | Craft rings, amulets, necklaces, and bracelets |
| Pottery | Shape clay into useful items at a pottery wheel |
| Glass Blowing | Craft glass items from molten glass |
| Spinning | Spin raw materials into usable thread and string |
| Weaving | Weave thread into cloth and fabric items |
| Battlestave Crafting | Attach orbs to battlestaves for magic weapons |
| Dragonhide Crafting | Craft armor from dragon leather |
| Chisel Work | Detailed crafting using a chisel tool |
| Tanning | Convert raw hides into usable leather at a tanner |
| Stringing | String unstrung items like amulets and bows |
| Silver Crafting | Craft items from silver bars at a furnace |
| Gold Crafting | Craft items from gold bars at a furnace |
| Shell Crafting | Craft items from shells and ocean materials |

### Fletching

| Plugin | Description |
|---|---|
| Fletching Skill Toggle | Enable or disable the Fletching skill entirely |
| Bow Stringing | Attach bowstrings to unstrung bows |
| Arrow Making | Combine arrowheads with shafts and feathers |
| Bolt Making | Craft bolts from metal tips and shafts |
| Dart Making | Craft darts from metal tips and feathers |
| Javelin Making | Craft javelins from metal tips and shafts |
| Crossbow Making | Craft crossbows from limbs and stocks |
| Broad Ammunition | Craft broad arrows and bolts requiring slayer unlock |
| Shield Making | Craft wooden and metal shields |
| Ogre Arrows | Craft specialized arrows for ogre content |

### Construction

| Plugin | Description |
|---|---|
| Construction Skill Toggle | Enable or disable the Construction skill entirely |
| Flatpack Furniture | Build furniture as tradeable flatpack items |
| Room Building | Build rooms within player-owned houses |
| Furniture Hotspots | Designated spots within rooms for furniture placement |
| Butler Service | Hire NPC butlers to fetch materials from the bank |
| Garden Building | Build and customize outdoor garden spaces |
| Dungeon Building | Build underground dungeon rooms with traps and monsters |
| Trophy Display | Display boss heads and achievement trophies |
| Portal Chambers | Build portals for teleportation to various locations |
| Costume Room | Dedicated storage for cosmetic and holiday items |

### Invention

| Plugin | Description |
|---|---|
| Invention Skill Toggle | Enable or disable the Invention skill entirely |
| Augmenting | Attach augmentors to equipment for perk slots |
| Perks | Random perks applied to augmented equipment |
| Disassembly | Break down items into invention materials and components |
| Siphoning | Drain XP from augmented equipment without destroying it |
| Charge Pack | Central energy system powering all augmented items |
| Tech Trees | Unlock invention blueprints through research trees |
| Machines | Build automated machines that process items passively |

### Divination Crafting

| Plugin | Description |
|---|---|
| Divination Crafting | Use divination energy to transmute and create items |

---

## Skills -- Activity

### Agility

| Plugin | Description |
|---|---|
| Agility Skill Toggle | Enable or disable the Agility skill entirely |
| Rooftop Courses | Agility courses across city rooftops |
| Marks of Grace | Collect marks on rooftop courses for graceful outfit |
| Shortcuts | Unlock world shortcuts at agility level thresholds |
| Hallowed Sepulchre | High-level agility course with looting and obstacles |
| Agility Arena | Ticket-based agility minigame in Brimhaven |
| Anachronia Course | Large-scale agility course with timestamps |

### Prayer Training

| Plugin | Description |
|---|---|
| Prayer Skill Toggle | Enable or disable the Prayer skill entirely |
| Bone Burying | Bury bones for prayer XP |
| Altar Offering | Use bones on altars for multiplied prayer XP |
| Ectofuntus | Worship system requiring bonemeal and slime |
| Ensouled Heads | Reanimate monster heads for prayer XP through combat |

### Dungeoneering

| Plugin | Description |
|---|---|
| Dungeoneering Skill Toggle | Enable or disable the Dungeoneering skill entirely |
| Floor Generation | Procedurally generated dungeon floors |
| Key/Door Puzzles | Doors requiring keys found throughout the dungeon |
| Boss Encounters | Unique boss fights at the end of each dungeon floor |
| Token Rewards | Earn tokens to spend on dungeoneering reward items |

### Sailing

| Plugin | Description |
|---|---|
| Sailing Skill Toggle | Enable or disable the Sailing skill entirely |
| Ship Building | Construct and upgrade ships for ocean exploration |
| Navigation | Chart courses across the ocean map |
| Island Discovery | Discover procedurally generated or hidden islands |
| Sea Encounters | Random events and dangers while sailing |
| Port Management | Manage a port with workers, ships, and trade routes |

### Socializing

| Plugin | Description |
|---|---|
| Socializing Skill Toggle | Enable or disable the Socializing skill entirely |
| NPC Reputation | Build reputation with NPCs through interaction |
| Trade Negotiation | Skill checks when negotiating with NPC merchants |
| Event Hosting | Gain XP by hosting and participating in social events |

### Bank Standing

| Plugin | Description |
|---|---|
| Bank Standing Skill Toggle | Enable or disable Bank Standing XP tracking |
| Idle Processing | Process items while standing at the bank for XP |
| Social Banking | Bonus XP for bank standing near other players |

### Pet Raising

| Plugin | Description |
|---|---|
| Pet Raising Skill Toggle | Enable or disable Pet Raising as a trainable skill |
| Pet Feeding | Feed pets to maintain happiness and growth |
| Pet Training | Train pets to learn abilities and improve stats |
| Pet Breeding | Breed pets to create offspring with inherited traits |
| Pet Shows | Enter pets in competitions for rewards and ranking |

---

## Skills -- Inspired Fantasy

### Fantasy Combat (10)

| Plugin | Description |
|---|---|
| Blood Magic | Combat skill using health as a resource for powerful attacks |
| Shapeshifting | Transform into creatures with unique combat abilities |
| Wardening | Protective magic focused on shields, barriers, and wards |
| Beast Mastery | Tame and command wild creatures in combat |
| Runic Combat | Inscribe runes on the battlefield for area effects |
| Shadow Arts | Stealth-based combat with invisibility and ambush mechanics |
| Elemental Fury | Channel raw elements for devastating area attacks |
| Spirit Binding | Bind defeated spirits to fight alongside you |
| Berserking | Sacrifice defence for escalating damage in prolonged fights |
| Oath Magic | Swear magical oaths that grant power with binding restrictions |

### Fantasy Gathering (12)

| Plugin | Description |
|---|---|
| Herbalism | Gather magical herbs from enchanted groves and ley lines |
| Prospecting | Detect and extract rare magical minerals from the earth |
| Foraging | Gather wild food, mushrooms, and alchemical ingredients |
| Beast Tracking | Track mythical creatures to harvest rare materials |
| Deep Mining | Mine in procedurally generated deep caverns with hazards |
| Stargazing | Gather celestial energy during astronomical events |
| Root Pulling | Extract magical roots from ancient trees |
| Coral Harvesting | Gather underwater coral with magical properties |
| Essence Siphoning | Extract magical essence from ley line nodes |
| Fossil Hunting | Excavate and reconstruct ancient creature fossils |
| Mushroom Cultivation | Grow and harvest magical fungi in dark environments |
| Crystal Singing | Sing crystals into growth at elven crystal formations |

### Fantasy Processing (12)

| Plugin | Description |
|---|---|
| Alchemy | Transform base materials into potions, elixirs, and transmuted metals |
| Inscription | Write magical scrolls and glyphs with spell effects |
| Tanning (Advanced) | Process monster hides into exotic magical leathers |
| Milling | Grind grains and herbs into flour and powders |
| Distilling | Distill potions and spirits for concentrated effects |
| Dyeing | Create and apply magical dyes to equipment and cosmetics |
| Essence Refining | Purify raw magical essence into usable energy |
| Soul Forging | Process captured souls into power sources for enchantments |
| Rune Pressing | Press raw rune stones into usable magical runes |
| Bone Carving | Carve bones into tools, weapons, and ritual items |
| Wax Working | Process beeswax into candles, seals, and ritual items |
| Parchment Making | Create magical parchment for inscription and scrolls |

### Fantasy Combining (18)

| Plugin | Description |
|---|---|
| Enchanting (Advanced) | Apply complex magical effects to weapons and armor |
| Runesmithing | Forge rune-infused weapons with magical properties |
| Wand Crafting | Craft wands and staves from magical wood and gems |
| Golem Building | Construct animated golems from clay, stone, or metal |
| Sigil Crafting | Create protective and offensive sigils for placement |
| Scroll Binding | Bind multiple spell scrolls into spellbooks |
| Totem Carving | Carve totems that provide area buffs when placed |
| Artificing | Create magical artifacts with unique properties |
| Puppet Making | Craft enchanted puppets that perform simple tasks |
| Trap Crafting | Build magical and mechanical traps for combat and hunting |
| Ship Building (Advanced) | Construct magical ships with enchanted components |
| Instrument Making | Craft musical instruments with magical resonance |
| Clockwork Assembly | Assemble clockwork devices with mechanical functions |
| Mask Making | Craft masks that grant temporary transformations |
| Map Making | Create magical maps that reveal hidden areas |
| Brew Mastery | Combine advanced brewing with magical ingredients |
| Jewel Fusion | Fuse multiple gems into powerful combined stones |
| Saddle Crafting | Craft saddles and barding for mountable creatures |

### Fantasy Activity (23)

| Plugin | Description |
|---|---|
| Taming | Tame wild creatures for mounts, pets, and companions |
| Performing | Play instruments and perform for crowds to earn tips and fame |
| Meditation | Enter deep focus states for passive XP and stat recovery |
| Cartography | Map unexplored regions for XP and tradeable maps |
| Diplomacy | Negotiate with NPC factions for quest and trade advantages |
| Archaeology (Advanced) | Excavate ancient ruins for powerful relics and lore |
| Climbing | Scale cliffs, towers, and mountains for shortcuts and resources |
| Swimming | Navigate underwater areas and discover submerged content |
| Riding | Train mount riding for speed, combat mounts, and racing |
| Falconry | Train birds of prey for hunting and scouting |
| Cooking (Gourmet) | Prepare elaborate meals with multi-course stat buffs |
| Gardening | Maintain decorative and functional gardens with unique yields |
| Astronomy | Study stars for navigation bonuses and celestial events |
| Lore Mastery | Study ancient texts to unlock hidden knowledge and bonuses |
| Monster Research | Study monsters to discover weaknesses and rare drops |
| Treasure Hunting | Follow clue trails to buried treasure across the world |
| Arena Fighting | Compete in structured arena combat for ranking and rewards |
| Gambling Mastery | Improved odds and access to exclusive games of chance |
| Teaching | Train other players for shared XP bonuses |
| Exploration | Gain XP for discovering new areas and landmarks |
| Siege Warfare | Participate in large-scale siege battles with war machines |
| Weather Reading | Predict weather patterns for gathering and travel bonuses |
| Dream Walking | Enter a dream realm for unique challenges and rewards |

---

## Skills -- Inspired Futuristic

### Futuristic Combat (8)

| Plugin | Description |
|---|---|
| Psionics | Mental combat using telekinesis, mind control, and psychic blasts |
| Cybernetics | Install cybernetic augmentations for combat enhancements |
| Energy Weapons | Construct and wield laser, plasma, and particle weapons |
| Nanite Warfare | Deploy nanite swarms for healing, damage, and area control |
| Mech Piloting | Pilot mechanical suits with unique weapon systems |
| Drone Command | Deploy combat drones for autonomous or directed attacks |
| Temporal Combat | Manipulate time to slow enemies or accelerate self |
| Gravity Manipulation | Alter gravity fields for crowd control and movement |

### Futuristic Gathering (6)

| Plugin | Description |
|---|---|
| Data Mining | Extract valuable data from terminals and networks |
| Quantum Harvesting | Collect quantum particles from anomalies and rifts |
| Bio Sampling | Collect biological samples from organisms and environments |
| Scrap Salvaging | Salvage usable components from technological debris |
| Energy Tapping | Tap into power grids and reactors for energy resources |
| Signal Interception | Intercept and decode transmissions for intelligence |

### Futuristic Processing (6)

| Plugin | Description |
|---|---|
| Nano Fabrication | Process raw materials at the molecular level |
| Data Processing | Compile and analyze raw data into usable intelligence |
| Bio Engineering | Process biological samples into serums and modifications |
| Recycling | Break down scrap into refined component materials |
| Encryption | Process data into secured and tradeable encrypted packets |
| Chemical Synthesis | Synthesize complex chemicals from base compounds |

### Futuristic Combining (12)

| Plugin | Description |
|---|---|
| Robotics | Build autonomous robots for combat, gathering, and service |
| Circuit Design | Design and build electronic circuits for devices |
| Weapon Modding | Modify weapons with attachments, scopes, and upgrades |
| Armor Plating | Combine materials into advanced protective plating |
| Vehicle Assembly | Build ground, air, and water vehicles from components |
| Holographics | Create holographic projections for deception and utility |
| Prosthetics | Craft cybernetic limbs and organs for augmentation |
| AI Programming | Program AI companions and automated systems |
| Force Field Generation | Build portable force field devices and emitters |
| Terraforming Tools | Craft devices that reshape terrain and environment |
| Bioprinting | Print biological tissues and organs from bio-ink |
| Neural Interface Crafting | Build brain-computer interfaces for skill uploads |

### Futuristic Activity (10)

| Plugin | Description |
|---|---|
| Hacking | Breach security systems for access, loot, and intelligence |
| Piloting | Fly spacecraft and aircraft for exploration and combat |
| Zero-G Training | Train in zero gravity environments for space operations |
| Virtual Reality | Enter VR simulations for safe training and unique challenges |
| Terraforming | Reshape planetary terrain and ecosystems |
| Space Walking | EVA operations for repairs, exploration, and resource gathering |
| Genetic Splicing | Modify your character's DNA for stat alterations |
| Network Surfing | Navigate cyberspace for data, programs, and digital loot |
| Cryogenics | Manage cryogenic systems for time manipulation mechanics |
| Quantum Entanglement | Create linked objects for instant communication and teleportation |

---

## Quests

| Plugin | Description |
|---|---|
| Quest Points | Award quest points on completion for unlocking content |
| Boostable Requirements | Allow stat boosts to meet quest skill requirements |
| Repeatable Quests | Allow certain quests to be completed more than once |
| Subquests | Break large quests into independently completable subquests |
| Holiday Quests | Seasonal event quests with limited-time rewards |
| Quest Series/Chains | Group quests into multi-part story arcs |
| Step-by-Step Helper | Display quest guide overlay with current step highlighted |
| Map Markers | Show quest-related markers on the world map |
| Item Highlighting | Highlight quest-required items in inventory and world |
| Dialogue Helper | Highlight correct dialogue options during quest conversations |
| Puzzle Solver | Provide optional hints or solutions for quest puzzles |
| Shortest Path | Calculate optimal walking path to next quest objective |
| Item Lookup | Look up where to obtain quest-required items |
| Optimal Quest Order | Suggest efficient quest completion order based on requirements |
| TTS Fallback | Text-to-speech narration for quest dialogue |

---

## Monsters

| Plugin | Description |
|---|---|
| Types/Attributes | Monsters have types and attributes affecting combat interactions |
| Immunities | Monsters can be immune to specific damage types or effects |
| Slayer System | Certain monsters require a slayer assignment and level to kill |
| Aggression Mechanics | Monsters aggress players based on level difference and timer |
| Elemental Weaknesses | Monsters have elemental weaknesses that increase damage taken |
| Shared Drop Tables | Global drop tables shared across monster types |
| Drop Rate Broadcasting | Announce rare drops to the server |
| KC Tracking | Track kill count per monster type |
| Session Dashboard | Display kills, drops, and XP per session |
| Probability Calculator | Calculate expected kills for a specific drop |
| Bestiary Completion | Track which monsters have been encountered and killed |
| NPC Nicknames | Assign custom display names to NPCs |
| Reverse Drop Lookup | Search for which monsters drop a specific item |
| NPC Highlighting | Highlight specific NPCs with colored outlines |
| Aggression Timer | Display countdown until monster aggression expires |
| Opponent Info Panel | Show target monster stats, weaknesses, and drop info |

---

## Bosses and Raids

| Plugin | Description |
|---|---|
| Player Scaling | Boss difficulty scales with party size |
| Phase System | Bosses transition through multiple combat phases |
| Difficulty Modes | Multiple difficulty tiers per boss encounter |
| Enrage Timer | Boss damage increases the longer the fight lasts |
| MVP Loot | Player who dealt the most damage gets bonus loot |
| Damage Tracking | Track damage dealt by each party member |
| KC Hiscores | Leaderboard ranking by boss kill count |
| Instance System | Private boss instances for solo or group play |
| Skilling Bosses | Boss encounters that use skilling mechanics instead of combat |
| Death Mechanics | Configure item loss and respawn behavior on boss deaths |
| Respawn Timer | Configurable boss respawn cooldown |
| Healer Tracking | Track healing done by support players |
| KPH Tracking | Calculate kills per hour for efficiency tracking |
| Raid Randomizer | Randomize room order and boss selection in raids |
| Ready Check | Party-wide ready check before starting encounters |
| Party Connection Status | Display ping and connection status for party members |
| Mistake Tracker | Log avoidable damage and mechanical failures |
| Special Attack Counter | Track special attack usage and timing |

---

## Items

| Plugin | Description |
|---|---|
| Stackability | Configure which items stack in inventory |
| Noting | Convert items to noted form for bulk storage and trading |
| Grand Exchange | Centralized auction house for buying and selling items |
| Alchemy | Convert items to gold using magic spells |
| Weight | Items have weight values affecting run energy |
| Combo Eating | Eat multiple food types in a single tick |
| Potion Doses | Potions contain configurable number of doses |
| Potion Decay | Potion effects wear off over time |
| Barbarian Mixes | Combine potions with fish for dual-purpose consumables |
| Divine Potions | Potions that drain a resource while maintaining a stat boost |
| Despawn Timers | Dropped items despawn after a configurable duration |
| Disassembly | Break items into invention components |
| Augmentation | Attach augmentors to items for perk systems |
| Containers | Items that hold other items like looting bags and seed boxes |
| Teleport Charges | Items with limited teleport uses |
| Multiple Currencies | Support for gold, tokens, points, and custom currencies |
| Loot Filters | Filter ground items by value, type, or custom rules |
| Item Locking | Lock items to prevent accidental drop, trade, or alch |
| Activity Checklists | Track which activities have been completed for item acquisition |
| Source Lookup | Look up where to obtain any item |
| Data Export | Export item data to external tools |
| Drop Sounds | Play custom sounds when specific items drop |
| Ground Item Organization | Organize ground item display by value and type |
| Cooldown Visualization | Display cooldown timers on items with use restrictions |
| Per-Item Sounds | Assign custom sounds to specific items |
| Examine System | Display item descriptions, stats, and lore on examine |
| Charge Tracking | Track remaining charges on degradable and charged items |
| ID Overlay | Display item IDs as an overlay for debugging |
| Stat Tooltips | Show stat bonuses and effects on item hover |

---

## NPCs

| Plugin | Description |
|---|---|
| Schedules | NPCs follow daily schedules, moving between locations |
| Wander | NPCs wander within defined areas when idle |
| Services | NPCs provide services like banking, healing, and trading |
| Faction/Reputation | NPCs belong to factions with reputation-based interactions |
| Random Events | NPCs trigger random events for players |
| Walk-Through | NPCs can be walked through rather than blocking movement |
| Block Reset | Prevent NPC position reset when interacting |
| Transport | NPCs provide transportation services between locations |
| Guards | NPCs that attack skulled players or enforce area rules |
| Musicians | NPCs that restore run energy when players rest nearby |
| Custom Types | Define custom NPC types with configurable behavior |

---

## Shops

| Plugin | Description |
|---|---|
| General Store | Default shop that buys and sells common items |
| Specialty Shop | Shops that sell specific item categories |
| Guild Shop | Shops only accessible to guild members |
| Quest-Locked Shop | Shops that require quest completion to access |
| Minigame Currency Shop | Shops that accept minigame-specific currencies |
| Traveling Merchant | Merchant that appears at random locations with rotating stock |
| Black Market | Hidden shop with rare or restricted items at premium prices |
| Dynamic Pricing | Prices adjust based on supply and demand |
| Per-World Stock | Shop stock is shared across all players on a world |
| Per-Account Stock | Shop stock is tracked individually per player account |
| Ironman Stock | Separate stock pools for ironman accounts |
| Buy-Back | Option to repurchase recently sold items |
| Shop Runs | Optimized routes for buying daily shop stock |
| Bulk Discount | Price reduction when purchasing large quantities |
| Price Comparison | Compare prices across multiple shops |
| Infinite Stock | Configure shops with unlimited item quantities |
| Hop Prevention | Rate limiting on world-hopping for shop stock |
| Player-Fed Shop | Shops stocked by player-sold items |
| Clan Stores | Shops accessible only to clan members with clan-funded stock |
| Custom Types | Define custom shop types with configurable rules |

---

## Terrain

| Plugin | Description |
|---|---|
| Paradigm Selection | Choose between top-down 2D, isometric, or 3D terrain rendering |
| Custom Tiles | Define custom tile types with unique textures and properties |
| Variants | Multiple texture variants per tile type for visual variety |
| Tinting | Apply color tints to tiles for biome and mood effects |
| Elevation | Tiles support configurable height levels |
| Slopes | Smooth transitions between different elevation levels |
| Fall Damage | Players take damage when falling from elevated terrain |
| Jump | Players can jump across gaps or to lower elevations |
| Biomes | Group tiles into biome regions with shared properties |
| Biome Transitions | Smooth visual blending between adjacent biomes |
| Procedural Generation | Algorithmically generate terrain layouts |
| Import/Export | Import and export terrain data as shareable files |
| Chunks | Divide the world into loadable chunk regions |
| Layers | Support multiple vertical layers for multi-level maps |
| Speed Modifiers | Certain terrain types slow or speed player movement |
| Environmental Effects | Terrain triggers visual and gameplay effects like rain or heat |
| Destructible Terrain | Terrain that can be damaged or destroyed by players |
| Growth/Change | Terrain evolves over time based on player activity or seasons |
| Tile Walkability | Per-tile configuration of whether players can walk on it |
| Edge Walkability | Per-edge configuration of movement blocking on tile borders |

---

## Buildings

| Plugin | Description |
|---|---|
| Edge Walls | Walls placed on tile edges rather than occupying full tiles |
| Wall Types | Multiple wall materials with different properties |
| Wall Decoration | Attach decorative items to wall surfaces |
| Windows | Wall openings that allow light and line of sight |
| Door Locking | Doors can be locked and require keys or levels to open |
| Door Types | Multiple door styles with different behaviors |
| Room Detection | Automatically detect enclosed rooms from wall placement |
| Room Types | Assign room types that provide specific functions |
| Room Instancing | Rooms can instance separately from the world |
| Room Capacity | Limit the number of players in a room |
| Ceilings | Enclosed rooms have ceilings blocking rain and flight |
| Roofs | Visible roof rendering on buildings |
| Lighting | Light sources illuminate rooms and areas |
| Darkness | Unlit areas are dark and may require light sources |
| Light Fuel | Light sources consume fuel and burn out over time |
| Floor Override | Override terrain tiles inside buildings with custom floors |
| Templates/Blueprints | Save and load building designs as reusable templates |
| Breakable Walls | Walls that can be damaged and destroyed |
| Climbable Walls | Walls that can be climbed with sufficient agility |
| Line-of-Sight Blocking | Walls block line of sight for combat and visibility |
| Sound Blocking | Walls muffle or block sound propagation |
| Projectile Blocking | Walls block ranged attacks and projectiles |
| Secret Doors | Hidden doors disguised as normal walls |

---

## Locations

| Plugin | Description |
|---|---|
| Nested Locations | Locations can contain sub-locations with inherited properties |
| Environmental Hazards | Locations contain damaging effects like lava, poison, or cold |
| Mitigation | Equipment or skills that reduce environmental hazard damage |
| Weather | Dynamic weather systems affecting gameplay and visuals |
| Darkness | Locations can be dark, requiring light sources to navigate |
| Underwater | Submerged areas with breath timers and movement changes |
| Run Energy Modifier | Locations modify run energy drain rates |
| Prayer Drain Modifier | Locations modify prayer point drain rates |
| Music | Locations trigger specific music tracks on entry |
| Teleport Restrictions | Certain locations block or restrict teleportation |
| Custom Rules | Per-location custom gameplay rule overrides |
| Waypoints | Player-placed markers for navigation |
| Distance Tool | Measure tile distances between two points |
| Day/Night Cycle | Time-based lighting changes across the world |
| GPS | Coordinate display and navigation system |
| Tile Marker Profiles | Save and switch between sets of tile markers |
| Tile Labels | Add text labels to marked tiles |
| Teleport Analytics | Track teleport usage frequency and destinations |
| Dynamic Weather | Weather patterns change based on time and location |
| Instance Minimap | Separate minimap rendering for instanced areas |
| Object Highlighting | Highlight interactable objects in the world |
| Lots | Subdivide land into ownable and buildable lots |
| Chunk Streaming | Load and unload world chunks based on player proximity |

---

## Tick System

| Plugin | Description |
|---|---|
| Tick Manipulation | Enable tick manipulation as a core game mechanic |
| Per-Skill Manipulation | Configure tick manipulation availability per skill |
| Flinch Mechanics | NPCs have a flinch timer exploitable for safe damage |
| Movement Manipulation | Manipulate movement ticks for prayer or attack timing |
| Tick Eating | Eat food on specific ticks to avoid death |
| Combo Eating | Combine food and potions on the same tick |
| Prayer Flicking | Toggle prayers on/off each tick to avoid point drain |
| PID | Player ID determines action priority in same-tick conflicts |
| Skull System | Attacking other players flags you for full loot on death |
| PJ Timer | Cooldown preventing third parties from interrupting PvP fights |
| Freeze Immunity | Brief immunity to freeze effects after one expires |
| Custom Tick Rate | Configure the server tick duration in milliseconds |
| Auto-Manipulation | Automated tick manipulation for accessibility |
| Line-of-Sight Visualization | Display line-of-sight tiles during combat |
| Combat Roll Transparency | Show hit chance and damage roll calculations |
| Practice Mode | Safe environment to practice tick manipulation techniques |
| Replay Logger | Record and replay tick-perfect action sequences |
| Priority Queue | Configurable action priority when multiple actions queue |
| Action Progress Bar | Visual progress bar showing time until next action tick |

---

## Achievements

| Plugin | Description |
|---|---|
| Achievement Points | Earn points for completing achievements |
| Server Firsts | Special recognition for first player to complete an achievement |
| Category Leaderboards | Ranked leaderboards per achievement category |
| Custom Achievements | DM-defined custom achievements with custom criteria |
| Achievement Tiers | Bronze, silver, gold, and elite tiers per achievement |
| Hidden Achievements | Secret achievements revealed only on completion |
| Time-Gated Achievements | Achievements only earnable during specific time windows |
| Feats | One-time extraordinary accomplishments with unique rewards |
| Completionist | Meta-achievement for completing all achievements |
| Personal Goals | Player-defined custom goals with tracking |
| Speedrun Splitter | Track segment times for speedrun attempts |
| Custom Milestones | DM-defined milestone markers for server-specific goals |

---

## Collection Log

| Plugin | Description |
|---|---|
| KC Display | Show kill count alongside collection log entries |
| Duplicates | Track duplicate drops beyond the first |
| Count Cap | Configure maximum tracked count per item |
| Milestone Rewards | Award prizes at collection completion milestones |
| Broadcast | Announce new collection log entries to the server |
| Dry Streak Tracking | Track consecutive kills without a target drop |
| Luck Calculator | Statistical analysis of drop luck versus expected rate |
| Bestiary Completion | Track monster encounters as part of the collection |
| Examine Encyclopedia | Comprehensive item encyclopedia built from examined items |

---

## Minigames

| Plugin | Description |
|---|---|
| Safe Deaths | Deaths in the minigame do not result in item loss |
| Dangerous Deaths | Deaths in the minigame follow normal death mechanics |
| Staking | Wager items or currency on minigame outcomes |
| Reward Currencies | Minigames award their own currency for reward shops |
| Reward Shops | Dedicated shops for spending minigame currencies |
| Leaderboards | Ranked scoreboards for minigame performance |
| Roles | Assign player roles within team-based minigames |
| Matchmaking | Automated team balancing and opponent matching |
| Spectating | Watch ongoing minigame matches as a spectator |
| Practice Mode | Play minigames without rewards for learning |
| Difficulty Scaling | Minigame difficulty adjusts based on participants |
| Power-ups | Temporary boosts that spawn during minigame play |
| Activity Check | Kick idle players from active minigame sessions |
| Passive/Idle Minigames | Minigames that progress while the player is idle |
| Prop Hunt | Hide-and-seek minigame where players disguise as objects |
| Collectible Card Game | In-game trading card game with collectible cards |
| Ready Check | Party-wide confirmation before starting a minigame |

---

## Puzzles

| Plugin | Description |
|---|---|
| Puzzle Timer | Track completion time for puzzle attempts |
| Hints | Optional hint system with escalating specificity |
| Fail Conditions | Puzzles can fail and require restart under certain conditions |
| Randomization | Puzzle solutions randomize per attempt or per player |
| Puzzle Leaderboards | Ranked completion times for competitive puzzles |
| Practice Mode | Attempt puzzles without quest progress for practice |
| Spectating | Watch other players attempt puzzles |
| Puzzle Builder | DM tool for creating custom puzzle content |

---

## Dailies

| Plugin | Description |
|---|---|
| Daily Reset | Activities that reset once per day |
| Weekly Reset | Activities that reset once per week |
| Monthly Reset | Activities that reset once per month |
| Custom Reset Intervals | Define custom reset periods for any activity |
| Streaks | Track consecutive daily completion streaks |
| Streak Bonuses | Award bonus rewards for maintaining streaks |
| Shop Run Tracking | Track daily shop buying routes and completion |
| Passive Accumulation | Resources accumulate passively over time for collection |
| Distractions and Diversions | Periodic random world activities with unique rewards |
| Profit Tracking | Calculate daily profit from repeated activities |
| Timer Display | Show countdown timers until next reset |
| Notifications | Alert players when dailies are available |
| Priority Sorting | Sort dailies by priority, value, or completion status |
| Achievement Scaling | Daily rewards scale with achievement progress |
| Random World Events | Unscheduled server-wide events with participation rewards |
| Login Streaks | Track consecutive login days for escalating rewards |

---

## Games of Chance

| Plugin | Description |
|---|---|
| Master Toggle | Enable or disable all games of chance at once |
| House Edge Config | Configure the house edge percentage for each game |
| Min/Max Bet | Set minimum and maximum wager limits |
| Anti-Addiction Timer | Warn or restrict play after extended gambling sessions |
| Payout Multiplier | Configure payout ratios per game |
| Loss Streak Protection | Reduce losses or increase odds after consecutive losses |
| Win Cap | Maximum winnings per session or per day |
| Lucky Charm Items | Equipment that modifies gambling odds |
| Gambler Rank | Prestige ranks earned through gambling volume |
| Statistics Dashboard | Track personal win/loss history and statistics |
| Spectator Mode | Watch other players gamble without participating |
| Tournament Mode | Scheduled gambling tournaments with entry fees and prize pools |
| Leaderboard | Rankings by total winnings or tournament performance |
| Rigged Detection | Anti-cheat system for detecting manipulation |
| Sound Effects | Custom audio for wins, losses, and jackpots |
| Particle Effects | Visual effects for big wins and jackpots |
| Cooldown Timer | Mandatory wait between gambling sessions |
| Dice Game | Classic dice rolling with configurable rules |
| Coin Flip | Simple heads-or-tails wager |
| Card Games | Poker, blackjack, and other card-based games |
| Slot Machine | Slot machine with configurable symbols and paylines |
| Roulette | Roulette wheel with number and color betting |
| Wheel of Fortune | Spinning wheel with weighted prize segments |
| Scratch Cards | Purchasable scratch-off cards with hidden prizes |
| Treasure Chest Gamble | Open random chests for a chance at valuable items |
| Rock Paper Scissors | Player-versus-player or player-versus-NPC wager game |
| Number Guessing | Guess a random number within a range |
| Horse Racing | Bet on NPC horse races with randomized outcomes |
| Monster Fight Betting | Bet on outcomes of NPC monster fights |
| Fishing Derby | Competitive fishing with wagers on largest catch |
| Arena Betting | Bet on player or NPC arena combat outcomes |
| Lottery | Periodic lottery drawings with pooled prize funds |
| Raffle | Ticket-based raffle drawings for specific prizes |
| Bingo | Multiplayer bingo with randomized boards |
| Trivia | Answer trivia questions for prizes |
| Auction Gamble | Blind auction where highest bidder wins a mystery item |
| Loot Box Opening | Purchase and open loot boxes with random contents |
| Gacha Pull | Gacha-style random pulls with pity systems |
| Prediction Market | Bet on real game events like boss kill times |
| Dig Site Gamble | Pay to dig at random spots for buried items |
| Darts | Skill-based dart throwing with wagers |
| Shell Game | Track a hidden item under shuffled shells |
| Claw Machine | Skill-timing game with random prize selection |
| Cooking Roulette | Cook a random dish with variable quality outcomes |
| Puzzle Race Betting | Bet on who completes a puzzle fastest |
| Skill Challenge Wager | Wager on completing a skill challenge within a time limit |
| Memory Match | Card matching game with hidden pairs |
| Mining Jackpot | Random jackpot chance while mining specific rocks |
| Fishing Jackpot | Random jackpot chance while fishing specific spots |
| Woodcutting Jackpot | Random jackpot chance while cutting specific trees |
| Thieving Jackpot | Random jackpot chance while pickpocketing |
| Slayer Jackpot | Random jackpot chance on slayer task kills |
| Drop Simulator | Simulate drop tables to preview expected loot without killing |

---

## Account Management

| Plugin | Description |
|---|---|
| Profile | Player profile page with customizable display |
| Bio/Avatar | Custom biography text and avatar image |
| Titles | Unlockable and selectable display titles |
| Two-Factor Authentication | TOTP-based two-factor login security |
| Bank PIN | Secondary PIN required to access the bank |
| Trusted Devices | Remember devices to skip 2FA on repeat logins |
| Session Management | View and revoke active login sessions |
| Privacy Settings | Control visibility of stats, location, and online status |
| Friend Groups | Organize friends into named groups |
| Activity Feed | Timeline of recent player activities and achievements |
| Referral System | Invite players for referral bonuses |
| Data Export | Export account data in standard formats |
| Account Deletion | Permanently delete account and all associated data |
| Account Merging | Merge two accounts into one |
| Keybind Customization | Rebind all keyboard and mouse controls |
| Accessibility Options | Color blind modes, font scaling, screen reader support |
| Save States | Save and restore account states for testing or rollback |
| Rollback Protection | Ability to roll back recent changes on request |
| Wellness Reminders | Periodic reminders to take breaks and hydrate |
| Teleport Protection | Confirmation prompt before using teleports |
| Per-Zone Audio | Volume controls that change based on current location |
| APM Counter | Track actions per minute during gameplay |
| Combat Warnings | Alert when entering dangerous combat situations |
| AFK Timer | Automatic logout or notification after idle period |
| Session Limits | Maximum playtime per session with enforced breaks |
| Pronouns | Configurable character pronouns for NPC dialogue |
| Cloud Save | Sync account data to cloud backup |
| Texture Packs | Swap texture sets for different visual themes |
| Multi-Panel Layout | Arrange multiple UI panels on screen simultaneously |
| Push Notifications | Mobile push notifications for in-game events |
| Auto-Screenshot | Automatically capture screenshots on achievements and drops |
| Focus Mute | Mute chat and notifications during boss fights |
| Keybind Profiles | Save and switch between keybind configurations |
| Action Log | Detailed log of all player actions for review |
| Custom Sounds | Replace default sounds with custom audio files |
| Idle Notification | Alert when the player has been idle too long |
| Logout Timer | Display countdown to auto-logout |
| Menu Reordering | Rearrange menu items and interface elements |
| Screen Markers | Place persistent markers on the game screen |
| World Switcher | Quick interface for changing game worlds |
| UI Theming | Customize colors, fonts, and styling of the interface |
| Panel Clone | Duplicate UI panels for multi-monitor setups |
| Settings Registry | Central registry tracking all active plugin settings |

---

## Character Overview

| Plugin | Description |
|---|---|
| Skills Summary | Overview of all skill levels and XP |
| Combat Summary | Combat stats, accuracy, and max hit calculations |
| Boss Log | Personal boss kill counts and best times |
| Collection Log | Summary of collection log completion percentage |
| Quest Tracker | Active and completed quest progress |
| Achievement Tracker | Achievement points and completion progress |
| Wealth Display | Total wealth calculation across bank, inventory, and equipment |
| Social Stats | Friend count, clan rank, and social activity metrics |
| Activity Heatmap | Visual heatmap of gameplay activity over time |
| Milestones | Timeline of major account milestones |
| Comparison Mode | Compare stats side-by-side with another player |
| XP Goals | Set and track XP targets per skill |
| BIS Comparison | Compare current gear to best-in-slot for an activity |
| Dry Streak Display | Display current dry streaks for tracked drops |
| Wealth Over Time | Graph of total wealth changes over time |
| Shareable Profile | Generate a shareable link or image of your profile |
| Damage Log | Detailed log of damage dealt and received |
| Session Metrics | Summary statistics for the current play session |
| Notes | Personal notes attached to the character profile |
| Effective Levels | Display effective levels after boosts and equipment |
| Activity Recommender | Suggest activities based on current stats and goals |
| Stat Graphs | Graphical visualizations of stat progression |
| Click Analytics | Track click patterns and efficiency metrics |
| Pedometer | Track total tiles walked and distance traveled |
| Character Data Export | Export full character data for external analysis |
| DPS Meter | Real-time damage-per-second display during combat |
| Regen Meter | Display hitpoint and prayer regeneration timers |
| Poison Tracker | Show poison damage ticks and remaining duration |
| Buff/Debuff Bar | Display active buffs and debuffs with durations |
| XP Globes | Floating XP indicators showing recent gains per skill |
| Character Creation | Character appearance and name creation system |

---

## Inventory and Bank

| Plugin | Description |
|---|---|
| Weight Limit | Inventory has a maximum carry weight |
| Stack Rules | Configure which items stack and stack size limits |
| Inventory Presets | Save and load inventory configurations |
| Shift-Drop | Hold shift to drop items quickly |
| Bank Tabs | Organize bank into named and sortable tabs |
| Placeholders | Empty placeholders remain when withdrawing items |
| Bank Presets | Save and load full equipment and inventory from bank |
| Value Display | Show item values in bank and inventory |
| Bank PIN | PIN required to access the bank interface |
| Separate Per Mode | Isolated banks for different game modes |
| Shared Bank | Shared bank accessible across characters on the same account |
| Space Expansion | Increase bank capacity through upgrades or purchases |
| Item Lock | Lock items in bank to prevent withdrawal or removal |
| Changelog | Log of all bank deposits and withdrawals |
| Heatmap | Visual heatmap of most-accessed bank slots |
| Fuzzy Search | Search bank contents with approximate text matching |
| Snapshot Sharing | Share a snapshot of bank contents with other players |
| Banked XP | Calculate total XP stored as bankable resources |
| Item Locator | Search for items across bank, inventory, and equipment |
| Inventory Data Export | Export inventory and bank contents to external tools |
| Junk Detector | Flag low-value or obsolete items for cleanup |
| Item Tagging | Tag items with custom labels for organization |
| Color Tags | Color-code items and bank tabs for visual sorting |
| Container Overlay | Show contents of containers without opening them |
| Bucket Fill Undo | Undo accidental bulk operations in bank |

---

## Death System

| Plugin | Description |
|---|---|
| Safe Deaths | Deaths in certain areas do not result in item loss |
| Dangerous Deaths | Deaths result in item loss based on configured rules |
| Protect Item Prayer | Prayer that protects one additional item on death |
| Gravestone | Items drop to a gravestone reclaimable within a time limit |
| Death Office | NPC where lost items can be reclaimed for a fee |
| Death Cost | Configurable fee to reclaim items on death |
| Free Death Periods | Temporary periods where deaths carry no penalty |
| Respawn Choice | Choose respawn location after death |
| PvP Loot | Items dropped on PvP death are lootable by the killer |
| Loot Keys | Condense PvP loot into a single key item |
| Smite Prayer | Drain opponent prayer to prevent protect item |
| Hardcore Death | Permanent consequences for death in hardcore mode |
| Skull on Death | Skulled players lose all items on death |

---

## Player Housing

| Plugin | Description |
|---|---|
| Master Toggle | Enable or disable the player housing system |
| Room System | Build and arrange rooms within a player-owned house |
| Furniture Tiers | Furniture quality tiers requiring construction levels |
| Portal Room | Room with portals for teleportation to locations |
| Trophy Room | Display boss trophies, capes, and achievement rewards |
| Storage Rooms | Dedicated rooms for storing items, costumes, and tools |
| Butler/Servant | Hire NPC servants to assist with construction and banking |
| Visitor System | Allow other players to visit your house |
| House Rating | Community rating system for player houses |
| PvP Room | Combat area within the house for dueling |
| Dungeon Builder | Build custom dungeon floors with traps and monsters |
| Flatpack Trading | Trade pre-built furniture as flatpack items |
| Housing Contracts | Assignable construction contracts for other players |
| House Themes | Apply visual themes to the entire house |
| Pet Room | Dedicated room for housing and displaying pets |
| Party Room | Room with balloon drops and celebration features |
| Music Room | Room for playing and listening to unlocked music tracks |
| Diagonal Half-Paint | Paint walls with diagonal split between two colors |

---

## Clan System

| Plugin | Description |
|---|---|
| Clan Ranks | Hierarchical rank system with configurable permissions |
| Clan Hall/Citadel | Shared clan space with upgradeable structures |
| Clan Coffer | Shared gold fund for clan expenses and events |
| Clan Bank | Shared item storage accessible by permitted members |
| Clan Tax | Automatic percentage taken from member earnings |
| Clan Events | Organized clan activities with scheduling and tracking |
| Clan Achievements | Clan-wide achievements earned through collective effort |
| Clan Avatar | Shared clan avatar that provides nearby XP bonuses |
| Clan Wars | Organized PvP battles between clans |
| Alliance System | Form alliances between multiple clans |
| Multi-Clan Membership | Players can belong to multiple clans simultaneously |
| Clan Broadcasting | Announce clan achievements and drops to all members |
| Clan Polls | Internal voting system for clan decisions |
| Ranked Wars | Competitive ranked clan battles with ELO ratings |
| Clan Salary | Automated salary payments to members from the coffer |
| Auto-Rank | Automatic rank promotion based on configurable criteria |
| Clan Territory | Clans can claim and control world territory |
| Guild Finder | Matchmaking system for players seeking clans |
| Sub-Groups | Create sub-groups within the clan for teams and squads |
| Clan Progression | Clan-wide leveling system with milestone unlocks |
| Contribution Tracking | Track individual member contributions to the clan |
| Guild Crafting | Collaborative crafting projects requiring multiple members |
| Item Lending | Lend items to clan members with automatic return |
| Clan Donations | Members donate items or gold to the clan fund |
| Proximity XP Bonus | Bonus XP when training near clan members |
| Combat Analytics | Detailed combat statistics for clan PvP events |
| Clan Data Export | Export clan statistics and history |
| Shared Goals | Clan-wide goals with progress tracking |
| Attendance Tracking | Track member attendance at clan events |
| Shared Items | Clan-owned items accessible by permitted members |
| Shared Map Markers | Clan-wide map markers visible to all members |
| Loot Broadcast | Announce member rare drops to the clan chat |

---

## Communication

| Plugin | Description |
|---|---|
| Public Chat | Area-based chat visible to nearby players |
| Global Chat | Server-wide chat channel |
| Trade Chat | Dedicated channel for buying and selling |
| Help Chat | Dedicated channel for asking and answering questions |
| Looking for Group | Channel for finding parties and groups |
| Custom Channels | Create custom named chat channels |
| Quick Chat | Pre-built chat messages selectable from a menu |
| Quick Chat Only Mode | Restrict players to quick chat messages only |
| Voice Chat | Real-time voice communication |
| Proximity Voice Chat | Voice chat audible only to nearby players |
| Proxy Voice Chat | Voice chat relayed through in-game objects |
| Voice Modulation | Modify voice pitch and effects in voice chat |
| Text-to-Speech | Convert text chat to spoken audio |
| Speech-to-Text | Convert voice chat to text display |
| Emojis | In-game emoji reactions in chat |
| GIFs | Animated GIF support in chat |
| Emotes | Character animations triggered by chat commands |
| Built-in Bots | Server-provided automated chat bots |
| Custom Bots | Player-created chat bots with scripting API |
| Player Bots | Bots that simulate player behavior for testing |
| @Mentions | Tag specific players in chat with notifications |
| Message Editing | Edit sent chat messages |
| Message Deletion | Delete sent chat messages |
| Threading | Reply threads on specific messages |
| Chat Bubbles | Display chat text as speech bubbles above characters |
| Rich Embeds | Embed formatted content blocks in chat |
| Link Previews | Preview linked content inline in chat |
| Chat Export | Export chat history to file |
| Cross-World Chat | Chat across different game worlds |
| Auto-Moderation | Automated chat filtering and moderation |
| Slow Mode | Rate limiting on chat messages per channel |
| Shadow Mute | Mute players without their knowledge |
| No Filter Mode | Option to disable chat filters for mature servers |
| Translation | Automatic translation between languages |
| Custom Emoji Packs | Upload and use custom emoji sets |
| Item Linking | Link items in chat to display their stats |
| Audio Notifications | Sound alerts for chat mentions and messages |
| Spatial Voice Chat | 3D positional audio for voice chat |
| Tactical Pings | Ping the game world with contextual markers |
| Rich Interactive Messages | Chat messages with clickable buttons and forms |
| External Bridges | Bridge chat to external platforms like Discord |
| Notification Center | Centralized panel for all notifications |
| Drawing Tool | Draw diagrams and sketches in chat |
| AI Assistant | AI-powered chat assistant for help and information |
| Streaming Integration | Stream gameplay with chat overlay |
| Transcript Export | Export full chat transcripts with timestamps |
| Clipboard Sharing | Share clipboard contents in chat |
| Slash Commands | Execute game commands via chat slash syntax |

---

## Friends and Social

| Plugin | Description |
|---|---|
| Friend Groups | Organize friends into named categories |
| Activity Feed | See recent activities of friends |
| Referral System | Earn rewards for inviting new players |
| Player Profiles | View detailed profiles of other players |
| Player Search | Search for players by name, stats, or activity |
| Presence Status | Set and display online, away, busy, or custom status |
| Live Location Map | See friend locations on the world map in real time |
| Friend Export/Import | Export and import friend lists |
| Lending Tracker | Track items lent to and borrowed from friends |
| Looking for Group | Find groups and parties for activities |
| Name Change History | View past display names of other players |
| Spectate | Watch friends play in real time |
| Timezone Display | Show friend timezones for coordination |
| Ban List | Manage blocked and ignored players |
| Friend Notes | Attach personal notes to friends |
| Player Highlighting | Highlight specific players with colored indicators |

---

## Economy

| Plugin | Description |
|---|---|
| Grand Exchange | Centralized auction house for item trading |
| GE Tax | Percentage tax on Grand Exchange transactions |
| Item Sink | Systems that remove items from the economy |
| Player Trading | Direct player-to-player item and gold trading |
| Drop Trading | Transfer items by dropping and picking up |
| Item Lending | Temporarily lend items to other players |
| Shop System | NPC shops for buying and selling items |
| High/Low Alchemy | Convert items to gold using magic |
| Buy Limits | Maximum purchase quantity per item per time period |
| Price Guidance | Suggested price ranges based on recent trades |
| Trade Logging | Log all trades for audit and dispute resolution |
| Wealth Dashboard | Server-wide wealth distribution analytics |
| Price Intervention | Admin tools for correcting extreme price manipulation |
| Multiple Currencies | Support for multiple currency types and conversion |
| Account Trade Restrictions | Restrict trading on new or flagged accounts |
| Self-Correcting Economy | Automated systems that stabilize prices and supply |
| Dynamic Tax Rate | Tax rate adjusts based on economic conditions |
| Dynamic Drop Rates | Drop rates adjust based on item supply |
| Dynamic Shop Prices | Shop prices adjust based on economic conditions |
| Vulnerability Scanner | Detect potential economic exploits before they spread |
| Auto-Response System | Automated economic interventions triggered by thresholds |
| Stress Testing | Simulate economic scenarios to test stability |
| Bounty System | Place bounties on other players for PvP rewards |
| Transparency Dashboard | Public dashboard showing economic health metrics |
| Price Alerts | Notify players when watched items hit target prices |
| Flipping Tools | Tools for tracking buy/sell margins on items |
| GP/Hr Tracker | Track gold earned per hour across activities |
| Transaction Ledger | Complete history of all personal transactions |
| Auto Loot Split | Automatically divide loot among party members |
| Drop Parties | Host events where items rain down for players to collect |
| Shop Blacklist | Prevent specific items from being sold to shops |
| Trade Scam Protection | Warnings and safeguards against common trade scams |

---

## Monetization

| Plugin | Description |
|---|---|
| RWT Marketplace | Real-world trading marketplace with platform oversight |
| Service Marketplace | Players sell services like boss carries and coaching |
| Role-Based Access Control | Monetization permissions based on player or DM roles |
| Cosmetic Shop | Purchasable cosmetic items with no gameplay advantage |
| Subscriptions | Recurring payment plans for premium features |
| Battle Pass | Seasonal progression track with free and premium tiers |
| Loot Boxes | Purchasable random item containers |
| Gacha System | Gacha-style pulls with collection and pity mechanics |
| Donations | Accept voluntary donations from players |
| Pay-to-Accelerate | Purchase XP boosts or time-saving conveniences |
| Cryptocurrency Support | Accept cryptocurrency payments |
| Escrow System | Hold funds in escrow for secure transactions |
| Anti-Fraud Detection | Detect and prevent fraudulent payment activity |
| Seller Ratings | Reputation and rating system for marketplace sellers |
| Platform Cut | Configurable percentage taken from marketplace sales |
| DM Cut | Revenue share for dungeon master content creators |
| P2W Meter | Transparency score measuring pay-to-win impact |
| Gifting | Purchase items or currency as gifts for other players |
| Advertisements | Optional ad placements with revenue sharing |
| Spending Caps | Maximum spending limits per day, week, or month |
| Parental Controls | Spending restrictions and content filtering for minors |
| Pity System | Guaranteed rewards after a set number of purchases |
| Odds Display | Display exact probability for all random purchases |

---

## Rules and Moderation

| Plugin | Description |
|---|---|
| Rule System | Configurable server rules displayed to all players |
| Strike System | Accumulated strikes leading to escalating punishments |
| Appeals | Players can appeal punishments through a formal process |
| Report System | Players can report rule violations by others |
| Mod Tools | Administrative tools for moderators to investigate and act |
| Auto-Escalation | Repeated offenses automatically escalate punishment severity |
| Peer Review | Multiple moderators review actions for consistency |
| Reporter Reputation | Track reporter accuracy to prioritize reliable reports |
| Report Feedback | Notify reporters of outcomes on their filed reports |
| Transparency Reports | Public reports on moderation activity and statistics |
| Community Service | Alternative punishment through required community tasks |
| Quarantine | Restrict flagged accounts to limited gameplay while under review |
| Skill Rollback | Roll back illegitimate skill gains on offending accounts |
| Wealth Rollback | Roll back illegitimate wealth gains on offending accounts |
| Jail System | Send rule-breaking players to an in-game jail area |
| Evidence Reporting | Attach screenshots and logs to player reports |
| Ban Portability | Share ban lists across federated servers |

---

## Bot Detection

| Plugin | Description |
|---|---|
| Master Toggle | Enable or disable the entire bot detection system |
| Random Events | Periodic events requiring human interaction to dismiss |
| CAPTCHA | Challenge-response tests to verify human players |
| Honeypots | Hidden traps that only automated scripts would trigger |
| Bot Score | Behavioral scoring system that flags suspicious accounts |
| Auto-Punishment | Automated punishment when bot score exceeds threshold |
| Machine Learning Detection | ML model trained on player behavior to detect bots |
| Licensed Bots | Official bot accounts with approved automation |
| Bot API | API for authorized bot development and integration |
| Bot Marketplace | Marketplace for approved bot scripts and tools |
| Shadow Flagging | Silently flag suspicious accounts for manual review |
| Pattern Disruption | Randomly alter game patterns to break bot scripts |

---

## Modes

| Plugin | Description |
|---|---|
| Mode System | Framework for defining custom game modes |
| Multi-Mode Accounts | Play multiple modes on a single account |
| Mode Conversion | Convert an account from one mode to another |
| Seasonal Modes | Time-limited modes with fresh-start economies |
| Relic System | Choose permanent passive buffs from a relic selection |
| Task/Points System | Complete tasks for points toward mode-specific rewards |
| Mode Leaderboards | Separate hiscores per game mode |
| Mode Trophies | Cosmetic trophies for mode completion and milestones |
| Bank Raiding | PvP mode where killers can access victim bank |
| Area Restrictions | Restrict mode to specific map areas |
| Community Proposals | Players propose and vote on new seasonal modes |
| Prestige System | Reset progress for prestige rewards and ranking |
| Randomizer Mode | Randomized skill unlocks and item sources |
| Clogman Mode | Mode focused on completing the collection log |
| Chunk-Locked Mode | Gameplay restricted to unlocked map chunks |
| Taskman Mode | All activities assigned through a task system |
| Faction/Tribal Mode | Compete as a member of a faction or tribe |
| Relic Permanent Mode | Relics chosen are permanent and cannot be swapped |
| Gear Tier Mode | Restrict equipment to specific tier levels |
| Skull System (PvP Mode) | Enhanced skull mechanics for PvP-focused modes |

---

## Server Stats and Voting

| Plugin | Description |
|---|---|
| Server Dashboard | Admin dashboard with real-time server metrics |
| Population Display | Show current online player count |
| Skill Hiscores | Ranked leaderboards for skill levels and XP |
| Boss Hiscores | Ranked leaderboards for boss kill counts |
| Clan Hiscores | Ranked leaderboards for clan achievements |
| Mode Hiscores | Separate hiscores filtered by game mode |
| Opt-In Hiscores | Players choose whether to appear on leaderboards |
| Anonymous Hiscores | Option to appear on hiscores without revealing identity |
| External Voting | Integration with external server voting sites |
| Vote Rewards | In-game rewards for voting on external sites |
| Vote Streaks | Bonus rewards for consecutive daily votes |
| Community Polls | Create polls for player feedback |
| Binding Polls | Poll results that automatically implement changes |
| Advisory Polls | Poll results used as non-binding feedback |
| Veto Power | Admin ability to override poll results |
| Minimum Turnout | Polls require minimum participation to be valid |
| Secret Ballot | Anonymous voting on sensitive polls |
| Bug Reports | In-game bug reporting system |
| Feature Requests | Player-submitted feature request system |
| Public Roadmap | Visible development roadmap with planned features |
| Changelog | Published log of all updates and changes |
| Rank Notifications | Notify players when their hiscore rank changes |
| Data Visualization | Charts and graphs for server statistics |
| Network Diagnostics | Player-facing network latency and connection tools |
| Performance Metrics | Server performance monitoring and display |
| World Heatmap | Visual heatmap of player distribution across the world |

---

## Dialogue

| Plugin | Description |
|---|---|
| Scripted Dialogue | Pre-written NPC dialogue triggered by interaction |
| Branching Dialogue | Dialogue trees with multiple choice paths |
| AI Freeform Dialogue | AI-generated NPC responses to player text input |
| Hybrid Dialogue | Combination of scripted structure with AI-generated content |
| Relationship/Affinity | NPC dialogue changes based on player relationship level |
| Romance Options | Romantic dialogue paths with compatible NPCs |
| Morality System | Dialogue choices affect a morality or alignment score |
| NPC Memory | NPCs remember previous conversations and player actions |
| NPC-to-NPC Dialogue | NPCs converse with each other autonomously |
| Voice Acting | Pre-recorded voice audio for NPC dialogue |
| Timed Responses | Dialogue options expire after a countdown |
| Dialogue Log | Searchable log of all past NPC conversations |
| Chat Heads | Close-up NPC portraits during dialogue |
| Gift System | Give gifts to NPCs to improve relationship |
| Gossip System | NPCs share information about world events and other NPCs |
| Quest Journal | Dialogue updates recorded in a quest journal |
| TTS Synthesis | Text-to-speech generation for unvoiced dialogue |
| Dialogue Export | Export dialogue transcripts for external use |

---

## External Integrations

| Plugin | Description |
|---|---|
| Webhooks | Send game events to external URLs via webhooks |
| HTTP API | RESTful API for reading game data externally |
| Write Access API | API endpoints for modifying game state externally |
| Data Export | Bulk export game data in standard formats |
| Auto-Export | Scheduled automatic data exports |
| Time-Series Database | Stream game metrics to time-series databases |
| Chat Bridges | Bridge in-game chat to external chat platforms |
| Streaming Integration | OBS and streaming platform overlay support |
| IoT Integration | Connect game events to IoT devices |
| Cloud Save | Sync game data to external cloud storage |
| Third-Party Sync | Sync player data with third-party tracking sites |
| Per-Event Toggles | Enable or disable external triggers per event type |
| Crowdsourced Data | Submit anonymized data to community databases |
| Wiki Integration | Link in-game items and NPCs to wiki pages |
| Console API | Browser developer console commands for debugging |
| Discord Bridge | Direct integration with Discord servers and bots |
| Seeded RNG | Reproducible random number generation for testing |

---

## Music and Audio

| Plugin | Description |
|---|---|
| Music System | Background music playback tied to locations and events |
| Track Unlocking | Music tracks unlock as players discover new areas |
| Playlists | Create custom playlists from unlocked tracks |
| Jukebox | In-game jukebox for playing music on demand |
| DJ Mode | Players can broadcast music to nearby players |
| Custom Uploads | Upload custom music files for personal playback |
| Now Playing | Display currently playing track name and artist |
| Per-Zone Soundtrack | Assign specific soundtracks to world zones |
| Dynamic Music Layers | Music layers change based on combat state and danger |
| Custom Sound Effects | Replace default game sounds with custom audio |
| Sound Packs | Downloadable themed sound effect packages |
| Visual Sound Indicators | On-screen indicators for directional audio cues |

---

## Pet and Companion

| Plugin | Description |
|---|---|
| Pet System | Core system for obtaining and displaying pets |
| Pet Naming | Give custom names to owned pets |
| Pet Transmog | Override pet appearance with unlocked skins |
| Pet Raising | Level and grow pets over time through interaction |
| Pet Happiness/Hunger | Pets have needs that affect their behavior and bonuses |
| Pet Abilities | Pets learn abilities that assist the player |
| Combat Assist | Pets participate in combat alongside the player |
| Pet Foraging | Pets automatically gather nearby resources |
| Pet Storage/Housing | Dedicated storage and display for pet collections |
| Pet Insurance | Insure pets against loss on death |
| Pet Collection Tracking | Track all obtained pets and missing entries |

---

## Engine Architecture

| Plugin | Description |
|---|---|
| WebSocket Server | Real-time bidirectional communication via WebSocket |
| Binary Chunk Protocol | Efficient binary encoding for chunk data transmission |
| Proximity Filtering | Only send data relevant to a player's nearby area |
| Edge Wall System | Wall collision stored as tile-edge bitmasks |
| Layer System | Multiple vertical planes rendered independently |
| Dual Render Pipeline | Switch between 2D canvas and 3D Three.js rendering |
| PS1 Procedural Textures | Retro procedural texture generation for tiles |
| A* Pathfinding | A-star pathfinding for NPC and click-to-move navigation |
| Plugin Dual-Side Architecture | Plugins run on both client and server with shared config |
| Console API | Developer console for runtime debugging and testing |
| Panel Customization | Drag, resize, and arrange UI panels |
| Seeded RNG | Deterministic random generation for replay and testing |
| Auto-Save | Periodic automatic world and player data saves |
| Chunk Eviction | Unload inactive chunks from memory to manage resources |

---

## Deduplicated Total

The following plugins appear in multiple categories and are counted only once:

| Plugin | Appears In |
|---|---|
| Edge Walls | Buildings, Engine Architecture |
| Skull System | Tick System, Modes |
| Prayer Flicking | Skills -- Combat, Tick System |
| Combo Eating | Items, Tick System |
| Tick Manipulation | Skills -- Gathering (global), Tick System |
| Grand Exchange | Items, Economy |
| Alchemy | Items, Economy |
| Multiple Currencies | Items, Economy |
| Disassembly | Items, Skills -- Combining (Invention) |
| Augmentation | Items, Skills -- Combining (Invention) |
| Room Detection | Buildings, Locations |
| Roofs | Buildings, Locations |
| Chunk Streaming | Locations, Engine Architecture |
| Layers | Terrain, Engine Architecture |
| Seeded RNG | External Integrations, Engine Architecture |
| Console API | External Integrations, Engine Architecture |
| Slayer System | Skills -- Combat, Monsters |
| Friend Groups | Account Management, Friends and Social |
| Activity Feed | Account Management, Friends and Social |
| Referral System | Account Management, Friends and Social |
| Prestige System | Skills -- Core, Modes |
| Cosmetic Transmog | Equipment (Fashion/Transmog and Cosmetic Transmog counted as one) |
| Bestiary Completion | Monsters, Collection Log |
| Ready Check | Bosses and Raids, Minigames |
| Spectating | Minigames, Puzzles |
| Practice Mode | Minigames, Puzzles, Tick System |
| Leaderboards | Minigames, Puzzles, Games of Chance |
| Bank PIN | Account Management, Inventory and Bank |
| Data Export | Account Management, Items, External Integrations |
| Looking for Group | Communication, Friends and Social |
| Per-Zone Audio | Account Management, Music and Audio |

**Cross-category duplicates removed: ~31**

| Metric | Count |
|---|---|
| Total plugins listed across all categories | ~863 |
| Cross-category duplicates | ~31 |
| **Deduplicated total** | **~832** |
