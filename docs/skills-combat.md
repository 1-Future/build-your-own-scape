# Combat Skills

Game design document for all combat skills. This system covers melee (Attack, Strength, Defence), Hitpoints, Ranged, Magic, Prayer, Slayer, and optional RS3 extensions (abilities, adrenaline, dual wielding, Summoning, Necromancy). Every property is configurable per world by the dungeon master.

---

## Shared Combat Properties

Every combat skill shares a core set of properties governing levelling, XP distribution, temporary boosts, and damage calculation.

### Core

| Property | Type | Description |
|---|---|---|
| Skill level | Integer | 1--99 or 1--120, configurable per world |
| XP curve | Formula | Configurable XP required per level |
| XP per damage dealt | Formula | Default: 4x style XP + 1.33x Hitpoints XP per hit |
| Training style | Enum | Determines which skill receives XP from combat |

---

### Combat Formula Engine

The combat formula engine calculates effective levels, max hits, accuracy, and DPS. All formulas follow a strict order of operations. Dungeon masters can override constants but should preserve the step ordering.

#### Effective Level Calculation

Applied identically for Attack, Strength, Defence, Ranged, and Magic when computing rolls.

| Step | Operation |
|---|---|
| 1 | (Base level + Potion boost) x Prayer multiplier |
| 2 | Round down |
| 3 | Add style bonus (+3 Accurate/Aggressive/Defensive, +1 Controlled, +0 none) |
| 4 | Add +8 (base offset) |
| 5 | Multiply by void/set bonus multiplier (if applicable) |
| 6 | Round down |

#### Max Hit (Melee)

| Step | Operation |
|---|---|
| 1 | Effective Strength x (Equipment Strength Bonus + 64) + 320 |
| 2 | Divide by 640, round down |
| 3 | Multiply by target-specific gear bonus (slayer helm, salve amulet, etc.) |
| 4 | Round down |

#### Accuracy Roll

| Context | Formula |
|---|---|
| Attacker | Effective Attack Level x (Equipment Attack Bonus + 64) x target-specific multiplier |

#### Defence Roll

| Context | Formula |
|---|---|
| Monsters | (Defence Level + 9) x (Target Defence Bonus + 64) |
| Players | Effective Defence x (Target Defence Bonus + 64) |

#### Hit Chance

| Condition | Formula |
|---|---|
| Attack Roll > Defence Roll | 1 - (Defence Roll + 2) / (2 x (Attack Roll + 1)) |
| Attack Roll <= Defence Roll | Attack Roll / (2 x (Defence Roll + 1)) |

#### DPS

| Metric | Formula |
|---|---|
| Average damage per attack | Hit Chance x (Max Hit / 2) |
| DPS | Average damage / (Attack speed in seconds) |

---

### Temporary Boosts

| Property | Type | Description |
|---|---|---|
| Potion boost formula | Formula | Flat + % of level, configurable per potion tier |
| Prayer multiplier | Float | % boost to effective level, configurable per prayer |
| Equipment boost | Integer | Invisible level additions from gear |
| Void/set multiplier | Float | Multiplicative bonus for wearing full sets |
| Boost decay rate | Rate | Ticks per level lost (default: 1 level per minute) |
| Preserve effect | Float | Decay rate reduction % (default: 50%) |
| Divine potion | Duration | No decay for fixed duration (default: 5 minutes) |

---

### Level Milestone System

| Property | Type | Description |
|---|---|---|
| Equipment unlock table | Level to Items | Level thresholds that unlock equippable items |
| Content unlock table | Level to Content | Level thresholds that unlock activities, areas, quests |
| Prayer unlock table | Level to Prayers | Level thresholds that unlock prayers |
| Ability unlock table | Level to Abilities | Level thresholds that unlock abilities (if ability system enabled) |

---

## Attack

### Function

Determines melee accuracy. Does not affect damage.

### Properties

| Property | Type | Description |
|---|---|---|
| Attack types | Enum list | Stab, Slash, Crush (configurable, DM can add custom types) |
| Style bonus (Accurate) | Integer | +3 to effective level (default) |
| Style bonus (Controlled) | Integer | +1 to effective level (default) |
| Equipment attack bonuses | Per type | Separate bonus value per attack type (stab, slash, crush) |
| Weapon tier table | Level to Items | Which weapons require which Attack level |

### XP Gain

| Style | XP per damage | Also gains |
|---|---|---|
| Accurate | 4x Attack | 1.33x Hitpoints |
| Controlled | 1.33x Attack | 1.33x Strength, 1.33x Defence, 1.33x Hitpoints |

---

## Strength

### Function

Determines melee max hit. Does not affect accuracy.

### Properties

| Property | Type | Description |
|---|---|---|
| Equipment strength bonus | Integer | Separate from attack bonus, determines damage output |
| Style bonus (Aggressive) | Integer | +3 to effective level (default) |
| Style bonus (Controlled) | Integer | +1 to effective level (default) |
| Max hit formula | Formula | Effective Strength x (Equipment Strength Bonus + 64) / 640, rounded down |
| Weapon strength requirements | Level to Items | Which weapons require which Strength level |

### XP Gain

| Style | XP per damage | Also gains |
|---|---|---|
| Aggressive | 4x Strength | 1.33x Hitpoints |
| Controlled | 1.33x Strength | 1.33x Attack, 1.33x Defence, 1.33x Hitpoints |

---

## Defence

### Function

Determines the chance of avoiding hits entirely. Does not reduce damage taken when a hit lands.

### Properties

| Property | Type | Description |
|---|---|---|
| Defence types | Enum list | Stab, Slash, Crush, Magic, Ranged (Light, Standard, Heavy) |
| Equipment defence bonuses | Per type | Separate bonus value per defence type |
| Armour tier table | Level to Items | Which armour requires which Defence level |
| Magic defence formula | Formula | 70% Magic level + 30% Defence level + magic defence bonus |
| Ranged defence | Note | Ranged level does not contribute to ranged defence. Only Defence level + armour bonus apply |
| Style bonus (Defensive) | Integer | +3 to effective level (default) |
| Style bonus (Controlled) | Integer | +1 to effective level (default) |
| Combat level contribution | Note | Defence raises combat level regardless of other combat stats |

### XP Gain

| Style | XP per damage | Also gains |
|---|---|---|
| Defensive | 4x Defence | 1.33x Hitpoints |
| Controlled | 1.33x Defence | 1.33x Attack, 1.33x Strength, 1.33x Hitpoints |
| Longrange (Ranged) | 2x Defence | 2x Ranged, 1.33x Hitpoints |

---

## Hitpoints

Also referred to as Constitution in RS3 terminology.

### Function

Determines maximum health. The player dies when Hitpoints reaches 0.

### Properties

| Property | Type | Description |
|---|---|---|
| Max HP | Integer | Equal to Hitpoints level |
| Starting level | Integer | 10 (unique among skills -- all others start at 1) |
| XP per damage | Float | 1.33x per damage dealt, always, regardless of combat style |
| Natural regen rate | Rate | 1 HP per minute (default) |
| Regen modifiers | List | Rapid Heal prayer (2x), Regen bracelet (2x), HP cape (2x). Stacking rules configurable |
| Overheal | Toggle | Allow temporary HP above max from food or potions |
| Overheal decay | Rate | Decays at same rate as stat boosts (1 per minute, default) |
| Death threshold | Integer | 0 HP triggers death (configurable, could be negative for hardcore modes) |

### Healing Sources

| Category | Description |
|---|---|
| Food | Instant healing on consumption |
| Potions | Instant healing plus temporary stat buffs |
| Passive | Regeneration, leech effects, prayer restoration |
| Prayer | Restore HP via prayer effects |
| Spell | Heal other players via spellcasting |

### Self-Damage

| Property | Type | Description |
|---|---|---|
| Self-damage items | Toggle | Items that intentionally damage the player (rock cake, overload) |
| Self-damage lethal | Toggle per item | Whether self-damage can kill or stops at 1 HP |

---

## Ranged

### Function

Determines ranged accuracy and ranged damage. Both scale with Ranged level, unlike melee where accuracy and damage are separate skills.

### Properties

| Property | Type | Description |
|---|---|---|
| Ammo weight classes | Enum | Light (darts, knives), Standard (arrows), Heavy (bolts, javelins) |
| Weapon provides | Stat | Ranged attack bonus (accuracy) |
| Ammo provides | Stat | Ranged strength bonus (damage) |
| Thrown weapons | Note | Provide both attack and strength bonuses in a single item |
| Attack distance | Integer per weapon | Number of tiles of range, varies by weapon type |
| Style bonus (Accurate) | Integer | +3 to effective Ranged level |
| Style bonus (Rapid) | Effect | +1 attack speed (reduces ticks between attacks) |
| Style bonus (Longrange) | Effect | +2 tiles range, +3 Defence bonus, splits XP with Defence |

### Ammo System

| Property | Type | Description |
|---|---|---|
| Ammo consumption | Rate | Chance to save ammo per shot (configurable, affected by equipment such as Ava's devices) |
| Ammo recovery | Toggle | Retrieve fired ammo from the ground after combat |

### Bolt Enchantments

| Property | Type | Description |
|---|---|---|
| Bolt enchantment effects | List | Special proc effects on enchanted bolts |
| Bolt proc chance | Float | Base 10%, modified by equipment (e.g., Zaryte crossbow doubles chance) |

Built-in bolt enchantment types (DM can add custom types):

| Enchantment | Effect |
|---|---|
| Ruby | Deals damage equal to % of target's current HP |
| Diamond | Ignores a portion of target's defence |
| Dragonstone | Applies dragonfire damage |
| Onyx | Heals attacker for a portion of damage dealt |
| Opal | Deals bonus lightning damage |

### Area of Effect

| Property | Type | Description |
|---|---|---|
| Chinchompa AoE | Mechanic | Thrown explosive that hits all targets in a 3x3 or 5x5 area |

### XP Gain

| Style | XP per damage | Also gains |
|---|---|---|
| Accurate | 4x Ranged | 1.33x Hitpoints |
| Rapid | 4x Ranged | 1.33x Hitpoints |
| Longrange | 2x Ranged + 2x Defence | 1.33x Hitpoints |

---

## Magic

### Function

Determines spell accuracy. The spell itself determines damage, not the Magic level (unlike melee and ranged where level drives damage).

### Properties

| Property | Type | Description |
|---|---|---|
| Spellbook system | List | Standard, Ancient, Lunar, Arceuus (DM can create custom spellbooks) |
| Active spellbook | State | Player can only access one spellbook at a time |
| Spellbook switching | Method | Altar, Lunar spell, or DM-configured method |
| Magic damage bonus | Float | Equipment can add % bonus damage to spells (default: 0%) |
| Magic defence formula | Formula | 70% Magic level + 30% Defence level + magic defence bonus |
| Splash mechanic | Toggle | Allow casting spells with 0% accuracy for XP (no damage, only cast XP) |

### Rune System

| Property | Type | Description |
|---|---|---|
| Rune cost | Per spell | Spells consume runes from inventory on cast |
| Rune substitution | Equipment | Elemental staves replace the corresponding rune cost (e.g., fire staff eliminates fire rune cost) |

### Casting Mechanics

| Property | Type | Description |
|---|---|---|
| Autocast | Toggle per weapon | Weapon automatically casts the selected spell each attack tick |
| Powered staves | Mechanic | Built-in spell requiring no runes, works on any spellbook |
| Spell max hit | Per spell | Each spell has a fixed max hit, independent of Magic level |

### Spell Types

| Type | Description |
|---|---|
| Combat | Offensive spells that deal damage |
| Teleport | Spells that transport the player to a location |
| Utility | Non-combat effects (alchemy, enchanting, binding) |
| Enchant | Apply magical properties to items |
| Curse | Debuff spells that weaken targets |

### Spell Properties (per spell)

| Property | Type | Description |
|---|---|---|
| Spell ID | Integer | Unique identifier |
| Name | String | Display name |
| Spellbook | Enum | Which spellbook the spell belongs to |
| Level requirement | Integer | Magic level required to cast |
| Rune cost | List | Runes consumed per cast |
| Base max hit | Integer | Maximum damage (combat spells only) |
| Effect | Varies | Freeze, teleport, enchant, cure, buff, debuff |
| Cast animation | ID | Visual animation played on cast |
| Projectile | ID | Visual projectile traveling to target |
| Autocastable | Boolean | Whether the spell can be set to autocast |

---

## Prayer

### Function

Provides toggleable buffs that drain prayer points over time. Only one prayer per conflict group can be active simultaneously.

### Properties

| Property | Type | Description |
|---|---|---|
| Prayer points | Integer | Maximum equals Prayer level. Consumed while prayers are active |
| Drain resistance | Formula | 60 + (2 x prayer bonus from equipment) |
| Drain formula | Formula | Seconds to drain 1 point = 0.6 x (drain_resistance / total_drain_effect) |
| Prayer groups | List | Conflicting groups (only one active per group at a time) |
| Protection PvE | Float | Damage reduction vs NPCs (default: 100%) |
| Protection PvP | Float | Damage reduction vs players (default: 40%) |
| Prayer flicking | Toggle | Allow activate/deactivate per tick for zero-cost usage |
| 1-tick flicking | Toggle | Allow zero-cost prayer through frame-perfect toggling |

### Prayer Properties (per prayer)

| Property | Type | Description |
|---|---|---|
| Prayer ID | Integer | Unique identifier |
| Name | String | Display name |
| Level requirement | Integer | Prayer level required to activate |
| Drain effect | Float | Higher values drain prayer points faster |
| Group | Enum | Conflict group (offence, defence, overhead, utility) |
| Effect type | Enum | Stat boost, protection, utility, restoration |
| Effect value | Varies | e.g., +15% strength, protect from melee |
| Overhead icon | ID | Icon displayed above the player's head (overhead prayers only) |

### Training

| Property | Type | Description |
|---|---|---|
| Bone types | List | Bone ID to base XP. Tiered from normal bones to superior dragon bones |
| Altar multiplier | Float | Gilded altar = 3.5x, Ectofuntus = 4x, Chaos altar = 3.5x with 50% bone save |
| Ensouled heads | Mechanic | Reanimate creatures for prayer XP through combat |
| Bone burying | XP | Base XP granted per bone type when buried |
| Scatter ashes | XP | Base XP granted per ash type when scattered |

---

## Slayer

### Function

Task-based combat skill. A slayer master assigns a monster type and kill count. The player earns Slayer XP for each kill on task.

### Properties

| Property | Type | Description |
|---|---|---|
| Task system | Core | Master assigns a monster type and a kill count |
| Slayer masters | List | NPC ID, name, combat level requirement, slayer level requirement, task weight table |
| Task weighting | Per master | Each monster has a weight value (higher weight = more likely to be assigned) |
| Block list | Integer | Maximum blocked tasks (default: 6). Blocking costs slayer points |
| Prefer list | Integer | Maximum preferred tasks. Increases weight of selected task types |
| Skip cost | Integer | Slayer points required to skip an unwanted task |

### Streak System

| Property | Type | Description |
|---|---|---|
| Streak counter | Integer | Consecutive task completions without skipping |
| Slayer points per task | Per master | Higher-level masters award more points |
| Streak bonus points | Formula | Extra points at streak milestones (10th, 50th, 100th, etc.) |
| Turael skipping | Toggle | Reset task at the lowest-level master (resets streak to 0) |

### Slayer Rewards

| Property | Type | Description |
|---|---|---|
| Slayer point shop | List | Items and unlocks purchasable with slayer points |
| Slayer-only monsters | Toggle | Monsters that can only be damaged with the required slayer level |
| Equipment requirements | Per monster | Specific gear required to deal damage (facemask, mirror shield, etc.) |
| Superior monsters | Toggle | Rare enhanced spawns with better drop tables (default: 1/200 chance, toggleable) |
| Boss tasks | Toggle | Slayer masters can assign boss variants of task monsters |

### Task Variants

| Property | Type | Description |
|---|---|---|
| Konar location lock | Toggle | Task restricted to a specific dungeon or area for a bonus loot table |
| Wilderness tasks | Toggle | Separate task list for wilderness-exclusive slayer master |

---

## RS3 Combat Extensions

All extensions in this section are disabled by default. Dungeon masters opt in per feature.

---

### Ability System

| Property | Type | Description |
|---|---|---|
| Ability system | Toggle | Enable or disable ability-based combat (vs auto-attack only) |
| Global cooldown | Ticks | Time between ability uses (default: 3 ticks / 1.8 seconds) |
| Action bar | Integer | Number of ability slots on the action bar (default: 14) |
| Revolution mode | Toggle | Auto-fire basic abilities in order from the action bar |
| Full manual mode | Default | Player triggers every ability manually |
| Legacy mode | Toggle | Disable abilities entirely, use auto-attacks and special attacks only |

#### Ability Tiers

| Tier | Description |
|---|---|
| Basic | Generate adrenaline. Low cooldown. Moderate damage |
| Threshold | Require 50% adrenaline, drain 15%. High damage |
| Ultimate | Require 60--100% adrenaline. Massive effects |

#### Ability Properties (per ability)

| Property | Type | Description |
|---|---|---|
| Ability ID | Integer | Unique identifier |
| Name | String | Display name |
| Tier | Enum | Basic, Threshold, Ultimate |
| Style | Enum | Melee, Ranged, Magic, Defence, Constitution, Necromancy |
| Level requirement | Integer | Skill level required to unlock |
| Cooldown | Ticks | Cooldown duration after use |
| Damage range | % range | Min--max damage as a percentage of ability damage |
| Channeled | Boolean | Whether the ability locks the player in place during cast |
| Weapon requirement | Enum | Dual wield only, two-handed only, any, or specific weapon type |

---

### Adrenaline System

| Property | Type | Description |
|---|---|---|
| Adrenaline bar | 0--100% | Resource pool for threshold and ultimate abilities |
| Basic generation | Float | Default: 9% per basic ability hit |
| Threshold cost | Float | Default: requires 50%, drains 15% |
| Ultimate cost | Float | Default: requires and drains 60--100% |
| Out-of-combat decay | Toggle | Adrenaline drains when not in combat |
| Persistent rage | Toggle | Adrenaline does not decay (relic or perk) |

---

### Dual Wielding

| Property | Type | Description |
|---|---|---|
| Dual wield | Toggle | Allow main-hand + off-hand weapons simultaneously |
| DW vs 2H balance | Config | Equal DPS (default) or differentiated damage profiles |
| DW-specific abilities | List | Abilities only available when dual wielding |
| 2H-specific abilities | List | Abilities only available with two-handed weapons |

---

### Summoning

| Property | Type | Description |
|---|---|---|
| Summoning skill | Toggle | Enable the companion summoning skill |
| Summoning points | Resource | Drain over time while a familiar is active |
| Special move points | Resource | Separate resource consumed by scroll abilities |

#### Pouch and Scroll Creation

| Property | Type | Description |
|---|---|---|
| Pouch creation | Recipe | Charm + spirit shards + secondary ingredient at an obelisk |
| Scroll creation | Conversion | 1 pouch converts into 10 scrolls |

#### Familiar Types

| Type | Description |
|---|---|
| Combat | Attacks alongside the player |
| Beast of Burden | Carries extra inventory items |
| Healer | Passively heals the player over time |
| Forager | Gathers resources automatically |
| Booster | Provides invisible skill level boosts |
| Teleporter | Grants teleportation to specific locations |
| Scout | Reveals nearby resources or enemies |

#### Familiar Properties

| Property | Type | Description |
|---|---|---|
| Duration tiers | List | 16, 32, 48, or 64 minutes depending on familiar tier |
| Area restrictions | List | Locations where familiars cannot enter (e.g., certain boss rooms, minigames) |

#### Charm System

| Property | Type | Description |
|---|---|---|
| Charm drop system | Mechanic | Monsters drop charms used in pouch creation |
| Charm tiers | List | Gold, Green, Crimson, Blue (increasing rarity and pouch quality) |

---

### Necromancy

| Property | Type | Description |
|---|---|---|
| Necromancy skill | Toggle | Enable the fourth combat style, outside the combat triangle |
| No-miss mechanic | Toggle | Hit chance acts as a damage multiplier instead of a binary miss chance |
| Damage cap | Integer | Maximum hit per attack (default: 30,000) |

#### Conjure System

| Property | Type | Description |
|---|---|---|
| Conjure system | Mechanic | Summon undead allies with their own abilities |
| Conjure types | List | Skeleton, Zombie, Ghost, Guardian |

#### Stack Mechanics

| Stack Type | Description |
|---|---|
| Necrosis stacks | Consumed by abilities for bonus damage |
| Residual Soul stacks | Consumed for special ability effects |
| Valor stacks | Built up over time, consumed for powerful finishers |

#### Progression

| Property | Type | Description |
|---|---|---|
| Talent tree | System | Unlock abilities by spending talent points earned from combat XP and ritual souls |
| Ritual training | Mechanic | Non-combat training at ritual sites (draw glyphs, place items, channel energy) |

#### Resources

| Resource | Description |
|---|---|
| Necrotic runes | 4 types (spirit, bone, flesh, miasma) crafted at a special altar |
| Ectoplasm | Consumed by conjure abilities |

---

## Module Toggles

Dungeon masters configure the following per world. All toggles in the RS3 Extensions section default to off.

| Toggle | Default | Description |
|---|---|---|
| Combat triangle | On | Melee beats Ranged, Ranged beats Magic, Magic beats Melee |
| Attack/Strength split | On | Separate accuracy and damage skills (off = combined melee skill) |
| Defence as avoidance | On | Defence prevents hits entirely, does not reduce damage |
| Protection prayers PvE | 100% | Full damage protection vs NPCs |
| Protection prayers PvP | 40% | Partial damage protection vs players |
| Prayer flicking | On | Allow free prayer through per-tick toggling |
| Hitpoints starting level | 10 | Starting HP level (unique among skills) |
| Natural HP regen | On | Passive health regeneration out of combat |
| Overheal | On | Allow temporary HP above maximum |
| Slayer task system | On | Task-based combat training via slayer masters |
| Superior monsters | On | Rare enhanced monster spawns on slayer tasks |
| Splash training | On | Cast spells with 0% accuracy for Magic XP |
| XP per damage ratio | 4:1.33 | Style XP to Hitpoints XP ratio per hit |
| Ammo consumption | On | Ranged attacks consume ammunition |
| Bolt enchantments | On | Special proc effects on enchanted bolts |
| Ability system | Off | RS3-style ability-based combat |
| Adrenaline | Off | Resource bar for threshold and ultimate abilities |
| Dual wielding | Off | Main-hand and off-hand weapons simultaneously |
| Summoning | Off | Companion summoning skill |
| Necromancy | Off | Fourth combat style outside the triangle |

---

## Views

| View | Description |
|---|---|
| All Combat Skills | Full list of all combat skills with levels, XP, and status |
| By Style | Filter by Melee, Ranged, Magic, or Prayer |
| Formula Calculator | Input stats and equipment to see effective levels, max hits, and accuracy |
| DPS Simulator | Compare weapon and gear setups with calculated DPS output |
| Prayer List | Browse all prayers with drain rates, effects, and level requirements |
| Slayer Task Browser | Browse slayer masters, task tables, weights, and unlock requirements |
| Ability List | Browse all abilities by tier, style, and weapon requirement (if ability system enabled) |
