# Dialogue

Game design document for the Dialogue plugin. This system covers NPC conversation modes, dialogue trees, branching consequences, AI-powered freeform chat, relationship tracking, and UI presentation. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the dialogue module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Scripted Dialogue | Classic fixed dialogue trees with predetermined lines and options |
| Branching Dialogue | Choice-driven conversations with consequences, flags, and variable tracking |
| AI Freeform Dialogue | LLM-powered NPC responses based on persona definitions and world context |
| Hybrid Mode | Scripted for quests and shops, AI freeform for casual conversation, per NPC |
| Relationship / Affinity | Per-NPC affinity scores that unlock dialogue, quests, items, and areas |
| Romance | Romantic relationship paths with compatible NPCs |
| Morality Tracking | Player choices accumulate morality or alignment scores that affect dialogue |
| NPC Memory | NPCs remember past conversations and reference them in future interactions |
| NPC-to-NPC Dialogue | NPCs converse with each other as ambient world dialogue |
| Voice Acting | Dialogue lines can have associated audio files for voice playback |
| Timed Responses | Player must choose a response within a time limit or a default is selected |
| Dialogue Log | Scrollable history of past conversation lines within the current session |
| Chat Head Portraits | Animated or static character portraits displayed during conversation |
| Gift System | Players can give items to NPCs to raise affinity scores |
| Gossip System | NPCs share information about player actions with other NPCs |

---

## Dialogue Modes

A global setting determines which dialogue modes are available. Each NPC can use one mode or mix modes depending on context.

| Mode | Description |
|---|---|
| Scripted | Classic fixed dialogue trees. Every line and option is predetermined by the dungeon master. |
| Branching | Dating sim or visual novel style. Choices carry consequences, set flags, and alter future conversations. |
| AI Freeform | LLM-powered. The NPC responds dynamically based on a persona definition and world context. |
| Hybrid | Scripted for structured interactions (quests, shops, tutorials). AI freeform for casual chat and lore questions. |

---

## 1. Scripted Dialogue

Traditional dialogue trees where every line, option, and outcome is authored in advance. Each tree is linked to an NPC by NPC ID.

### Dialogue Tree Structure

A dialogue tree is a graph of nodes. Each node contains one NPC line and up to five player response options. Each option points to the next node.

#### Nodes

| Column | Type | Description |
|---|---|---|
| Node ID | Integer | Unique identifier within the tree |
| NPC ID | Integer | Foreign key to the NPC this tree belongs to |
| NPC Line | Text | The line the NPC speaks at this node |
| Mood / Emotion | Enum | Emotional tag for animation selection (happy, angry, suspicious, sad, neutral, scared, excited) |
| Voice Acting File | String | Path to the audio file for this line, if voice acting is enabled |
| Typing Speed | Enum | Letter by letter, instant, or custom words per minute |
| Auto-advance | Enum | Click to continue, timed (specify seconds), or none |
| Chat Head | Enum | Portrait position (left, right, none) |
| Random Variants | Text List | Pool of alternative lines the NPC randomly picks from for variety |

#### Player Options

One row per option. A node can have one to five options, or zero for terminal nodes.

| Column | Type | Description |
|---|---|---|
| Node ID | Integer | Foreign key to the parent node |
| Option Index | Integer | Display order (1 through 5) |
| Option Text | Text | The line the player can choose |
| Next Node ID | Integer | Node to advance to when selected, or null for conversation end |

#### Option Conditions

Conditions determine whether a player option is visible or hidden. Multiple conditions per option are AND-joined.

| Column | Type | Description |
|---|---|---|
| Node ID | Integer | Foreign key to the parent node |
| Option Index | Integer | Which option this condition applies to |
| Condition Type | Enum | Quest state, skill level, item in inventory, reputation threshold, flag set, previous choice |
| Condition Key | String | Identifier for the condition (quest ID, skill name, item ID, flag name) |
| Condition Value | String | Required value (quest stage, minimum level, minimum reputation, true/false) |
| Operator | Enum | Equals, greater than, less than, not equals, contains |

#### Node Conditions

Conditions determine whether an entire node is available. Used for time-gated, weather-gated, or quest-progress-gated dialogue.

| Column | Type | Description |
|---|---|---|
| Node ID | Integer | The node this condition gates |
| Condition Type | Enum | Time of day, weather, quest progress, flag set, calendar event |
| Condition Key | String | Identifier for the condition |
| Condition Value | String | Required value |

#### Actions on Select

Actions fire when the player selects an option. Multiple actions per option are executed in sequence.

| Column | Type | Description |
|---|---|---|
| Node ID | Integer | Foreign key to the parent node |
| Option Index | Integer | Which option triggers this action |
| Action Type | Enum | Give item, remove item, start quest, advance quest, open shop, teleport, trigger cutscene, set flag, add XP, change reputation, play animation, play sound |
| Action Target | String | Identifier for the target (item ID, quest ID, shop ID, coordinate, flag name) |
| Action Value | String | Quantity, destination, flag value, or other parameter |

### Scripted Dialogue Properties

| Property | Type | Description |
|---|---|---|
| Repeatable | Boolean | Whether the dialogue resets after completion or plays only once |
| One-time Lines | Boolean | Specific nodes are skipped on repeat conversations |
| Random Entry | Boolean | NPC picks a random starting node from a pool on each interaction |

---

## 2. Branching Dialogue

Extends scripted dialogue with persistent consequences. Uses all tables from Section 1, plus the systems below. Linked by NPC ID.

### Consequence System

Choices set flags and variables that persist across the entire world and affect future dialogue with all NPCs.

#### Flags and Variables

| Column | Type | Description |
|---|---|---|
| Flag ID | String | Unique identifier for the flag or variable |
| Type | Enum | Boolean (flag), integer (counter), string (state) |
| Scope | Enum | Global (world-wide), NPC-specific, faction-specific, quest-specific |
| Set By | String | Node ID and option index that sets this value |
| Permanent | Boolean | Whether this flag can be unset or is locked once set |

#### Consequence Effects

| Effect | Description |
|---|---|
| Flag mutation | Set, increment, decrement, or clear a flag or variable |
| Cross-NPC propagation | Flags affect dialogue options and nodes across all NPCs in the world |
| Faction reputation change | Modify the player's standing with a faction (positive or negative) |
| Morality shift | Adjust the player's morality or alignment score |
| Locked branch | Dialogue branches that require a flag, affinity threshold, or reputation level to appear |
| Point of no return | Irreversible choices that permanently close other branches, with a warning prompt |
| Multiple endings | Quest or story conclusions that vary based on accumulated flags |
| Default on timeout | If timed responses are enabled, an unanswered prompt selects a default option |

#### Timed Responses

| Column | Type | Description |
|---|---|---|
| Node ID | Integer | The node where timing applies |
| Time Limit | Integer | Seconds the player has to choose |
| Default Option | Integer | Option index selected if time expires |
| Show Timer | Boolean | Whether the countdown is visible to the player |
| Penalty | String | Optional consequence for letting time expire (reputation loss, mood change) |

### Relationship System

Per-NPC affinity tracking that gates dialogue, quests, items, and areas behind relationship progress.

#### Affinity Scores

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | The NPC this score tracks |
| Player ID | Integer | The player this score belongs to |
| Affinity | Integer | Current relationship score, range -100 to 100 |
| Tier | Enum | Stranger, acquaintance, friend, close friend, best friend |
| Romance | Boolean | Whether the romantic relationship path is active |

#### Affinity Configuration

| Setting | Type | Description |
|---|---|---|
| Tier Thresholds | Integer List | Affinity values where each tier begins (e.g., -100, -25, 0, 25, 50, 80) |
| Decay Enabled | Boolean | Whether affinity decreases over time without interaction |
| Decay Rate | Integer | Points lost per game day when decay is enabled |
| Decay Floor | Integer | Minimum affinity that decay cannot drop below |
| Rival Links | NPC ID List | Raising affinity with this NPC lowers affinity with linked rivals |
| Rival Penalty | Float | Multiplier for how much rival affinity decreases (e.g., 0.5 = half the gain) |

#### Gift System

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | The NPC who receives gifts |
| Item ID | Integer | The item that can be given |
| Affinity Change | Integer | How much affinity changes when this item is given |
| Dialogue Response | Text | What the NPC says when receiving this item |
| Daily Limit | Integer | Maximum gifts this NPC accepts per game day |
| Diminishing Returns | Boolean | Whether repeated gifts of the same item yield less affinity |

#### Friendship and Romance Tiers

| Tier | Threshold | Unlocks |
|---|---|---|
| Stranger | -100 to -26 | Hostile or dismissive dialogue only |
| Acquaintance | -25 to -1 | Basic dialogue, no personal topics |
| Neutral | 0 to 24 | Standard dialogue, can ask about the world |
| Friend | 25 to 49 | Personal dialogue, minor favors, some quests |
| Close Friend | 50 to 79 | Exclusive quests, discounted shop prices, unique items |
| Best Friend | 80 to 100 | All dialogue unlocked, companion system, unique storylines |
| Romantic Partner | 80 to 100 (with romance flag) | Romance-specific dialogue, co-op quests, shared housing |

---

## 3. AI Freeform Dialogue

LLM-powered conversation where the NPC responds dynamically based on a persona definition. Linked by NPC ID. All structured interactions (quests, shops, trades) still use scripted triggers --- AI freeform handles casual conversation, lore questions, and ambient interaction only.

### NPC Persona Definition

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | The NPC this persona belongs to |
| Name | String | NPC display name |
| Role | String | Functional role (shopkeeper, guard, scholar, farmer, quest giver) |
| Personality | Text | Free-text personality description used as LLM system prompt context |
| Background | Text | Backstory and lore relevant to this NPC |
| World Knowledge | Text | Scoped world information this NPC is aware of (local area, faction, trade routes) |
| Knowledge Boundaries | Text | Topics this NPC explicitly does not know about (other regions, secret quests) |
| Speech Style | Enum | Formal, casual, pirate, medieval, sci-fi, custom |
| Custom Speech Rules | Text | Specific vocabulary, catchphrases, or grammatical quirks |
| Default Mood | Enum | Baseline emotional state (cheerful, grumpy, nervous, stoic) |
| Reactive Mood | Boolean | Whether mood shifts based on player actions and conversation tone |

### NPC Memory

| Setting | Type | Description |
|---|---|---|
| Memory Enabled | Boolean | Whether the NPC remembers past conversations with this player |
| Memory Depth | Integer | Number of past conversation turns stored per player |
| Memory Scope | Enum | Per-player (each player gets unique memory) or global (all players share memory) |
| Memory Persistence | Enum | Session only, daily reset, permanent |
| Summary Mode | Boolean | Old conversations are summarized rather than stored verbatim to save context |

### Guardrails

Restrictions that prevent AI freeform dialogue from breaking game systems or immersion.

| Guardrail | Description |
|---|---|
| No item grants | AI cannot give items, gold, or XP through conversation. Must use scripted triggers. |
| No quest advancement | AI cannot start, advance, or complete quests. Must use scripted triggers. |
| No solution reveals | AI cannot reveal quest puzzle solutions or hidden location coordinates. |
| Character lock | AI must stay in character. Attempts to break character return a deflection line. |
| Scripted fallback | If the AI cannot generate a valid response, fall back to a scripted line pool. |
| Response length limit | Maximum token count per response to prevent monologues. |
| Profanity filter | Configurable word filter applied to AI output. |
| Topic restrictions | Dungeon master defines banned topics per NPC or globally. |

### LLM Configuration

| Setting | Type | Description |
|---|---|---|
| Model Selection | Enum | Local small model (on-device), cloud API, or pluggable (player chooses) |
| Model ID | String | Specific model identifier (e.g., qwen-0.5b, claude-sonnet, gpt-4o-mini) |
| Temperature | Float | Randomness of responses (0.0 = deterministic, 1.0 = creative) |
| Context Window | Integer | Maximum tokens of conversation history sent per request |
| Rate Limit | Integer | Maximum AI responses per player per minute |
| Cost Cap | Float | Maximum API cost per player per day (cloud models only) |
| Offline Fallback | Enum | Scripted lines, cached responses, or disable NPC |
| Latency Tolerance | Integer | Maximum milliseconds to wait for a response before falling back |

### NPC-to-NPC Dialogue

Ambient conversations between NPCs that bring the world to life.

| Setting | Type | Description |
|---|---|---|
| Ambient Dialogue | Boolean | NPCs converse with each other when idle |
| Gossip | Boolean | NPCs reference player actions in ambient dialogue |
| Knowledge Sharing | Boolean | NPCs exchange world knowledge through conversation |
| Schedule Awareness | Boolean | NPCs have different dialogue at different times of day and locations |
| Player Eavesdrop | Boolean | Players within proximity can read NPC-to-NPC conversations |
| Gossip Propagation | Enum | Instant (all NPCs know immediately), gradual (spreads over game time), network (follows NPC social connections) |

---

## 4. Dialogue UI Settings

Presentation and display settings for dialogue across all modes.

### Display Style

| Setting | Type | Description |
|---|---|---|
| Chat Box Style | Enum | Classic text box, visual novel (fullscreen overlay), speech bubble (in-world), chat window (scrolling log) |
| Portrait Display | Enum | Left side, right side, alternating, or none |
| Portrait Animation | Enum | Animated (lip sync, expressions), static image, or none |
| Name Display | Boolean | Whether the speaker's name is shown above the dialogue text |
| Name Color | Enum | Default, role-based (quest givers gold, merchants green), custom per NPC |

### Text Settings

| Setting | Type | Description |
|---|---|---|
| Text Speed | Enum | Instant, typewriter (letter by letter), or custom words per minute |
| Custom WPM | Integer | Words per minute when text speed is set to custom |
| Font Override | String | Per-NPC font or style override for unique characters |
| Skip Button | Boolean | Whether the player can skip text animations to show the full line instantly |
| Auto-advance | Boolean | Whether dialogue advances automatically after a delay |
| Auto-advance Delay | Integer | Seconds to wait before auto-advancing |

### History and Accessibility

| Setting | Type | Description |
|---|---|---|
| Dialogue Log | Boolean | Scrollable history of all lines in the current conversation |
| Persistent Log | Boolean | Dialogue log persists after the conversation ends (viewable in a menu) |
| Translation Support | Boolean | Dialogue text is passed through a translation layer for localization |
| Text-to-Speech | Boolean | Dialogue lines are read aloud by a system voice (accessibility) |
| Font Size Override | Enum | Small, medium, large, or player-defined |

---

## 5. Linked Spreadsheets Summary

All dialogue data is linked by NPC ID. The following spreadsheets define the full data model.

| Spreadsheet | Key | Description |
|---|---|---|
| Dialogue Nodes | Node ID, NPC ID | NPC lines, emotion tags, voice files, display settings |
| Player Options | Node ID, Option Index | Response text and next node pointers |
| Option Conditions | Node ID, Option Index | Visibility conditions for each player option |
| Node Conditions | Node ID | Visibility conditions for entire nodes |
| Actions on Select | Node ID, Option Index | Effects triggered when an option is chosen |
| Flags and Variables | Flag ID | Persistent state set by branching choices |
| Affinity Scores | NPC ID, Player ID | Per-player relationship scores and tiers |
| Gift Definitions | NPC ID, Item ID | Items NPCs accept and their affinity effects |
| NPC Personas | NPC ID | AI persona definitions for freeform dialogue |
| NPC Memory | NPC ID, Player ID | Stored conversation history for AI recall |

---

## 6. Views

| View | Description |
|---|---|
| All NPCs with Dialogue | Master list of every NPC that has dialogue data, filterable by mode and location |
| By Relationship Score | NPCs sorted by the current player's affinity rating |
| By Quest Involvement | NPCs grouped by the quests they participate in |
| Dialogue Tree Visualizer | Flowchart view of a scripted dialogue tree with node connections and conditions |
| Conversation History | Per-player, per-NPC log of all past dialogue interactions |
