# Dailies

Recurring activities that reset on a timer. Players complete them for rewards, streaks, and progression. The system supports daily, weekly, monthly, and custom interval resets.

---

## Activity Types

### Free Claims

No effort required beyond showing up. Rewards loyalty and consistent play.

| Type | Example |
|------|---------|
| NPC collection | Talk to NPC to receive free items |
| Passive currency | Collect accumulated currency from a passive system |
| Free spell casts | Limited free uses of specific spells per period |
| Skill freebies | Skill-specific handouts (free ores, free herbs, etc.) |

### Shop Runs

Buy limited stock from specific shops and resell for profit. The core daily money-maker for many players.

| Property | Details |
|----------|---------|
| Stock reset | Timer-based, resets independently per shop |
| Quantity scaling | Higher achievement/diary tier unlocks more stock |
| Profit source | Buy at NPC price, sell at market price |

### Task-Based

Require active play to complete. Vary in difficulty and time commitment.

| Task Type | Description |
|-----------|-------------|
| Kill target | Kill a specific monster or boss |
| Minigame | Complete a round of a specific minigame |
| D&D (Distraction & Diversion) | One-off activities on a reset timer |
| Skilling challenge | Random skill assignment with a target to hit |
| Reputation/faction | Perform tasks for a faction to gain standing |

### Passive Collection

Resources accumulate over time without active play. Must be collected before hitting a cap.

| Property | Details |
|----------|---------|
| Accumulation | Resources build up passively (kingdom, farms, ports) |
| Cap | Must collect before reaching maximum or resources are lost |
| Scaling | Output scales with player investment (gold, materials, upgrades) |

---

## Data Schema

Each daily is a single row in the system. All fields below define one daily activity.

### Core Fields

| Field | Type | Description |
|-------|------|-------------|
| ID | integer | Unique identifier |
| Name | string | Display name |
| Description | string | What the player does |
| Category | enum | Skilling, Combat, Social, Economy, Misc |
| Frequency | enum | Daily, Weekly, Monthly, Custom interval |
| Task type | enum | Claim, Shop, Kill, Minigame, D&D, Skilling, Reputation |
| Skill involved | string | Associated skill (if any) |
| Location | string | Where the activity takes place |
| Requirements | object | Skill level, quest, item prerequisites |

### Reset Fields

| Field | Type | Description |
|-------|------|-------------|
| Reset type | enum | Fixed time (UTC), soft reset (X hours after last completion), cooldown after completion |
| Reset day | string | Daily = midnight UTC, Weekly = specific day, Monthly = 1st of month |
| Stack count | integer | How many days of completions can be banked |
| Timer display | string | Countdown to next reset |
| Completion status | boolean | Whether the current period is complete |
| Streak counter | integer | Consecutive completions without missing a reset |

### Reward Fields

| Field | Type | Description |
|-------|------|-------------|
| XP reward | integer | Experience points granted |
| Item reward | object | Specific item(s) granted |
| Currency reward | integer | Gold or other currency granted |
| Random reward | object | Loot table reference for random rolls |
| First completion bonus | object | One-time bonus for first ever completion |
| Scaling reward | boolean | Whether reward scales with skill level |

### Scaling Rules

| Scaling Factor | Effect |
|----------------|--------|
| Skill level | Higher skill level increases reward quantity or quality |
| Diary/achievement tier | Unlocks better rewards or higher stock quantities |
| Quest completion | Specific quests unlock enhanced versions of dailies |

### Limits

| Field | Type | Description |
|-------|------|-------------|
| Daily cap | integer | Maximum completions per day |
| Weekly cap | integer | Maximum completions per week |
| Cooldown | integer | Minimum time between attempts (seconds) |

---

## Tracker and Timer Properties

Each daily carries metadata for the tracker UI, enabling players to sort and prioritize their daily routine.

| Property | Type | Description |
|----------|------|-------------|
| Priority ranking | integer | Sort position based on GP/hr or XP/hr |
| Estimated time | integer | Minutes to complete |
| Estimated reward value | integer | Approximate gold value of rewards |
| Notification: on reset | boolean | Alert when the daily resets and is available again |
| Notification: before reset | boolean | Alert when a reset is approaching (reminder to complete) |
| Notification: streak warning | boolean | Alert when a streak is about to break |

---

## Module Toggles

Each subsystem can be enabled or disabled independently. This controls what features are active in the game.

| Module | Description |
|--------|-------------|
| Daily reset system | Core daily reset logic |
| Weekly reset system | Weekly reset logic |
| Monthly reset system | Monthly reset logic |
| Custom interval resets | Arbitrary interval resets (e.g., every 3 days) |
| Streak system | Track consecutive completions |
| Streak bonuses | Grant bonus rewards for maintaining streaks |
| Shop run tracking | Track shop stock and reset timers |
| Passive accumulation | Resources build up over time |
| D&D system | Distraction & Diversion activities |
| Profit tracking | Calculate and display GP/hr for each daily |
| Timer display | Show countdown timers in the UI |
| Notifications | Push alerts for resets, reminders, streak warnings |
| Priority sorting | Sort dailies by profit, time, or custom priority |
| Achievement tier scaling | Scale rewards and stock based on diary/achievement tier |

---

## Views

The tracker supports multiple views for filtering and sorting the daily list.

| View | Filter/Sort |
|------|-------------|
| All | Every daily in the system, no filter |
| By Frequency | Group by Daily, Weekly, Monthly, Custom |
| By Type | Group by Claims, Shops, Tasks, Passive |
| By Skill | Group by associated skill |
| By Profit | Sort by estimated GP value, highest first |
| By Time to Complete | Sort by estimated minutes, shortest first |
| Completed/Incomplete | Split into done and not-done for current period |
| By Priority | Sort by player-assigned or calculated priority ranking |

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---------|-------------|
| Random World Events | Periodic surprise events that occur in the world. Anti-AFK mechanic and content driver. Types: invasion (monsters attack a town), resource surge (bonus nodes spawn), wandering merchant (rare shop appears), weather event (special conditions). DM configures frequency, types, and rewards. |
| Login Streak Rewards | Daily login streak counter with escalating rewards for consecutive days. Streak break policy (miss one day = reset, or grace period). Milestone rewards at 7, 14, 30, 60, 90, 365 days. |
