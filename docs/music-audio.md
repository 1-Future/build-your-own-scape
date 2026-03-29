# Music and Audio System

Game design document for the Music and Audio plugin. This system covers music track unlocking, playlists, the jukebox, per-zone soundtracks, custom music uploads, the now-playing HUD, volume controls, and sound effects.

---

## Module Toggles

Each toggle enables or disables a subsystem within the music and audio module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Music System | Master toggle for all music playback |
| Track Unlocking | Tracks must be discovered by visiting locations or completing objectives |
| Playlists | Players can create and manage custom playlists |
| Jukebox | Placeable jukebox objects in player housing and clan halls |
| DJ Mode | One player controls music playback for a group via the jukebox |
| Custom Music Uploads | DM or players can upload custom music tracks |
| Now Playing HUD | Display current track name on screen |
| Per-Zone Soundtrack | Each zone has a default assigned track |
| Dynamic Music Layers | Add or remove intensity layers based on combat and activity state |
| Custom Sound Effects | DM can upload or assign custom sounds to game events |
| Sound Packs | Community-created themed sound effect packs |
| Visual Sound Indicators | Accessibility feature showing visual cues for audio events |

---

## 1. Music Tracks

Every music track in the game is a discoverable, trackable asset. Tracks are unlocked through gameplay and contribute toward the music completionist goal.

### Track Data Model

| Field | Type | Description |
|---|---|---|
| id | string | Unique track identifier |
| name | string | Display name of the track |
| file_path | string | Path to the audio file |
| duration | integer (seconds) | Length of the track |
| unlock_zone_id | integer or null | Zone ID where the track is unlocked by visiting |
| unlock_condition | string | How the track is unlocked (`visit_area`, `complete_quest`, `kill_boss`, `achievement`, `default`) |
| unlock_condition_id | integer or null | ID of the quest, boss, or achievement required (null if visit-based) |
| unlocked_by_default | boolean | Whether the track is available from the start |
| mood | string | Descriptive mood tag (`peaceful`, `combat`, `eerie`, `triumphant`, `melancholy`, `festive`) |
| category | string | Grouping category (`overworld`, `dungeon`, `boss`, `minigame`, `event`, `ambient`) |
| composer | string | Credit for the track composer |
| added_version | string | Game version when the track was added |

### Music Cape

When a player unlocks every track in the game, they earn the Music Cape equivalent. This is a cosmetic reward and achievement. The total track count is displayed in the music panel alongside the player's unlock count.

---

## 2. Playlists

Players can organize unlocked tracks into custom playlists.

### Playlist Data Model

| Field | Type | Description |
|---|---|---|
| id | string | Unique playlist identifier |
| player_id | string | Owner of the playlist |
| name | string | Player-assigned playlist name |
| tracks | array[string] | Ordered list of track IDs |
| shuffle | boolean | Whether playback order is randomized |
| loop | boolean | Whether the playlist repeats when it ends |
| created_at | datetime | When the playlist was created |
| updated_at | datetime | When the playlist was last modified |

### Special Playlists

| Playlist | Description |
|---|---|
| Favorites | Quick-star any track to add it to the favorites list |
| Recently Played | Automatically populated with the last 50 tracks played |
| All Unlocked | Virtual playlist containing every unlocked track |
| Zone Default | Virtual playlist containing all tracks assigned to zones the player has visited |

---

## 3. Jukebox System

An in-game object that allows browsing and playing music. Placeable in player housing and clan halls.

### Jukebox Features

| Feature | Description |
|---|---|
| Browse All Unlocked | View every track the player has unlocked |
| Search | Search tracks by name, zone, mood, or category |
| Play for Self | Play a track audible only to the jukebox user |
| Play for Nearby | Play a track audible to all players within range |
| Queue | Add tracks to a play queue |
| DJ Mode | One player is designated DJ and controls playback for the group |
| Now Playing Display | The jukebox object displays the current track name above it |

### DJ Mode Configuration

| Field | Type | Description |
|---|---|---|
| DJ Player ID | string | The player currently acting as DJ |
| Broadcast Range | integer (tiles) | How far the jukebox music carries |
| Allow Requests | boolean | Whether other players can request tracks |
| Request Queue | array[string] | Queued track requests from other players |
| Vote Skip | boolean | Whether listeners can vote to skip the current track |
| Vote Skip Threshold | float (%) | Percentage of listeners required to skip |

---

## 4. Per-Zone Soundtrack

Each zone in the game has a default soundtrack configured by the dungeon master. Music transitions are configurable.

### Zone Music Assignment

| Field | Type | Description |
|---|---|---|
| zone_id | integer | The zone this assignment applies to |
| default_track_id | string | Track that plays when the player enters this zone |
| day_track_id | string or null | Track override for daytime (null = use default) |
| night_track_id | string or null | Track override for nighttime (null = use default) |
| weather_overrides | JSON | Map of weather type to track ID (e.g., `{"rain": "track_042", "storm": "track_099"}`) |
| transition_type | enum | `crossfade`, `hard_cut`, `silence_gap` |
| crossfade_duration | integer (ms) | Duration of crossfade between tracks (if crossfade) |
| silence_gap_duration | integer (ms) | Duration of silence between tracks (if silence gap) |

### Override Priority

When multiple music triggers are active, the highest priority source plays.

| Priority | Source | Description |
|---|---|---|
| 1 (highest) | Event Music | Server-wide or local event soundtrack |
| 2 | Boss Music | Active boss encounter soundtrack |
| 3 | Combat Music | Generic combat intensity layer or combat-specific track |
| 4 | Weather Override | Weather-specific track for the current zone |
| 5 | Day/Night Override | Time-of-day variant for the current zone |
| 6 (lowest) | Zone Default | The zone's default track |

### Dynamic Music Layers

When enabled, the music system adds or removes intensity layers based on game state rather than switching tracks entirely.

| Layer | Trigger | Description |
|---|---|---|
| Base | Always | The ambient foundation layer for the zone |
| Combat | Player enters combat | Adds percussion, tempo increase, or combat instruments |
| Danger | Player HP below 25% | Adds tension elements (heartbeat, dissonance) |
| Peaceful | Player is skilling | Removes intensity, adds calm elements |
| Victory | Boss killed or combat ends | Brief triumphant swell before returning to base |

---

## 5. Custom Music

Support for dungeon masters and optionally players to upload custom audio tracks.

### Upload Configuration

| Field | Type | Description |
|---|---|---|
| Uploader | enum | `dm_only` or `dm_and_players` (DM controls) |
| Supported Formats | list[string] | `mp3`, `ogg`, `wav`, `flac` |
| Max File Size | integer (MB) | Maximum file size per track (default 10 MB) |
| Max Total Storage | integer (MB) | Maximum total custom music storage per server |
| Content Moderation | boolean | Flag uploaded tracks for review before they become available |
| Auto-Scan | boolean | Automated scan for copyrighted content (if available) |

### Community Music Packs

Pre-assembled collections of tracks organized by theme. Dungeon masters can browse, download, and install packs.

| Field | Type | Description |
|---|---|---|
| Pack ID | string | Unique identifier for the music pack |
| Pack Name | string | Display name |
| Theme | string | Descriptive theme (`medieval`, `sci-fi`, `horror`, `tropical`, `dungeon`) |
| Track Count | integer | Number of tracks in the pack |
| Total Size | integer (MB) | Total download size |
| License | string | License type (CC0, CC-BY, custom) |
| Author | string | Pack creator credit |
| Preview Track | string | Sample track for preview before download |

---

## 6. Now Playing HUD

A heads-up display element showing the currently playing track.

### HUD Configuration

| Field | Type | Description |
|---|---|---|
| Enabled | boolean | Whether the now playing display is visible |
| Show on Unlock | boolean | Display when a new track is unlocked |
| Show on Zone Change | boolean | Display when the track changes due to zone transition |
| Show on Manual Play | boolean | Display when the player manually selects a track |
| Position | enum | `top_left`, `top_right`, `bottom_left`, `bottom_right`, `custom` |
| Custom X | integer (pixels) | Horizontal offset (if position is custom) |
| Custom Y | integer (pixels) | Vertical offset (if position is custom) |
| Display Duration | integer (seconds) | How long the display remains visible (0 = always visible) |
| Fade Duration | integer (ms) | Fade in and fade out animation duration |
| Font Size | enum | `small`, `medium`, `large` |
| Show Track Number | boolean | Display "X / Total" alongside the track name |
| Always Visible | boolean | Override display duration and keep the HUD permanently visible |

---

## 7. Volume Controls

Granular volume controls for every audio category.

### Volume Channels

| Channel | Description |
|---|---|
| Master | Controls overall volume for all audio |
| Music | Background music tracks |
| Sound Effects | Action-triggered sounds (attacks, gathering, UI clicks) |
| Ambient | Environmental background sounds (wind, water, fire, crowd) |
| Voice Chat | Player-to-player voice communication |
| UI Sounds | Interface interaction sounds (button clicks, tab switches, notifications) |
| Combat Sounds | Hit sounds, special attack effects, prayer activation |
| Skilling Sounds | Resource gathering, crafting, processing sounds |
| Environment | Zone-specific ambient loops (cave drips, forest birds, ocean waves) |
| NPC Dialogue | NPC speech and dialogue audio cues |
| Notifications | Achievement popups, level up jingles, drop alerts |

### Volume Settings

| Field | Type | Description |
|---|---|---|
| Channel | string | One of the volume channels above |
| Volume | float (0.0 - 1.0) | Volume level for this channel |
| Muted | boolean | Whether this channel is muted regardless of volume level |

### Per-Zone Volume Override (DM-configured)

| Field | Type | Description |
|---|---|---|
| zone_id | integer | Zone this override applies to |
| channel | string | Which volume channel to override |
| volume_multiplier | float | Multiplier applied to the player's volume setting (e.g., 0.5 = half volume) |
| description | string | Reason for override (e.g., "library is a quiet zone") |

---

## 8. Sound Effects

Configurable sounds for game events, with support for custom uploads and themed sound packs.

### Event Sounds

| Event | Default Sound | Description |
|---|---|---|
| Level Up | Fanfare jingle | Plays when a skill level is gained |
| Rare Drop | Chime with sparkle | Plays when a rare item drops |
| Quest Complete | Triumphant horn | Plays when a quest is completed |
| Achievement | Medal ping | Plays when an achievement is earned |
| Death | Low thud | Plays when the player dies |
| PvP Kill | Sharp strike | Plays when the player kills another player |
| Pet Obtain | Magical chime | Plays when a pet is obtained |
| Trade Complete | Coin clink | Plays when a trade finishes |
| Boss Spawn | Rumble | Plays when a boss encounter begins |
| Collection Log Entry | Book page turn | Plays when a new collection log entry is added |

### Tiered Drop Sounds

Different sounds play based on the value tier of dropped items.

| Tier | Value Range | Sound Character |
|---|---|---|
| Common | 0 - 999 GP | Soft tap |
| Uncommon | 1,000 - 49,999 GP | Light chime |
| Rare | 50,000 - 999,999 GP | Clear bell |
| Very Rare | 1,000,000 - 9,999,999 GP | Resonant gong |
| Ultra Rare | 10,000,000+ GP | Full fanfare |

### DM Custom Sounds

| Field | Type | Description |
|---|---|---|
| Event | string | Which game event this sound applies to |
| File Path | string | Path to the custom audio file |
| Supported Formats | list[string] | `mp3`, `ogg`, `wav` |
| Max File Size | integer (KB) | Maximum file size per sound effect (default 500 KB) |
| Volume | float (0.0 - 1.0) | Default playback volume for this sound |
| Priority | integer | If multiple sounds trigger simultaneously, higher priority plays first |

### Sound Packs

Community-created themed sound effect sets that replace default sounds.

| Field | Type | Description |
|---|---|---|
| Pack ID | string | Unique identifier |
| Pack Name | string | Display name |
| Theme | string | Descriptive theme (`retro_8bit`, `orchestral`, `minimal`, `comedy`, `horror`) |
| Sound Count | integer | Number of sounds in the pack |
| Author | string | Pack creator credit |
| License | string | License type |
| Replaces | list[string] | Which default event sounds this pack overrides |

### Accessibility: Visual Sound Indicators

For deaf and hard-of-hearing players, visual indicators replace or supplement audio cues.

| Indicator | Description |
|---|---|
| Screen Flash | Brief screen border flash on important events (color-coded by event type) |
| Directional Arrow | Arrow pointing toward the source of a sound (footsteps, combat, NPC) |
| Subtitle Bar | Text description of ambient and environmental sounds |
| Vibration | Controller vibration on supported devices (mapped to sound intensity) |
| Icon Popup | Small icon appears on screen representing the sound (sword clash, pickaxe, water) |

---

## Views

Predefined views for the music and audio interface.

| View | Description |
|---|---|
| Track Library | All tracks listed with unlock status, mood, category, and zone. Filterable and searchable |
| Playlists | Player-created playlists with track listing, reorder, and playback controls |
| Now Playing | Current track with progress bar, play/pause, skip, and volume |
| Volume Mixer | All volume channels with sliders and mute toggles |
| Sound Settings | Per-event sound configuration, sound pack selection, and accessibility toggles |
| Jukebox | Browse, search, queue, and DJ mode controls (when interacting with a jukebox object) |
| Music Unlock Progress | Completion percentage per zone and category, with hints for undiscovered tracks |
