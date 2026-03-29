# Modes

## Overview

A mode is a preset ruleset layered on top of the base game. The dungeon master creates modes -- each one is a configuration of toggles and restrictions pulled from every other plugin. Modes define how a player experiences the world: what they can trade, how they die, where they can go, and how fast they progress.

Players select a mode when creating a character. Some modes are permanent (the character lives under those rules forever), others are seasonal (time-limited events with fresh economies and unique rewards).

---

## Mode List

Every mode has a unique ID, display name, and a set of metadata that controls how it integrates with the rest of the game.

| Field              | Description                                                        |
|--------------------|--------------------------------------------------------------------|
| Mode ID            | Unique identifier (e.g., `ironman`, `deadman`, `league-s1`)       |
| Name               | Display name shown to players                                      |
| Description        | One-line summary of the mode's rules                               |
| Icon               | Visual badge displayed on the player's profile and hiscores        |
| Type               | Permanent, Seasonal, or Community                                  |
| Duration           | Season length (seasonal only). Permanent modes have no duration    |
| Separate Progress  | Whether the mode starts fresh or carries over from a main account  |
| Separate Hiscores  | Whether the mode has its own leaderboard or shares the global one  |

---

## Mode Rules

Modes are built from the following rule categories. Each category exposes toggles and values that the dungeon master configures per mode.

### Trading

| Rule              | Type      | Default  | Description                                      |
|-------------------|-----------|----------|--------------------------------------------------|
| Player Trading    | On/Off    | On       | Direct player-to-player item trading             |
| Grand Exchange    | On/Off    | On       | Access to the automated marketplace              |
| Pick Up Drops     | On/Off    | On       | Pick up items dropped by other players           |
| Receive Items     | On/Off    | On       | Accept items given by other players              |
| Group Exemption   | On/Off    | Off      | Allow trading within a designated group           |
| Group Size        | 2-5       | 5        | Maximum players in a trading group               |

### Death

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| Permadeath        | On/Off       | Off      | Character dies permanently or converts on death  |
| Lives             | 1/2/3/Buy    | 1        | Number of deaths before permadeath triggers      |
| Safe Activities   | List         | None     | Activities exempt from permadeath (minigames)    |
| Death Converts To | Mode ID      | Normal   | Mode the character converts to on final death    |
| Bank Raiding      | On/Off       | Off      | Killer receives a percentage of victim's bank    |
| Bank Raid Percent | 0-100%       | 0        | Percentage of bank lost on death                 |
| Bank Key System   | On/Off       | Off      | Death drops a key to access victim's bank        |
| Safe Deposit Box  | On/Off       | Off      | Protected storage unaffected by bank raiding     |

### PvP

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| PvP Everywhere    | On/Off       | Off      | All areas are PvP-enabled                        |
| PvP Zones Only    | On/Off       | On       | PvP restricted to designated zones               |
| Skull System      | On/Off       | On       | Attackers receive a skull marking them            |
| Skull Timer       | Minutes      | 20       | Duration the skull persists after attacking       |
| Combat Brackets   | On/Off       | Off      | Restrict PvP to similar combat level ranges      |
| PJ Timer          | Seconds      | 12       | Delay before another player can attack you       |
| Guard System      | On/Off       | On       | NPC guards in safe zones that attack skulled players |
| Loot Rules        | Enum         | Standard | What the killer receives (all, top 3, keys, nothing) |
| XP From PvP       | On/Off       | On       | Gain combat XP from attacking other players      |

### XP Rates

| Rule                | Type       | Default  | Description                                      |
|---------------------|------------|----------|--------------------------------------------------|
| Global Multiplier   | 1x-16x+   | 1x       | Flat XP multiplier applied to all skills         |
| Per-Skill Multiplier | Map       | 1x each  | Individual multiplier overrides per skill         |
| Scaling             | On/Off     | Off      | Higher multiplier at low levels, tapering off    |
| Instance XP Cap     | Number     | None     | Maximum XP earnable per action/instance          |
| Drop Rate Multiplier | 0.5x-5x+ | 1x       | Multiplier on item drop rates                    |

### Area Restrictions

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| Starting Area     | Area ID      | Default  | Where the player spawns at character creation    |
| Unlock Method     | Enum         | None     | How new areas become accessible (tasks, points, quests, levels) |
| Locked Areas      | List         | None     | Areas locked at the start of the mode            |
| Unlock Order      | Fixed/Choice | Choice   | Whether areas unlock in a set order or player picks |

### Relics and Perks

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| Relic Tiers       | Number       | 0        | Number of relic tiers available                  |
| Relics Per Tier   | Number       | 3        | Choices offered at each tier                     |
| Effects           | Categories   | None     | XP, combat, gathering, movement, QoL             |
| Active Slots      | Number       | 0        | How many relics can be active simultaneously     |
| Swapping          | Locked/Free  | Locked   | Whether relics can be changed after selection    |

### Tasks and Points

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| Task List         | Mode-specific| None     | Set of completable objectives for this mode      |
| Task Tiers        | List         | Easy/Med/Hard/Elite/Master | Difficulty categories    |
| Points Per Task   | Map          | Varies   | Point reward per tier                            |
| Point Thresholds  | List         | None     | Milestones that grant rewards or unlocks         |
| Leaderboard       | On/Off       | Off      | Track and display point totals                   |
| Trophy Tiers      | List         | Bronze/Iron/Steel/Rune/Dragon | Visual rank badges    |

### Account Restrictions

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| Bank Access       | On/Off       | On       | Whether the player can use a bank                |
| Storage Alternatives | List      | None     | Alternative storage (looting bag, death pile)    |
| Group Shared Bank | On/Off       | Off      | Shared bank accessible by all group members      |
| Accept Aid        | On/Off       | On       | Receive buffs and heals from other players       |
| Enter Houses      | On/Off       | On       | Enter other players' constructed houses          |
| Shop Restrictions | List         | None     | Shops or shop categories that are disabled       |

### Seasonal

| Rule              | Type         | Default  | Description                                      |
|-------------------|--------------|----------|--------------------------------------------------|
| Start Date        | Date         | None     | When the season begins                           |
| End Date          | Date         | None     | When the season ends                             |
| Fresh Economy     | On/Off       | On       | All players start with zero wealth and items     |
| Rewards Transfer  | On/Off       | On       | Earned rewards carry to the player's main game   |
| Reward Shop       | On/Off       | Off      | Post-season shop to spend earned points          |

---

## Pre-built Templates

These templates ship with the mode system. The dungeon master can use them as-is or clone them as a starting point for custom modes.

| Template           | Type      | Key Rules                                                                 |
|--------------------|-----------|---------------------------------------------------------------------------|
| Normal             | Permanent | No restrictions. Full trading, standard death, 1x XP                     |
| Ironman            | Permanent | No trading, no GE, no receiving items. Fully self-sufficient             |
| Ultimate Ironman   | Permanent | Ironman rules plus no bank access. All items carried or stored creatively |
| Hardcore Ironman   | Permanent | Ironman rules plus one life. Death converts to standard Ironman          |
| Group Ironman      | Permanent | Trade within a group of 2-5. No outside trading or GE                    |
| Hardcore Group     | Permanent | Group Ironman plus shared lives and permadeath                           |
| Deadman            | Permanent | PvP everywhere, bank raiding, skull system, 5x XP, high-stakes death    |
| League             | Seasonal  | Area-locked, relics, task list, boosted XP, separate hiscores           |
| Fresh Start        | Seasonal  | Everyone starts at zero. Separate economy and hiscores                   |
| PvP World          | Permanent | PvP everywhere except designated safe zones. Standard XP                 |
| Skill Pure         | Permanent | Combat restricted. Cannot train combat skills above a threshold          |
| One Chunk          | Permanent | Locked to a single 64x64 tile chunk. All progress within that area       |
| Custom             | Permanent | Blank slate. Every rule starts at default. DM configures from scratch    |

---

## Snowflake and Restriction Modes

Community-driven restriction modes that formalize popular self-imposed challenges. Each is a pre-configured template that the dungeon master can enable.

### Movement and Area

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| One Chunk          | Locked to a single 64x64 tile chunk for the entire account            |
| Tileman            | Each XP drop unlocks one walkable tile. Start with almost nothing     |
| Area Locked        | Restricted to a single region (e.g., Karamja Only, Desert Only)       |

### Skill Restrictions

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| OSAAT              | One Skill At A Time. Must reach 99 before training the next skill     |
| 10 HP              | Hitpoints locked at 10. Cannot gain HP experience                      |
| Defence Pure       | Defence trained exclusively. No offensive combat stats                 |
| Backwards          | Train skills in reverse order (99 to 1 requirement unlocks)            |

### Item and Equipment

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Bronzeman          | Items unlock permanently after obtaining them once                     |
| No Equipment       | Fists only. No weapons, armor, or tools equipped                       |
| F2P Locked         | Only free-to-play content available. Members content disabled          |
| Vegetarian/Vegan   | Cannot kill animals or use animal-derived products                     |
| No Bank            | Bank access disabled entirely. Carry or lose                           |

### Task and RNG

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Taskman            | Random task generator assigns your next objective. No free choice      |
| RNG Mode           | Random skill, task, and area assigned daily. Adapt or fall behind      |
| Speedrun           | Timed completion of specific objectives. Fastest time wins             |

### Combat Variants

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Pacifist           | Cannot deal combat damage. Must find alternative progression paths     |
| Nightmare          | Damage capped per hit. Bosses take significantly longer                |
| Bounty Mode        | Assigned player targets to hunt. Rewards for successful kills          |
| Permadeath Race    | First to die is eliminated. Last player standing wins                  |

### Economy

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Merchant Only      | All progress through trading. Cannot gather, craft, or fight           |
| Debt Mode          | Start with negative currency. Must earn your way to zero               |
| Decay Mode         | Skills decay over time if not actively trained                         |

### Social and Multiplayer

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Mirror Mode        | Two linked players must match each other's progress exactly            |
| Hardcore Seasonal  | Seasonal mode with permadeath. One life per season                     |

### Creative and Fun

| Mode               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Build Mode         | Creative mode. Unlimited resources, no restrictions                    |
| Spectator          | Watch only. Cannot interact with the game world                        |
| Mentor Mode        | Paired with a new player. Earn rewards for guiding them                |
| Photo Mode         | Exploration focused. XP for discovering new areas and vistas           |
| Lore Mode          | Story focused. Combat auto-completed so the player can follow the narrative |
| Sandbox            | No levels, everything unlocked. Pure experimentation                   |

---

## Community and Player-Created Modes

Players can propose new modes through a formalized process. This turns the "snowflake account" tradition into a first-class system.

| Step               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| Proposal           | Player submits a mode definition using the rule categories above       |
| Community Vote     | Other players vote on the proposal. Threshold set by the DM            |
| DM Approval        | Dungeon master reviews and approves before the mode goes live          |
| Launch             | Approved mode appears in the mode selection screen                     |
| Iteration          | Creator can submit rule changes, subject to the same approval process  |

---

## Module Toggles

The mode system itself is modular. Each subsystem can be enabled or disabled at the world level.

| Toggle                   | Default | Description                                         |
|--------------------------|---------|-----------------------------------------------------|
| Mode System              | On      | Master toggle for the entire mode plugin             |
| Multiple Modes Per World | On      | Allow different players to play different modes      |
| Mode Conversion          | On      | Allow death or other events to convert a mode        |
| Seasonal Modes           | Off     | Enable time-limited seasonal mode support            |
| Relic System             | Off     | Enable relics and perks within modes                 |
| Task/Points System       | Off     | Enable task lists and point tracking                 |
| Leaderboards             | On      | Display per-mode hiscores                            |
| Trophy Rewards           | Off     | Award visual trophies for point thresholds           |
| Bank Raiding             | Off     | Allow bank raiding on death in supported modes       |
| Skull System             | On      | Enable skulling for PvP aggressors                   |
| Area Restrictions        | Off     | Enable area locking and unlock progression           |
| Community Mode Proposals | Off     | Allow players to propose and vote on new modes       |

---

## Views

| View               | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| All Modes          | Full list of every available mode in the world                         |
| By Type            | Filter by permanent, seasonal, or community-created                    |
| Active Modes       | Currently running modes (especially relevant for seasonal)             |
| Leaderboards       | Per-mode hiscores with trophy tier indicators                          |
| Mode Comparison    | Side-by-side comparison of two or more modes' rule configurations      |
