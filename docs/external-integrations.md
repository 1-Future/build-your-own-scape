# External Integrations and API

Game design document for the External Integrations plugin. This system covers webhook event triggers, HTTP API exposure, data export, time-series metrics, chat bridges, streaming overlays, IoT triggers, cloud sync, and third-party stat site integration.

---

## Module Toggles

Each toggle enables or disables a subsystem within the external integrations module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Webhook System | Fire HTTP requests on configurable game events |
| HTTP API | Expose game state to external tools via REST endpoints |
| API Write Access | Allow external tools to modify game state (dangerous) |
| Data Export | On-demand or scheduled export of account data |
| Auto-Export Schedule | Automatically export data on a recurring schedule |
| Time-Series Database | Push metrics to InfluxDB or Prometheus |
| Chat Bridges | Bidirectional message relay between game chat and external platforms |
| Streaming Integration | Twitch, OBS, and YouTube overlay and event hooks |
| IoT Triggers | Smart home event triggers (Hue, Home Assistant, IFTTT) |
| Cloud Save | Cross-device account and settings sync |
| Third-Party Stats Sync | Auto-upload stats to external hiscore and tracker sites |
| Per-Event Webhook Toggles | Enable or disable individual event types within the webhook system |

---

## 1. Webhook System

Configurable event triggers that fire HTTP requests to external services when game events occur.

### Supported Events

| Event | Description |
|---|---|
| Rare Drop | Player receives a drop at or above a configurable rarity threshold |
| Level Up | Player gains a level in any skill |
| Death | Player dies (PvM or PvP) |
| Achievement | Player completes an achievement |
| Collection Log | Player adds a new entry to the collection log |
| Quest Completion | Player completes a quest |
| Boss Kill | Player kills a boss (with optional KC threshold) |
| Pet Drop | Player receives a pet |
| PvP Kill | Player kills another player |
| Clan Event | A clan event starts, ends, or a milestone is reached |
| Trade Completion | Player completes a trade above a configurable value |
| Milestone Achievement | Player reaches a configurable milestone (total level, total XP, total wealth) |

### Per-Event Configuration

| Field | Type | Description |
|---|---|---|
| Enabled | Boolean | Whether this event type fires webhooks |
| Target URL | String | Destination endpoint URL |
| Format | Enum | `JSON` or `text` |
| Include Screenshot | Boolean | Attach a screenshot of the game at the moment of the event |
| Cooldown | Integer (seconds) | Minimum time between fires for this event type |
| Filter: Minimum Value | Integer | Only fire if the event value exceeds this threshold (e.g., drop value in GP) |
| Filter: Specific Items | List[Integer] | Only fire for these specific item IDs (empty = all) |
| Filter: Specific Skills | List[String] | Only fire for these specific skills (level up events) |

### Output Targets

| Field | Type | Description |
|---|---|---|
| Target Type | Enum | `discord_webhook`, `telegram_bot`, `slack`, `custom_http`, `email`, `sms` |
| URL | String | Endpoint URL or address |
| Auth Token | String | Authentication token or API key |
| Channel ID | String | Channel, room, or chat ID for the target platform |
| Message Template | String | Customizable format string with variable substitution (e.g., `{player} received {item} from {source}`) |
| Retry on Failure | Boolean | Retry delivery on HTTP error |
| Max Retries | Integer | Maximum number of retry attempts |
| Rate Limit | Integer (per minute) | Maximum webhook fires per minute to this target |

---

## 2. HTTP API

Expose game state to external tools via authenticated REST endpoints.

### Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/api/player/stats` | GET | All skill levels, XP, and combat level |
| `/api/player/bank` | GET | Full bank contents with quantities |
| `/api/player/equipment` | GET | Currently worn equipment |
| `/api/player/killcount` | GET | Kill counts per boss and monster |
| `/api/player/collection-log` | GET | Collection log progress and entries |
| `/api/player/quests` | GET | Quest completion status per quest |
| `/api/player/clan` | GET | Clan membership, rank, and clan info |
| `/api/player/ge-offers` | GET | Active Grand Exchange buy and sell offers |
| `/api/player/friends` | GET | Friends list with online status |
| `/api/player/location` | GET | Current location (zone, coordinates) |
| `/api/player/achievements` | GET | Achievement progress and completion |
| `/api/player/pets` | GET | Owned pets and active pet |

### API Configuration

| Field | Type | Description |
|---|---|---|
| API Key | String | Unique key per player, used for authentication |
| Rate Limit | Integer (per minute) | Maximum requests per minute per key |
| Read Only | Boolean | Default `true`. API can only read game state |
| Write Access | Boolean | Allow external tools to modify game state (requires explicit opt-in) |
| CORS Allowed Origins | List[String] | Origins permitted to make cross-origin requests |
| Response Format | Enum | `JSON`, `CSV`, `XML` |
| IP Allowlist | List[String] | Restrict API access to specific IP addresses (empty = all) |

### Write Endpoints (when Write Access is enabled)

| Endpoint | Method | Description |
|---|---|---|
| `/api/player/chat` | POST | Send a chat message as the player |
| `/api/player/ge-offers` | POST | Place a Grand Exchange offer |
| `/api/player/friends` | POST | Add or remove a friend |

---

## 3. Data Export

On-demand or scheduled export of account data in portable formats.

### Exportable Data Sets

| Data Set | Description |
|---|---|
| Bank Contents | Full bank with item IDs, names, quantities, and values |
| Stats Snapshot | All skill levels, XP, and combat level at time of export |
| Loot Logs | Historical loot received from monsters, bosses, and clue scrolls |
| Trade History | All completed trades with timestamps, items, and counterparties |
| Chat Logs | Chat messages sent and received (public, clan, private) |
| Achievement Progress | All achievements with completion status and progress |
| Full Account Snapshot | Every data set above combined into a single export |
| Kill Counts | All boss and monster kill counts |
| Collection Log | Full collection log with obtained and unobtained entries |

### Export Configuration

| Field | Type | Description |
|---|---|---|
| Format | Enum | `CSV`, `JSON`, `SQLite` |
| Schedule | Enum | `manual`, `daily`, `weekly`, `on_milestone` |
| Milestone Trigger | String | Which milestone triggers auto-export (e.g., level up, quest complete, 99) |
| Storage Target | Enum | `local_file`, `cloud_upload`, `third_party_service` |
| Cloud Provider | String | Cloud storage provider name and credentials (if cloud upload) |
| Compression | Boolean | Compress export file (gzip) |
| Include Timestamps | Boolean | Include timestamps on all records |
| Date Range | JSON | Filter export to a date range (e.g., `{"from": "2026-01-01", "to": "2026-03-28"}`) |

---

## 4. Time-Series Database

Push game metrics to InfluxDB or Prometheus for visualization in Grafana or similar dashboards.

### Supported Metrics

| Metric | Description |
|---|---|
| XP per Hour | XP gained per hour, per skill |
| GP per Hour | Gold earned per hour (loot + trades) |
| Kills per Hour | Monster and boss kills per hour |
| Item Prices over Time | Grand Exchange price history per item |
| Money Supply | Total GP in the game economy over time |
| Player Count | Online player count over time |
| Deaths per Hour | Player deaths per hour (server-wide or per player) |
| Trade Volume | Total trade volume in GP over time |
| Boss Kill Times | Average and best kill times per boss |

### Configuration

| Field | Type | Description |
|---|---|---|
| Backend | Enum | `influxdb`, `prometheus` |
| Endpoint URL | String | Database endpoint |
| Auth Token | String | Authentication credentials |
| Push Interval | Integer (seconds) | How often metrics are pushed (default 60) |
| Retention Period | Integer (days) | How long metrics are retained |
| Player-Level Metrics | Boolean | Track metrics per individual player (vs server-wide only) |
| Custom Tags | JSON | Additional tags attached to all metrics (e.g., `{"server": "world-1"}`) |

---

## 5. Chat Bridges

Bidirectional message relay between in-game chat channels and external communication platforms.

### Supported Platforms

| Platform | Protocol | Description |
|---|---|---|
| IRC | IRC | Traditional IRC bridge |
| Discord | Webhook + Bot | Discord server integration |
| Telegram | Bot API | Telegram group chat bridge |
| Matrix | Matrix protocol | Decentralized chat bridge |

### Bridge Configuration

| Field | Type | Description |
|---|---|---|
| Platform | Enum | One of the supported platforms |
| Direction | Enum | `game_to_external`, `external_to_game`, `bidirectional` |
| Game Channel | Enum | `public`, `clan`, `group`, `trade`, `custom` |
| External Channel ID | String | Channel, room, or group ID on the external platform |
| Bot Token | String | Authentication token for the external platform bot |
| Message Format: Outbound | String | Template for messages going from game to external (e.g., `[{game_channel}] {player}: {message}`) |
| Message Format: Inbound | String | Template for messages coming from external to game (e.g., `[Discord] {user}: {message}`) |
| User Identity Mapping | JSON | Map game usernames to external platform usernames |
| Moderation Sync | Boolean | Sync moderation actions (mute in game = mute in external and vice versa) |
| Filter Profanity | Boolean | Apply the game's profanity filter to bridged messages |
| Relay Join/Leave | Boolean | Announce player logins and logouts in the external channel |

---

## 6. Streaming Integration

Hooks for live streaming platforms and recording software.

### Twitch

| Feature | Description |
|---|---|
| Live Loadout Overlay | Display current equipment, stats, and inventory as a browser source |
| Chat Integration | Twitch chat commands query game state (e.g., `!stats`, `!kc vorkath`) |
| Subscriber Alerts | In-game notification when a viewer subscribes |
| Channel Point Rewards | Channel point redemptions trigger in-game events (DM-configured, e.g., spawn monster, change weather) |
| Viewer KC Lookup | Twitch chat command to look up any player's kill count |

### OBS

| Feature | Description |
|---|---|
| Auto-Replay Save | Automatically save replay buffer on configurable events (rare drop, death, PvP kill) |
| Scene Switching | Automatically switch OBS scenes based on game state (e.g., boss fight, skilling, idle) |
| Event Markers | Insert markers into the recording timeline on game events |

### YouTube

| Feature | Description |
|---|---|
| Live Stats Overlay | Display player stats as a browser source overlay |
| Chat Integration | YouTube live chat commands query game state |

### General Streaming Settings

| Field | Type | Description |
|---|---|---|
| Green Screen Mode | Boolean | Render background as solid color for chroma key |
| Stream-Safe Mode | Boolean | Hide player name, private messages, and sensitive info |
| Hide Friends List | Boolean | Prevent friends list from appearing on stream |
| Overlay Position | Enum | `top_left`, `top_right`, `bottom_left`, `bottom_right`, `custom` |
| Overlay Opacity | Float (0-1) | Transparency of overlay elements |
| Event Alert Duration | Integer (seconds) | How long event popups display on the overlay |

---

## 7. IoT and Smart Home

Trigger smart home devices based on game events. Fun and entirely optional.

### Supported Platforms

| Platform | Description |
|---|---|
| Home Assistant | Open source home automation |
| Philips Hue | Smart lighting |
| IFTTT | If-This-Then-That automation service |
| Custom Webhook | Any device or service accepting HTTP requests |

### Event-to-Action Mapping

| Field | Type | Description |
|---|---|---|
| Game Event | Enum | Any supported webhook event (rare drop, death, level up, etc.) |
| Action | String | Action to perform (e.g., `flash_lights`, `set_color`, `play_sound`, `send_notification`) |
| Target Device | String | Device or group identifier on the smart home platform |
| Parameters | JSON | Action-specific parameters (e.g., `{"color": "#00FF00", "duration": 5}`) |
| Enabled | Boolean | Whether this mapping is active |

### Preset Mappings

| Event | Default Action |
|---|---|
| Rare Drop | Flash lights green |
| Death | Pulse lights red |
| Level Up | Rainbow celebration pattern |
| Low HP Warning | Slow red pulse |
| Pet Drop | Flash lights gold |
| PvP Kill | Flash lights blue |
| Boss Kill | Brief white flash |
| 99 Achieved | Extended rainbow cycle |

---

## 8. Cloud Save and Sync

Cross-device account and settings synchronization.

### Syncable Data

| Data Type | Description |
|---|---|
| Keybinds | Custom key mappings |
| UI Layout | Panel positions, sizes, and visibility |
| Chat Filters | Custom chat filter rules |
| Bank Tags | Item tagging and organization |
| Tile Markers | Ground tile markers and colors |
| Plugin Presets | Per-plugin configuration sets |
| Music Playlists | Player-created music playlists |
| Notification Settings | Per-event notification preferences |
| Friends Notes | Notes attached to friends list entries |

### Sync Configuration

| Field | Type | Description |
|---|---|---|
| Sync Enabled | Boolean | Master toggle for cloud sync |
| Sync Provider | Enum | `self_hosted`, `s3`, `google_drive`, `dropbox` |
| Sync Frequency | Enum | `realtime`, `on_logout`, `hourly`, `manual` |
| Conflict Resolution | Enum | `newer_wins`, `manual_merge`, `server_wins`, `client_wins` |
| Encryption | Boolean | Encrypt synced data at rest |
| Sync Scope | List[String] | Which data types to sync (subset of syncable data) |
| Last Sync Timestamp | DateTime | When the last successful sync occurred |

---

## 9. Third-Party Stats Sites

Auto-upload player statistics to external tracking and hiscore services.

### Features

| Feature | Description |
|---|---|
| Auto-Upload on Level Up | Push updated stats when the player gains a level |
| Hiscore Sync | Keep external hiscore boards up to date with current stats |
| XP Tracker Sync | Feed XP gains to external XP tracking services |
| Group Aggregation | Aggregate stats for clans or groups on external sites |
| Public Profile Generation | Generate a public-facing profile page with stats, achievements, and collection log |
| Datapoint History | Push periodic snapshots for historical XP gain graphs |

### Configuration

| Field | Type | Description |
|---|---|---|
| Service Name | String | Name of the third-party service |
| Service URL | String | Base URL of the tracking service |
| API Key | String | Authentication key for the service |
| Auto-Upload Enabled | Boolean | Whether stats are automatically pushed |
| Upload Events | List[Enum] | Which events trigger an upload (`level_up`, `quest_complete`, `logout`, `milestone`) |
| Public Profile | Boolean | Whether the player's profile is publicly visible on the service |
| Opt-Out Fields | List[String] | Data fields excluded from upload (e.g., bank value, location) |
| Upload Frequency | Enum | `on_event`, `hourly`, `daily` |

---

## Data Model

### Webhook Record

| Field | Type | Description |
|---|---|---|
| id | string | Unique webhook configuration identifier |
| player_id | string | Player who owns this webhook |
| event_type | string | One of the supported event types |
| target_type | string | Destination platform type |
| target_url | string | Destination URL |
| auth_token | string | Authentication token (encrypted at rest) |
| channel_id | string | Target channel or room ID |
| message_template | string | Format string with variable substitution |
| enabled | boolean | Whether this webhook is active |
| cooldown_seconds | integer | Minimum seconds between fires |
| include_screenshot | boolean | Attach screenshot to payload |
| filter_min_value | integer or null | Minimum event value to trigger |
| filter_items | array or null | Specific item IDs to trigger on |
| retry_on_failure | boolean | Retry on HTTP error |
| max_retries | integer | Maximum retry attempts |
| rate_limit_per_minute | integer | Maximum fires per minute |
| last_fired | datetime or null | Timestamp of last successful fire |
| created_at | datetime | When this webhook was configured |

### API Key Record

| Field | Type | Description |
|---|---|---|
| id | string | Unique key identifier |
| player_id | string | Player who owns this key |
| key_hash | string | Hashed API key (never stored in plaintext) |
| permissions | string | `read_only` or `read_write` |
| rate_limit | integer | Requests per minute |
| ip_allowlist | array | Allowed IP addresses (empty = all) |
| cors_origins | array | Allowed CORS origins |
| created_at | datetime | When the key was created |
| last_used | datetime or null | Timestamp of last API request |
| revoked | boolean | Whether this key has been revoked |

### Export Record

| Field | Type | Description |
|---|---|---|
| id | string | Unique export identifier |
| player_id | string | Player who requested the export |
| data_set | string | Which data set was exported |
| format | string | Export file format |
| file_path | string | Location of the export file |
| file_size | integer | Size in bytes |
| created_at | datetime | When the export was generated |
| schedule | string or null | Recurring schedule if auto-export |
| compressed | boolean | Whether the file is gzipped |

---

## Views

Predefined filtered views for managing integrations.

| View | Description |
|---|---|
| Webhook Configuration | List all webhooks with event type, target, status, and last fire time |
| API Key Management | List all API keys with permissions, rate limits, and revocation status |
| Export History | List all exports with format, size, date, and download link |
| Connected Services Dashboard | Overview of all active integrations with connection status |
| Integration Health | Last successful ping, error count, and uptime per connected service |
| Chat Bridge Status | Per-bridge connection status, message count, and last relay time |
| Streaming Overlay Preview | Live preview of configured streaming overlays |
| Sync Status | Last sync time, conflict count, and data freshness per sync type |
