# Combat System

Game design document for the Combat System plugin. This system defines the runtime fight loop -- how combat plays out tick by tick, from target selection to loot drop. It consumes data from equipment.md, skills-combat.md, monsters.md, and tick-system.md.

---

## 1. Combat Loop

Every tick, active combatants advance through the following pipeline. Each step feeds the next.

| Step | Name | Description |
|---|---|---|
| 1 | Target Acquisition | Determine who or what the player is attacking |
| 2 | Range Check | Verify the target is within weapon range |
| 3 | Attack Style Determination | Resolve which combat style (melee, ranged, magic) applies |
| 4 | Accuracy Roll | Roll attacker's accuracy against defender's defence |
| 5 | Damage Roll | Roll a damage value from 0 to max hit |
| 6 | Hit Application | Subtract HP, show hitsplat, apply on-hit effects |
| 7 | XP Award | Grant combat and hitpoints XP based on damage dealt |
| 8 | Death Check | Evaluate whether the target has reached 0 HP |
| 9 | Loot Generation | If dead, roll drop table and spawn loot |

---

## 2. Target System

| Feature | Description |
|---|---|
| Click to Attack | Left-click an NPC or player to set them as the active target |
| Auto-Retaliate | When attacked, automatically target the attacker if no current target |
| Tab Targeting | Cycle through nearby valid targets with a keybind |
| Nearest Enemy | Default target selection picks the closest hostile entity |
| Target Lock | Maintain target even if they move out of range temporarily |
| Target Switching | Click a new entity mid-fight to change targets immediately |
| Multi-Target | Chinchompas, AoE spells, and halberds can hit multiple targets in an area |
| Target Priority | Configurable priority mode -- lowest HP, highest threat, or nearest |

---

## 3. Attack Cycle

| Mechanic | Description |
|---|---|
| Attack Speed | Weapon-dependent, measured in ticks between attacks |
| Attack Queue | Next attack is scheduled at `current_tick + weapon_speed` |
| First Attack Delay | Configurable -- instant first attack or wait one full cycle |
| Attack Interruption | Eating, moving, or switching gear resets the attack timer |
| Attack Animation | Plays on the attack tick, duration is cosmetic only |
| Projectile Travel Time | Ranged and magic projectiles have travel ticks based on distance to target |

---

## 4. Accuracy Resolution

Step-by-step accuracy calculation for each attack.

| Step | Formula / Description |
|---|---|
| Effective Attack Level | Base level + stance bonus + prayer bonus + potion bonus |
| Equipment Attack Bonus | Sum of all worn equipment bonuses for the active style |
| Attack Roll | `effective_attack * (equipment_bonus + 64)` |
| Effective Defence Level | Target base defence + target stance bonus + target prayer bonus |
| Target Defence Bonus | Sum of target equipment bonuses for the relevant defence style |
| Defence Roll | `effective_defence * (target_defence_bonus + 64)` |
| Hit Chance | If attack roll > defence roll: `1 - (defence_roll + 2) / (2 * (attack_roll + 1))`. Otherwise: `attack_roll / (2 * (defence_roll + 1))` |
| Accuracy Roll | Roll random 0-1 against hit chance. Pass = hit, fail = miss (0 damage hitsplat) |

---

## 5. Damage Resolution

Step-by-step max hit and damage calculation for each successful hit.

| Step | Formula / Description |
|---|---|
| Effective Strength Level | Base level + stance bonus + prayer bonus + potion bonus |
| Equipment Strength Bonus | Sum of all worn equipment strength bonuses for the active style |
| Max Hit | `0.5 + effective_strength * (equipment_strength_bonus + 64) / 640` (floored) |
| Damage Roll | Uniform random integer from 0 to max hit (inclusive) |
| Damage Modifiers | Multiplicative bonuses applied in order -- slayer helm, salve amulet, void, prayers |
| Target-Specific Modifiers | Weakness bonuses -- dragon weakness, demon weakness, undead weakness |
| PvP Damage Cap | Optional cap on maximum damage in PvP contexts |
| Final Damage | Result after all modifiers and caps applied |

---

## 6. Hit Application

| Mechanic | Description |
|---|---|
| HP Subtraction | Subtract final damage from target's current hitpoints |
| Hitsplat Display | Visual indicator on target -- type determined by damage context |
| On-Hit Effects | Trigger any secondary effects attached to the hit |
| Prayer Drain (Smite) | Drain 1 prayer point per 4 damage dealt if Smite is active |
| XP Award | `damage * style_multiplier` to the active combat skill, plus hitpoints XP |

### Hitsplat Types

| Color | Meaning |
|---|---|
| Red | Standard damage dealt |
| Blue | Blocked (0 damage, attack was a miss) |
| Green | Poison damage |
| Dark Green | Venom damage |
| Purple | Healed (damage absorbed or converted to healing) |

### On-Hit Effects

| Effect | Description |
|---|---|
| Poison | Apply poison status, dealing recurring damage over time |
| Venom | Apply venom status, dealing escalating damage over time |
| Freeze | Immobilize target for a set number of ticks |
| Stat Drain | Reduce one or more of the target's combat stats |
| Leech | Transfer stat points from target to attacker |
| Heal | Attacker heals for a portion of damage dealt |

---

## 7. Death Resolution

### NPC Death

| Step | Description |
|---|---|
| HP Reaches 0 | NPC is flagged as dead |
| Death Animation | Plays NPC death animation |
| Drop Table Roll | Roll the NPC's drop table for loot |
| Rare Drop Table | Additional roll on the shared rare drop table if applicable |
| Collection Log | Update the killer's collection log if a new unique is received |
| Kill Count | Increment the killer's KC for this NPC |
| Pet Roll | Roll for pet drop at the configured rate |
| Loot Spawn | Items appear on the ground at the death tile |
| Respawn Timer | NPC respawn scheduled at `current_tick + respawn_ticks` |

### Player Death

| Step | Description |
|---|---|
| HP Reaches 0 | Player is flagged as dead |
| Protection Prayer Check | Protect Item prayer allows keeping 1 additional item |
| Skull Check | Skulled players keep 0 items instead of 3 |
| Item Value Sort | All carried items sorted by value descending |
| Items Kept | Top 3 (or 4 with Protect Item) items are retained |
| Items Dropped | Remaining items are dropped or sent to grave/death office |
| Respawn | Player respawns at configured location |

---

## 8. Multi-Combat vs Single Combat

| Feature | Single Combat | Multi-Combat |
|---|---|---|
| Attackers per target | 1 | Unlimited |
| PJ Timer | Active -- prevents others from attacking your target for a set number of ticks after you disengage | Not applicable |
| Zone designation | Default for most areas | Specific zones marked by the world builder |
| Clan implications | 1v1 encounters only | Full clan vs clan piling |

---

## 9. PvP-Specific Mechanics

| Mechanic | Description |
|---|---|
| Skull System | Attacking another player first marks you as skulled -- lose all items on death instead of keeping 3 |
| Teleblock | Spell that prevents the target from teleporting for a duration |
| Freeze / Bind | Spells that root the target in place, preventing movement |
| Smite | Prayer that drains opponent prayer by 1 point per 4 damage, forcing them to lose Protect Item |
| Spec Stacking | Using multiple special attacks in quick succession to burst a target for a KO |
| Combo Eating | Consuming food + karambwan + brew in a single tick for rapid healing |
| Vengeance | Spell that reflects 75% of the next incoming hit back to the attacker |
| PJ Timer | Delay after a fight ends before another player can attack you, preventing pile-jumping |

---

## 10. Safe Minigame Combat

| Feature | Description |
|---|---|
| Item Loss | None -- all items retained on death |
| Respawn Location | Within the minigame area |
| Stat Restoration | All stats restored to full on death |
| Kill Credit | Deaths and kills count toward minigame points and rewards |
| Separation | Completely independent from main-world death mechanics |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `combat.triangle` | true | Melee > Ranged > Magic > Melee style advantage |
| `combat.auto_retaliate` | true | Auto-target attackers when idle |
| `combat.tab_targeting` | false | Keybind to cycle nearby targets |
| `combat.aoe_attacks` | true | Multi-target weapons and spells |
| `combat.projectile_travel` | true | Ranged/magic projectiles have travel time based on distance |
| `combat.skull_system` | true | PvP skull mechanic on first attack |
| `combat.teleblock` | true | Spell to prevent teleportation |
| `combat.freeze_bind` | true | Spells that immobilize targets |
| `combat.smite` | true | Prayer that drains opponent prayer on hit |
| `combat.spec_stacking` | true | Multiple special attacks in quick succession |
| `combat.combo_eating` | true | Multiple consumables in a single tick |
| `combat.vengeance` | true | Reflect damage spell |
| `combat.pj_timer` | true | Protection from pile-jumping after combat ends |
| `combat.safe_death_zones` | true | Minigame areas with no item loss |
| `combat.multi_combat_zones` | true | Zones allowing unlimited attackers per target |
| `combat.death_item_loss` | true | Items lost on death outside safe zones |
| `combat.protection_prayers` | true | Prayers that reduce incoming damage by style |
| `combat.hitsplat_styles` | true | Colored hitsplat indicators |
| `combat.on_hit_effects` | true | Poison, venom, freeze, drain, leech, heal |
| `combat.pet_rolls` | true | Chance for pet drop on NPC kill |

---

## Views

| View | Description |
|---|---|
| Combat Interface | Active target, attack style selector, special attack bar, auto-retaliate toggle |
| Target Info Panel | Target name, HP bar, combat level, weakness indicator |
| DPS Calculator | Input gear, stats, prayers, and target to estimate damage per second |
| Hit Chance Calculator | Shows accuracy percentage against a selected target |
| Max Hit Calculator | Shows maximum possible hit with current loadout |
| Combat Log | Scrollable feed of recent hits, misses, XP drops, and kills |
