# World Events

Large-scale server-wide or zone-wide events that affect all players. Beyond dailies -- these are the big moments that bring the community together.

---

## Scheduled Events

Calendar-based events with known dates and times.

| Type | Description |
|---|---|
| Boss Invasions | World boss spawns in the overworld at a scheduled time |
| Resource Surges | Bonus resource nodes spawn everywhere for a limited window |
| Double XP Weekends | All XP gains doubled server-wide |
| Community Goals | Server-wide collective target (e.g. "kill 1M dragons collectively") |
| Holiday Events | Christmas, Halloween, Easter with themed content and exclusive rewards |

---

## Dynamic Events

Triggered by game state, not calendar. These occur when conditions are met in the world.

| Type | Description |
|---|---|
| Meteor Crash | Shooting stars on steroids -- massive resource deposit crashes into a zone |
| Monster Migration | New monsters appear in unusual areas, disrupting normal spawns |
| Economy Events | Market crash -- prices shift dramatically across the Grand Exchange |
| Natural Disasters | Volcano eruption blocks areas, flooding changes terrain layout |
| NPC Uprising | NPCs rebel and become hostile in previously safe zones |
| Disease Outbreak | Players receive debuffs and must find a cure through skilling or questing |
| Portal Storms | Random teleports send players to unusual or hidden locations |

---

## World Bosses

| Property | Description |
|---|---|
| Visibility | Giant bosses visible to all players in the overworld |
| Participation | Requires many players to defeat |
| Spawn Rules | Timed schedule or condition-triggered |
| Phases | Multi-phase fight with area-wide mechanics |
| Loot Distribution | Damage contribution based -- everyone who helps gets rewards |
| Announcements | Server-wide broadcast on spawn |
| Rewards | Unique drops, titles, cosmetics on kill |
| Scaling | Difficulty and HP scale with participating player count |

---

## Community Goals

Server-wide objectives with collective progress tracking.

| Property | Description |
|---|---|
| Examples | "Donate 10B GP to build a new city", "Kill 1M of this monster to unlock new area" |
| Progress Bar | Visible to all players server-wide |
| Tiered Rewards | Bronze at 25%, Silver at 50%, Gold at 75%, Platinum at 100% |
| Individual Tracking | Each player's personal contribution is recorded |
| Leaderboard | Top contributors ranked and displayed |
| Completion Reward | Everyone on the server receives a reward when the goal is met |

---

## Seasonal / Holiday Events

| Holiday | Content |
|---|---|
| Christmas | Snow, presents, Santa NPC, ice skating, snowball fights |
| Halloween | Undead invasion, haunted areas, costume contest, trick-or-treating |
| Easter | Egg hunts, rabbit NPCs, chocolate rewards |
| Custom Holidays | Dungeon master creates original holidays and themes |

| Property | Description |
|---|---|
| Exclusive Items | Available only during the event window |
| Recurrence | Repeatable annually or one-time, configurable per event |

---

## PvP Events

| Type | Description |
|---|---|
| Wilderness Chaos | Increased loot and risk in the wilderness |
| Deadman Tournaments | Bracket-style final showdown |
| Castle Wars Tournaments | Organized team competitions |
| Clan War Championships | Clan vs clan ranked battles |
| Last Man Standing | Free-for-all survival tournament |
| Bounty Hunter Seasons | Seasonal target-hunting with ranked rewards |
| PK Leaderboard Resets | Fresh leaderboard for a new competitive period |

---

## Competitions

| Type | Description |
|---|---|
| Skill of the Month | Most XP gained in a specific skill during the event period |
| Boss Racing | Fastest kill time for a designated boss |
| Treasure Hunts | Server-wide clue chase with hidden objectives |
| Build Competitions | Best player-built structure, community judged |
| Fashion Shows | Community-voted outfit contest |
| Speedrun Races | First to complete an objective from a fresh account |

---

## Event Properties

| Property | Type | Description |
|---|---|---|
| Event ID | Integer | Unique identifier |
| Name | String | Display name |
| Type | Enum | Scheduled, Dynamic, World Boss, Community Goal, Holiday, PvP, Competition |
| Start Condition | Varies | Calendar date, game state trigger, or manual dungeon master activation |
| Duration | Time | How long the event runs |
| Zone | Location ID | Everywhere, specific zones, or instanced |
| Scaling | Boolean | Adjusts difficulty with player count |
| Rewards | Array | Items, XP, titles, cosmetics, currencies |
| Broadcast | Boolean | Server-wide announcement on start and end |
| Recurring | Schedule | One-time, daily, weekly, monthly, or annual |
| Min Players | Integer | Minimum required to start (for group events) |
| Max Participants | Integer | Player cap, or unlimited |
| Progress Tracking | Boolean | Per-player and server-wide progress bars |

---

## Module Toggles

| Toggle | Default |
|---|---|
| Scheduled Events | Off |
| Dynamic Events | Off |
| World Bosses | Off |
| Community Goals | Off |
| Holiday Events | Off |
| PvP Events | Off |
| Competitions | Off |
| Event Calendar | Off |
| Event Rewards | Off |
| Server-Wide Broadcasts | Off |
| Event Scaling | Off |
| DM Manual Activation | Off |
| Recurring Events | Off |
| Event History Log | Off |

---

## Views

| View | Description |
|---|---|
| Event Calendar | Upcoming scheduled events with dates and times |
| Active Events | Currently running events with status and progress |
| Event Progress | Per-player and server-wide progress bars |
| Leaderboards | Rankings for competitive events |
| Reward Preview | Viewable rewards before participating |
| Event History | Log of past events and outcomes |
