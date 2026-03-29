# Tick System

Game design document for the Tick System plugin. The tick system governs all time-dependent mechanics -- combat, movement, skilling, respawns, and cooldowns. Dungeon masters configure the system per-world.

---

## Core Settings

| Setting                | Default | Description                              |
|------------------------|---------|------------------------------------------|
| Tick rate              | 0.6s    | Duration of one game tick                |
| Ticks per second display | On   | HUD element showing current tick rate    |

---

## Tick Modes

The dungeon master selects a tick mode for their world. Each mode changes how time flows.

| Mode           | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| Fixed          | Custom tick rate between 0.1s and 2.0s. All actions resolve on the tick.    |
| Variable       | Real-time. No tick boundary. Actions resolve instantly.                     |
| Turn-based     | No clock. Each player takes a turn, then the tick advances.                 |
| Hybrid         | Different tick rates per context (e.g., combat at 0.6s, skilling at 1.2s). |
| Rhythm         | Tick rate locked to a BPM. Actions sync to the beat.                        |
| Simultaneous   | All player inputs for a tick are collected, then batch-resolved at once.    |
| Action Point   | Each player gets a budget of action points per tick to spend on actions.    |

---

## Combat Ticks

| Mechanic                | Unit   | Description                                                        |
|-------------------------|--------|--------------------------------------------------------------------|
| Attack speed per weapon | Ticks  | Number of ticks between auto-attacks for a given weapon.           |
| Eat delay               | Ticks  | Ticks locked out of other actions after eating food.               |
| Potion delay            | Ticks  | Ticks locked out of other actions after drinking a potion.         |
| Prayer switch delay     | Ticks  | Minimum ticks between prayer switches.                             |
| Combo eating window     | Ticks  | Window within a tick that allows consuming multiple items.         |
| PID rotation cycle      | Ticks  | How often PID (Player ID) priority rotates among players.          |
| Tick eating             | Toggle | When enabled, a player can heal on the same tick as lethal damage. |

---

## Movement Ticks

| Mechanic                  | Value  | Description                                                   |
|---------------------------|--------|---------------------------------------------------------------|
| Tiles per tick (walking)  | 1      | Distance covered per tick while walking.                      |
| Tiles per tick (running)  | 2      | Distance covered per tick while running.                      |
| Pathing recalculation rate| Ticks  | How often the pathfinding algorithm recalculates the route.   |
| Stall mechanics           | Ticks  | Actions that freeze the character for a specified tick count. |

---

## Skilling Ticks

| Setting                    | Description                                                             |
|----------------------------|-------------------------------------------------------------------------|
| Default action rate        | Ticks per action for each skill (e.g., fishing = 5 ticks per attempt).  |
| Manipulated rate floor     | Fastest possible tick rate achievable through manipulation.             |
| Manipulation allowed       | Toggle per skill. When off, the default rate is enforced.               |
| Delay items                | Items that reset the skilling timer, enabling tick manipulation.        |
| Flinch manipulation        | Toggle. Allows using NPC aggro to reset the skilling timer.            |
| Movement manipulation      | Toggle. Allows movement actions to reset the skilling timer.           |

---

## Respawn Ticks

| Timer                     | Description                                               |
|---------------------------|-----------------------------------------------------------|
| Monster respawn timer     | Ticks until a killed monster reappears.                   |
| Resource node respawn     | Ticks until a depleted resource node regenerates.         |
| Item despawn timer        | Ticks until a dropped item disappears from the ground.    |
| Player respawn on death   | Ticks until a dead player respawns at their spawn point.  |

---

## Cooldown Ticks

| Cooldown                  | Description                                               |
|---------------------------|-----------------------------------------------------------|
| Special attack regen rate | Ticks to regenerate one unit of special attack energy.    |
| Prayer drain rate         | Ticks between prayer point deductions.                    |
| Run energy regen rate     | Ticks between run energy recovery increments.             |
| Potion effect decay rate  | Ticks until a potion stat boost decays by one level.      |
| Stat boost decay rate     | Ticks until a non-potion stat boost decays by one level.  |

---

## Delay Item Registry

Linked spreadsheet. Each entry defines an item or item combination that resets a skilling timer.

| Field                | Description                                                   |
|----------------------|---------------------------------------------------------------|
| Item ID / item combo | The item or combination of items that triggers the delay.     |
| Tick delay value     | How many ticks the delay lasts (2, 3, or 4).                  |
| Consumable           | Yes or No. Whether the item is consumed on use.               |
| Skills it works with | Which skills this delay item can be used to manipulate.       |
| Setup requirements   | Any prerequisites (inventory layout, position, aggro range).  |

---

## Skilling Tick Manipulation

Normal versus manipulated action rates for core gathering and production skills.

| Skill        | Normal Rate (ticks) | Manipulated Rate (ticks) | Method                          |
|--------------|--------------------:|-------------------------:|---------------------------------|
| Fishing      | 5                   | 3                        | Delay item between casts        |
| Woodcutting  | 4                   | 2                        | Delay item between chops        |
| Mining       | 4                   | 3                        | Delay item between swings       |
| Cooking      | Variable            | 1                        | Delay item timing               |
| Hunter       | 4                   | 1-3                      | Movement or flinch manipulation |

---

## Combat Tick Mechanics

| Mechanic                   | Description                                                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------|
| PID system                 | Determines which player hits first when two players attack on the same tick.                    |
| PID rotation interval      | How often PID priority shuffles among all players in the world.                                |
| Tick eating                | Toggle. When on, a player can eat food on the same tick they would die, surviving the hit.      |
| Combo eating               | Toggle. When on, food + karambwan + potion can all be consumed on the same tick.                |
| Prayer flicking            | Toggle. When on, activating and deactivating a prayer within 1 tick grants its effect for free. |
| Vengeance timing           | Tick window for casting vengeance relative to an incoming hit.                                  |
| Spec timing                | Tick window for using a special attack after a weapon switch.                                   |
| 1-tick prayer switching    | Switching prayers between hits to protect against different attack styles each tick.            |
| Eat-to-spec delay          | Ticks between eating and being able to use a special attack.                                    |
| Attack delay after eating  | Ticks added to the next auto-attack after consuming food.                                       |

---

## PvP Tick Mechanics

| Mechanic            | Description                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------|
| Skull system        | Attacking another player first marks you as skulled. Skulled players risk all items on death.         |
| Skull timer         | Duration in ticks before the skull clears.                                                           |
| Skull tricking      | Toggle. When on, mechanics that trick a player into skulling are allowed.                            |
| PJ timer            | Ticks after a fight ends before another player can attack you. Prevents pile-jumping.                |
| Freeze timer        | Ticks a player is held in place by a freeze spell.                                                   |
| Freeze immunity     | Ticks of immunity after a freeze ends, during which no new freeze can be applied.                    |
| TB timer            | Duration of teleblock in ticks. Prevents teleportation for the duration.                             |
| Smite drain rate    | Prayer points drained per tick of damage dealt while the Smite prayer is active.                     |

---

## Action Queue System

| Setting                   | Description                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------------------|
| Queue depth               | Maximum number of actions that can be queued ahead of the current tick.                           |
| Queue priority            | Resolution order: last input wins, first input wins, or priority-based.                           |
| Input window tolerance    | Milliseconds before or after a tick boundary during which input is accepted for that tick.         |
| Action canceling          | Whether a queued action can be canceled by a new input before its tick arrives.                    |
| Skill timer inheritance   | The root toggle of tick manipulation. On = actions share timers (manipulation possible). Off = every action resets its timer independently (manipulation impossible). |

---

## Tick Processing Priority

The order in which the server processes events each tick. This list is configurable -- the dungeon master can reorder it to change game feel.

| Priority | Step                        | Description                                            |
|---------:|-----------------------------|--------------------------------------------------------|
| 1        | Player input registration   | All queued player inputs are read.                     |
| 2        | Movement processing         | Characters move along their paths.                     |
| 3        | Combat hit calculation      | Attack rolls and damage are calculated.                |
| 4        | Prayer/protection check     | Active prayers and protection effects are evaluated.   |
| 5        | Damage application          | Calculated damage is applied to health.                |
| 6        | Healing/food processing     | Food and healing effects are applied.                  |
| 7        | Stat boost/drain application| Stat changes from potions, prayers, and debuffs apply. |
| 8        | Poison/DoT application      | Ongoing damage effects tick.                           |
| 9        | Death check                 | Any entity at or below 0 HP is killed.                 |
| 10       | XP award                    | Experience is granted for completed actions.           |
| 11       | Loot generation             | Drops are rolled and placed for dead entities.         |
| 12       | Respawn timers              | Respawn countdowns advance.                            |

**Order matters.** Healing before death check means tick eating works -- a player can eat on the same tick they take lethal damage and survive. Death check before healing means tick eating is impossible.

---

## Dungeon Master Controls

| Control                     | Description                                                                                  |
|-----------------------------|----------------------------------------------------------------------------------------------|
| Global tick rate multiplier | Scales the base tick rate up or down for the entire world.                                   |
| Per-skill tick override     | Override the default tick rate for individual skills.                                         |
| Manipulation difficulty     | Easy = larger input windows for tick manipulation. Hard = frame-perfect timing required.     |
| Auto-manipulation mode      | The system performs tick manipulation automatically, removing the skill ceiling entirely.     |

---

## Module Toggles

Master switches that enable or disable entire subsystems.

| Toggle                      | Scope     | Description                                                    |
|-----------------------------|-----------|----------------------------------------------------------------|
| Tick manipulation           | Global    | Master toggle for all tick manipulation mechanics.             |
| Per-skill manipulation      | Per skill | Enable or disable manipulation for individual skills.          |
| Flinch manipulation         | Global    | Allow NPC aggro-based timer resets.                            |
| Movement manipulation       | Global    | Allow movement-based timer resets.                             |
| Tick eating                 | Global    | Allow healing on the same tick as lethal damage.               |
| Combo eating                | Global    | Allow consuming multiple items on the same tick.               |
| Prayer flicking             | Global    | Allow free prayer usage via within-tick toggling.              |
| PID system                  | Global    | Enable player ID priority for same-tick resolution.            |
| Skull system                | Global    | Enable skulling on first-attack in PvP.                        |
| PJ timer                    | Global    | Enable pile-jump protection between fights.                    |
| Freeze immunity window      | Global    | Enable post-freeze immunity period.                            |
| Custom tick rate            | Global    | Allow non-default tick rates.                                  |
| Auto-manipulation mode      | Global    | System performs tick manipulation on behalf of the player.      |

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---------|-------------|
| Line-of-Sight Visualization | Visual overlay showing line-of-sight between player and target. Shows what tiles are visible, what is blocked by obstacles. Useful for PvP and boss positioning. |
| Combat Roll Transparency | Display the actual accuracy/hit roll behind each attack. Shows the random number generated, the threshold needed to hit, and whether it passed. Demystifies combat math. |
| Tick Practice Mode | Training grounds where players can practice tick-perfect actions with visual feedback, accuracy grading, and ghost playback of perfect runs. |
| Tick Replay Logger | Record and replay tick-perfect action sequences. Save replays for analysis. Share replays with others for learning. |
| Priority-Based Tick Queue | 4-priority action scheduling system. Priority 0 = movement, 1 = player actions, 2 = NPC actions, 3 = world events. Key-based deduplication — scheduling with same key replaces previous entry. cancel via cancelScheduled(key). Proven pattern from working implementation |
| Action Progress Bar | RS3-style progress bar for skilling and combat actions. Shows completion percentage of current action. Fills over the action's tick duration. Visual feedback for multi-tick actions |
