# Collection Log

The collection log tracks every unique item a player has obtained, organized by source and category. It serves as a long-term completionist goal, providing visibility into what has been found, what remains, and how close a player is to full completion.

---

## Item Registry

Each item in the collection log is defined by the following fields.

| Field            | Type    | Description                                                                 |
|------------------|---------|-----------------------------------------------------------------------------|
| ID               | integer | Unique collection log entry ID (auto-incremented)                           |
| Item Name        | string  | Display name of the item                                                    |
| Item ID          | integer | Foreign key linking to the equipment/items table                            |
| Source           | string  | Where the item is obtained (boss name, clue tier, minigame, skill, etc.)    |
| Category         | enum    | Top-level grouping: Bosses, Raids, Clues, Minigames, Skilling, Other       |
| Obtained Count   | integer | How many times the player has received this item (starts at 0)              |
| Count Cap        | integer | Maximum tracked count per item (0 = unlimited). Configurable per dungeon.   |
| Duplicate Entry  | boolean | Whether this item also appears in another category's log                    |

A single item can appear in multiple categories. For example, a skilling resource dropped by a boss would have entries under both Bosses and Skilling. The `Duplicate Entry` flag marks these cross-listings so completion calculations can avoid double-counting.

---

## Completion System

### Slot Tracking

Completion is measured by the number of unique slots filled (obtained at least once), not by obtained count. A slot is filled when `Obtained Count >= 1` for that entry.

| Metric          | Description                                          |
|-----------------|------------------------------------------------------|
| Total Slots     | Sum of all unique collection log entries              |
| Filled Slots    | Entries where `Obtained Count >= 1`                   |
| Completion %    | `(Filled Slots / Total Slots) * 100`                  |

### Milestone Thresholds

Default milestone thresholds are listed below. Dungeon masters can add, remove, or modify thresholds through server configuration.

| Milestone | Default Slots Required |
|-----------|------------------------|
| 1         | 100                    |
| 2         | 300                    |
| 3         | 500                    |
| 4         | 700                    |
| 5         | 900                    |
| 6         | 1000                   |
| 7         | 1100                   |
| 8         | 1200                   |
| 9         | 1525                   |

### Milestone Rewards

Each milestone unlocks a reward. Reward type is configurable per milestone.

| Reward Type | Description                                                      |
|-------------|------------------------------------------------------------------|
| Cosmetic    | Visual override (armor recolor, aura, particle effect)           |
| Title       | Chat title displayed before/after player name                    |
| Item        | Functional or cosmetic item granted to inventory                 |

Rewards are granted once per milestone. If a dungeon master lowers a threshold after a player has already passed it, the reward is granted retroactively on next login.

---

## Module Toggles

These toggles control which collection log features are active on a given server. All are independently configurable by the dungeon master.

| Toggle                    | Default | Description                                                        |
|---------------------------|---------|--------------------------------------------------------------------|
| Kill Count Display        | On      | Show kill count per source alongside item entries                  |
| Duplicate Entries         | On      | Allow the same item to appear in multiple categories               |
| Count Cap                 | Off     | Enforce a maximum tracked count per item (uses each entry's cap)   |
| Milestone Rewards         | On      | Grant rewards when milestone thresholds are reached                |
| Broadcast on Rare Obtain  | On      | Send a server-wide announcement when a rare item is obtained       |

When `Duplicate Entries` is toggled off, each item appears only in its primary category. Primary category is determined by the source that is listed first in the item registry.

When `Count Cap` is toggled on, obtained counts stop incrementing at the entry's configured cap. The item still drops normally; only the counter stops.

Broadcast rarity is determined by the item's drop rate as defined in the monster or source loot table. The rarity threshold for triggering a broadcast is configurable (default: 1/256 or rarer).

---

## Views

The collection log UI supports the following view modes.

| View              | Description                                                              |
|-------------------|--------------------------------------------------------------------------|
| All               | Every entry in the log, sorted by ID                                     |
| By Category       | Entries grouped under Bosses, Raids, Clues, Minigames, Skilling, Other   |
| By Completion %   | Categories or sources ranked by percentage of slots filled               |
| Missing Items     | Only entries where `Obtained Count = 0`                                  |
| By Source          | Entries grouped by specific source (individual boss, clue tier, etc.)    |

### By Category Breakdown

| Category   | Typical Sources                                                  |
|------------|------------------------------------------------------------------|
| Bosses     | Named boss monsters, world bosses                                |
| Raids      | Multi-boss encounters, raid-exclusive drops                      |
| Clues      | Easy, Medium, Hard, Elite, Master clue scroll reward pools       |
| Minigames  | Minigame-specific reward shops and drop tables                   |
| Skilling   | Rare skilling drops, skilling pet rolls, tool upgrades           |
| Other      | Event items, achievement rewards, miscellaneous sources          |

Within each view, entries display: item icon, item name, obtained count (or count/cap if capped), and a visual indicator for obtained vs. missing (filled vs. grayed out).
