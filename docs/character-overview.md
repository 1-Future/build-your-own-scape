# Character Overview

Game design document for the character overview panel. This is the player's central dashboard -- a single view that surfaces everything about their account. Each section is a toggleable module so players can customize what they see.

---

## 1. At-a-Glance Dashboard

The top-level summary shown when opening the character panel.

| Field              | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| Character name     | Display name, editable via name change system                |
| Title              | Equipped title earned from achievements, quests, or events   |
| Clan               | Current clan name and rank icon                              |
| Combat level       | Derived from combat skill levels (see section 3)             |
| Total level        | Sum of all skill levels                                      |
| Total XP           | Combined XP across all skills                                |
| Time played        | Cumulative session time since account creation               |
| Account mode       | Normal, Ironman, Hardcore Ironman, Ultimate Ironman, etc.    |
| Account age        | Days since account creation                                  |
| Current world      | Server/world number and region                               |
| Current location   | Map region name and coordinates                              |
| Current gear       | Visual paper doll showing all equipped slots                 |
| Net worth          | Total estimated value of all owned items (bank + equipped)   |

---

## 2. Skills Overview

Full breakdown of every skill on the account.

### Per-Skill Data

| Field               | Description                                                |
| -------------------- | ---------------------------------------------------------- |
| Level                | Current level (1--99, or 1--120 for extended skills)       |
| XP                   | Total experience earned in the skill                       |
| Rank                 | Hiscores rank among all players                            |
| XP to next level     | Remaining XP needed to reach the next level                |
| % to next level      | Progress bar showing percentage toward next level          |
| Virtual level        | Togglable display showing levels beyond 99 based on XP    |

### Aggregated Stats

| Stat                    | Description                                             |
| ------------------------ | ------------------------------------------------------- |
| XP gained today          | XP earned since daily reset                             |
| XP gained this week      | XP earned since weekly reset                            |
| XP gained this month     | XP earned since first of the month                      |
| XP/hr (current session)  | Live XP rate calculated from the active session         |
| Estimated time to goal   | Hours/days remaining at current XP rate to reach goal   |
| Skill goals              | Player-set target level or XP per skill                 |
| Skill comparison         | Side-by-side view vs another player's skill levels      |
| Most trained skill       | Skill with the highest total XP                         |
| Least trained skill      | Skill with the lowest total XP                          |
| Skills at 99             | Count of skills that have reached level 99              |
| Skills remaining to max  | Count of skills still below 99                          |

---

## 3. Combat Overview

Offensive and defensive stats derived from levels, gear, and prayers.

### Core Stats

| Stat                  | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Combat level breakdown | Individual contribution of Attack, Strength, Defence, Hitpoints, Prayer, Ranged, Magic to combat level |
| Max hit (per style)    | Highest possible damage for melee, ranged, and magic     |
| Attack speed           | Weapon attack interval in game ticks                     |
| Accuracy (per style)   | Hit chance percentage against a reference target         |

### Defensive Stats

| Stat              | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| Stab defence       | Defence bonus against stab attacks                           |
| Slash defence      | Defence bonus against slash attacks                          |
| Crush defence      | Defence bonus against crush attacks                          |
| Magic defence      | Defence bonus against magic attacks                          |
| Ranged defence     | Defence bonus against ranged attacks                         |

### Derived Stats

| Stat                    | Description                                             |
| ------------------------ | ------------------------------------------------------- |
| DPS calculator           | Damage per second against a selected monster             |
| Gear score               | Aggregate score representing overall equipment quality   |
| Current vs BIS           | Comparison of equipped gear against best-in-slot setup   |
| Prayer bonus total       | Sum of prayer bonuses across all equipped items          |
| Weight total             | Combined weight of all equipped items in kg              |
| Speed                    | Movement speed modifier from equipped items              |

---

## 4. Boss and Combat Log

Tracking all PvM activity across the account.

### Per-Boss Data

| Field                | Description                                               |
| --------------------- | --------------------------------------------------------- |
| Kill count            | Total kills, sortable by KC or alphabetical               |
| Fastest kill time     | Personal best kill duration                               |
| Average kill time     | Mean kill duration across all kills                       |
| Deaths                | Total deaths to this boss                                 |
| K/D ratio             | Kills divided by deaths                                   |
| Unique drops obtained | Count of unique drops received vs total possible uniques  |
| Dry streak            | Kills since last unique drop                              |
| Estimated GP earned   | Total estimated value of all drops from this boss         |

### Aggregated Stats

| Stat                        | Description                                        |
| ----------------------------- | -------------------------------------------------- |
| Total boss KC                 | Sum of all boss kill counts                        |
| Combat achievements completed | Count completed vs total available                 |
| Slayer task history           | Log of recent slayer assignments and completions   |

---

## 5. Collection Log Progress

Tracking unique item acquisition across all content.

| Stat                  | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Overall % complete     | Percentage of all collection log slots filled            |
| Per category %         | Completion percentage broken down by category            |
| Total unique items     | Count of distinct items obtained                         |
| Most recent unlock     | Last item added to the collection log                    |
| Rarest item obtained   | Item with the lowest drop rate that the player owns      |
| Missing items list     | Filterable list of items not yet obtained                |
| Estimated time to complete | Projected hours to fill remaining slots at current rate |

---

## 6. Quest Progress

Status of all quests and quest-related content.

| Stat                  | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Quests completed       | Count completed vs total available                       |
| Quest points           | Points earned vs maximum possible                        |
| Current active quest   | Name of the in-progress quest and current step           |
| Quest series progress  | Completion status within multi-part quest lines          |
| Difficulty breakdown   | Count of completed quests per difficulty tier             |
| Rewards unlocked       | Notable rewards obtained from completed quests           |

---

## 7. Achievement Progress

Tracking all achievement diary and achievement system completion.

| Stat                   | Description                                             |
| ----------------------- | ------------------------------------------------------- |
| Total achievement points | Cumulative points from all completed achievements      |
| Achievements completed   | Count completed vs total available                     |
| Per category progress    | Completion percentage by achievement category          |
| Tier breakdown           | Count completed per tier: Bronze, Silver, Gold, Master |
| Recent achievements      | Last 5--10 achievements unlocked                       |
| Closest to completing    | Achievements with the fewest remaining requirements    |

---

## 8. Wealth Breakdown

Full financial picture of the account.

### Current Wealth

| Stat                    | Description                                            |
| ------------------------ | ------------------------------------------------------ |
| Total net worth          | Combined value of all assets                           |
| Bank value (by category) | Value subtotals grouped by item category              |
| Top 10 most valuable     | Highest value items in bank and equipment             |
| GP on hand               | Gold pieces in inventory                               |
| GP in clan coffer        | Gold deposited in clan treasury                        |
| GE active offers value   | Total value of pending Grand Exchange buy/sell offers  |

### Historical Wealth

| Stat                    | Description                                            |
| ------------------------ | ------------------------------------------------------ |
| Wealth over time graph   | Line chart of net worth over days/weeks/months         |
| Income sources breakdown | Percentage pie chart: bossing, skilling, trading, etc. |
| Spending breakdown       | Percentage pie chart: gear, supplies, construction, etc.|

---

## 9. Social Stats

Player interaction and community participation metrics.

| Stat                  | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Friends count          | Number of players on friends list                        |
| Clan membership        | Current clan name and rank within the clan               |
| Clan contribution score| Cumulative contribution points to clan activities        |
| Events attended        | Count of in-game events participated in                  |
| Trades completed       | Total number of player-to-player trades                  |
| Items lent             | Count of items lent to other players                     |
| Items donated          | Count of items given away                                |
| Players mentored       | Number of players assisted through mentorship system     |
| Chat messages sent     | Lifetime message count                                   |
| Time in voice chat     | Cumulative hours in voice channels                       |

---

## 10. Activity Heatmap

Behavioral analytics showing how the player spends their time.

| Stat                  | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Time distribution      | Pie chart: % bossing, skilling, questing, bankstanding, socializing |
| Peak play hours        | Most active hours of the day and days of the week        |
| Most visited locations | Top locations by time spent                              |
| Most killed monsters   | Top monsters by kill count                               |
| Most used items        | Top items by interaction frequency                       |
| Session history        | Log of sessions over the last 30 days with duration      |

---

## 11. Milestones Timeline

Chronological record of major account events. Shareable as a generated image or link.

| Event Type         | Examples                                                    |
| ------------------- | ----------------------------------------------------------- |
| Account created     | Date of account creation                                    |
| First 99            | First skill to reach level 99, with date                    |
| Quest cape          | Date all quests were completed                              |
| First pet           | First skilling or boss pet obtained, with date              |
| First rare drop     | First high-value unique drop, with date and item name       |
| Max combat          | Date combat level reached maximum                           |
| Max total           | Date all skills reached 99                                  |
| Joined clan         | Clan name and join date                                     |
| Mode conversions    | Any account mode changes (e.g., de-ironed) with date       |
| Speedrun records    | Personal best speedrun times with date achieved             |

Sharing options: generate a shareable image or a public link to the milestone timeline.

---

## 12. Comparison Mode

Side-by-side player comparisons across all tracked data.

| Comparison Type       | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Player vs player       | Compare skills, KC, collection log, wealth, achievements with any player |
| Player vs clan average | Compare personal stats against clan-wide averages        |
| Player vs global average | Compare personal stats against server-wide averages    |
| Player vs past self    | Compare current stats to a snapshot from 30 days ago     |

---

## Module Toggles

All sections are independently toggleable. Players can show or hide each module to customize their character overview.

| Module                | Default |
| ---------------------- | ------- |
| Skills overview        | On      |
| Combat overview        | On      |
| Boss log               | On      |
| Collection log tracker | On      |
| Quest tracker          | On      |
| Achievement tracker    | On      |
| Wealth tracker         | On      |
| Social stats           | Off     |
| Activity heatmap       | Off     |
| Milestones timeline    | On      |
| Comparison mode        | Off     |
| XP goals               | On      |
| BIS comparison         | Off     |
| Dry streak tracker     | On      |
| Wealth over time       | Off     |
| Shareable profile      | Off     |

---

## Additional Features (Plugin Audit)

| Feature | Description |
| ------------------ | ------------------------------------------------------------ |
| Combat Damage Log | Scrollable history of all damage dealt and received with source attribution, timestamps, and hit/miss information. Per-fight and per-session views |
| Session Metrics Dashboard | Real-time dashboard showing XP/hr, GP/hr, kills/hr, and other performance metrics for the current session with trends |
| On-Screen Notes | Persistent in-game notepad/sticky notes. Rich text formatting. Position anywhere on screen. Multiple notes. Save across sessions |
| Effective Level Display | Show effective (boosted/drained) skill levels with all active modifiers calculated (potions, prayers, equipment, debuffs) as a single number |
| Activity Recommender | "What should I do next?" system that suggests activities based on current stats, goals, uncompleted content, and efficiency. Considers quests, skills, bosses, and achievements |
| Historical Stat Graphs | Graph skill XP, wealth, KC, and other stats over time with daily/weekly/monthly views. Identify trends and milestones |
| Click/Input Analytics | Track mouse clicks, keyboard presses, and APM over time. Heatmap of most-clicked screen areas. Session stats |
| Pedometer | Count tiles walked/run. Total distance traveled per session and lifetime. Includes running vs walking breakdown |
| Player Data Export | Export all character data (stats, bank, KC, achievements, collection log) to JSON/CSV for external analysis or sharing |
| DPS Meter | Real-time damage-per-second counter during combat with session totals and per-target breakdown |
| Regen Progress Meter | Visual bar showing tick-by-tick progress toward next natural HP and special attack regeneration |
| Poison/Status Tracker | Poison and venom damage tracker showing per-tick damage values and time until status expires |
| Timer/Buff/Debuff Bar | Unified system showing all active timed effects (buffs, debuffs, immunity timers) with countdowns |
| XP Progress Globes | Per-skill circular progress indicators showing visual arc toward next level with hover detail stats |
