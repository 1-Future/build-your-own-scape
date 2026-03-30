# Communication

The communication system covers all forms of player-to-player and system-to-player messaging. Every subsystem is modular --- Dungeon Masters (world creators) toggle each feature on or off per world.

---

## 1. Chat Channels

All text communication flows through typed channels. Each channel has independent permissions, rate limits, and retention rules.

### Channel Types

| Channel | Scope | Default Access | Description |
|---------|-------|----------------|-------------|
| Public | Nearby tiles | All players | Local area chat visible to players within proximity range |
| Private / Whisper | 1-on-1 | All players | Direct messages between two players |
| Clan | Clan members | Clan members | Restricted to members of the same clan |
| Group | Party / raid / team | Group members | Temporary channel for the duration of a group activity |
| Trade | Server-wide | All players | Buy and sell offers, price negotiations |
| Global | Server-wide | All players (toggle) | General server-wide conversation, can be toggled off by the player |
| Help | Server-wide | All players | Newbie questions and community answers |
| LFG | Server-wide | All players | Looking for group postings for bosses, raids, minigames |
| Officer | Clan leadership | Ranked clan members | Private channel for clan officers and above |
| Alliance | Cross-clan | Alliance members | Communication between allied clans |
| Custom | DM-defined | DM-defined | World creators define name, scope, and rules |
| Area | Location-based | Players in area | Tied to a specific map region (e.g., a city, dungeon floor) |
| Shout | Larger radius | All players | Extended-range public chat, costs GP or has a cooldown |
| Roleplay | Server-wide or area | All players | Dedicated channel for in-character roleplay |

### Per-Channel Configuration

| Setting | Description |
|---------|-------------|
| Read permissions | Who can see messages (rank-based, role-based, proximity-based) |
| Send permissions | Who can post messages (rank-based, role-based) |
| Rate limit | Maximum messages per time window |
| Cooldown | Delay between consecutive messages from the same player |
| Character limit | Maximum characters per message |
| History retention | How long messages are stored (hours, days, forever) |
| Cross-world | Whether the channel bridges multiple world instances |

---

## 2. Quick Chat

Pre-scripted messages that players can send without typing. Useful for accessibility, restricted accounts, and fast communication during combat.

### Message Categories

| Category | Examples |
|----------|---------|
| Stat announcements | "I just reached 99 Mining!" / "My combat level is 126." |
| Boss KC sharing | "My Vorkath kill count is 1,243." |
| Skill XP sharing | "I have 14.2M Woodcutting XP." |
| Quest progress | "I've completed Dragon Slayer." / "I'm on step 4 of Recipe for Disaster." |
| Equipment sharing | "I'm wearing full Rune." / broadcasts current gear |
| Location sharing | "I'm at Lumbridge." / shares current coordinates |
| LFG messages | "Looking for 2 more for Corp Beast." |
| Social responses | Yes / No / Thanks / GG / BRB / AFK / Nice / Sorry |
| Context-sensitive | Different options appear based on location (e.g., bank options at a bank, trade options at the GE) |
| Custom presets | DM creates world-specific quick chat lines |

### Quick Chat Settings

| Setting | Description |
|---------|-------------|
| Quick chat only mode | Restricts a player to pre-scripted messages only (no free typing) |
| Quick chat worlds | Entire world instances where only quick chat is allowed |
| Keyboard shortcuts | F-key bindings for frequently used quick chat messages |
| Search | Text search across all quick chat options |
| Favorites | Player pins their most-used quick chat messages |
| Recent | History of recently sent quick chat messages |

---

## 3. Voice Chat

Two voice chat modes: proximity-based (spatial audio in the game world) and channel-based (traditional voice rooms).

### Proximity Voice Chat

| Feature | Description |
|---------|-------------|
| Voice range | Maximum tile distance at which a player can be heard |
| Volume falloff | Voice gets quieter with distance from the speaker |
| Directional audio | Stereo panning based on speaker position relative to listener |
| Input mode | Push to talk, open mic, or voice activation threshold |
| Mute / deafen | Per-player mute and self-deafen controls |
| Per-player volume | Listener adjusts individual speaker volumes independently |

### Proxy Voice Chat (Server-Relayed)

All voice data routes through the server. No peer-to-peer connections, no direct IP exposure.

| Feature | Description |
|---------|-------------|
| No direct IP exposure | All audio relayed through server, players never connect directly |
| Server-side mixing | Server handles audio mixing to reduce client load |
| Encryption | Voice data encrypted in transit |
| Recording prevention | Toggle to block local recording of voice channels |
| Voice modulation | Fun voice effects (pitch shift, echo, robot) --- toggleable per world |
| Text-to-speech | Accessibility feature: reads text chat aloud for visually impaired players |
| Speech-to-text | Accessibility feature: transcribes voice chat to text for hearing impaired players |

### Voice Chat Settings

| Setting | Description |
|---------|-------------|
| VC enabled | Global on/off toggle for all voice chat |
| Proximity VC | Enable/disable proximity-based spatial voice |
| Channel VC only | Restrict voice to traditional channel rooms (no proximity) |
| VC in PvP | Whether players can hear enemies in PvP zones |
| VC in raids | Auto-join voice channel when entering a raid instance |

---

## 4. Emotes and Expressions

Three layers of expression: text emojis, GIFs, and in-game character animations.

### Text Emojis

| Feature | Description |
|---------|-------------|
| Toggle | DM can enable or disable emojis per world |
| Custom packs | DM uploads custom emoji packs for the world |
| Emoji reactions | React to messages with emojis (Discord-style) |
| Unlockable emojis | Earned through quests, achievements, or events |
| Clan emojis | Clan-exclusive emojis uploaded by clan leadership |
| Animated | Toggle for animated emoji support |

### GIFs

| Feature | Description |
|---------|-------------|
| Toggle | DM can enable or disable GIFs per world |
| Search | Integrated GIF library or custom uploads |
| Size limit | Maximum file size and display dimensions |
| NSFW filter | Automatic filtering of inappropriate GIFs |
| GIF reactions | React to messages with GIFs |

### In-Game Emotes (Character Animations)

| Feature | Description |
|---------|-------------|
| Default emotes | Wave, dance, bow, cry, laugh, sit, think, clap, salute, shrug |
| Unlockable emotes | Earned through quests, achievements, events, or shops |
| Custom emotes | Premium or earned custom character animations |
| Emote wheel | Radial menu for quick emote selection |
| Synchronized emotes | Two-player emotes (handshake, high five, dance together) |
| Emote with prop | Animations that include a prop object (e.g., playing a lute, waving a flag) |

---

## 5. Chat Bots

Automated assistants that live in chat channels. Split into built-in system bots and custom/player-created bots.

### Built-In Bots

| Bot | Function |
|-----|----------|
| Welcome bot | Greets new players, provides starter info |
| Help bot | Answers common questions, links to wiki/guides |
| Price check bot | Returns current market prices for items |
| Drop rate bot | Reports drop rates for monsters and bosses |
| Timer bot | Tracks respawn timers, event countdowns, farming patches |
| Moderation bot | Enforces chat rules, issues warnings, auto-mutes |
| Announcement bot | Broadcasts server events, updates, maintenance notices |
| Trivia bot | Runs trivia games in chat channels |
| Poll bot | Creates and tallies polls |
| Loot tracker bot | Logs and reports loot drops for the session or lifetime |

### Custom Bots

| Feature | Description |
|---------|-------------|
| DM-created bots | World creators build custom bots for their world |
| Bot scripting API | Programmatic interface for bot behavior |
| Bot permissions | Which channels a bot can read and post in |
| Bot rate limit | Maximum messages per time window per bot |
| Player-created bots | Toggle to allow players to create and run bots |
| Bot marketplace | Shared library of community-made bots |

### Discord-Style Integration

| Feature | Description |
|---------|-------------|
| Webhook support | External services push messages into chat channels |
| Bot commands | `!` or `/` prefix for triggering bot actions |
| Slash commands | `/price dragon bones`, `/kc vorkath`, `/timer corp` |
| Rich embeds | Bots post formatted cards with images, fields, and links |

---

## 6. Messaging Features

General features that apply across all text-based communication.

| Feature | Description |
|---------|-------------|
| Formatting | Bold, italic, underline, colored text (toggleable per world) |
| @mentions | Tag a player by name, triggers a notification |
| @everyone / @clan | Ping all members of a channel or clan (permission-gated) |
| Reply threading | Reply to a specific message, creates a thread |
| Pin messages | Pin important messages to the top of a channel |
| Edit messages | Edit sent messages (toggleable per world) |
| Delete messages | Delete own messages |
| Forward messages | Forward a message to another channel or player |
| Read receipts | See when a whisper/DM has been read (toggleable) |
| Message search | Full-text search across chat history |
| Chat export | Export chat logs to file |
| Link previews | Inline preview of linked content (toggleable) |
| Spoiler tags | Hide text behind a click-to-reveal spoiler |

---

## 7. Chat Appearance

Player-configurable visual settings for the chat interface.

| Setting | Options |
|---------|---------|
| Box position | Bottom, side, floating (draggable) |
| Size | Resizable width and height |
| Transparency | Background opacity slider |
| Font size | Small, medium, large, or custom pixel value |
| Font style | Default, monospace, serif, or custom |
| Colors per channel | Each channel type has a distinct text color |
| Timestamps | Toggle message timestamps on or off |
| Level icons | Show player level or skill icons next to names |
| Chat bubbles | Speech bubbles above characters in the game world (toggle + duration) |
| Compact mode | Reduced spacing between messages for higher density |

---

## 8. Chat Moderation

Tools for moderators, clan officers, and automated systems to maintain chat quality.

### Moderation Actions

| Action | Description |
|--------|-------------|
| Mute player | Temporary or permanent mute from one or all channels |
| Slow mode | Per-channel enforced delay between messages |
| Auto-mod | Automated keyword filter, spam detection, link blocking |
| Shadow mute | Player sees their own messages but nobody else does |
| Chat logs | Searchable moderation log of all messages |
| Report message | Right-click a message to flag it for review |
| Word filter customization | Moderators add or remove words from the filter list |
| Regex filters | Pattern-based filters for advanced moderation rules |
| Warning system | Tracked warnings per player, escalating consequences |

### Chat Filter Toggles

Each filter is independently toggleable per channel or per world.

| Filter | Description |
|--------|-------------|
| Profanity filter | On, off, or custom word list |
| Slur filter | Separate from general profanity, stricter enforcement |
| Spam filter | Detects repeated messages and floods |
| Link filter | Blocks or requires approval for URLs |
| Advertising filter | Blocks gold-selling, boosting, and real-world trade spam |
| Custom blocked words | DM or moderator-defined blocked terms |
| No filter mode | DM accepts responsibility, all filters off (adults-only worlds) |
| Per-channel filtering | Different filter levels per channel |

---

## Module Toggles

Every subsystem can be independently enabled or disabled by the Dungeon Master at the world level.

### Channels

| Toggle | Default |
|--------|---------|
| Public chat | On |
| Global chat | On |
| Trade chat | On |
| Help chat | On |
| LFG chat | On |
| Custom channels | On |

### Communication Features

| Toggle | Default |
|--------|---------|
| Quick chat system | On |
| Quick chat only mode | Off |
| Voice chat | On |
| Proximity VC | On |
| Proxy VC | On |
| Voice modulation | Off |
| Text-to-speech | On |
| Speech-to-text | On |

### Expression

| Toggle | Default |
|--------|---------|
| Text emojis | On |
| GIFs | On |
| In-game emotes | On |

### Bots

| Toggle | Default |
|--------|---------|
| Chat bots (built-in) | On |
| Custom bots (DM) | On |
| Player-created bots | Off |

### Messaging

| Toggle | Default |
|--------|---------|
| @mentions | On |
| Message editing | On |
| Message deleting | On |
| Reply threading | On |
| Chat bubbles | On |
| Rich embeds | On |
| Link previews | On |
| Chat export | On |
| Cross-world chat | Off |

### Moderation

| Toggle | Default |
|--------|---------|
| Auto-mod | On |
| Slow mode | Off |
| Shadow mute | On |
| No filter mode | Off |

---

## Advanced Communication Mechanics

Proven communication systems studied from the most successful multiplayer games. Each is a standalone toggleable mechanic.

---

### Context-Aware Ping System
A single button ping that reads what you're pointing at and generates the appropriate callout automatically. Ping an enemy = "Enemy spotted here." Ping loot = "Level 3 armor here." Ping a location = "Suggesting we move here." Ping a teammate's ping = "Acknowledged." Double-ping = "Danger." Character voices auto-narrate the ping with personality. Players can communicate an entire match without typing or speaking.

| Property | Type | Description |
|----------|------|-------------|
| Ping types | Enum list | Generic, Enemy, Loot, Location, Danger, Defend, Attack, Assist, On My Way, Retreat, Acknowledged, Cancelled |
| Auto-context | Boolean | Ping reads what cursor is hovering and selects appropriate type |
| Voice callout | Boolean | Character speaks the ping aloud (e.g. "Enemy over there!") |
| Visual marker | Sprite | Icon placed in world at ping location |
| Marker duration | Ticks | How long ping marker stays visible |
| Cooldown | Ticks | Time between pings (prevents spam) |
| Ping wheel | Boolean | Hold ping button for radial menu of specific ping types |
| Teammate response | Boolean | Others can ping your ping to acknowledge |
| Minimap indicator | Boolean | Ping shows on minimap with directional arrow |
| Sound | Sound ID | Audio cue when ping is placed (different per type) |
| Visibility | Enum | Party only, Clan, Nearby players, Everyone |

---

### Proximity Voice Chat (Spatial Audio)
Voice volume scales with in-game distance between players. Close = loud and clear. Far = quiet and faded. Creates natural social spaces --- walk up to someone and start talking. Walk away and they fade out. Directional audio (left/right speaker based on player position).

| Property | Type | Description |
|----------|------|-------------|
| Max range | Tiles | Maximum distance voice carries |
| Falloff curve | Enum | Linear, logarithmic, realistic (inverse square) |
| Directional | Boolean | Stereo positioning based on speaker location |
| Obstruction | Boolean | Walls muffle/block voice (LoS-based) |
| Zone override | Boolean | Some zones disable proximity (libraries, stealth areas) |
| Zone amplify | Boolean | Some zones amplify range (amphitheaters, town squares) |
| Enemy audible | Boolean | Can enemies hear you (PvP tension) |
| Disguise voice | Boolean | Voice modulation in certain zones (anonymity) |

---

### Limited Communication (Constraint as Design)
Intentionally restricting communication to create tension, meaning, or creativity.

| Method | Description |
|--------|-------------|
| Single button expression | One "chirp" or "call" button --- all communication through timing, position, and repetition. Creates profound bonds through limitation |
| Walkie-talkie system | Voice only works through a held item. Limited range, static/distortion at edges, can be intercepted by enemies. Communication is a resource |
| Discussion phases | Communication only allowed during specific game phases (between rounds, at rest points). Forces deliberate conversation |
| Signal-only zones | Areas where text/voice is disabled --- players must communicate through movement, emotes, and pings |
| Language barrier | Different factions literally can't read each other's text chat --- must communicate through emotes and actions |
| Dead silence zones | No communication at all in certain areas --- creates isolation and dread |

---

### Asynchronous World Messages
Players leave messages IN the world for future players to discover.

| Property | Type | Description |
|----------|------|-------------|
| Message type | Enum | Template (pick from pre-written phrases), Freeform (write anything --- if moderated), Drawing (simple sketch) |
| Template system | String builder | "Try [action] near [object]" or "[Warning] ahead" --- combinable fragments that prevent abuse while allowing creativity |
| Placement | Position | Message appears at the tile where it was left |
| Persistence | Duration | How long message lasts (hours, days, permanent until removed) |
| Rating system | Up/Down | Other players rate messages (good messages persist longer, bad messages fade) |
| Ghost echo | Boolean | Briefly see the ghost/shadow of the player who left the message |
| Spoiler control | Boolean | Messages near quest content can be flagged/hidden |
| DM moderation | Boolean | DM can review and remove messages |
| Humor category | Boolean | Tag messages as helpful vs funny (filter by preference) |
| Max messages per player | Integer | Prevent spam --- limited active messages at once |

---

### Communication Hierarchy
Structured voice channels with chain of command --- not everyone talks to everyone.

| Channel | Who Talks | Who Listens | Use Case |
|---------|-----------|-------------|----------|
| Local | Anyone nearby | Anyone nearby | General socializing, proximity-based |
| Party/Squad | Party members | Party members | Coordinated group content |
| Leader | Party/squad leader | All leaders + own squad | Cross-party coordination in raids |
| Raid | Raid leader | Everyone in raid | Instructions, callouts during boss fights |
| Clan officers | Officers | Officers | Strategic planning |
| Clan | Any member | All members | General clan communication |
| Broadcast | DM/Admin | Everyone on server | Announcements, events |

Players can be in multiple channels simultaneously with per-channel volume control. Push-to-talk can be bound per channel (e.g., T for party, Y for raid leader).

---

### Emotes as Language
Emotes that communicate specific meanings understood universally without text or voice.

| Emote Convention | Meaning |
|-----------------|---------|
| Sit/rest near someone | "I'm friendly, I want to hang out" |
| Dance near someone | "Celebrate with me" or "Let's party" |
| Point at ground | "Come here" or "Stand here" |
| Point at object | "Look at this" or "Use this" |
| Wave | "Hello" or "Goodbye" |
| Bow | "Thank you" or "Respect" |
| Shake head/No | "Don't do that" or "I disagree" |
| Nod/Yes | "Go ahead" or "I agree" |
| Cry | "I'm sad" or "That's unfortunate" |
| Flex | "I'm strong" or "Look at my gear" |
| Synchronized dance | Group bond, celebration, community identity |

### Emote Combos
Emote sequences that create emergent communication:
- Wave -> Point -> Follow = "Come with me"
- Sit -> Point at ground = "Rest here, it's safe"
- Angry -> Point at player = "You did something wrong"
- Dance -> Dance -> Dance = "Party! Everyone join!"
- Bow -> Bow (returned) = Mutual respect, ready to duel

---

### Honor/Reputation System for Communication
Reward positive communication behavior rather than only punishing negative.

| Feature | Description |
|---------|-------------|
| Honor vote | After group content, vote for who communicated best (most helpful, most positive, best callouts) |
| Honor levels | Accumulate honor over time. Higher honor = cosmetic rewards (chat border, name color, badge) |
| Honor decay | Honor slowly decreases over time --- must actively be positive, not just avoid being negative |
| Toxicity penalty | Reports AND low honor both reduce standing. Consistently toxic = restricted communication |
| Mentor bonus | Helping new players (answering questions, guiding) earns bonus honor |
| Shotcaller badge | Players recognized for good ping/callout usage in group content |
| Community pillar | Title for players who maintain high honor for extended periods |

---

### Quick Chat Wheel (Radial Menu)
Hold a key to open a radial wheel with 8-12 preset phrases. Categorized, customizable, instant.

| Property | Type | Description |
|----------|------|-------------|
| Wheel slots | 8-12 | Number of phrases on the wheel |
| Categories | Enum | Combat, Social, Trade, Navigation, Emote, Custom |
| Customizable | Boolean | Player can swap out default phrases for their own |
| Voice line | Boolean | Character speaks the phrase aloud with personality |
| Icon | Sprite | Visual icon per phrase for non-readers |
| Localized | Boolean | Phrase auto-translates for recipient's language |
| Cooldown | Ticks | Prevent spam |
| Memorable lines | Toggle | Some phrases become community culture/memes --- encourage personality in default lines |

---

### Structured Communication Moments
Deliberate game phases where communication is the GAMEPLAY --- not background utility.

| Moment | Description |
|--------|-------------|
| Pre-boss briefing | Forced pause before boss fight. Party must discuss strategy. Timer before fight starts |
| Post-round discussion | After a PvP round or minigame wave, brief discussion period before next round |
| Trial/court | Players present evidence and argue (for rules/moderation, or in-game quest mechanic) |
| Negotiation | Two parties must reach agreement (clan territory disputes, trade deals, quest branching) |
| Voting | Players vote on outcome, forced to communicate preferences before vote |
| Confession/reveal | Quest moment where NPC or player reveals information --- triggers dialogue phase |
| Campfire | Rest mechanic where players gathered around campfire get bonus if they chat/emote (socializing XP boost) |

---

### Non-Verbal Communication Tools
For players who can't or don't want to use text or voice.

| Tool | Description |
|------|-------------|
| Ping system | Context-aware pings (covered above) |
| Emote wheel | Quick emote access (covered above) |
| Reaction bubbles | Quick reaction icons above head (thumbs up, heart, laugh, sad, angry, question mark) --- click and it shows briefly |
| Drawing tool | Simple drawing on ground or map --- arrow, circle, X mark, checkmark. Visible to party |
| Waypoint sharing | Place waypoint, party sees it on their map and minimap |
| Ready check | Thumbs up/down before group content |
| Inventory show | Flash your equipped gear to others (show what you're wearing without trading) |
| Item linking | Link an item in chat by clicking it --- others see the item's stats |
| Status indicator | Set a status above your head --- "LFG", "Trading", "AFK", "Mentoring", "Do Not Disturb" |
| Music performance | Play instruments as communication --- play a tune, others recognize it |

---

### Module Toggles (append to existing toggles)

| Toggle | Default | Description |
|--------|---------|-------------|
| Context-aware pings | On | Smart ping system that reads cursor target |
| Ping wheel | On | Radial menu for specific ping types |
| Ping voice callouts | On | Characters speak pings aloud |
| Proximity voice (spatial) | Off | Distance-based voice with falloff |
| Voice obstruction | Off | Walls muffle proximity voice |
| Enemy voice audible | Off | Opponents can hear your proximity voice |
| Limited communication zones | Off | Areas with restricted communication |
| Asynchronous world messages | Off | Leave messages in the world for others |
| Message templates | On | Template-based world message system (prevents abuse) |
| Message rating | On | Players upvote/downvote world messages |
| Communication hierarchy | On | Structured voice channels with roles |
| Honor system | Off | Reputation rewards for positive communication |
| Quick chat wheel | On | Radial menu for preset phrases |
| Custom wheel phrases | On | Players edit their wheel phrases |
| Structured communication moments | Off | Forced discussion phases in gameplay |
| Reaction bubbles | On | Quick reaction icons above head |
| Drawing tool | Off | Simple drawing visible to party |
| Status indicators | On | LFG, Trading, AFK above head |
| Item linking | On | Share item stats in chat by clicking |
| Emote combos | On | Emote sequences create emergent meaning |
| Campfire social boost | Off | XP bonus for socializing at rest points |

---

## Views

The communication system exposes the following UI panels and screens.

| View | Description |
|------|-------------|
| Chat settings | Player-level preferences for appearance, notifications, and filters |
| Channel directory | Browse and join available channels |
| Bot management | Configure, enable, and disable bots (DM/moderator) |
| Mod tools | Moderation dashboard with logs, reports, mute controls |
| Chat history / search | Full-text search across past messages |
| Emoji / emote library | Browse, search, and favorite emojis and in-game emotes |
| Quick chat browser | Browse and search all quick chat options |
| VC channel list | Browse and join voice chat channels |

---

## Additional Features (Plugin Audit)

| Feature | Description |
|---------|-------------|
| Chat Translation | Real-time language translation in chat. Incoming messages auto-translated to your language. Outgoing messages optionally translated. Powered by translation API. |
| Custom Emoji | Player-uploadable or server-defined custom emoji for chat. Emoji packs. Clan-specific emoji. Unlockable emoji from achievements. |
| Item Linking | Share item links in chat that others can hover to inspect stats, requirements, and value. Click to open item details. Similar to WoW item linking. |
| Chat Audio Notifications | Per-channel and per-keyword sound alerts. Different sounds for PM, clan chat, mentions, specific words. Configurable per trigger. |
| Proximity Voice Chat | Spatial voice chat where volume scales with player distance. Hear nearby players, volume fades with distance. Directional audio (left/right). |
| Tactical Ping System | Quick communication pings on the game world (like League of Legends). Alert, danger, assist, on my way, retreat. Visible to party/clan. Hotkey-triggered. |
| Rich Interactive Chat | Clickable elements in chat messages: item links, coordinate links (click to mark on map), player name links (click to inspect). Formatted messages with embeds. |
| External Chat Bridges | Bidirectional bridges between in-game chat and external platforms (Discord, IRC, Telegram, Matrix). Per-channel mapping. Identity linking. |
| Notification Center | Unified notification feed/panel collecting all game notifications (drops, levels, trades, clan events, friend logins) in one scrollable history. Filter by type. |
| In-Game Drawing Tool | Drawing/painting on the world map or screen. Annotate positions, draw strategies, sketch plans. Visible to party. Erasable. |
| AI Game Assistant | In-game AI assistant NPC or chatbot that answers questions about game mechanics, suggests activities, and provides information. Uses game database, not external knowledge. |
| Streaming Integration | Twitch/YouTube integration. Live stats overlay, chat integration, subscriber alerts, scene switching based on game state. |
| Chat Transcript Export | Export chat history to file (text, JSON, HTML). Configurable date range. Per-channel export. Searchable archive. |
| Chat Clipboard | Copy individual chat messages or selections to clipboard. Right-click to copy. |
| Chat Slash Commands | In-chat commands (e.g. !kc, !lvl, !price) that query and display player stats inline. Configurable command prefix. Extensible command list |
