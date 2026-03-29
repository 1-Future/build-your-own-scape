# Account Management

Game design document for player account management systems. Covers profile, security, settings, privacy, social features, save states, and account actions.

---

## 1. Profile

### Display Name

| Field | Details |
|---|---|
| Display name | Changeable with cooldown period between changes |
| Previous names | History of past names, visible or hidden via toggle |
| Name change cost | Configurable (free, in-game currency, or token) |
| Name restrictions | Character limit, no special characters, profanity filter, no impersonation of staff/NPCs |

### Identity

| Field | Details |
|---|---|
| Profile bio | Free-text field with character limit |
| Avatar / profile picture | Upload or select from unlocked in-game options |
| Title display | Earned from achievements, quests, or clan rank. Shown beside display name |
| Online status | Online, Away, Invisible, Do Not Disturb |
| Profile visibility | Public, Friends Only, Private |

---

## 2. Security

### Authentication

| Feature | Details |
|---|---|
| Password | Change password, enforced complexity requirements (length, mixed case, symbols) |
| Two-factor authentication | Authenticator app (TOTP), SMS, or email-based codes |
| Recovery | Linked email and/or phone number for account recovery |
| Login notifications | Alert on new login via email or in-game notification |
| Login history | Log of IP address, device info, and timestamp for each session |

### Bank PIN

| Feature | Details |
|---|---|
| PIN type | Fixed digits (4-6 digit code) or authenticator-based (TOTP) |
| PIN removal delay | Configurable 3-7 day waiting period before PIN is removed |
| PIN change | Requires current PIN to set a new one |

### Session and Device Management

| Feature | Details |
|---|---|
| Trusted devices | List of recognized devices that skip 2FA prompts |
| Session management | View all active sessions with device/IP info. Kill individual sessions or all others |
| IP whitelist | Optional toggle to restrict login to approved IPs only |
| Account lock | Self-service lock for vacations or breaks. Requires full auth to unlock |

---

## 3. Account Settings

### Gameplay

| Setting | Options |
|---|---|
| Attack options (players) | Left-click attack, Hidden, Right-click only |
| Attack options (NPCs) | Left-click attack, Hidden, Right-click only |
| Loot notifications | Toggle on/off, configurable value threshold |
| Drop warnings | Confirm before dropping items over a set GP value |
| XP tracker display | Toggle visibility, position on screen |
| Hitsplats style | Default, minimal, numeric only, colorblind-safe |
| Inventory layout | Grid size, icon scale, sorting preference |

### Chat

| Setting | Options |
|---|---|
| Public chat | On, Friends Only, Off |
| Private messages | On, Friends Only, Off |
| Clan chat | On, Off |
| Trade messages | On, Friends Only, Off |
| Game messages | On, Filtered, Off |
| Profanity filter | On, Off |
| Chat colors/font | Customizable text color and font selection |
| Block list | Manage blocked players (add, remove, view) |

### Audio

| Setting | Options |
|---|---|
| Music volume | 0-100% slider |
| SFX volume | 0-100% slider |
| Ambient volume | 0-100% slider |
| Voice volume | 0-100% slider |
| Mute all | Master mute toggle |
| Music playlist | Preferences for unlocked tracks, shuffle, favorites |

### Display

| Setting | Options |
|---|---|
| Brightness | 0-100% slider |
| UI scale | 50-200% slider |
| Minimap settings | Size, opacity, rotation lock, overlay icons |
| Camera zoom | Min/max range, scroll sensitivity |
| Render distance | Low, Medium, High, Ultra |
| FPS cap | 30, 60, 120, Uncapped |
| Colorblind mode | Protanopia, Deuteranopia, Tritanopia filters |
| High contrast | Toggle for UI elements |
| Screen reader | Accessibility text-to-speech for UI elements |
| Reduced motion | Disable non-essential animations and screen shake |

### Notifications

| Event | Toggle |
|---|---|
| Login alerts | Notify when account is logged in from a new device |
| Friend online | Notify when a friend comes online |
| Clan events | Notify for clan announcements, wars, scheduled events |
| Daily reset | Notify when daily tasks and shops reset |
| Achievement unlocked | Notify on achievement completion |
| Valuable drop | Notify when a drop exceeds the configured threshold |
| Trade request | Notify on incoming trade requests |
| Private message | Notify on incoming PMs (sound, visual, or both) |

### Keybinds

| Feature | Details |
|---|---|
| Rebindable keys | All gameplay keys can be reassigned |
| Preset layouts | Default, WASD, Legacy, Custom |
| Key profiles | Multiple saved keybind profiles, switchable in settings |

---

## 4. Privacy

### Visibility Controls

Who can see specific information about the player.

| Data | Options |
|---|---|
| Stats / levels | Everyone, Friends, Clan, Nobody |
| Equipment | Everyone, Friends, Clan, Nobody |
| Bank value | Everyone, Friends, Clan, Nobody |
| Online status | Everyone, Friends, Clan, Nobody |
| Current location | Everyone, Friends, Clan, Nobody |

### Interaction Controls

Who can interact with the player.

| Action | Options |
|---|---|
| Trade me | Everyone, Friends, Clan, Nobody |
| Follow me | Everyone, Friends, Clan, Nobody |
| PM me | Everyone, Friends, Nobody |
| Invite to groups | Everyone, Friends, Clan, Nobody |
| Invite to clans | Everyone, Friends, Nobody |

### Public Presence

| Setting | Details |
|---|---|
| Appear on hiscores | Toggle on/off |
| Appear in clan list publicly | Toggle on/off |
| Opt out of random guild invites | Toggle on/off |

---

## 5. Social

### Friends

| Feature | Details |
|---|---|
| Friends list | Add, remove, search players |
| Favorites / best friends | Pin players to top of list, distinct visual indicator |
| Friend notes | Private text note per friend |
| Friend capacity | Maximum number of friends (configurable by DM or server) |
| Friend groups / tags | Organize friends into custom groups (e.g., "PvP crew", "Skillers") |
| Friend activity feed | Toggle. Shows recent achievements, level-ups, drops from friends |

### Ignore

| Feature | Details |
|---|---|
| Ignore list | Block players from messaging, trading, and following |
| Manage | Add, remove, view ignored players |

### Referrals

| Feature | Details |
|---|---|
| Referral system | Unique referral code per player. Rewards for both referrer and referred |

---

## 6. Save States

### Manual Saves

| Feature | Details |
|---|---|
| Snapshot | Player can manually snapshot their account state |
| Save slots | Limited number of slots (DM/server configurable) |
| Save cooldown | One save per configurable interval (e.g., once per 7 days) |
| Save expiry | Old saves auto-delete after a set period (toggle, DM configurable) |

### Auto-Saves

| Feature | Details |
|---|---|
| System snapshots | Automatic snapshots taken every X days (server-side) |
| Player control | None. Players cannot trigger or delete auto-saves |
| Retention | DM sets retention period: 7, 30, or 90 days |
| Mod use | Available to moderators for investigation and dispute resolution |

### Rollback Requests

Player-initiated request to restore a previous save.

| Step | Details |
|---|---|
| Submit request | Player selects a save point to restore to |
| Reason required | Player must provide a written reason for the rollback |
| Pre-approval audit | System flags unusual activity: recent trades, drops, IP changes, time gaps |
| Suspicious activity flag | Auto-flag if items were dropped/traded shortly before request (dupe prevention) |
| DM/mod review | A moderator reviews the request and approves or denies |
| Rollback scope | Full account, Inventory only, Bank only, or Stats only |
| Dupe prevention | Claw back items received from the rolled-back account. Reverse GP and GE transactions |
| Rollback history | All rollback events visible to moderators |
| Lifetime limit | Maximum number of rollbacks per account (DM configurable) |
| Cooldown | Enforced cooldown period after a successful rollback |

---

## 7. Account Actions

| Action | Details |
|---|---|
| Change display name | Subject to cooldown and name restrictions |
| Change password | Requires current password and 2FA if enabled |
| Enable / disable 2FA | Setup wizard for TOTP, SMS, or email |
| Set / change / remove bank PIN | PIN removal subject to delay period |
| Transfer clan ownership | Assign ownership to another clan member |
| Convert account mode | Switch account type (e.g., standard to ironman). May be one-way |
| Delete account | Permanent. Requires confirmation period (e.g., 30 days) before irreversible |
| Export account data | GDPR-compliant full data export (JSON/CSV) |
| Merge accounts | Optional toggle. Combine two accounts into one (DM approval required) |
| Link accounts across worlds | Associate characters on different servers under one login |

---

## Module Toggles

Server operators (DMs) can enable or disable individual account management features.

| Module | Default |
|---|---|
| Profile customization | Enabled |
| Bio / avatar | Enabled |
| Titles | Enabled |
| Two-factor authentication | Enabled |
| Bank PIN | Enabled |
| Trusted devices | Enabled |
| Session management | Enabled |
| Privacy controls | Enabled |
| Friend groups | Enabled |
| Activity feed | Enabled |
| Referral system | Disabled |
| Account export (GDPR) | Enabled |
| Account deletion | Enabled |
| Account merging | Disabled |
| Keybind customization | Enabled |
| Colorblind / accessibility | Enabled |
| Save states | Enabled |
| Rollback system | Enabled |

---

## Views

The account management system is organized into the following UI panels.

| View | Purpose |
|---|---|
| Profile | Public-facing player card. Name, title, bio, avatar, online status |
| Security Dashboard | Password, 2FA, bank PIN, trusted devices, sessions |
| Settings Panel | Gameplay, chat, audio, display, notifications, keybinds |
| Privacy Controls | Visibility and interaction permissions |
| Social | Friends list, ignore list, friend groups, referrals |
| Progression Summary | Stats overview, achievements, milestones, XP totals |
| Account Timeline | Chronological log of account events (name changes, mode conversions, rollbacks) |
| Login History | Table of recent logins with IP, device, timestamp, and status |
