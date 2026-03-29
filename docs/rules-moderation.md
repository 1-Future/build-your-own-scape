# Rules and Moderation

Game design document for the rules enforcement and moderation systems. Covers rule definitions, punishment escalation, appeals, player reports, and moderator tools.

---

## 1. Rule List

### Rule Structure

Each rule is defined with the following fields:

| Field | Description |
|---|---|
| Rule ID | Unique identifier (e.g., `CONDUCT-001`, `ECON-003`) |
| Name | Short human-readable name |
| Description | Full explanation of what the rule prohibits or requires |
| Category | One of: Conduct, Economy, Combat, Chat, Exploits, Account, Custom |
| Severity | Minor, Moderate, Major, or Critical |
| Example violations | Concrete examples of behavior that triggers this rule |
| Punishment tier | Default punishment applied on first offense |

### Categories

| Category | Scope |
|---|---|
| Conduct | General behavior, toxicity, griefing, disruptive play |
| Economy | Scamming, real-world trading, price manipulation, gold farming |
| Combat | Bug abuse in PvP, safespotting exploits, boosting, ragging |
| Chat | Spam, hate speech, advertising, impersonation, inappropriate language |
| Exploits | Bug abuse, duplication glitches, unintended mechanics abuse |
| Account | Account sharing, multi-logging, selling accounts, ban evasion |
| Custom | Server-defined rules added by administrators |

### Severity Levels

| Severity | Description | Typical First Offense |
|---|---|---|
| Minor | Low-impact violations, usually ignorance-based | Warning |
| Moderate | Disruptive behavior that affects other players | Mute or short temp ban |
| Major | Serious violations that damage game integrity | Extended temp ban or wealth rollback |
| Critical | Severe violations that threaten the game or community | Permanent ban or IP ban |

### Example Rules

| Rule ID | Name | Category | Severity | Example Violations |
|---|---|---|---|---|
| CONDUCT-001 | Disruptive behavior | Conduct | Minor | Intentionally blocking paths, following players to harass |
| CONDUCT-002 | Griefing | Conduct | Moderate | Repeatedly crashing another player's activity, luring |
| CHAT-001 | Spam | Chat | Minor | Flooding chat with repeated messages, autochat abuse |
| CHAT-002 | Hate speech | Chat | Critical | Slurs, targeted harassment based on identity |
| CHAT-003 | Advertising | Chat | Moderate | Promoting third-party services, phishing links |
| ECON-001 | Scamming | Economy | Major | Trust trades, fake giveaways, item switch scams |
| ECON-002 | Real-world trading | Economy | Critical | Buying or selling gold, items, or accounts for real money |
| ECON-003 | Price manipulation | Economy | Major | Coordinated market manipulation, merching cartels |
| COMBAT-001 | PvP bug abuse | Combat | Major | Using unintended mechanics to gain combat advantage |
| COMBAT-002 | Ragging | Combat | Moderate | Repeatedly attacking the same player with minimal risk gear |
| EXPLOIT-001 | Bug abuse | Exploits | Major | Exploiting known bugs for personal gain |
| EXPLOIT-002 | Duplication | Exploits | Critical | Using any method to duplicate items or currency |
| ACCOUNT-001 | Account sharing | Account | Moderate | Allowing another person to access your account |
| ACCOUNT-002 | Ban evasion | Account | Critical | Creating new accounts to circumvent an active ban |
| ACCOUNT-003 | Multi-logging | Account | Moderate | Using multiple accounts simultaneously to gain advantage |

---

## 2. Punishment System

### Punishment Types

| Punishment | Description | Duration / Scope |
|---|---|---|
| Warning | Formal notice added to player record | Permanent record, no gameplay impact |
| Mute | Player cannot send chat messages | Configurable: 1 hour to 30 days |
| Temp ban | Player cannot log in | Configurable: 1 hour to 90 days |
| Permanent ban | Account permanently disabled | Indefinite, appealable after configurable cooldown |
| IP ban | All connections from the IP address blocked | Indefinite |
| Hardware ban | Device fingerprint blocked from connecting | Indefinite |
| Skill rollback | XP and levels reverted in specific skills | Targeted to affected skills and time range |
| Wealth rollback | Gold and items removed from account | Amount determined by investigation |
| Trade ban | Player cannot trade with other players | Configurable duration |
| GE ban | Player cannot use the Grand Exchange | Configurable duration |
| PvP ban | Player cannot enter PvP areas or attack players | Configurable duration |
| Quarantine | Player can log in and play but is isolated from others | Configurable duration, no chat/trade/multiplayer |
| Community service | Player must complete X hours of in-game tasks to lift ban | Configurable task type and duration |
| Demotion | Clan rank and privileges stripped | Permanent until manually restored |

### Strike System

| Strike Count | Default Action | Notes |
|---|---|---|
| 1st offense | Warning | Formal notice, no gameplay restriction |
| 2nd offense | Mute (24 hours) | Escalates if same category |
| 3rd offense | Temp ban (3 days) | Duration configurable per category |
| 4th offense | Temp ban (14 days) | Extended duration |
| 5th offense | Permanent ban | Auto-escalation ceiling |

### Strike Configuration

| Setting | Description | Default |
|---|---|---|
| Strike decay | Strikes expire after X consecutive clean days | 90 days |
| Per-category strikes | Each rule category tracks strikes independently | Enabled |
| Auto-escalation | System automatically applies next punishment tier | Enabled |
| Manual override | Moderators can skip tiers for severe cases | Enabled |
| Critical bypass | Critical severity rules skip strike ladder entirely | Enabled |
| Maximum active strikes | Total strikes before forced permanent ban review | 5 |

---

## 3. Appeal System

### Appeal Submission

| Field | Description |
|---|---|
| Punishment reference | Auto-linked to the specific punishment being appealed |
| Player statement | Free-text explanation of why the punishment should be reversed |
| Evidence attachment | Optional file upload (screenshots, recordings) |
| Appeal type | Full reversal, reduction, or reclassification |

### Appeal Processing

| Stage | Description |
|---|---|
| Submitted | Appeal enters the queue with timestamp |
| Pending | Awaiting moderator assignment |
| Under review | Assigned moderator is investigating |
| Peer review | Multiple moderators vote on outcome (if peer review toggle enabled) |
| Accepted | Punishment reversed or reduced, record updated |
| Denied | Punishment upheld, player notified with reason |

### Appeal Configuration

| Setting | Description | Default |
|---|---|---|
| Appeal cooldown | Minimum days between appeals for the same punishment | 14 days |
| Auto-appeal (first offense) | First-time minor offenses automatically enter appeal review | Enabled |
| Peer review | Require multiple moderators to vote on appeal outcome | Disabled |
| Peer review threshold | Number of moderator votes required to resolve | 3 |
| Transparency | Player can view their full punishment and appeal history | Enabled |
| Appeal window | Maximum days after punishment to submit an appeal | 30 days |
| Max evidence attachments | Number of files a player can attach per appeal | 5 |

---

## 4. Report System

### Report Categories

| Category | Description |
|---|---|
| Botting | Suspected automated gameplay or macro use |
| Scamming | Deceptive trade practices or trust abuse |
| Harassment | Targeted abuse, stalking, or threats |
| Bug abuse | Exploiting glitches or unintended mechanics |
| Real-world trading | Buying or selling in-game assets for real money |
| Inappropriate name | Offensive, impersonating, or policy-violating display name |
| Other | Anything not covered by the above categories |

### Report Structure

| Field | Description |
|---|---|
| Reporter | Account that submitted the report (kept anonymous to target) |
| Target player | Account being reported |
| Category | Selected from the report categories above |
| Description | Free-text explanation from the reporter |
| Evidence | Optional screenshots or context (auto-captures chat log) |
| Timestamp | When the report was submitted |
| World / Location | Where the reported behavior occurred |

### Report Prioritization

| Factor | Effect |
|---|---|
| Volume escalation | Multiple reports on the same player within a time window auto-escalates priority |
| Reporter reputation | Reporters with historically accurate reports receive higher priority weighting |
| False reporter penalty | Reporters who consistently file false reports are deprioritized |
| Category weight | Critical categories (RWT, harassment) start at higher base priority |
| Player history | Targets with existing strikes receive elevated priority |

### Report Configuration

| Setting | Description | Default |
|---|---|---|
| Anonymous reports | Reporter identity hidden from the reported player | Enabled |
| Report feedback | Notify reporter of the outcome (action taken or dismissed) | Disabled |
| Reporter reputation tracking | Track accuracy of each reporter over time | Enabled |
| Auto-escalation threshold | Number of reports on same player before auto-escalation | 3 |
| Evidence required | Require at least one piece of evidence to submit | Disabled |
| Report cooldown | Minimum time between reports from the same reporter on the same target | 24 hours |

---

## 5. Moderation Tools

### Communication

| Tool | Description |
|---|---|
| Mod chat | Dedicated chat channel visible only to moderators |
| Player message | Send a direct system message to a specific player |
| Broadcast | Send a server-wide announcement |

### Surveillance

| Tool | Description |
|---|---|
| Spectate mode | Invisibly observe a player in real time without their knowledge |
| Teleport to player | Instantly move to a player's current location |
| Check player logs | View trade history, chat history, movement history, and XP gains |
| Inventory inspection | View a player's current inventory, bank, and equipment |
| Session info | View player's current session duration, IP (hashed), and connection details |

### Enforcement

| Tool | Description |
|---|---|
| Freeze player | Prevent a player from moving or performing actions |
| Kick from world | Force disconnect a player from the server |
| Apply punishment | Issue any punishment type from the punishment table |
| Roll back player actions | Revert specific actions (trades, drops, XP gains) within a time range |
| Force name change | Flag a display name as inappropriate and require the player to choose a new one |

### Accountability

| Feature | Description |
|---|---|
| Mod action log | Every moderator action is recorded with timestamp, target, action type, and reason |
| Action review | Senior moderators can audit junior moderator actions |
| Mod statistics | Track actions per moderator for workload and behavior monitoring |
| Abuse detection | Flag unusual patterns in moderator behavior (mass bans, targeting specific players) |

### Moderator Permissions

| Rank | Permissions |
|---|---|
| Junior Moderator | Mute, kick, spectate, check logs, freeze, file internal reports |
| Moderator | All junior permissions plus temp ban (up to 7 days), force name change, roll back actions |
| Senior Moderator | All moderator permissions plus extended temp ban, wealth rollback, skill rollback, appeal review |
| Administrator | All permissions including permanent ban, IP ban, hardware ban, mod action audit, rule management |

---

## 6. Module Toggles

All systems can be independently enabled or disabled by server administrators.

| Toggle | Description | Default |
|---|---|---|
| Rule system | Enable the rule list and category framework | Enabled |
| Strike system | Enable strike-based punishment escalation | Enabled |
| Appeal system | Allow players to submit appeals | Enabled |
| Report system | Allow players to submit reports | Enabled |
| Mod tools | Enable the moderation toolset | Enabled |
| Auto-escalation | Automatically escalate punishment on repeat offenses | Enabled |
| Peer review appeals | Require multiple moderators to vote on appeals | Disabled |
| Reporter reputation | Track and weight reporter accuracy | Enabled |
| Report feedback | Notify reporters of report outcomes | Disabled |
| Transparency | Let players view their full punishment record | Enabled |
| Community service punishment | Enable community service as a punishment option | Disabled |
| Quarantine mode | Enable quarantine as a punishment option | Disabled |
| Skill rollback | Enable skill XP rollback as a punishment option | Enabled |
| Wealth rollback | Enable gold/item rollback as a punishment option | Enabled |

---

## 7. Views

### Player-Facing Views

| View | Description |
|---|---|
| My record | Player's own punishment history, active strikes, and appeal status |
| Appeal status | Current state of any submitted appeals |
| Report confirmation | Acknowledgment that a report was received, plus outcome if feedback is enabled |
| Rules reference | Browsable list of all active rules with descriptions and examples |

### Moderator Views

| View | Description |
|---|---|
| Active reports | Queue of unresolved reports sorted by priority |
| Punishment history | Full punishment record for any player, searchable and filterable |
| Appeal queue | List of pending and in-review appeals, assignable to moderators |
| Mod action log | Chronological log of all moderator actions, filterable by mod, action type, or target |
| Rule violation stats | Aggregate data on violations by category, severity, and time period |
| Strike leaderboard | Internal view showing players with the most active strikes (for proactive monitoring) |
| Moderator performance | Per-moderator statistics on actions taken, appeals overturned, and report resolutions |
