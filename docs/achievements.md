# Achievements

## Overview

The Achievements plugin tracks player milestones across every aspect of the game. Achievements are organized into categories, each with its own set of completable objectives. Every achievement awards configurable achievement points. Dungeon masters control which categories and systems are active per world.

Achievements support multiple tiers, hidden unlocks, time-gated objectives, and ultra-rare feats. The system is designed to reward breadth, depth, and creativity in how players engage with the world.

---

## Achievement Categories

### Location

| Achievement | Description |
|-------------|-------------|
| Regional Explorer | Visit every area in a region |
| Shortcut Unlocker | Unlock all shortcuts in a region |
| Zone Completionist | Complete all activities in a zone |
| Hidden Finder | Discover all hidden locations in a region |

### Combat

| Achievement | Description |
|-------------|-------------|
| Boss KC Milestones | Kill a boss 1 / 10 / 100 / 500 / 1,000 / 5,000 times |
| Flawless Kill | Kill a boss without taking any damage |
| Speedkill | Kill a boss under its par time |
| Restricted Kill | Kill a boss with restricted gear (e.g., bronze only, no armor) |
| PvP Kill Streak | Achieve kill streaks of 3 / 5 / 10 / 25 in PvP |
| Gauntlet | Complete a multi-boss gauntlet without banking or dying |

### Collection

| Achievement | Description |
|-------------|-------------|
| Category Completionist | Own every item in a category |
| Hoarder | Accumulate 1,000,000 of a single item |
| Collection Log | Fill an entire collection log category |
| Variant Collector | Own every variant or color of an item |
| Full Set | Complete a full equipment set (all slots, matching tier) |

### Skilling

| Achievement | Description |
|-------------|-------------|
| Production Speed | Reach production speed milestones (e.g., 1,000 fish/hour) |
| Streak | Process X items consecutively without failing |
| Mastery | Max a skill (99 or 120, depending on world config) |
| XP Milestones | Reach 1M / 10M / 100M / 200M XP in a single skill |
| Total Level | Reach total level milestones (500 / 1,000 / 1,500 / 2,000 / max) |
| Bankless | Train a skill to a target level without banking |
| Multi-Skill Combo | Perform actions that require two or more skills in sequence |

### Grand Exchange / Economy

| Achievement | Description |
|-------------|-------------|
| Wealth Milestones | Reach total wealth thresholds (1M / 10M / 100M / 1B / max cash) |
| Flip Profit | Earn cumulative profit from buy-low-sell-high trades |
| Volume Trader | Buy or sell X total items through the exchange |
| Diverse Trader | Trade X unique item types |

### Pet

| Achievement | Description |
|-------------|-------------|
| First Pet | Obtain any pet |
| Pet Collector | Collect X unique pets |
| Boss Pet Complete | Obtain all boss pets |
| Skilling Pet Complete | Obtain all skilling pets |
| Pet Raising | Reach pet raising milestones (level, happiness, tricks learned) |

### Clue Scroll / Treasure Trail

| Achievement | Description |
|-------------|-------------|
| Tier Completions | Complete X clues per tier (Beginner / Easy / Medium / Hard / Elite / Master) |
| Rare Reward | Obtain a rare clue scroll reward |
| Total Clues | Complete X clues across all tiers |
| Speed Clue | Complete a clue scroll within a time limit |

### Social

| Achievement | Description |
|-------------|-------------|
| Trader Network | Trade with X unique players |
| Chat Milestones | Send X messages in public / clan / group chat |
| Voice Time | Spend X hours in voice chat |
| Friends List | Add X friends |
| Group Activities | Complete X group activities (group bosses, minigames, raids) |

### Account Build

| Achievement | Description |
|-------------|-------------|
| Pure Milestones | Reach combat stat milestones while keeping defence at 1 |
| Ironman Milestones | Reach progression milestones on an ironman account |
| Area-Locked | Complete objectives while area-locked to a single region |
| Snowflake | Complete objectives under custom restriction rulesets |
| Hardcore Survival | Survive X days on a hardcore account without dying |

### Diary / Task

| Achievement | Description |
|-------------|-------------|
| Regional Tasks | Complete all tasks in a specific region |
| Tier Sweep | Complete all Easy / Medium / Hard / Elite tasks across all regions |
| Diary Completionist | Complete every diary in the game |

### Fashionscape

| Achievement | Description |
|-------------|-------------|
| Unique Outfits | Wear X unique outfit combinations |
| Fashion Show | Participate in X fashion show events |
| Outfit Ratings | Receive X total outfit rating points from other players |

### Minigame

| Achievement | Description |
|-------------|-------------|
| Win Streak | Achieve a win streak of X in a minigame |
| High Score | Reach a high score threshold in a minigame |
| Points Milestones | Earn X total points across all minigames |
| Minigame Tourist | Complete at least one round of every minigame |

### Music

| Achievement | Description |
|-------------|-------------|
| Track Collector | Unlock X music tracks |
| Regional Tracks | Unlock all tracks in a region |
| Full Soundtrack | Unlock every music track in the game |
| Listener | Listen to X unique tracks from start to finish |

### Miscellaneous

| Achievement | Description |
|-------------|-------------|
| Time Played | Reach cumulative playtime milestones (24h / 168h / 720h / 2,000h) |
| Login Streak | Log in X days in a row |
| Bank Standing | Spend X cumulative hours standing in a bank |
| Total Deaths | Die X times (embrace the grind) |
| Server First | Be the first player on the server to complete an objective |

---

## Achievement Systems

### Achievement Points

Every achievement awards a configurable number of points. Points scale with difficulty. Dungeon masters set point values per achievement or use defaults.

| Tier | Default Points | Description |
|------|---------------|-------------|
| Bronze | 1 | Easy objectives, low time investment |
| Silver | 5 | Moderate difficulty, some grinding |
| Gold | 10 | Hard objectives, significant time or skill |
| Master | 25 | Extreme difficulty, endgame-tier |

### Meta Achievements

Meta achievements unlock when a player completes a threshold of other achievements. Examples: "Complete 50 achievements in Combat" or "Earn 500 total achievement points."

### Area Task Tiers

Area tasks are organized into four difficulty tiers per region. Completing a tier unlocks a region-specific reward (lamp, cosmetic, utility item, or permanent buff).

| Tier | Difficulty | Typical Reward |
|------|-----------|----------------|
| Easy | Low-level skills and quests | XP lamp, minor cosmetic |
| Medium | Mid-level requirements | Better lamp, utility item |
| Hard | High-level requirements | Significant lamp, elite cosmetic |
| Elite | Near-max requirements | Best lamp, aura, or permanent regional buff |

### Lore Achievements

Lore achievements are tied to the world's story. They require reading books, exploring lore locations, completing quest chains, or discovering hidden NPC dialogue. These are intended for players who engage with narrative content rather than pure efficiency.

### Hidden Achievements

Hidden achievements are not visible in the achievement panel until a prerequisite is met. The prerequisite can be another achievement, a quest completion, a skill level, or a location visit. Once the prerequisite is met, the hidden achievement appears and can be completed normally.

### Time-Gated Achievements

Some achievements are only completable within a time window.

| Type | Window | Example |
|------|--------|---------|
| Daily | Resets every 24 hours | "Win 3 minigame rounds today" |
| Weekly | Resets every 7 days | "Complete 10 clue scrolls this week" |
| Monthly | Resets every calendar month | "Earn 1M XP this month" |
| Seasonal | Tied to in-game events | "Complete the Halloween event" |

### Feats

Feats are ultra-rare achievements that award no points. They exist purely for bragging rights. Feats are tracked separately and displayed on the player's profile. Examples: kill a boss in under 30 seconds, survive 1,000 days on hardcore, achieve a 1-tick action chain of 100+.

### Completionist

The completionist achievement requires completing every non-feat, non-time-gated achievement in the game. It is the final meta achievement. Worlds can configure whether completionist requires all categories or a subset.

---

## Module Toggles

Dungeon masters can enable or disable the following systems independently per world.

| Toggle | Default | Description |
|--------|---------|-------------|
| Achievement Points | On | Track and display point totals |
| Server First Tracking | On | Record and announce the first player to complete each achievement |
| Category Leaderboards | On | Per-category leaderboards ranked by points or completion count |
| Custom Achievements | Off | Allow the dungeon master to define custom achievements |
| Achievement Tiers | On | Bronze / Silver / Gold / Master tier labels on achievements |
| Hidden Achievements | On | Support for hidden achievements with prerequisites |
| Time-Gated Achievements | On | Support for daily, weekly, monthly, and seasonal achievements |
| Feats | On | Ultra-rare bragging-rights-only achievements |
| Completionist | On | Final meta achievement requiring full completion |

---

## Data Model

Each achievement record contains the following fields.

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier |
| name | string | Display name |
| description | string | What the player must do |
| category | string | One of the categories listed above |
| tier | string | Bronze, Silver, Gold, or Master |
| points | integer | Achievement points awarded on completion |
| hidden | boolean | Whether the achievement is hidden until prerequisites are met |
| prerequisites | array | List of achievement IDs or conditions required to reveal / attempt |
| time_gate | string or null | Daily, Weekly, Monthly, Seasonal, or null |
| feat | boolean | Whether this is a feat (no points, bragging rights only) |
| repeatable | boolean | Whether completion can be repeated (for time-gated achievements) |
| progress_type | string | Binary (done/not done) or Counter (X / target) |
| target | integer or null | Target count for counter-type achievements |
| reward | object or null | Optional reward on completion (item, XP, cosmetic, title) |

---

## Notification and Display

When a player completes an achievement, the system fires an event that can trigger:

- A chat message visible to the player (and optionally to nearby players or the server)
- A sound effect
- A visual popup with the achievement name, description, and points earned
- A server-wide broadcast for server firsts

The achievement panel in the UI shows all categories, completion percentage per category, total points, and a filterable list of all achievements (completed, in progress, locked, hidden).
