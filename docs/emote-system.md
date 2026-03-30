# Emote System

Player expressions, gestures, and animations triggered on demand. More than cosmetic -- emotes are used in clue scrolls, social interaction, clan events, and player expression.

---

## 1. Default Emotes

Available to all players from account creation. No unlock required.

| Emote | Category | Looping |
|---|---|---|
| Wave | Expression | No |
| Bow | Expression | No |
| Dance | Dance | Yes |
| Dance 2 | Dance | Yes |
| Dance 3 | Dance | Yes |
| Clap | Expression | No |
| Cry | Expression | No |
| Laugh | Expression | No |
| Think | Expression | No |
| Shrug | Expression | No |
| Yes | Expression | No |
| No | Expression | No |
| Angry | Expression | No |
| Cheer | Expression | No |
| Beckon | Expression | No |
| Panic | Expression | No |
| Sit | Action | Yes |
| Push-up | Action | Yes |
| Headbang | Action | Yes |
| Salute | Expression | No |
| Stomp | Action | No |
| Flex | Action | No |
| Spin | Action | No |
| Yawn | Expression | No |
| Stretch | Action | No |
| Blow Kiss | Expression | No |
| Goblin Bow | Dance | No |
| Goblin Dance | Dance | Yes |

---

## 2. Unlockable Emotes

Emotes earned through gameplay, achievements, or purchases. Each has a specific unlock method and condition.

| Unlock Method | Description | Examples |
|---|---|---|
| Quest | Awarded on quest completion | Quest-specific victory emotes, celebration dances |
| Achievement | Unlocked by reaching milestones | Skill cape emotes (one per mastered skill), combat achievement emotes |
| Event | Available during limited-time events | Holiday emotes (snowball throw, pumpkin head, egg juggle) |
| Minigame | Earned through minigame participation or rewards | Victory dances, team salutes |
| Clue Scroll | Received as clue scroll rewards | Rare emotes from master clues |
| Cosmetic Shop | Purchased with premium currency (if monetization enabled) | Premium dance styles, flashy celebrations |

---

## 3. Emote Properties

| Property | Type | Description |
|---|---|---|
| Emote ID | Integer | Unique identifier |
| Name | String | Display name shown in emote panel |
| Animation ID | Integer | Reference to the animation data in the animation system |
| Duration | Integer (ticks) | How long the emote plays |
| Interruptible | Boolean | Whether walking cancels the emote |
| Looping | Boolean | Repeats until cancelled (true) or plays once (false) |
| Sound Effect | Audio ID | Sound played when emote activates |
| Unlock Method | Enum | Default, Quest, Achievement, Purchase, Event, Minigame, Clue |
| Unlock Condition | Varies | Specific quest ID, achievement ID, item cost, or event name |
| Members Only | Boolean | Restricted to members-only worlds |
| Category | Enum | Expression, Dance, Action, Holiday, Skill, Custom |

---

## 4. Synchronized Emotes

Two or more players perform an emote together with animations that sync up.

| Property | Description |
|---|---|
| Proximity Requirement | Both players must be within a configurable tile radius |
| Activation | Both activate the same sync-eligible emote near each other, or one player targets another and sends a sync request |
| Partner Selection | Optional -- target a specific player and request a synchronized emote |
| Sync Window | Time window in ticks for the second player to activate the emote |
| Animation Sync | Animations lock to the same start tick once both players confirm |

### Synchronized Emote List

| Emote | Players Required | Description |
|---|---|---|
| Synchronized Dance | 2+ | Coordinated dance routine |
| High-Five | 2 | Players face each other and slap hands |
| Handshake | 2 | Formal greeting animation |
| Group Cheer | 2+ | Collective celebration, scales with participant count |

---

## 5. Emotes with Props

Some emotes spawn a temporary prop object that appears for the emote duration and disappears after.

| Emote | Prop | Behavior |
|---|---|---|
| Air Guitar | Guitar model | Spawns in player hands, removed on emote end |
| Uri | NPC (Uri) | Spawns nearby for clue emote verification, despawns after confirmation |
| Flag Wave | Flag model | Spawns in player hand, flag design matches clan or custom colors |
| Snowball Throw | Snowball projectile | Spawns and travels toward target, holiday event only |
| Sit (Throne) | Throne chair | Spawns under player, cosmetic override variant |

| Property | Description |
|---|---|
| Prop Spawn | Prop appears at emote start tick |
| Prop Duration | Matches emote duration exactly |
| Prop Cleanup | Prop removed from world on emote end or interruption |
| Prop Visibility | Visible to all nearby players |

---

## 6. Emote Overrides

Replace default emote animations with cosmetic variants. Only visual -- no gameplay change.

| Override Type | Description | Source |
|---|---|---|
| Rest Override | Different sit/rest animation | Cosmetic shop or achievement |
| Dance Override | Different dance style | Cosmetic shop or achievement |
| Victory Override | Different celebration animation | Cosmetic shop or achievement |
| Teleport Override | Different departure/arrival animation | Cosmetic shop or achievement |
| Walk Override | Different walk/run cycle | Cosmetic shop or achievement |

| Property | Description |
|---|---|
| Override Slot | Each emote category has one override slot |
| Revert | Player can revert to default at any time |
| Preview | Player can preview override before equipping |
| No Gameplay Effect | Overrides are purely cosmetic -- no stat or mechanic change |
| Stacking | Only one override per slot -- new override replaces previous |

---

## 7. Emotes in Gameplay

Emotes serve functional roles beyond expression.

| Use Case | Description |
|---|---|
| Clue Scroll Emote Steps | Wear specific items and perform a specific emote at a specific location to advance a clue scroll. Uri NPC appears to verify |
| Social Skill XP | If a socializing skill is enabled, performing emotes near other players grants XP |
| Clan Events | Fashion shows, dance competitions, and ceremony events use emotes as participation mechanics |
| Communication | Quick non-verbal expression -- wave to greet, bow to show respect, beckon to call someone over |
| NPC Interaction | Certain NPCs respond to specific emotes (bowing to a king, dancing for a bard) |

---

## 8. Custom Emotes

DM-created emotes and player-suggested emotes.

| Feature | Description |
|---|---|
| DM Creation | DM assigns an animation, sets unlock condition, picks category, and names the emote |
| Animation Assignment | DM selects from existing animations or uploads a custom animation |
| Unlock Condition | DM defines how players earn the emote (quest, achievement, purchase, free) |
| Category Assignment | DM places emote in an existing category or creates a new one |
| Player Suggestions | If community proposals are enabled, players can submit emote ideas for DM review |
| Approval Workflow | Suggested emotes go through a review queue before being added to the game |

---

## Module Toggles

| Toggle | Default |
|---|---|
| Default Emotes | Off |
| Unlockable Emotes | Off |
| Synchronized Emotes | Off |
| Emotes with Props | Off |
| Emote Overrides | Off |
| Clue Scroll Emote Steps | Off |
| Social Skill XP from Emotes | Off |
| Custom Emotes (DM) | Off |
| Player Emote Suggestions | Off |
| Members-Only Emotes | Off |
| Cosmetic Shop Emotes | Off |

---

## Views

| View | Description |
|---|---|
| Emote Panel | Grid of all available emotes, greyed out if locked, with unlock hints |
| Emote Unlocks | List of all unlockable emotes with progress toward each |
| Override Manager | Equip and preview emote overrides per slot |
| Sync Emote Requests | Pending sync emote invitations from other players |
| Custom Emote Editor | DM tool for creating and managing custom emotes |
| Emote Suggestion Queue | DM review queue for player-submitted emote ideas |
