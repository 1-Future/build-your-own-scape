# Plugin Audit -- 1605 RuneLite Plugins Analyzed

## Overview

1605 RuneLite plugins were categorized against 28 existing design docs. This audit identifies gaps, new features, and new docs needed.

| Category | Count |
|---|---|
| COVERED (already in docs) | ~850 |
| NEW FEATURE (gaps identified) | ~180 |
| RENDERING (visual/client-side only) | ~250 |
| MEME/JOKE (novelty) | ~120 |
| OSRS-SPECIFIC (not generalizable) | ~205 |
| **Total** | **1605** |

---

## New Docs Needed

Four new plugin documents are required. These represent entire systems with no current coverage.

### 1. External Integrations / API

Plugins that bridge game state to external services and tools. No existing doc covers outbound data, webhooks, or third-party platform connectivity.

| Feature | Description |
|---|---|
| Webhooks | Discord, Telegram, Slack, custom HTTP endpoint notifications on game events |
| HTTP API | Expose live game state (stats, inventory, location) to external tools |
| Data export | CSV, JSON, InfluxDB export for offline analysis |
| IoT bridges | Home Assistant, Philips Hue integration (e.g. flash lights on rare drop) |
| Chat bridges | IRC, Discord, Telegram relay for in-game chat |
| Streaming integration | Twitch chat overlay, OBS replay buffer triggers, stream alerts |
| Third-party stat sites | TempleOSRS, CrystalMathLabs, WiseOldMan stat push |
| Cloud save/sync | Cross-device settings and progress synchronization |
| Socket broadcasting | WebSocket/TCP broadcast of game events for custom dashboards |

### 2. Music / Audio System

Plugins that modify, extend, or track the in-game music system. No existing doc addresses audio as a first-class system.

| Feature | Description |
|---|---|
| Track unlocking | Unlock music tracks per area/quest/achievement |
| Playlists and favorites | Player-curated playlists from unlocked tracks |
| Jukebox system | Furniture or location that lets players browse and play tracks |
| Per-zone soundtracks | Automatic music changes based on current zone/biome |
| Custom music replacement | Upload or replace tracks with custom audio files |
| Now-playing HUD overlay | On-screen display of current track name and artist |
| Volume per category | Independent volume sliders for music, SFX, ambient, UI |
| Music cape/completion | Tracking and reward for unlocking all music tracks |

### 3. Pet / Companion System

Plugins that add, modify, or track pets and companions. No existing doc covers pet acquisition, cosmetics, or companion behavior.

| Feature | Description |
|---|---|
| Pet acquisition | Boss drops, skilling milestones, purchase, quest rewards |
| Pet naming | Assign custom names to owned pets |
| Pet cosmetic transmog | Reskin pets to other creature models |
| Pet raising/leveling | Feed, train, and level pets (ties to Pet Raising skill if enabled) |
| Pet insurance | Protection against loss on death |
| Pet display/showcase | Display cases, follower parade, inspection by other players |
| Companion behaviors | Follow patterns, idle animations, emotes, interaction triggers |

### 4. Player Housing

Plugins that add or modify player-owned housing. No existing doc covers room layout, furniture, or housing social features.

| Feature | Description |
|---|---|
| Rooms | Configurable room layout on a grid, unlockable room types |
| Furniture/decoration | Craftable and purchasable furniture placement |
| Storage | House-specific bank, display cases, armour stands |
| Portal room | Teleport hub with configurable destinations |
| Trophy room | Achievement display (boss heads, capes, rare items) |
| Garden/farm patches | Outdoor area with farming skill integration |
| Visitor permissions | Lock, friends-only, or public access settings |
| Construction skill integration | Building and upgrading tied to Construction skill |
| House parties | Social events hosted in player-owned houses |
| POH rating/touring | Visit and rate other players' houses |

---

## Features to Add to Existing Docs

Gaps found in the 28 existing plugin documents. Each section lists features that should be added to the corresponding doc.

### Equipment
`docs/equipment.md`

| Feature | Description |
|---|---|
| Slot locking | Prevent accidental unequip of specific slots |
| Cosmetic transmog/glamour | Appearance override system beyond fashion toggle |
| Equipment screenshot/sharing | Export equipped gear as image for sharing |
| Empty slot warnings | Alert when entering combat with empty gear slots |
| Equipment rule enforcement | Module that enforces gear restrictions per activity |
| Spellbook/ability bar presets | Save and swap spellbook and ability bar configurations |
| Gear switch practice mode | Training mode for practicing gear switches with timing feedback |

### Skills
`docs/skills.md`

| Feature | Description |
|---|---|
| Success rate transparency | Show percentage chance of success for current action |
| Per-method XP rate tracking | Compare XP rates across different training methods |
| Resource node timers | Countdown until node moves or depletes |
| Resource node map tracking | Show resource node locations on world map |
| XP lock toggle | Lock XP gain per skill for pures and restricted builds |
| Prestige/rebirth system | Reset a skill to level 1 in exchange for prestige ranks |
| Task assignment history | Log of all slayer/assignment tasks received |
| Resting mechanic | Sit animation to regenerate run energy faster |
| Banked experience calculator | Calculate total XP stored in banked resources |

### Quests
`docs/quests.md`

| Feature | Description |
|---|---|
| Optimal quest ordering | Routing planner with prerequisite graph visualization |
| TTS dialogue fallback | Text-to-speech for quest dialogue when no voice acting exists |

### Monsters
`docs/monsters.md`

| Feature | Description |
|---|---|
| Per-monster session dashboard | Live tracking of loot/hr, kills/hr, profit per monster |
| Drop rate calculator | Probability and statistics for drop rates |
| Bestiary completion tracker | Track which monsters have been killed at least once |
| NPC nicknames/aliases | Player-defined custom names for NPCs |
| Reverse drop table lookup | Search by item to see all monsters that drop it |

### Bosses and Raids
`docs/bosses-raids.md`

| Feature | Description |
|---|---|
| Healer role tracking | Track healing output in group content |
| KPH efficiency tracking | Kills per hour with trend analysis |
| Raid randomizer/mutator mode | Roguelike modifier system for raid encounters |
| Ready check system | Pre-fight ready confirmation for all party members |
| Party connection monitoring | AFK detection and connection status for group members |

### Items
`docs/items.md`

| Feature | Description |
|---|---|
| Configurable loot filters | Show, hide, or highlight ground items by value, type, or rarity |
| Item locking | Prevent drop, trade, or alch on specific items |
| Activity-based checklists | Item reminders per activity (e.g. "bring antifire for dragons") |
| Item source lookup | Reverse drop table showing all sources for an item |
| Item data export | Copy or export item data for sharing |
| Drop rarity sound effects | Tiered audio cues by drop value |
| Ground item organization | Sorting and filtering for ground items |
| Consumable cooldown display | Visual timer for potion and food cooldowns |
| Custom per-item drop sounds | Assign unique sounds to specific item drops |

### Locations
`docs/locations.md`

| Feature | Description |
|---|---|
| User-placed waypoints | Custom markers on the world map |
| Distance measurement tool | Measure tile distance between two points |
| Day/night cycle | Time-of-day system affecting gameplay and visuals |
| GPS-style pathfinding | Turn-by-turn navigation to a destination |
| Tile marker profiles | Save and load tile marker sets per activity |
| Tile markers with labels | Text annotations on marked tiles |
| Teleport usage analytics | History and frequency tracking for teleport usage |
| Dynamic weather system | Weather effects beyond environmental damage |

### Tick System
`docs/tick-system.md`

| Feature | Description |
|---|---|
| Line-of-sight visualization | Show LOS from player to target |
| Combat roll transparency | Display actual hit and accuracy roll values |

### Achievements
`docs/achievements.md`

| Feature | Description |
|---|---|
| Personal goal tracker | Custom XP, GP, and KC targets with progress bars |
| Speedrun auto-splitter | Integration with speedrun timing tools |
| Configurable milestone thresholds | Dungeon master sets which levels trigger milestones |

### Collection Log
`docs/collection-log.md`

| Feature | Description |
|---|---|
| Dry streak tracking | Track dry streaks with statistical percentile context |
| Drop rate luck calculator | Analyze overall luck across all tracked drops |
| Bestiary as collection category | Monster kill tracking as a collection log section |
| Examine log | Encyclopedia of all examined item and object texts |

### Minigames
`docs/minigames.md`

| Feature | Description |
|---|---|
| Prop Hunt template | Hide-and-seek game mode using object disguises |
| Collectible card game | In-game trading card game template |
| Ready check system | Pre-start confirmation for minigame participants |

### Dailies
`docs/dailies.md`

| Feature | Description |
|---|---|
| Random world events | Anti-AFK surprise encounters with bonus rewards |
| Login streak counter | Daily login tracking with streak-based rewards |

### Account Management
`docs/account-management.md`

| Feature | Description |
|---|---|
| Health/wellness reminders | Hydration, stretch, and break prompts on timer |
| Teleport misclick protection | Confirmation dialogs for teleport actions |
| Per-zone audio controls | Mute or adjust volume per area |
| Actions-per-minute metrics | APM tracking and display |
| Auto-retaliate warnings | Alert when auto-retaliate setting may cause problems |
| Idle/AFK timer | Visible countdown to logout or AFK status |
| Session time limit warnings | Alerts at configurable play duration thresholds |
| Player pronouns | Pronoun field on player profile |
| Cloud save/sync | Cross-device settings synchronization |
| Resource/texture packs | Community-created visual theme support |
| Multi-panel layout | Split client into multiple resizable panels |
| Push notifications | Send alerts to phone or external device |
| Auto-screenshot triggers | Capture screenshots on configurable events |
| Focus-loss auto-mute | Mute game audio when window loses focus |
| Keybind profiles | Save and load keybinding configurations |
| Action history/audit log | Searchable log of all player actions |
| Custom notification sounds | Per-trigger notification sound assignment |

### Character Overview
`docs/character-overview.md`

| Feature | Description |
|---|---|
| Combat damage log | Hitsplat history with timestamp and source |
| Session metrics dashboard | Live XP/hr, GP/hr, kills/hr display |
| On-screen notepad | Persistent notes overlay |
| Effective level display | Show boosted levels with all active modifiers |
| Activity recommender | Suggest next activity based on stats and progress |
| Historical stat graphs | Track stats over time with charts |
| Click/input analytics | Track click patterns and input frequency |
| Pedometer | Movement distance and step counter |
| Data export | Export player stats as CSV or JSON |

### Inventory and Bank
`docs/inventory-bank.md`

| Feature | Description |
|---|---|
| Item lock | Prevent withdrawal, sale, or drop of specific items |
| Bank changelog | Diff view of bank contents over time |
| Bank usage heatmap | Analytics showing most and least used bank slots |
| Multi-term/fuzzy search | Search bank with multiple terms or approximate matching |
| Bank snapshot sharing | Export bank contents as screenshot or data |
| Banked XP calculator | Calculate XP available from banked resources |
| Unified item locator | Search for items across all storage locations |
| Bank data export/API | Export bank data for external tools |

### Death System
`docs/death-system.md`

No significant gaps found. Existing doc covers death mechanics comprehensively.

### Clan System
`docs/clan-system.md`

| Feature | Description |
|---|---|
| Per-member combat analytics | KDR, damage dealt, damage taken per member |
| Clan data export | Export member list, ranks, and stats |
| Clan-wide shared goals | Group progress bars toward shared targets |
| Attendance tracking | Sign-up and check-in system for clan events |
| Shared item visibility | See clanmates' equipped gear or inventory |
| Shared map markers | Party-visible markers on the world map |
| Loot broadcast effects | Celebration animations on rare clan drops |

### Communication
`docs/communication.md`

| Feature | Description |
|---|---|
| Chat translation | Real-time language translation in chat |
| Custom emoji | Player or server-defined emoji in chat |
| Item linking | Hover-to-inspect item links in chat messages |
| Chat audio notifications | Per-channel or per-keyword sound alerts |
| Proximity voice/text | Distance-based communication falloff |
| Tactical ping system | Location and alert pings on the game world |
| Rich chat messages | Clickable items, coordinates, and previews in chat |
| External chat bridges | IRC, Discord, Telegram relay |
| Notification center | Centralized feed with notification history |
| In-game drawing tool | Annotate the map or screen for coordination |
| AI game assistant | In-game AI guide for questions and suggestions |
| Streaming integration | Twitch and OBS overlay and interaction support |
| Chat transcript export | Save chat logs as text files |
| Chat clipboard support | Copy-paste support for chat messages |

### Economy
`docs/economy.md`

| Feature | Description |
|---|---|
| Price alert/watchlist | Notify when items hit target buy or sell price |
| GE flipping tools | Margin calculator, profit tracker, flip recommendations |
| GP/hr tracker | Real-time profit rate display |
| Transaction history | Searchable log of all trades and purchases |
| Auto loot splitting | Automatic fair-share distribution in groups |
| Drop party system | Organized item giveaway event tool |
| Shop purchase blacklist | Block specific items from being bought at shops |

### Friends and Social
`docs/friends-social.md`

| Feature | Description |
|---|---|
| Player presence/status | Online, away, busy, invisible status options |
| Live friend location map | See friend positions on world map |
| Friends list export/import | Backup and restore friends list |
| Item lending tracker | Track lent and borrowed items with return dates |
| LFG/matchmaking board | Find groups for specific activities |
| Name change history | View previous names for players |
| Spectate/screen share | Watch a friend's screen in real time |
| Activity matchmaking | Auto-match players doing the same activity |
| Shareable ban list | Export personal block list for others to import |

### Rules and Moderation
`docs/rules-moderation.md`

| Feature | Description |
|---|---|
| Jail/prison sentence | Punishment type confining player to a jail area |
| Player-driven reporting | Report system with screenshot and log evidence |
| Ban list import/export | Share moderation lists between servers |

### Modes
`docs/modes.md`

| Feature | Description |
|---|---|
| New Game Plus / prestige | Replay content with increased difficulty after completion |
| Randomizer mode | Shuffle unlocks across skills, quests, and drops |
| Clogman mode | Restrict usable items to collection log drops only |
| Chunk-locked mode | Restrict play to unlocked map chunks |
| Taskman mode | Random task-driven challenge progression |
| Relic system | Permanent passive bonuses as a standalone mode feature |

### Server Stats and Voting
`docs/server-stats-voting.md`

| Feature | Description |
|---|---|
| Hiscore rank notifications | Alert when rank changes on hiscores |
| Data visualization tools | Histograms, charts, and graphs for server data |
| Network diagnostics | Ping and latency monitoring |
| Client performance metrics | FPS counter, load time tracking |

### Dialogue
`docs/dialogue.md`

| Feature | Description |
|---|---|
| NPC dialogue journal | Searchable history of all NPC conversations |
| TTS voice synthesis | Text-to-speech for NPC dialogue |
| Dialogue export | Extract dialogue trees as text or data files |

---

## Summary

| Metric | Value |
|---|---|
| Plugins analyzed | 1605 |
| Existing docs | 28 |
| New docs needed | 4 |
| Existing docs with gaps | 23 |
| Docs with no gaps | 1 (Death System) |
| Total new features identified | ~180 |
