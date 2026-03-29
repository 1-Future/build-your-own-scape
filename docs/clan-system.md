# Clan System

Game design document for the clan system. Covers identity, governance, economy, events, progression, and social features. Every subsystem is independently toggleable.

---

## 1. Clan Settings

### Identity

| Field              | Description                                          |
|--------------------|------------------------------------------------------|
| Clan ID            | Unique internal identifier                           |
| Name               | Display name                                         |
| Tag                | Short tag shown next to member names                 |
| Description/Motto  | Free-text clan description                           |
| Creation Date      | Timestamp of clan creation                           |
| Icon/Emblem        | Customizable clan icon                               |
| Colors             | Primary and secondary colors                         |
| Cloak Design       | Clan cloak cosmetic applied to members               |
| Banner/Vexillum    | Deployable banner design                             |
| Recruitment Status | Open, Application, Invite-Only, or Closed            |

### Membership

| Setting              | Description                                                   |
|----------------------|---------------------------------------------------------------|
| Member Cap           | Configurable max members (default 500)                        |
| Min Requirements     | Total level, combat level, quest points, playtime             |
| Application System   | Free join, apply-and-approve, or invite only                  |
| Guest Access         | Allow non-members to visit clan hall or join clan chat         |
| Multi-Clan           | Toggle whether members can belong to multiple clans (up to X) |
| Alliance System      | Formal alliance links between clans                           |

---

## 2. Rank System

Fully customizable rank hierarchy. Each rank has its own permission set.

### Rank Definition

| Field    | Description                             |
|----------|-----------------------------------------|
| Rank ID  | Unique identifier                       |
| Name     | Fully customizable display name         |
| Order    | Numeric position in hierarchy           |
| Icon     | Custom icon per rank                    |

### Permissions Per Rank

| Permission              | Type                              |
|-------------------------|-----------------------------------|
| Invite                  | Yes / No                          |
| Kick                    | Yes / No                          |
| Promote / Demote        | Yes / No                          |
| Access Coffer           | Yes / No                          |
| Withdraw                | Yes / No + daily limit            |
| Edit Settings           | Yes / No                          |
| Schedule Events         | Yes / No                          |
| Post Announcements      | Yes / No                          |
| Manage Alliances        | Yes / No                          |
| Access Clan Hall Rooms  | Yes / No                          |
| Use Clan Bank           | Yes / No                          |
| Start Polls             | Yes / No                          |
| Manage Challenges       | Yes / No                          |
| Mute Members            | Yes / No                          |
| Access Leadership Tools | Yes / No                          |

### Auto-Rank Rules

Auto-promote members when they reach configurable thresholds:

| Metric        | Example Threshold     |
|---------------|-----------------------|
| Total Level   | 1500+                 |
| Playtime      | 100+ hours            |
| Contributions | 10M+ GP donated       |

---

## 3. Clan Hall / Citadel

Tiered upgrade system. DM/game defines available tiers. Each tier has upgrade requirements.

### Rooms

| Room             | Purpose                                            |
|------------------|----------------------------------------------------|
| Meeting Hall     | Central gathering space                            |
| Bank             | Shared clan bank access point                      |
| Throne Room      | Leader seat, ceremony space                        |
| Trophy Room      | Display achievements and rare drops                |
| Armory           | Shared gear storage and lending                    |
| Skill Plots      | Skilling stations for members                      |
| Training Ground  | Combat practice area                               |
| Battlefield      | Internal clan PvP arena                            |
| Garden / Farm    | Farming and herb patches                           |
| Portal Room      | Configurable teleport destinations                 |
| Party Room       | Drop parties and social events                     |
| War Room         | War planning, strategy, and history                |
| Museum           | Collection log display, first-to-achieve wall      |

### Upkeep

| Mechanic          | Description                                        |
|-------------------|----------------------------------------------------|
| Room Customization| Furniture, layout, and cosmetic options per room    |
| Upkeep Cost       | Weekly resource/GP cost to maintain current tier    |
| Decay             | Citadel drops a tier if upkeep is not met           |

---

## 4. Clan Economy

### Clan Coffer

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Shared GP Pool       | Central gold fund for the clan                   |
| Deposit Limits       | Per-rank daily deposit caps                      |
| Withdrawal Limits    | Per-rank daily withdrawal caps                   |

### Clan Bank

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Shared Items         | Item storage accessible by permitted ranks       |
| Clan Shop            | Internal marketplace for members                 |

### Tax System (Toggle)

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Tax Rate             | Configurable percentage of member earnings       |
| Tax-Free Transfers   | Internal clan transfers exempt from tax           |

### Additional Economy

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Resource Pool        | Shared raw materials for clan crafting            |
| Clan Currency        | Internal points earned through participation     |
| Clan Salary          | Automated payouts to members (toggle)            |

---

## 5. Clan Events

### Event Definition

| Field            | Description                                          |
|------------------|------------------------------------------------------|
| Event ID         | Unique identifier                                    |
| Name             | Display name                                         |
| Description      | Free-text description                                |
| Type             | Boss night, skilling competition, PvP tournament, hide and seek, drop party, custom |
| Schedule         | Date, time, recurring toggle                         |
| Notifications    | Alerts at 60 / 30 / 15 / 5 minutes before start     |
| RSVP             | Members confirm attendance                           |
| Min / Max        | Participant count limits                             |
| Rewards          | Prizes or payouts                                    |
| Leaderboard      | Per-event ranking                                    |

### Activity Types

#### Bingo

| Setting        | Options                                              |
|----------------|------------------------------------------------------|
| Board Size     | 3x3, 4x4, 5x5                                       |
| Tile Types     | Boss KC, skilling, collection, custom                |
| Win Condition  | Line, blackout, X-pattern, custom                    |
| Mode           | Solo or team                                         |
| Time Limit     | Configurable duration                                |
| Proof System   | Screenshot or automatic detection                    |
| Prize Pool     | GP, items, or clan currency                          |
| Templates      | Reusable board presets                               |

#### Competitions and Races

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Skill of the Week | Highest XP gain in a chosen skill                 |
| Boss KC Race      | Most kills of a target boss                       |
| GP Race           | Most gold earned in a period                      |
| Collection Race   | Fill a target collection first                    |
| Speedrun          | Fastest completion of a challenge                 |
| Level Race        | First to reach a target level                     |
| Custom Metric     | Any trackable stat                                |

#### Group PvM

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Scheduled Trips   | Calendar-based boss runs                          |
| Learner Runs      | Teaching sessions for new PvM players             |
| Mass Events       | Large-group boss encounters                       |
| Split Tracker     | Loot split logging and fairness tracking          |
| KC Leaderboard    | Running kill count rankings                       |

#### PvP Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Clan Wars         | Organized clan-vs-clan fights                     |
| Internal Tournaments | Bracket-style intra-clan PvP                  |
| Wilderness Trips  | Group PKing excursions                            |
| LMS              | Last Man Standing events                           |
| Fight Pit         | Free-for-all arena                                |
| Risk Fights       | Wagered PvP                                       |

#### Social Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Drop Parties      | Item giveaways                                    |
| Fashion Shows     | Outfit competitions                               |
| House Parties     | Player-owned house gatherings                     |
| Hide and Seek     | World-based hiding game                           |
| Treasure Hunts    | Clue-based exploration                            |
| Scavenger Hunts   | Item collection challenges                        |
| Trivia            | Knowledge-based Q&A                               |
| Bank Standing     | AFK endurance contests                            |

#### Skilling Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Group Sessions    | Coordinated skilling at a location                |
| Relays            | Multi-skill team relay races                      |
| Efficiency Challenges | Max XP/hr competitions                       |
| Resource Drives   | Clan-wide material collection goals               |

#### Clan Challenges

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Clan-Wide Goals   | Collective targets (e.g. 1B total XP this week)  |
| Donation Drives   | Fundraising for clan coffer or bank               |
| Milestones        | Tracked collective achievements                   |

#### Drafts

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Blind Draft       | Random assignment of tasks or targets             |
| Snake Draft       | Alternating pick order                            |
| Fantasy League    | Pick players, score based on their performance    |

#### Relay Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Production Relay  | Chain crafting across team members                |
| Boss Relay        | Rotate boss kills across a team                   |
| Skill Relay       | Each member levels a different skill segment      |

#### Survival Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Wilderness Survival | Survive as long as possible in the wild        |
| Ironman Race      | Fresh start race with ironman restrictions        |
| Hunger Games      | PvP elimination in a restricted area              |
| Escape Room       | Puzzle-based timed challenge                      |

#### Economy Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Flip Competition  | Most profit from flipping in a time window        |
| Merching Challenge| Buy low, sell high challenge                      |
| Auction House     | Internal item auctions                            |
| Secret Santa      | Anonymous gift exchange                           |
| Clan Stock Market | Simulated stock trading game                      |

#### Creative Events

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| POH Contest       | Best player-owned house design                    |
| Fashion Show      | Best outfit competition                           |
| Storytelling      | Roleplaying or lore writing                       |
| Map Art           | Pixel art using in-game tiles                     |

#### Mentorship

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Buddy System      | Pair new members with veterans                    |
| Graduation        | Recognition when mentee hits milestones           |
| Teaching Sessions | Scheduled skill or boss tutorials                 |
| Mentor XP Bonus   | Bonus XP for mentoring activities                 |

#### Clan Wars (Expanded)

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Bingo vs Bingo    | Competing bingo boards between clans              |
| KC Race           | Cross-clan boss kill race                         |
| XP Race           | Cross-clan XP gain race                           |
| Minigame Tournament | Bracket across multiple minigames              |
| Alliance Wars     | Multi-clan coalition battles                      |
| Season-Long League| Extended league format with standings             |

#### Gambling (Toggle)

| Type              | Description                                       |
|-------------------|---------------------------------------------------|
| Prediction Markets| Bet on outcomes of events                         |
| Boss Drop Pools   | Pool GP, winner gets the rare drop payout         |
| Duel Tournaments  | Wagered 1v1 bracket                               |
| Raffle            | Ticket-based random draw                          |

---

## 6. Clan Achievements

Every achievement has four tiers: **Bronze, Silver, Gold, Master**.

### Categories

#### Milestone Achievements

| Achievement      | Metric                                             |
|------------------|----------------------------------------------------|
| Total XP         | Cumulative clan XP milestones                      |
| Total GP         | Cumulative clan wealth milestones                  |
| Boss KC          | Total boss kills across all members                |
| Deaths           | Total deaths (for humor or endurance tracking)     |
| Hours Played     | Cumulative playtime                                |
| Member Count     | Peak or sustained member count                     |
| Clan Age         | Time since creation                                |

#### First Achievements

| Achievement      | Description                                        |
|------------------|----------------------------------------------------|
| First Max        | First member to max all skills                     |
| First Pet        | First pet drop in clan                             |
| First Rare Drop  | First rare item drop in clan                       |
| First Quest Cape | First member to complete all quests                |

#### Collective Achievements

| Achievement                  | Description                                |
|------------------------------|--------------------------------------------|
| Every Member Has a 99        | All active members have at least one 99    |
| Every Member Completed Quest | All members finish a specific quest        |
| Kill Every Boss              | At least one kill on every boss            |
| Fill Clan Collection Log     | Complete the shared collection log         |
| All Members Online           | Every member online simultaneously         |

#### Competitive Achievements

| Achievement          | Description                                    |
|----------------------|------------------------------------------------|
| Win X Wars           | Accumulate clan war victories                  |
| Win X Bingo          | Win bingo events                               |
| Hold Territory X Days| Maintain territory control streak              |
| Top X Leaderboard    | Reach a position on the server leaderboard     |

#### Economy Achievements

| Achievement          | Description                                    |
|----------------------|------------------------------------------------|
| Coffer Reaches X     | Clan coffer balance milestones                 |
| Donations Hit X      | Total donation volume milestones               |
| Bank Holds X Uniques | Unique item count in clan bank                 |
| Fund Gear Sets       | Provide full gear sets to X members            |

#### Social Achievements

| Achievement                  | Description                                |
|------------------------------|--------------------------------------------|
| Host X Events                | Total events organized                     |
| X Members Attend One Event   | Single event attendance record             |
| Consecutive Event Days       | Streak of days with at least one event     |
| Every Member in an Event     | 100% participation in a single event       |
| Voice Chat Hours             | Cumulative voice time                      |
| Secret Santa Full Participation | Every member participates in exchange   |

#### Event-Specific Achievements

| Achievement                  | Description                                |
|------------------------------|--------------------------------------------|
| Bingo Blackout               | Complete every tile on a bingo board       |
| 100% Skill of Week Participation | Every member participates in SotW     |
| Full Relay Completion        | Complete a relay with no missed legs       |
| Survival Top 3 Same Squad    | Same squad places top 3 in survival       |

### Prestige Rewards

| Reward                     | Description                                  |
|----------------------------|----------------------------------------------|
| Clan Titles                | Exclusive titles for members                 |
| Trophy Room Displays       | Physical display in citadel trophy room      |
| Banner Upgrades            | Enhanced banner/vexillum designs             |
| Cloak Cosmetic Tiers       | Visual upgrades to clan cloak                |
| Clan Emote Unlocks         | Custom emotes for clan chat                  |
| Server-Wide Announcements  | Broadcast achievement to the game world      |

---

## 7. Clan Avatar / Mascot

| Feature               | Description                                       |
|------------------------|---------------------------------------------------|
| Clan Summon            | Follows members in the world                      |
| Passive Buffs          | XP boost, drop rate, skilling boost (configurable)|
| Upkeep Cost            | Resource drain to maintain the avatar             |
| Customizable Appearance| Visual design tied to clan identity               |
| Active Limit           | Only X avatars active at once across the clan     |

---

## 8. Clan Wars

| Feature           | Description                                         |
|-------------------|-----------------------------------------------------|
| Challenge         | Issue and accept challenges to other clans          |
| War Rules         | Best of X, team size, gear restrictions, arena      |
| War History       | Win/loss record per opponent                        |
| Ranked Wars       | ELO or ladder-based matchmaking                     |
| War Rewards       | Cosmetics, trophies, clan currency                  |
| Spectating        | Non-participants can watch live                     |

---

## 9. Territory Control

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Claim Locations      | Clans claim specific world locations              |
| Resource Generation  | Controlled territory produces resources or tax    |
| Territory Wars       | Scheduled PvP to contest ownership               |
| Control Buffs        | Passive bonuses for members in owned territory    |
| Max Territories      | Cap on territories per clan                       |

---

## 10. Clan Communication

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Clan Chat            | General clan channel                             |
| Officer Chat         | Leadership-only channel                          |
| Alliance Chat        | Cross-clan channel for allied clans              |
| Announcement Board   | Pinned messages visible to all members           |
| Polls / Votes        | Democratic decision-making system                |
| Broadcasting         | Auto-announce member achievements (toggle per type)|
| MOTD                 | Message of the day on login                      |

---

## 11. Additional Features

### Guild Finder

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Searchable Directory | Browse all clans on the server                   |
| Filters              | Size, activity, focus, requirements              |
| Activity Score       | Calculated metric for clan health                |
| Trial / Guest Period | Temporary membership before committing           |
| Application Questions| Custom questions for applicants                  |
| Anti-Spam            | Rate limits on applications                      |

### Sub-Groups / Squads

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Teams Within Clan    | Named sub-groups for organization                |
| Squad Chat           | Private channel per squad                        |
| Squad Leaders        | Designated lead per squad                        |
| Squad Scheduling     | Independent event calendar per squad             |

### Clan Progression

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Clan Level           | Earned from aggregate member activity            |
| Level Unlocks        | Features or perks gated behind clan level        |
| Clan Skill Tree      | Specialization paths (PvM, skilling, PvP, social)|
| Daily / Weekly Tasks | Clan-wide task board with rewards                |
| Clan Milestones      | Major progression checkpoints                    |

### Activity and Contribution Tracking

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Per-Member Score     | Individual contribution metric                   |
| Activity Dashboard   | Visual overview of clan engagement               |
| Auto-Kick Inactive   | Remove members after X days offline (toggle)     |
| Contribution Requirements | Minimum activity to maintain rank            |
| Internal Leaderboard | Rank members by contribution                     |

### Guild Crafting

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Clan-Exclusive Recipes | Items only craftable by clan members           |
| Clan Gear            | Branded equipment with clan cosmetics            |
| Clan Crafting Stations | Special stations in the citadel                |

### Conflict Resolution

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Anonymous Polls      | Vote on disputes without revealing identity      |
| Kick Cooldown        | Mandatory delay before a kick takes effect       |
| Demotion Appeal      | Process for contesting rank changes              |
| Officer Vote for Kicks | Require majority officer approval for kicks    |

### Cross-Clan Systems

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Alliance System      | Formal pacts between clans                       |
| Alliance Chat        | Shared communication channel                     |
| Alliance Wars        | Multi-clan coalition combat                      |
| Federation / Coalition | Larger groupings of allied clans               |
| Rivalry System       | Declared rivals with bonus PvP rewards           |

### Lending

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Time-Limited Lending | Loan items with automatic return timer           |
| Auto-Return          | Items return to lender when timer expires         |
| Lender Recall        | Lender can reclaim items early                   |
| Insurance            | Optional protection against loss                 |
| Lending History      | Full audit trail of all loans                    |
| Limits Per Rank      | Rank-based caps on lending                       |

### Donations

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Item / GP Donations  | Contribute to clan bank or coffer                |
| Donation Tracking    | Per-member lifetime donation totals              |
| Donation Leaderboard | Rank members by generosity                       |
| Tax-Free Transfers   | Internal clan trades exempt from tax             |
| Donation Goals       | Target amounts for specific fundraising drives   |

### Proximity XP Boost

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Bonus Near Clanmates | Extra XP when training near clan members         |
| Scales With Count    | More nearby members increases the bonus          |
| Avatar Range         | Clan avatar extends the proximity radius         |
| DM Sets Percentage   | Configurable bonus rate                          |
| Toggle Per Activity  | Enable or disable for specific skills            |

### Clan Buffs

| Feature              | Description                                      |
|----------------------|--------------------------------------------------|
| Active Member Bonus  | Bonus scaling with online member count           |
| Event Participation  | Bonus for attending scheduled events             |
| Clan Streak Bonus    | Consecutive day activity bonus                   |
| Territory Bonus      | Passive buff from controlled territories         |

---

## 12. Module Toggles

Every subsystem can be independently enabled or disabled by clan leadership.

| Module                | Default |
|-----------------------|---------|
| Rank System           | On      |
| Clan Hall / Citadel   | On      |
| Coffer / Economy      | On      |
| Clan Bank             | On      |
| Tax System            | Off     |
| Event System          | On      |
| Clan Achievements     | On      |
| Clan Avatar           | Off     |
| Clan Wars             | On      |
| Alliance System       | Off     |
| Multi-Clan            | Off     |
| Broadcasting          | On      |
| Polls / Voting        | On      |
| Ranked Wars           | Off     |
| Clan Salary           | Off     |
| Auto-Rank             | Off     |
| Territory Control     | Off     |
| Guild Finder          | On      |
| Sub-Groups            | Off     |
| Clan Progression      | On      |
| Contribution Tracking | On      |
| Guild Crafting        | Off     |
| Lending               | Off     |
| Donations             | On      |
| Proximity XP Boost    | Off     |

---

## 13. Views

### Members View

Roster with columns for name, rank, last online, total level, combat level, contribution score, join date.

### Events View

Upcoming events with RSVP status, past events with results, recurring event calendar.

### Economy View

Coffer balance, recent deposits and withdrawals, transaction history, donation leaderboard.

### Achievements View

Progress on milestones, firsts wall, competitive standings, tier progress per achievement.

### Wars View

Win/loss record, ELO rating, war history with details, rival clan standings.

### Settings View

All configurable options from Sections 1-2, module toggles, rank permissions editor, auto-rank rule builder.

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---------|-------------|
| Per-Member Combat Analytics | Track KDR, damage dealt, damage taken, healing done, and deaths per clan member during clan wars and group PvM. Leaderboard and historical comparison. |
| Clan Data Export | Export clan member list, stats, contributions, and activity data to CSV/JSON. Useful for external tracking tools and spreadsheets. |
| Clan-Wide Shared Goals | Set goals for the entire clan with shared progress bars. "Kill 10,000 dragons as a clan" or "Earn 1B total XP this month." Every member's contribution counts. |
| Group Attendance Tracking | Sign-up, check-in, and participation tracking for events. Shows who RSVP'd, who showed up, and participation rate per member over time. |
| Shared Item Visibility | View what items group/clan members have across their banks and inventories. Useful for coordinating gear and supplies for group content. |
| Shared Map Markers | Party/clan members can place markers on the world map visible to each other. Useful for coordination during events, bossing, and exploration. |
| Loot Broadcast Celebrations | Clan-wide broadcast when a member receives a rare drop with celebration effects (fireworks, sound, special chat message). Configurable rarity threshold. |
