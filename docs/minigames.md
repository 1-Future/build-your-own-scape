# Minigames

Game design document for the minigame system. Every minigame is assembled from a shared set of configurable modules. This document defines the 16 game mode templates, the rule parameters for each minigame instance, the supporting systems, and the module toggles that control which systems are active.

---

## Game Mode Templates

| #  | Template              | Description                                                                 |
|----|-----------------------|-----------------------------------------------------------------------------|
| 1  | Wave Survival         | Defend against increasingly difficult waves of enemies.                     |
| 2  | Capture the Flag      | Steal the opposing team's flag and return it to your base.                  |
| 3  | Battle Royale         | Last player or team standing in a shrinking arena.                          |
| 4  | Objective Defence     | Protect a structure or NPC from waves of attackers.                         |
| 5  | 1v1 Duel              | Single combat between two players with configurable rules.                 |
| 6  | Role-Based Team       | Each team member fills a distinct role (tank, healer, DPS, support).       |
| 7  | Gather-Craft-Fight    | Collect resources, craft gear, then fight. Gauntlet style.                 |
| 8  | Obstacle Course       | Navigate terrain hazards and platforming challenges against the clock.     |
| 9  | Timed Collection      | Gather as many target items as possible before the timer expires.          |
| 10 | Passive Management    | Allocate workers and resources over time. Idle/management style.           |
| 11 | Escort/Protect        | Guide a vulnerable NPC or object safely through hostile territory.         |
| 12 | Skilling Boss         | Defeat a boss using non-combat skills (mining, woodcutting, etc.).         |
| 13 | Tower/Floor Climbing  | Ascend through progressively harder floors with increasing rewards.        |
| 14 | Board Game            | Turn-based movement on a board with random events and player interaction.  |
| 15 | Stealth               | Complete objectives without being detected by NPCs or players.            |
| 16 | Delivery              | Transport items between points under time pressure or hostile conditions.  |

---

## Core Rules

Every minigame instance defines values for the following rule categories.

### Players

| Parameter             | Options                                                        |
|-----------------------|----------------------------------------------------------------|
| Min players           | Integer                                                        |
| Max players           | Integer                                                        |
| Mode                  | Solo, Team, Free-for-all                                       |
| Team count            | Integer (if team mode)                                         |
| Team size             | Integer (if team mode)                                         |
| Role system           | Yes / No. If yes, define role names and abilities.             |
| Matchmaking           | Random, Queue, Invite-only                                     |
| Spectating allowed    | Yes / No                                                       |

### Arena

| Parameter             | Options                                                        |
|-----------------------|----------------------------------------------------------------|
| Map type              | Fixed, Randomized, Shrinking                                   |
| Instanced             | Yes / No                                                       |
| Multiple maps         | Yes / No. If yes, rotation list.                               |
| Zones                 | Base, Arena, Resource area (define per map)                     |

### Equipment Rules

| Parameter                   | Options                                                  |
|-----------------------------|----------------------------------------------------------|
| Gear source                 | Bring your own, Provided (standardized loadout), Gather and craft in-game |
| Item restrictions           | No food, No prayer, No specials (any combination)        |
| Equipment slot restrictions | List of disabled slots                                   |
| Combat style restrictions   | Melee only, Ranged only, Magic only, Any                 |
| Fun weapons only            | Yes / No                                                 |

### Combat Rules

| Parameter             | Options                                                        |
|-----------------------|----------------------------------------------------------------|
| Death type            | Safe (keep items) / Dangerous (lose items)                     |
| Combat type           | PvP, PvE, Both                                                 |
| Combat zone           | Single combat / Multi combat                                   |
| Prayer allowed        | Yes / No                                                       |
| Special attacks       | Yes / No                                                       |
| Friendly fire         | Yes / No                                                       |

### Win/Lose Conditions

| Condition                    | Description                                              |
|------------------------------|----------------------------------------------------------|
| Kill all enemies             | All hostile NPCs or players eliminated.                  |
| Survive X waves              | Endure a set number of enemy waves.                      |
| Last player standing         | Be the final survivor.                                   |
| Capture objective            | Interact with or hold a target object/location.          |
| Score most points            | Highest score when time expires.                         |
| Protect target               | Keep an NPC or object alive/intact.                      |
| Deplete boss resource        | Reduce a boss-specific resource (not just HP) to zero.   |
| Timer expires                | Game ends when the clock runs out.                       |
| All players dead              | Total party wipe equals failure.                        |
| Custom condition             | Game-specific logic defined per minigame.                |

### Phases

| Parameter               | Options                                                    |
|--------------------------|------------------------------------------------------------|
| Number of phases         | Integer                                                    |
| Phase types              | Gather, Craft, Fight, Defend, Escape                       |
| Phase timer              | Duration per phase (seconds)                               |
| Phase transition trigger | Timer expiry, All enemies dead, Objective complete, Manual |

---

## Systems

### Scoring

| Parameter                | Description                                                |
|--------------------------|------------------------------------------------------------|
| Point actions            | Map of action to point value (e.g., kill = 10, assist = 5) |
| Penalties                | Point deductions (e.g., death = -3, friendly fire = -5)    |
| Point cap                | Maximum achievable score (0 = uncapped)                    |
| Team vs individual       | Points tracked per team, per player, or both               |
| Minimum threshold        | Minimum score required to receive rewards                  |
| Activity check           | AFK prevention. Require input every N seconds or forfeit.  |

### Power-ups

| Parameter        | Options                                                          |
|------------------|------------------------------------------------------------------|
| Spawn locations  | Fixed positions, Random positions, Both                          |
| Spawn timer      | Seconds between spawns                                           |
| Effects          | Damage boost, Heal, Speed, Invincibility, Instant kill           |
| Duration         | Seconds the effect lasts                                         |

### Rewards

| Parameter            | Description                                                    |
|----------------------|----------------------------------------------------------------|
| Currency name        | Minigame-specific token or point name                          |
| Earn rate            | Currency earned per game, per kill, per minute, etc.           |
| Reward shop          | Linked item shop (separate spreadsheet/table)                  |
| Direct drops         | Items dropped during gameplay (linked to drop table)           |
| XP rewards           | Skill XP granted on completion                                 |
| Scaling rewards      | Rewards increase with difficulty tier or performance           |
| Consolation rewards  | Minimum reward for participation (loss/early elimination)      |
| Pet chance           | Drop rate for minigame pet (e.g., 1/3000)                     |
| Unique items         | Items obtainable only from this minigame                       |

### Difficulty Scaling

| Parameter            | Options                                                        |
|----------------------|----------------------------------------------------------------|
| Tiers                | Novice, Intermediate, Veteran                                  |
| Hard mode toggle     | Yes / No. Increases difficulty and rewards.                    |
| Player count scaling | Enemy count/stats scale with number of players                 |
| Level scaling        | Enemy stats scale with average player level                    |

### Duel/Stake System

Derived from the Duel Arena model. Applies to 1v1 Duel and any PvP minigame that opts in.

| Parameter               | Options                                                     |
|--------------------------|-------------------------------------------------------------|
| Wager toggle             | On / Off                                                    |
| Wager cap                | Maximum gold or item value per stake                        |
| Wager tax                | Percentage removed as gold sink (e.g., 1%)                  |
| Rule toggles (11 total)  | See table below                                             |
| Equipment slot disabling | Disable specific equipment slots for the duel               |
| Presets                  | Save and load rule configurations                           |
| Scoreboard              | Track wins, losses, and wager history per player            |

**Duel Rule Toggles**

| #  | Toggle              | Effect when enabled                          |
|----|---------------------|----------------------------------------------|
| 1  | No melee            | Melee attacks disabled                       |
| 2  | No ranged           | Ranged attacks disabled                      |
| 3  | No magic            | Magic attacks disabled                       |
| 4  | No special attacks  | Special attack bar disabled                  |
| 5  | Fun weapons         | Only joke/novelty weapons allowed            |
| 6  | No forfeit          | Cannot leave the duel early                  |
| 7  | No drinks           | Potions disabled                             |
| 8  | No food             | Eating disabled                              |
| 9  | No prayer           | Prayer disabled                              |
| 10 | No movement         | Players are locked in place                  |
| 11 | Obstacles           | Pillars and barriers placed in the arena     |

### Passive/Idle Minigames

Derived from the Miscellania management model. Applies to Passive Management and any minigame that opts in.

| Parameter                | Description                                               |
|--------------------------|-----------------------------------------------------------|
| Worker allocation        | Assign workers to different resource-generating tasks     |
| Resource generation      | Resources accumulate over real time while offline         |
| Approval/maintenance     | Activity or upkeep required to maintain production rate   |
| Coffer/funding system    | Deposit gold to fund worker wages and operations          |
| Daily collection         | Resources collected once per day or on a cooldown         |
| Upgrade path             | Invest to unlock better workers, faster rates, new tasks  |

---

## Module Toggles

Each minigame instance activates or deactivates the following modules. This controls which systems are loaded and which UI elements appear.

| Module                | Default | Description                                          |
|-----------------------|---------|------------------------------------------------------|
| Safe deaths           | On      | Players keep items on death                          |
| Dangerous deaths      | Off     | Players lose items on death                          |
| Staking/Wagering      | Off     | Enable gold/item wagers between players              |
| Reward currencies     | On      | Minigame-specific token system                       |
| Reward shops          | On      | Spend tokens at a minigame vendor                    |
| Leaderboards          | On      | Track and display player rankings                    |
| Role system           | Off     | Assign roles with distinct abilities to players      |
| Team matchmaking      | Off     | Automatic team assignment via queue                  |
| Spectating            | On      | Allow non-participants to watch                      |
| Practice mode         | On      | No rewards, no penalties, learn the minigame         |
| Difficulty scaling    | On      | Tiered difficulty with scaling rewards               |
| Power-ups             | Off     | Spawn collectible buffs during gameplay              |
| Activity check        | On      | AFK detection and removal                            |
| Passive/idle system   | Off     | Offline resource generation and management           |

---

## Views

The minigame browser supports filtering by the following dimensions.

| View             | Filter by                                                       |
|------------------|-----------------------------------------------------------------|
| All              | No filter. Full list.                                           |
| By Game Mode     | One of the 16 templates.                                        |
| By Player Count  | Solo, Small group (2-5), Large group (6+), Mass (20+).          |
| By PvP/PvE       | PvP only, PvE only, Both.                                       |
| By Skill         | Combat, Skilling, Hybrid.                                       |
| By Reward        | Filter by reward type (currency, unique items, XP, pets).       |
| By Difficulty    | Novice, Intermediate, Veteran, Hard mode.                       |
