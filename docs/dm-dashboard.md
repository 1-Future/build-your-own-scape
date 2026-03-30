# Dungeon Master Dashboard

The admin control panel for managing the game world. Every other doc describes what CAN be configured -- this doc describes the tool the DM uses to DO the configuring.

---

## 1. Overview Panel

The landing page when the DM opens the dashboard. At-a-glance health of the server.

| Widget | Description |
|---|---|
| Server status | Online/offline indicator, uptime counter, build version |
| Online players | Current count and scrollable list of connected players |
| Peak players | Peak count for today, this week, and all time |
| Server performance | CPU usage, memory usage, tick rate consistency graph |
| Active events | List of currently running world events |
| Recent alerts | Notification feed of flagged activity, crashes, economy anomalies |
| Quick actions | Restart server, broadcast message, enable maintenance mode |

---

## 2. Player Management

Tools for finding, inspecting, and acting on individual player accounts.

### Player Search

| Feature | Description |
|---|---|
| Search by name | Partial or exact match on display name |
| Search by IP | Find all accounts associated with an IP address |
| Search by account ID | Direct lookup by unique account identifier |
| Search by item | Find all players holding a specific item |

### Player Profile Viewer

| Section | Description |
|---|---|
| Stats | All skill levels, total XP, combat level |
| Bank | Full bank contents with value estimate |
| Equipment | Currently equipped items |
| History | Login/logout times, actions taken, trades completed |
| Notes | DM-written notes attached to the account |

### Player Action Tools

| Action | Description |
|---|---|
| Kick | Disconnect the player immediately |
| Ban | Ban the account (temporary or permanent, with reason) |
| Mute | Prevent the player from sending chat messages |
| Freeze | Lock the player in place (cannot move, can still chat) |
| Teleport-to | Move the DM to the player's current location |
| Teleport-here | Move the player to the DM's current location |
| Spectate | Observe the player's screen and actions invisibly |

### Account Actions

| Action | Description |
|---|---|
| Reset password | Force a password reset on the account |
| Change mode | Switch the account's game mode (with confirmation) |
| Convert account | Convert between account types (e.g. ironman to main) |
| Rollback | Restore the account to a previous backup state |
| Merge | Combine two accounts into one (transfers stats and items) |

### Bulk Tools

| Tool | Description |
|---|---|
| Player comparison | Compare two players side by side (stats, bank, history) |
| Mass mute | Mute all players currently online |
| Mass kick | Kick all players currently online |
| Broadcast to all | Send a system message to every connected player |
| Online player list | Sortable table with location, activity, combat level, playtime |
| Suspicious activity list | Auto-flagged accounts from bot detection and economy scanner |

---

## 3. Economy Panel

Monitoring and intervening in the game economy.

### Economy Overview

| Widget | Description |
|---|---|
| Money supply graph | Total GP in circulation over time |
| Inflation/deflation rate | Percentage change in money supply over configurable period |
| GE volume | Total trades and most traded items on the Grand Exchange |
| Price alerts | Items that have hit DM-configured price thresholds |
| Item sink stats | Count and value of items removed from the game this week |
| Gold sink stats | Total GP removed from the game this week |
| Wealth distribution | Breakdown by top 1%, median, and bottom 50% |
| Economy health score | A-F grade calculated from supply, distribution, and velocity |

### Economy Tools

| Tool | Description |
|---|---|
| Trade log viewer | Search trades by player, item, value, or date range |
| Adjust prices | Manually set GE guide prices for specific items |
| Add items | Inject items into the economy (NPC shops, drops, events) |
| Remove items | Remove items from circulation (emergency sink) |
| Emergency tax | Temporarily adjust GE tax rate to correct imbalance |

---

## 4. Content Management

Creating and editing all game content from the dashboard.

### Editors

| Editor | Description |
|---|---|
| Quest editor | Create and edit quests with a step-by-step builder |
| NPC editor | Configure NPC dialogue, shops, services, and spawn locations |
| Item editor | Create and modify items, set all item properties |
| Monster editor | Create and modify monsters, set stats, drops, and spawn rules |
| Shop editor | Manage shop stock, prices, and restock rates |
| Achievement editor | Create custom achievements with conditions and rewards |

### Management Tools

| Tool | Description |
|---|---|
| Event scheduler | Schedule world events, holidays, and seasonal content |
| Mode manager | Create and configure game modes (ironman, PvP, custom) |
| Location manager | Set zone properties, access rules, and area effects |
| Plugin manager | Enable, disable, and configure plugins from the plugin registry |

---

## 5. Moderation Queue

Processing player reports and managing the moderation team.

### Report Handling

| Feature | Description |
|---|---|
| Active reports | Sorted by priority (severity, age, reporter trust level) |
| Report review | View evidence, player history, chat logs, and bot score |
| Quick actions | Warn, mute, ban, or dismiss directly from the report |
| Appeal queue | Pending appeals with full context and original action details |

### Mod Team Management

| Feature | Description |
|---|---|
| Mod action log | Audit trail of every action taken by every moderator |
| Role assignment | Assign DM roles and permission levels to team members |
| Permission editor | Define what each role can see and do in the dashboard |

### Auto-Moderation

| Setting | Description |
|---|---|
| Keyword filters | Block or flag messages containing specific words |
| Spam thresholds | Max messages per interval before auto-mute |
| Bot score thresholds | Score levels that trigger auto-flag, auto-mute, or auto-ban |
| Report threshold | Number of reports on a player before auto-flag |

---

## 6. Analytics

Understanding player behavior and content engagement.

### Player Analytics

| Metric | Description |
|---|---|
| Retention rates | Day 1, day 7, and day 30 retention percentages |
| Session duration | Distribution of how long players stay per session |
| Peak hours heatmap | Activity by hour and day of week |
| New player funnel | Tutorial start to completion to day 7 active conversion |

### Content Analytics

| Metric | Description |
|---|---|
| Popular skills | Most trained skills by total XP and by unique players |
| Popular bosses | Most killed bosses by kill count and unique players |
| Popular quests | Most started and most completed quests |
| Feature engagement | Which features are actively used vs ignored |

### Technical Analytics

| Metric | Description |
|---|---|
| Error/crash log | Timestamped log of server errors and client crashes |
| Performance over time | CPU, memory, tick rate, and connection count graphs |
| Latency distribution | Player ping distribution and outliers |

---

## 7. Communication Tools

Reaching players inside and outside the game.

| Tool | Description |
|---|---|
| Server broadcast | Send a message to all online players |
| Targeted message | Send a message to a specific player or group |
| MOTD editor | Edit the message of the day shown on login |
| News/update editor | Write patch notes and announcements |
| Community poll creator | Create in-game polls for player feedback |
| Event announcement scheduler | Queue announcements for upcoming events |
| Social media integration | Post to Discord, Twitter, or other platforms from the dashboard |

---

## 8. Backup and Recovery

Protecting world data and recovering from disasters.

| Feature | Description |
|---|---|
| Manual backup | Trigger a full backup on demand |
| Automatic backup schedule | Configurable interval for automatic backups |
| Backup history | Browse all previous backups with timestamps and sizes |
| Restore from backup | Full or partial restore (world data, player data, economy data) |
| Export world | Package the entire world for sharing or migration |
| Import world | Load a world from an exported package |

---

## 9. Module Toggles

Every panel can be shown or hidden based on DM rank.

| Rank | Default Access |
|---|---|
| Junior DM | Moderation queue only |
| Senior DM | Moderation, economy panel, content editors |
| Owner | Full access to all panels |
| Custom | DM-defined permission sets per role |

---

## 10. Admin Views

| View | Description |
|---|---|
| Overview dashboard | Server health and quick actions |
| Player management | Search, inspect, and act on player accounts |
| Economy panel | Monitor and intervene in the economy |
| Content editors | Quest, NPC, item, monster, shop, achievement editors |
| Moderation queue | Reports, appeals, mod team management |
| Analytics | Retention, engagement, performance metrics |
| Communication | Broadcasts, MOTD, news, polls |
| Backup/recovery | Backup, restore, export, import |
| Plugin registry | Browse and manage installed plugins |
| Settings | Dashboard preferences, notification rules, theme |
