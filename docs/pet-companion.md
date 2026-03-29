# Pet and Companion System

Game design document for the Pet and Companion plugin. This system covers pet acquisition, naming, cosmetic transmog, raising and growth, abilities, storage, insurance, and collection tracking.

---

## Module Toggles

Each toggle enables or disables a subsystem within the pet module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Pet System | Master toggle for all pet functionality |
| Pet Naming | Players can assign custom names to their pets |
| Pet Transmog | Reskin pets to look like different creatures |
| Pet Raising | Pets grow through stages and gain XP from interactions |
| Happiness and Hunger | Pets have happiness and hunger meters that affect behavior |
| Pet Abilities | Pets provide passive or active gameplay benefits |
| Pet Combat Assist | Pets deal small amounts of damage in combat |
| Pet Foraging | Pets passively find items while following the player |
| Pet Storage and Housing | Pets can be stored in a pet house within player housing |
| Pet Insurance | Pets can be insured against loss on death |
| Pet Collection Tracking | Track total pets obtained and progress toward completion |

---

## 1. Pet List

Every obtainable pet in the game. One row per pet.

### Pet Data Model

| Field | Type | Description |
|---|---|---|
| id | string | Unique pet identifier |
| name | string | Default display name |
| source | enum | How the pet is obtained (`boss_drop`, `skilling_milestone`, `quest_reward`, `achievement`, `purchase`, `event`, `minigame`, `random`) |
| source_id | integer or null | ID of the boss, quest, achievement, or minigame that awards this pet |
| drop_rate | string | Drop chance expressed as `1/X` (e.g., `1/3000`) |
| obtain_method | string | Human-readable description of how to obtain (e.g., "Kill Vorkath", "Reach 99 Mining", "Complete Dragon Slayer II") |
| skill_requirement | integer or null | Minimum skill level required to obtain (skilling pets) |
| skill_type | string or null | Which skill the pet is associated with (skilling pets) |
| tradeable | boolean | Whether the pet can be traded between players |
| insurable | boolean | Whether the pet can be insured |
| insurance_cost | integer | GP cost to insure this pet |
| rarity_tier | enum | `common`, `uncommon`, `rare`, `very_rare`, `ultra_rare` |
| model_id | string | Reference to the pet's 3D model or sprite |
| examine_text | string | Text shown when examining the pet |
| release_version | string | Game version when the pet was added |

---

## 2. Pet Properties

Properties assigned to a pet instance once a player obtains it.

### Instance Data Model

| Field | Type | Description |
|---|---|---|
| instance_id | string | Unique identifier for this specific pet instance |
| pet_id | string | Reference to the pet definition |
| owner_id | string | Player who owns this pet |
| custom_name | string or null | Player-assigned name (null = uses default name) |
| display_name_visible | boolean | Whether the custom name appears above the pet |
| active | boolean | Whether this pet is currently following the player |
| obtained_at | datetime | When the pet was obtained |
| obtained_kc | integer or null | Kill count at time of obtaining (boss pets) |

### Follow Behavior

| Behavior | Description |
|---|---|
| Walk Behind | Pet follows the player at walking pace, positioned one tile behind |
| Fly Above | Pet hovers above the player's head |
| Hover Beside | Pet floats to the left or right of the player |
| Orbit | Pet slowly circles the player |

### Interaction Emotes

| Emote | Description |
|---|---|
| Pet | Player pets the companion (happiness boost) |
| Feed | Player feeds the companion an item (hunger reduction, happiness boost) |
| Play | Player plays with the companion (happiness boost, trick XP) |
| Sit | Pet sits down beside the player |
| Dance | Pet performs a dance animation |
| Speak | Pet makes its characteristic sound |

### Idle Animations

Each pet has a set of idle animations that play randomly when the player is stationary.

| Field | Type | Description |
|---|---|---|
| animation_id | string | Unique animation identifier |
| pet_id | string | Which pet this animation belongs to |
| trigger | enum | `idle`, `combat`, `skilling`, `resting`, `random` |
| frequency | float | How often this animation plays (0.0 - 1.0, relative weight) |
| duration | integer (ticks) | Length of the animation |

### Sound Effects

| Field | Type | Description |
|---|---|---|
| pet_id | string | Which pet this sound belongs to |
| event | enum | `spawn`, `interact`, `idle`, `feed`, `happy`, `hungry`, `combat` |
| sound_file | string | Path to the audio file |
| volume | float (0.0 - 1.0) | Playback volume |

---

## 3. Pet Cosmetic Transmog

Reskin a pet to look like a different creature without changing its identity or stats.

### Transmog Data Model

| Field | Type | Description |
|---|---|---|
| transmog_id | string | Unique transmog identifier |
| pet_id | string | Which pet this transmog can be applied to |
| appearance_name | string | Display name of the transmog appearance |
| model_id | string | Reference to the alternate model or sprite |
| unlock_method | enum | `achievement`, `purchase`, `event`, `quest`, `default` |
| unlock_id | integer or null | ID of the achievement, quest, or event that unlocks this transmog |
| color_variant | boolean | Whether this is a color variant rather than a full model swap |
| color_options | JSON or null | Available colors if color variant (e.g., `{"primary": ["#FF0000", "#00FF00", "#0000FF"]}`) |
| revertible | boolean | Whether the player can revert to the original appearance (always true) |

### Transmog Rules

- A pet can have multiple transmog options available.
- Only one transmog can be active at a time per pet.
- Transmog is purely cosmetic and does not affect pet stats, abilities, or growth.
- The pet's identity in the collection log is unchanged by transmog.
- Reverting to the original appearance is always free.

---

## 4. Pet Raising

Pets grow through life stages, gain XP, and develop stats through player interaction. Ties into the Pet Raising skill if that skill is enabled.

### Growth Stages

| Stage | XP Required | Description |
|---|---|---|
| Baby | 0 | Newly obtained. Small model. Limited animations |
| Juvenile | 1,000 | Slightly larger. More animations unlocked |
| Adult | 10,000 | Full-size model. All animations available |
| Elder | 100,000 | Maxed growth. Visual distinction (scars, glow, aura). Bonus idle animations |

### XP Sources

| Source | XP per Action | Description |
|---|---|---|
| Feeding | 5 - 50 | Varies by food quality |
| Petting | 10 | Once per 5 minutes |
| Playing | 15 | Once per 10 minutes |
| Training (tricks) | 25 | Teaching the pet a new trick |
| Bonding (time spent active) | 1 per minute | Passive XP while the pet is following the player |
| Combat (pet assists in combat) | 5 per kill | Only if pet combat assist is enabled |

### Happiness Meter

| Range | Label | Effect |
|---|---|---|
| 80 - 100 | Joyful | Pet performs bonus idle animations, higher foraging chance |
| 50 - 79 | Content | Normal behavior |
| 20 - 49 | Unhappy | Reduced idle animations, no foraging |
| 0 - 19 | Miserable | Pet refuses to perform emotes, displays sad animations |

### Happiness Modifiers

| Action | Happiness Change | Description |
|---|---|---|
| Feed (favorite food) | +15 | Each pet has a favorite food type |
| Feed (any food) | +5 | Any valid food item |
| Pet emote | +10 | Player uses the pet interaction emote |
| Play emote | +10 | Player plays with the pet |
| Neglect (no interaction for 24h) | -20 | Happiness decays if ignored |
| Owner death | -10 | Pet is distressed when the owner dies |
| Long session (4h+ active) | +5 | Bonus for extended companionship |

### Hunger Meter

| Range | Label | Effect |
|---|---|---|
| 80 - 100 | Full | No effect |
| 50 - 79 | Satisfied | No effect |
| 20 - 49 | Hungry | Happiness decays 2x faster |
| 0 - 19 | Starving | Happiness decays 4x faster, pet whimpers periodically |

### Hunger Configuration

| Field | Type | Description |
|---|---|---|
| Drain Rate | integer (per hour) | How fast hunger decreases (default 5 per hour) |
| Feed Restore | integer | How much hunger is restored per feeding (varies by food) |
| Favorite Food | string | Item ID of the pet's preferred food (2x restore) |
| Starvation Penalty | boolean | Whether starvation has gameplay consequences beyond happiness |

### Pet Stats

| Stat | Description |
|---|---|
| Speed | How quickly the pet follows the player (cosmetic, does not affect player speed) |
| Loyalty | Increases with bonding time. Higher loyalty = faster happiness recovery |
| Tricks Learned | Number of tricks the pet has been taught |
| Total XP | Cumulative XP earned across all sources |
| Growth Stage | Current life stage (baby, juvenile, adult, elder) |

---

## 5. Pet Abilities

Optional gameplay benefits provided by pets. Every ability is DM-togglable.

### Ability List

| Ability | Type | Description |
|---|---|---|
| Combat Assist | Active | Pet deals small damage per attack (scales with pet level, capped) |
| Gathering Assist | Passive | Pet automatically picks up ground items within 1 tile |
| Alert | Passive | Pet warns the player when a PKer or dangerous NPC is nearby |
| Beast of Burden | Passive | Pet carries extra items (configurable capacity, 1 - 10 slots) |
| Skill Boost | Passive | Small invisible boost to a specific skill while the pet is active (+1 to +3) |
| Foraging | Passive | Pet occasionally finds items (herbs, seeds, bones) while following the player |
| Scouting | Passive | Pet reveals nearby resources or enemies on the minimap |
| Luck | Passive | Small increase to rare drop chance while pet is active |

### Ability Configuration

| Field | Type | Description |
|---|---|---|
| ability_id | string | Unique ability identifier |
| pet_id | string | Which pet has this ability (or `all` for universal abilities) |
| enabled | boolean | Whether this ability is active (DM toggle) |
| unlock_stage | enum | Minimum growth stage required (`baby`, `juvenile`, `adult`, `elder`) |
| cooldown | integer (seconds) | Cooldown between ability activations (active abilities) |
| proc_chance | float (%) | Chance of triggering per tick (passive abilities) |
| scaling | JSON or null | How the ability scales with pet stats (e.g., `{"loyalty": 0.5, "tricks": 0.1}`) |

### Combat Assist Details

| Field | Type | Description |
|---|---|---|
| Base Damage | integer | Minimum damage per pet attack |
| Max Damage | integer | Maximum damage per pet attack |
| Attack Speed | integer (ticks) | How often the pet attacks |
| Damage Type | enum | `melee`, `ranged`, `magic` |
| Scales with Pet Level | boolean | Whether damage increases with pet growth stage |
| PvP Enabled | boolean | Whether the pet attacks in PvP (DM toggle, default off) |

### Beast of Burden Details

| Field | Type | Description |
|---|---|---|
| Capacity | integer | Number of inventory slots the pet provides |
| Item Restrictions | list[string] or null | Items the pet cannot carry (e.g., untradeable, quest items) |
| Drops on Death | boolean | Whether beast of burden items drop on player death |
| Access Method | enum | `right_click_pet`, `inventory_button`, `automatic` |

---

## 6. Pet Storage

Systems for storing pets that are not currently following the player.

### Pet House (Player Housing)

| Field | Type | Description |
|---|---|---|
| Max Display Slots | integer | How many pets can be displayed in the pet house |
| Display Mode | enum | `roaming` (pets walk around the room) or `pedestal` (pets stand on display) |
| Visible to Visitors | boolean | Whether house visitors can see displayed pets |
| Interact | boolean | Whether visitors can interact with displayed pets (pet, examine) |

### Pet Bank

| Field | Type | Description |
|---|---|---|
| Capacity | integer | Maximum number of pets that can be stored |
| Search | boolean | Search stored pets by name, source, or rarity |
| Sort Options | list[string] | `name`, `date_obtained`, `rarity`, `source`, `growth_stage` |
| Quick Swap | boolean | Swap active pet directly from the pet bank interface |

### Active Pet Limit

| Field | Type | Description |
|---|---|---|
| Max Active | integer | Number of pets that can follow the player simultaneously (default 1) |
| Multi-Pet Mode | boolean | Allow more than one active pet (DM toggle, default off) |
| Multi-Pet Cap | integer | Maximum active pets if multi-pet mode is enabled (default 3) |

### Pet Showcase

| Feature | Description |
|---|---|
| Public Collection | Other players can view your pet collection via right-click profile |
| Showcase Display | Select up to 5 pets to feature on your public profile |
| Rarity Highlight | Ultra-rare pets are visually highlighted in the showcase |
| KC Display | Show the kill count at which boss pets were obtained |
| Growth Display | Show the growth stage of each showcased pet |

---

## 7. Pet Insurance

System for protecting pets against permanent loss on player death.

### Insurance Data Model

| Field | Type | Description |
|---|---|---|
| instance_id | string | The pet instance being insured |
| insured | boolean | Whether this pet is currently insured |
| insurance_cost | integer | GP paid to insure (set per pet in pet list) |
| reclaim_cost | integer | GP paid to reclaim after death (percentage of insurance cost, default 50%) |
| insured_at | datetime | When the pet was insured |
| reclaim_npc_id | integer | NPC from whom the pet is reclaimed |

### Death Behavior

| Scenario | Insured | Uninsured |
|---|---|---|
| PvM Death | Reclaim from NPC for fee | Lost forever (if DM enables permanent loss) |
| PvP Death | Reclaim from NPC for fee | Lost forever (if DM enables permanent loss) |
| Safe Death (minigame) | No effect | No effect |
| Disconnection | Pet returns to bank | Pet returns to bank |

### Configuration

| Field | Type | Description |
|---|---|---|
| Permanent Loss Enabled | boolean | Whether uninsured pets are permanently lost on death (DM toggle) |
| Insurance Cost Multiplier | float | Multiplier applied to base insurance costs (DM sets, default 1.0) |
| Reclaim Cost Percentage | float (%) | Percentage of insurance cost charged to reclaim (default 50%) |
| Grace Period | integer (deaths) | Number of deaths before permanent loss applies to new players (default 3) |
| Free Insurance Tier | enum or null | Rarity tier below which insurance is free (e.g., `common`) |

---

## 8. Pet Collection

Tracking system for all obtainable pets, modeled after the collection log.

### Collection Data Model

| Field | Type | Description |
|---|---|---|
| player_id | string | Player whose collection this is |
| total_obtainable | integer | Total number of pets in the game |
| total_obtained | integer | Number of unique pets the player has obtained |
| completion_percent | float (%) | Percentage of total pets obtained |
| obtained_list | array[object] | List of obtained pets with instance ID, date, and KC |
| missing_list | array[string] | List of pet IDs not yet obtained |

### Collection Categories

| Category | Description |
|---|---|
| Boss Pets | Pets obtained from boss kills |
| Skilling Pets | Pets obtained from skilling milestones |
| Quest Pets | Pets obtained from quest completion |
| Achievement Pets | Pets obtained from achievement completion |
| Minigame Pets | Pets obtained from minigame rewards |
| Event Pets | Pets obtained from limited-time events |
| Miscellaneous | Pets from other sources (purchase, random, etc.) |

### Milestones

| Milestone | Requirement | Reward |
|---|---|---|
| First Pet | Obtain any pet | Achievement unlock |
| Pet Collector I | Obtain 5 unique pets | Title: "Pet Lover" |
| Pet Collector II | Obtain 15 unique pets | Cosmetic: pet-themed outfit piece |
| Pet Collector III | Obtain 30 unique pets | Cosmetic: pet-themed outfit set |
| Boss Pet Master | Obtain all boss pets | Title: "Beast Whisperer" |
| Skilling Pet Master | Obtain all skilling pets | Title: "Nature's Friend" |
| All Pets | Obtain every pet in the game | Pet Completionist Cape, Title: "Zoologist" |

---

## Views

Predefined views for the pet and companion interface.

| View | Description |
|---|---|
| Pet Collection | Grid of all pets showing obtained vs unobtainable, filterable by category, source, and rarity |
| Active Pet | Current following pet with stats, growth stage, happiness, hunger, and interaction buttons |
| Pet Stats and Growth | Detailed view of a pet's XP, stage progress, tricks learned, and stat breakdown |
| Pet Transmog Wardrobe | Available transmog options for the selected pet with preview |
| Pet Insurance Status | List of all owned pets with insurance status, cost, and reclaim information |
| Pet Bank | Stored pets with search, sort, and quick-swap functionality |
| Pet Showcase | Configure which pets appear on your public profile |
