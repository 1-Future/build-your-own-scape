# Random Events

Periodic surprise encounters that interrupt players. Originally anti-AFK/anti-bot measures, evolved into mini-content and world flavor.

---

## 1. Event Types

| Type | Description | Example |
|---|---|---|
| Combat | Aggressive NPC appears and attacks the player. Must fight or flee | Evil Chicken, River Troll, Rock Golem, Tree Spirit |
| Skill | NPC offers a skill-related mini-challenge for bonus XP | Strange Plant (herblore), Certer (note exchange) |
| Transport | NPC teleports the player to a mini location. Must complete a task to return | Mysterious Old Man maze, Quiz Master island |
| Gift | NPC appears and gives free items or XP with no challenge | Genie (XP lamp), Drunken Dwarf (kebab and beer) |
| Quiz | NPC asks a question. Correct answer earns a reward | Quiz Master trivia |
| Nuisance | Annoying but harmless. No reward, no penalty. Adds flavor | Bees, frogs, swarm of flies |

---

## 2. Event Properties

| Property | Type | Description |
|---|---|---|
| Event ID | Integer | Unique identifier |
| Name | String | Display name |
| Type | Enum | Combat, Skill, Transport, Gift, Quiz, Nuisance |
| Trigger Condition | Chance/tick | Random chance per tick while performing a qualifying action |
| Minimum Level | Integer | Player must be at least this level for the event to trigger |
| Maximum Level | Integer | Player must be at or below this level (0 = no cap) |
| Activity Requirement | Enum | Only during skilling, only during idle, only during combat, or always |
| NPC | NPC ID | The NPC that appears for this event |
| Animation | Animation ID | Entry animation for the event NPC |
| Duration | Integer (ticks) | How long before the event auto-dismisses if ignored |
| Dismissable | Boolean | Whether the player can choose to ignore it |
| Penalty for Ignoring | Enum | Teleported to random location, lose some resources, or nothing |
| Reward for Completing | Varies | XP, items, unique rewards, or nothing |
| Frequency | Integer (minutes) | Minimum time between this event triggering for the same player |

---

## 3. Anti-AFK Purpose

Random events force player attention. Consequences for ignoring them are configurable.

| Severity | Behavior |
|---|---|
| Classic (Punishing) | Ignored events teleport the player to a random location. Combat events can kill AFK players. Resources may be lost |
| Modern (Gentle) | Ignored events simply dismiss themselves. No penalty. Rewards are optional |
| Off | Random events disabled entirely |

| Setting | Description |
|---|---|
| Severity Toggle | DM picks Classic, Modern, or Off |
| Teleport on Ignore | Toggle whether ignoring an event teleports the player |
| Resource Loss on Ignore | Toggle whether ignoring causes resource loss |
| Combat Event Danger | Toggle whether combat random events can kill the player |
| AFK Detection Threshold | Ticks of inactivity before anti-AFK events start triggering at higher rates |

---

## 4. Event NPCs

| NPC | Type | Behavior | Reward |
|---|---|---|---|
| Genie | Gift | Appears and offers an XP lamp | XP lamp (player chooses skill) |
| Quiz Master | Quiz | Teleports player to quiz island, asks trivia | Reward box with random items |
| Sandwich Lady | Gift | Offers a selection of food items | Free food (bread, triangle sandwich, chocolate, kebab) |
| Rick Turpentine | Gift | Appears and hands over gems | Random gems (sapphire through diamond) |
| Strange Plant | Skill | Grows a fruit. Player must pick it before it dies | Free fruit with minor stat boost |
| Mysterious Old Man | Transport | Teleports player to a maze or puzzle room | Reward on completion, return to original location |
| Evil Chicken | Combat | Aggressive chicken attacks the player, scales with player level | Feathers, raw chicken, bones |
| River Troll | Combat | Appears while fishing, attacks the player | Fish, water runes |
| Rock Golem | Combat | Appears while mining, attacks the player | Ores, gems |
| Tree Spirit | Combat | Appears while woodcutting, attacks the player | Logs, nature runes |
| Drunken Dwarf | Gift | Appears and gives free food and drink | Kebab and beer |
| Certer | Skill | Offers to note or unnote items | Convenience service, no tangible reward |

---

## 5. World Events vs Personal Events

| Scope | Description |
|---|---|
| Personal Events | Happen to one player only. Other players do not see the event NPC or interaction. All random events are personal by default |
| World Events | Affect an area and all players in it. Covered in world-events.md. Random events are NOT world events |

Cross reference: see [world-events.md](world-events.md) for server-wide and zone-wide events.

---

## 6. DM Configuration

| Setting | Description |
|---|---|
| Frequency Slider | Never, Rare, Occasional, Frequent, Constant |
| Per-Event Toggle | Enable or disable each individual random event |
| Custom Events | DM creates new random events with custom NPCs, rewards, and behavior |
| Safe Zones | Locations where random events never trigger (banks, boss arenas, minigames) |
| Cooldown | Minimum time between any random event for a given player |
| Level Scaling | Whether event difficulty and rewards scale with player level |
| Activity Filtering | Restrict which activities can trigger random events |

### Custom Event Creation

| Field | Description |
|---|---|
| Name | Display name for the event |
| Type | Combat, Skill, Transport, Gift, Quiz, or Nuisance |
| NPC | Select existing NPC or create a new one |
| Trigger Condition | What activity triggers the event and at what probability |
| Reward | Items, XP, or custom reward on completion |
| Penalty | What happens if the player ignores it |
| Duration | How long the event persists before auto-dismissing |
| Level Range | Minimum and maximum player level for triggering |

---

## Module Toggles

| Toggle | Default |
|---|---|
| Random Events | Off |
| Combat Events | Off |
| Skill Events | Off |
| Transport Events | Off |
| Gift Events | Off |
| Quiz Events | Off |
| Nuisance Events | Off |
| Anti-AFK Punishment | Off |
| Teleport on Ignore | Off |
| Resource Loss on Ignore | Off |
| Safe Zones | Off |
| Custom Events (DM) | Off |
| Level Scaling | Off |
| Per-Player Cooldown | Off |

---

## Views

| View | Description |
|---|---|
| Event Log | History of random events that have occurred for the player |
| Event Configuration | DM panel showing all events with enable/disable toggles and frequency settings |
| Custom Event Editor | DM tool for creating and editing custom random events |
| Safe Zone Manager | DM tool for defining locations where random events are suppressed |
| Event Statistics | Frequency and completion rates per event type, per player and server-wide |
