# Server Statistics and Voting

Game design document for server statistics, leaderboards, external voting, community polls, and feedback systems. Every subsystem is independently toggleable.

---

## 1. Server Stats Dashboard

Public-facing dashboard showing live and historical server health metrics.

### Population

| Metric                | Description                                          |
|-----------------------|------------------------------------------------------|
| Current Online        | Players connected right now                          |
| Peak Today            | Highest concurrent players today                     |
| Peak This Week        | Highest concurrent players this week                 |
| Peak All Time         | Highest concurrent players ever recorded             |
| Average Daily         | Average daily concurrent players (rolling 30 days)   |
| New Accounts (Week)   | Accounts created in the last 7 days                  |
| New Accounts (Month)  | Accounts created in the last 30 days                 |
| Total Registered      | All accounts ever created                            |
| Active (30-Day)       | Accounts that logged in within the last 30 days      |
| Retention Rate        | Percentage of new accounts still active after 30 days|

### Activity

| Metric                  | Description                                        |
|-------------------------|----------------------------------------------------|
| Total XP Today          | Combined XP earned by all players today            |
| Total XP This Week      | Combined XP earned by all players this week        |
| Total Boss Kills        | Boss kills across all players today                |
| Items Traded            | Number of item transactions today                  |
| GP in Circulation       | Total gold held by all players and in GE offers    |
| Quests Completed        | Total quest completions today                      |
| Most Popular Skill      | Skill with the most XP gained right now            |
| Most Popular Boss       | Boss with the most kills right now                 |
| Most Popular Area       | Region with the most players right now             |
| Server Uptime           | Percentage uptime over the last 30 days            |

### Economy

| Metric                    | Description                                      |
|---------------------------|--------------------------------------------------|
| GE Volume Today           | Total Grand Exchange transactions today          |
| Top Price Movers          | Items with the largest price changes today       |
| Inflation/Deflation       | Indicator based on GP supply vs item supply      |
| Gold Sink vs Source Ratio | Net gold entering vs leaving the economy         |

### Milestones

| Metric                | Description                                          |
|-----------------------|------------------------------------------------------|
| Recent Server Firsts  | First player to achieve X (max cape, boss KC, etc.)  |
| Fastest Max Cape      | Shortest time from account creation to max cape      |
| Highest Boss KC       | Highest kill count for any single boss               |
| Wealthiest Player     | Highest total wealth (opt-in only)                   |

---

## 2. Leaderboards / Hiscores

Ranked player listings across all measurable categories. Separate hiscores per game mode.

### Skills

| Category         | Description                                         |
|------------------|-----------------------------------------------------|
| Per-Skill Rank   | Ranking by XP in each individual skill              |
| Total Level      | Sum of all skill levels                             |
| Total XP         | Sum of all skill XP                                 |
| EHP              | Efficient Hours Played (XP divided by max rates)    |
| XP Gained (Week) | XP earned in the last 7 days                        |
| XP Gained (Month)| XP earned in the last 30 days                       |

### Combat

| Category              | Description                                     |
|-----------------------|-------------------------------------------------|
| Boss KC (Per Boss)    | Kill count for each specific boss               |
| Total Boss KC         | Combined kills across all bosses                |
| Fastest Kill          | Best time for each boss                         |
| Combat Achievements   | Total combat achievement points                 |
| PvP K/D              | Kill-to-death ratio in PvP                      |
| Slayer Streak         | Longest consecutive slayer task completion       |

### Collection

| Category         | Description                                         |
|------------------|-----------------------------------------------------|
| Log Completion % | Percentage of collection log slots filled           |
| Unique Items     | Total unique items obtained                         |
| Pet Count        | Number of skilling/boss pets obtained               |

### Economy

| Category              | Description                                     |
|-----------------------|-------------------------------------------------|
| Wealthiest (Opt-In)   | Highest total wealth, player must opt in        |
| Most GE Transactions  | Highest number of Grand Exchange trades         |
| Biggest Flip (Opt-In)  | Largest single profit from a GE buy-sell pair   |

### Minigames

| Category              | Description                                     |
|-----------------------|-------------------------------------------------|
| Per-Minigame Scores   | High score for each individual minigame         |
| LMS Wins              | Total Last Man Standing victories               |
| Raid Completions      | Total raid completions across all raids         |

### Clans

| Category              | Description                                     |
|-----------------------|-------------------------------------------------|
| Total XP              | Combined XP of all clan members                 |
| Total KC              | Combined boss KC of all clan members            |
| Member Count          | Number of active clan members                   |
| Achievement Points    | Clan-wide achievement score                     |
| Event Participation   | Number of clan events attended                  |
| War Record            | Clan vs clan win/loss record                    |

### Mode-Specific

| Category         | Description                                         |
|------------------|-----------------------------------------------------|
| Per-Mode Hiscores| Separate rankings for each game mode (ironman, etc.)|
| Seasonal/League  | Rankings for time-limited seasonal content          |
| Fresh Start      | Rankings for fresh start worlds                     |

### Hiscores Settings

| Setting              | Description                                        |
|----------------------|----------------------------------------------------|
| Opt-In / Opt-Out     | Players choose whether to appear on hiscores       |
| Anonymous Mode       | Appear on rankings with hidden username            |
| Display Top X        | Configurable number of entries shown (100, 500, etc.)|
| Update Frequency     | How often rankings recalculate (real-time, hourly, daily)|
| Historical Snapshots | Archived rankings at regular intervals             |

---

## 3. Server Voting (External)

RSPS-style external voting on toplist sites. Players vote to boost server visibility and earn rewards.

### Voting Rules

| Rule                | Description                                         |
|---------------------|-----------------------------------------------------|
| Cooldown            | Minimum time between votes (e.g. 12 hours, 24 hours)|
| Multiple Toplists   | Support voting on multiple toplist sites             |
| Anti-Cheat          | Verify votes are real via toplist callback/API       |
| Vote-to-Player Ratio| Track ratio to detect vote manipulation              |

### Vote Rewards

| Reward Type      | Description                                          |
|------------------|------------------------------------------------------|
| Cosmetics        | Untradeable cosmetic items                           |
| Small Lamp       | Minor XP lamp (configurable XP amount)               |
| Mystery Box      | Random reward box (configurable loot table)          |
| Vote Points      | Currency redeemable at a vote shop                   |

### Vote Streaks

| Mechanic         | Description                                          |
|------------------|------------------------------------------------------|
| Consecutive Days | Track daily voting streaks                           |
| Escalating Rewards| Streak multiplier increases reward quality           |
| Streak Reset     | Missing a day resets the streak to zero              |
| Streak Milestones| Bonus rewards at 7, 14, 30 day marks                |

### Vote Leaderboard

| Metric           | Description                                          |
|------------------|------------------------------------------------------|
| Total Votes      | Lifetime votes per player                            |
| Votes This Month | Monthly vote count per player                        |
| Current Streak   | Active consecutive day streak                        |

---

## 4. Community Voting (In-Game Polls)

In-game polling system for community-driven decisions. Configurable governance model.

### Poll Creation

| Field            | Description                                          |
|------------------|------------------------------------------------------|
| Title            | Poll question or topic                               |
| Description      | Detailed explanation of the proposal                 |
| Category         | Content update, balance change, rule change, cosmetic, event, mode proposal, or custom |
| Duration         | How long the poll stays open (configurable days)     |
| Poll Type        | Yes/No, multiple choice, ranked choice, approval, or budget allocation |
| Pass Threshold   | Required percentage to pass (50%, 66%, 75%, 80% -- configurable) |
| Binding          | Binding (DM must implement), advisory (DM considers), or hybrid |

### Poll Types

| Type                | Description                                       |
|---------------------|---------------------------------------------------|
| Yes / No            | Simple approve or reject                          |
| Multiple Choice     | Select one option from several                    |
| Ranked Choice       | Rank options by preference, instant runoff        |
| Approval Voting     | Approve any number of options, most approvals wins|
| Budget Allocation   | Distribute points across options (priority voting)|

### Eligibility

| Requirement      | Description                                          |
|------------------|------------------------------------------------------|
| Total Level      | Minimum total level to vote                          |
| Account Age      | Minimum account age to vote                          |
| Playtime         | Minimum hours played to vote                         |
| Vote Weight      | Equal weight for all voters, or weighted by criteria (toggle) |

### Governance Model

| Setting              | Description                                        |
|----------------------|----------------------------------------------------|
| Results Are Law      | DM is bound to implement passing polls             |
| Advisory             | DM considers results but has final say             |
| Hybrid               | Some polls binding, some advisory (per-poll toggle)|
| Veto Power           | DM can override but must explain publicly          |
| Minimum Turnout      | Poll is invalid if too few players vote            |
| Revote Cooldown      | Minimum time before the same topic can be re-polled|
| Amendment Proposals  | Players can propose modifications to existing polls|

### Transparency

| Setting              | Description                                        |
|----------------------|----------------------------------------------------|
| Vote Totals          | Current totals visible during voting (or hidden until close -- toggle) |
| Secret Ballot        | Per-player votes hidden from other players (toggle)|
| Voter List           | List of who voted is visible (toggle)              |
| Poll Archive         | All past polls and their results are browsable     |
| Dev Response         | DM must post a response after poll closes          |

---

## 5. Feedback System

Player-facing tools for reporting bugs, requesting features, and tracking server status.

### Reports and Requests

| Feature              | Description                                        |
|----------------------|----------------------------------------------------|
| Bug Reports          | Submit bugs with category, description, and steps to reproduce |
| Feature Requests     | Submit ideas with upvote/downvote from other players|
| Duplicate Detection  | Flag or merge similar reports and requests         |
| Status Tracking      | Each report has a status (new, acknowledged, in progress, resolved, wontfix) |

### Visibility

| Feature              | Description                                        |
|----------------------|----------------------------------------------------|
| Roadmap              | Public view of planned updates and priorities      |
| Changelog            | Log of all updates, patches, and hotfixes          |
| Known Issues         | Public list of confirmed bugs and workarounds      |
| Status Page          | Server status, scheduled maintenance, and incident history |

---

## 6. Module Toggles

Every subsystem can be independently enabled or disabled.

| Toggle                     | Default |
|----------------------------|---------|
| Server Stats Dashboard     | On      |
| Public Population Count    | On      |
| Hiscores (Per-Skill)       | On      |
| Hiscores (Boss)            | On      |
| Hiscores (Clan)            | On      |
| Hiscores (Mode-Specific)   | On      |
| Opt-In / Opt-Out           | On      |
| Anonymous Mode             | Off     |
| External Voting            | Off     |
| Vote Rewards               | Off     |
| Vote Streaks               | Off     |
| Community Polls            | On      |
| Binding Polls              | Off     |
| Advisory Polls             | On      |
| Veto Power                 | On      |
| Minimum Turnout            | On      |
| Secret Ballot              | On      |
| Bug Reports                | On      |
| Feature Requests           | On      |
| Roadmap                    | On      |
| Changelog                  | On      |

---

## 7. Views

| View                  | Description                                         |
|-----------------------|-----------------------------------------------------|
| Server Dashboard      | Live stats, population, economy indicators          |
| Hiscores              | Filterable by skill, boss, mode, clan, time range   |
| Active Polls          | Currently open polls with vote buttons              |
| Poll Archive          | Historical polls with results and dev responses     |
| Vote History          | Player's own voting history (polls and external)    |
| Bug Tracker           | Submitted bugs with status filters                  |
| Feature Request Board | Community ideas sorted by votes                     |
| Changelog / Roadmap   | Update history and planned work                     |

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---|---|
| Hiscore Rank Change Notifications | Alert players when their hiscore rank changes (moved up or down). Configurable per skill/category. Shows who passed you or who you passed. |
| Data Visualization Tools | Histograms, charts, and graphs for server-wide data. XP distribution, wealth distribution, most popular activities, player count trends. Interactive and filterable. |
| Client Network Diagnostics | Ping/latency monitoring with visual graph. Shows connection quality over time. Alerts on high latency spikes. Server response time tracking. |
| Client Performance Metrics | FPS counter, load times, memory usage, and rendering performance. Helps identify performance issues. Benchmark mode. |
| World Heatmap | Visual heatmap showing player density per world and per area. See where players congregate. Updated in real-time or periodically. |
