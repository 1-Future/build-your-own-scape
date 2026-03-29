# Bot Detection

Game design document for bot detection, prevention, and policy management.

---

## Bot Policy Toggle

The DM (server owner) selects one policy for their world. Each policy changes which detection and enforcement systems are active.

| Policy | Description |
|---|---|
| No Bots | Full detection enabled, auto-ban on confirmation |
| Bots Allowed | All detection disabled, bots play freely |
| Licensed Bots | Bots must register with the DM, get a visible bot tag, play by rules |
| Bot Zones | Bots allowed only in designated areas, banned elsewhere |
| AI Companions | Players run a personal AI alongside their character (sanctioned use) |
| Bot Economy | Separate economy for bot-produced vs human-produced goods |
| Custom | Mix and match any combination of the above |

---

## 1. Detection Methods

### Behavioral Signals

| Signal | Description |
|---|---|
| Click pattern analysis | Inhuman consistency in click timing and placement |
| Tick-perfect actions | Performing actions on exact game ticks over extended periods |
| Session length | Continuous play sessions of 20+ hours flagged |
| No camera movement | Camera remains static during gameplay |
| Identical pathing | Repeated identical movement paths with no deviation |
| No social interaction | No chat messages, no trades initiated, no emotes |
| Mouse movement | Straight-line mouse paths instead of natural curves |
| Reaction time | Responses too fast or too consistent to be human |
| No breaks | No idle time, no pauses, no AFK periods |

### Economic Signals

| Signal | Description |
|---|---|
| Repetitive gathering | High-volume resource gathering with no variety |
| Immediate sell patterns | Gathered items sold instantly with no storage or use |
| GP transfer to main | Gold moved from alt accounts to a single main account |
| Multi-account same activity | Multiple accounts on the same IP performing identical tasks |
| New account efficiency | New account immediately operating at peak efficiency |
| No progression variety | Account only trains one skill or repeats one activity |

### Account Signals

| Signal | Description |
|---|---|
| No learning curve | New account with no mistakes, no exploration, no wasted actions |
| Hardware ID overlap | Multiple accounts tied to the same hardware fingerprint |
| Random letter names | Account names matching bot naming patterns (e.g. xkjf82ql) |
| No tutorial engagement | Tutorial skipped or completed with zero interaction |
| No quest progress | Account has high skill levels but zero quest completions |

---

## 2. Anti-Bot Measures

### Passive Measures

| Measure | Description |
|---|---|
| Random events | NPC interrupts the player mid-activity, requires response |
| CAPTCHA | Occasional challenge during repetitive actions (toggle per world) |
| Heuristic scoring | Bot score calculated from 0-100 based on weighted signals |
| Shadow flagging | Suspected account is monitored silently without disruption |

### Active Measures

| Measure | Description |
|---|---|
| Honeypot resources | Fake resource nodes placed that only bots interact with |
| Admin whisper test | Staff sends a direct message to test for human response |
| Behavior challenge | System prompts the player to perform an unusual or unexpected action |
| Pattern disruption | Resource node locations and spawn times randomized to break scripts |

---

## 3. Bot Score System

Each account maintains a bot score from 0 to 100.

| Component | Description |
|---|---|
| Weighted signals | Each detection method contributes a weighted value to the total score |
| Thresholds | DM configures score thresholds for flag, mute, and ban actions |
| Score decay | Score decreases over time when the account exhibits normal behavior |
| False positive protection | High scores trigger manual review, not instant punishment |
| Appeal fast-track | Flagged players can appeal, which routes to DM review queue |

### Default Thresholds (DM configurable)

| Score Range | Action |
|---|---|
| 0-29 | No action |
| 30-49 | Flagged for monitoring |
| 50-69 | Muted (no chat, no trade) |
| 70-89 | Temporary ban pending review |
| 90-100 | Auto-ban |

---

## 4. Machine Learning (Optional Module)

| Capability | Description |
|---|---|
| Training data | Model trained on confirmed bot accounts |
| Pattern recognition | Detection accuracy improves over time with more data |
| Cluster detection | Identifies groups of coordinated bot accounts |
| Anomaly detection | Flags behavior that does not match known human or known bot patterns |
| Confidence scoring | Each ML prediction includes a confidence value for review decisions |

---

## 5. Licensed Bot System

Activated when the DM selects Licensed Bots or Custom policy.

| Feature | Description |
|---|---|
| Registration | Bot owner registers the bot account with the DM |
| Bot tag | Visible tag on the account identifying it as a licensed bot |
| Restrictions | Licensed bots cannot PvP, cannot enter restricted areas, are rate limited |
| Bot tax | Higher Grand Exchange tax rate on bot transactions |
| Separate hiscores | Bot accounts tracked on a separate leaderboard |
| Bot API | Official scripting interface for licensed bot development |
| Bot marketplace | Platform for sharing or selling bot scripts |

---

## 6. Module Toggles

Each system can be independently enabled or disabled by the DM.

| Module | Default (No Bots policy) |
|---|---|
| Bot detection (global) | On |
| Random events | On |
| CAPTCHA | Off |
| Honeypots | On |
| Bot score system | On |
| Auto-punishment | On |
| Machine learning | Off |
| Licensed bots | Off |
| Bot API | Off |
| Bot marketplace | Off |
| Shadow flagging | On |
| Pattern disruption | On |

---

## 7. Admin Views

| View | Description |
|---|---|
| Bot score heatmap | Visual map of where high-scoring accounts are active |
| Flagged accounts | List of accounts currently flagged with score breakdown |
| Detection effectiveness | Stats on which detection methods are catching the most bots |
| False positive rate | Percentage of flagged accounts that were confirmed human |
| Economic impact | Measurement of bot influence on resource prices and GP flow |
| Banned account stats | Total bans, ban reasons, appeal outcomes |
