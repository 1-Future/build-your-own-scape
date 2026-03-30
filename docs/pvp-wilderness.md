# PvP and Wilderness

Game design document for player-versus-player combat, risk zones, and competitive systems. Every aspect is modular and configurable by the dungeon master. This document draws from games that get PvP right and lessons from games that killed it.

---

## 1. PvP Zone Types

Every area in the world has a PvP classification that determines the rules of engagement, death penalties, and loot behavior.

| Zone Type | PvP Allowed | Death Penalty | Loot on Death | Example |
|---|---|---|---|---|
| Safe Zone | No | None | None | Towns, banks, respawn points |
| Duel Zone | Consensual only | None | None | Duel arenas, practice areas |
| Arena | Instanced, matched | None or cosmetic | None | Rated battlegrounds, LMS lobbies |
| PvP World | Everywhere except safe zones | Standard | Standard item loss | Full PvP server toggle |
| Contested Zone | Yes, opt-in flag | Reduced | Partial (flagged players only) | Territory control regions |
| Dangerous Zone | Yes, always on | Full | Standard wilderness rules | Wilderness levels 1-20 |
| Deep Dangerous Zone | Yes, always on | Full, no teleports | Standard wilderness rules | Wilderness levels 20+ |
| Full Loot Zone | Yes, always on | Full | Everything drops | Revenant caves, deep wilderness |
| Clan War Zone | Team-based only | Configurable | Configurable | Clan battlefields |

---

## 2. The Wilderness

A progressive danger zone inspired by OSRS. The deeper a player ventures, the more risk they accept -- and the greater the rewards available.

### Wilderness Levels

| Mechanic | Description |
|---|---|
| Level Range | 1 to 56. Each level expands the combat bracket by +/- 1 |
| Combat Bracket | A level 100 player in wilderness level 24 can attack players between level 76 and 124 |
| Entry Point | Wilderness ditch or barrier. First two tiles past the ditch are safe (buffer zone) |
| Depth Display | On-screen overlay shows current wilderness level at all times |
| Level Boundaries | No physical barriers between levels -- continuous open terrain |

### Teleport Restrictions

| Restriction | Description |
|---|---|
| Standard Teleports | Blocked above wilderness level 20 |
| Special Teleports | Items like amulet of glory allow teleporting up to level 30 |
| Above Level 30 | No teleports work. Escape requires running south or logging out |
| Teleblock Spell | Prevents all teleportation for 5 minutes (2.5 minutes if protecting from magic) |

### Safe Havens

| Feature | Description |
|---|---|
| Enclave | Safe zone in the middle of the wilderness with banking, restoration, and supplies |
| Teleblock Restriction | Teleblocked players cannot enter the enclave |
| Buffer Zones | Designated safe tiles at wilderness entry points |

---

## 3. Skull System

The skull system is the core risk mechanic of open-world PvP. It determines how many items a player keeps on death.

### Skull Rules

| Mechanic | Description |
|---|---|
| Unskulled Death | Keep 3 most valuable items (4 with Protect Item prayer) |
| Skulled Death | Keep 0 items (1 with Protect Item prayer) |
| Skull Trigger | Attacking another player first grants a skull |
| Skull Duration | Configurable timer (default 20 minutes). Resets on each new attack |
| Skull Icon | Visible above the player's head. All nearby players can see it |
| Skull on Login | Players who log out skulled retain the skull on login |

### Skull Prevention

| Feature | Description |
|---|---|
| Skull Prevention Toggle | Player setting that prevents attacking when it would cause a skull |
| Attack-Option Filtering | Hide "Attack" option on players to prevent misclicks |
| Skull Trick Protection | System prevents common deception methods (name swaps, familiar switching) |
| Retaliating | Attacking someone who attacked you first does not grant a skull |
| Area-of-Effect Warning | AoE abilities warn before hitting non-skulled players |

### Skull Tricking (Why Prevention Matters)

Skull tricking is the practice of deceiving a player into attacking first, causing them to skull and risk all their items. Common methods include:

| Trick | How It Works |
|---|---|
| Name Swap | Two players with similar names swap positions mid-fight |
| Familiar Bait | Using a summoned creature with a similar appearance to a player |
| Multi-Combat Lure | Dragging a fight into multi-combat where a partner attacks |
| Gear Swap | Changing appearance mid-fight to look like a different player |
| AoE Bait | Standing near a fight to get hit by area attacks |

---

## 4. Risk vs Reward Balance

The fundamental question of PvP design: why should a player risk their items? The reward must always justify the danger.

### Risk Tiers

| Tier | Risk Level | Reward Level | Example Content |
|---|---|---|---|
| Low Risk | Minimal items at stake | Moderate XP/GP | Wilderness agility course, low-level resources |
| Medium Risk | Standard gear risk | Good XP/GP, unique drops | Wilderness slayer, prayer training altar |
| High Risk | Best-in-slot gear risk | Excellent XP/GP, exclusive items | Wilderness bosses, revenant caves |
| Maximum Risk | Full inventory at stake | Best rates in the game, unique untradeables | Deep wilderness activities, high-value resource nodes |

### Reward Principles (Lessons Learned)

| Principle | Description |
|---|---|
| Exclusive Content | Some rewards must only be obtainable in PvP zones. If everything is available elsewhere, nobody enters |
| Better Rates | PvP zone training should be 20-50% faster than safe alternatives to justify the risk |
| Unique Drops | PvP-exclusive items that are useful but not mandatory. Players choose risk, not forced into it |
| Risk Scaling | Deeper zones must have proportionally better rewards. Level 50 wilderness must be worth more than level 10 |
| Economic Sink | PvP deaths remove items from the economy, keeping gear valuable |
| No Dead Zones | Every wilderness level should have a reason to be there. Empty space is wasted design |

---

## 5. PvP Combat Mechanics

PvP combat differs fundamentally from PvE. Systems that feel balanced against monsters can be broken against other players.

### PvP-Specific Adjustments

| Mechanic | PvE Behavior | PvP Behavior |
|---|---|---|
| Protection Prayers | Block 100% of damage type | Block 40% of damage type |
| Damage Caps | None (hit as hard as possible) | Optional cap to prevent one-shot kills |
| Attack Speed | Weapon-dependent | Some weapons slowed in PvP (e.g., blowpipe 25% slower) |
| Healing Rate | Unlimited | Optional tick-eating prevention or heal caps |
| Spec Restoration | Slow regeneration | No passive spec regen in PvP zones (optional) |
| Crowd Control | Duration-based | Reduced duration against players |
| Set Effects | Full effect | Some set effects disabled or reduced in PvP |

### KO Mechanics

The ability to kill an opponent through their food is what makes PvP exciting. Without KO potential, fights become wars of attrition.

| Mechanic | Description |
|---|---|
| Spec Combos | Switching to a special attack weapon for burst damage (e.g., AGS into Gmaul) |
| Vengeance | Reflect damage spell that punishes the attacker when timed with a spec |
| Stack Eating | Eating food + potion + karambwan in one tick to survive combos |
| Tick Manipulation | Precise timing of attacks and switches to maximize damage per tick |
| Gear Switching | Swapping between attack styles mid-fight to bypass single-style protection |
| Smite | Prayer that drains opponent prayer on each hit, forcing them to lose Protect Item |
| Freeze-Spec | Freezing opponent with magic then switching to melee spec weapon |

### PvP Damage Modifiers

| Modifier | Description |
|---|---|
| PvP Attack Bonus | Certain items gain bonus accuracy or damage specifically in PvP |
| PvP Defence Reduction | Certain defensive items have reduced effectiveness in PvP |
| Wilderness-Only Items | Equipment that only functions inside the wilderness |
| Risk-Based Bonus | Optional system: higher risk (more items carried) grants damage bonus |

---

## 6. Anti-Griefing Systems

Without anti-griefing measures, PvP zones become hostile wastelands that drive away the majority of players. These systems protect the experience without removing danger.

### PJ Timer (Player-Jacking Prevention)

| Feature | Description |
|---|---|
| Timer Duration | Configurable (default 12 seconds / 20 ticks). OSRS uses 9.6 seconds in PvP worlds |
| Function | After combat ends, a timer prevents other players from attacking you |
| Purpose | Stops teams from piling a solo player by taking turns attacking |
| Single-Combat Enforcement | In single-combat zones, only one player can attack you at a time |
| Logout Kill Credit | If a player logs out mid-fight, their opponent receives kill credit |

### Combat Brackets

| Feature | Description |
|---|---|
| Level Range | Wilderness level determines the combat level range for attacks |
| Build Diversity | Brackets naturally separate pures, zerkers, and mains |
| Bracket Abuse Prevention | Anti-boosting systems detect intentionally low combat levels with high stats |
| Free-to-Play Brackets | Separate brackets for F2P and members if applicable |

### Additional Protections

| Feature | Description |
|---|---|
| Spite Prevention | Cannot release valuable items (e.g., chinchompas) while in combat |
| Food Drop Rules | Dropped food despawns in 15 seconds and is invisible to other players |
| Supply Sharing Block | Cannot telegrab others' dropped food in single-combat zones |
| Safe Zone Guards | NPC guards in safe zones attack skulled players on sight |
| Death Pile Privacy | Loot from a PvP kill is visible only to the killer for 60 seconds |
| Raggers | Repeat attackers with minimal risk gear can be reported and penalized |

---

## 7. Lessons from Successful PvP Games

### Albion Online

Albion Online is widely considered the best PvP MMO as of 2025-2026. Its success stems from layered risk zones and a player-driven economy.

| Aspect | Implementation |
|---|---|
| Zone Progression | Blue (safe) > Yellow (non-lethal PvP) > Red (full loot, reputation cost) > Black (full loot, no rules) |
| Full Loot | Death in red/black zones drops everything. Gear is consumed, driving the crafting economy |
| Flagging System | In red zones, players must flag hostile to attack. Unflagged players lose reputation if they attack |
| Reputation | Killing unflagged players reduces reputation. Low reputation locks you out of cities |
| Economy Integration | PvP is the primary item sink. Without it, the economy inflates. Crafters depend on PvP deaths |
| Guild vs Guild | Structured 5v5 territory battles on a schedule. Competitive and organized |
| Classless System | "You are what you wear" -- no permanent class. Players can always adapt their build |

### EVE Online

EVE's 20+ year PvP ecosystem proves that meaningful loss creates meaningful gameplay.

| Aspect | Implementation |
|---|---|
| Security Zones | Highsec (1.0-0.5, CONCORD protection) > Lowsec (0.4-0.1, gate guns only) > Nullsec (0.0 to -1.0, no rules) > Wormholes (no local chat, unknown threats) |
| CONCORD | NPC police in highsec. They do not prevent attacks -- they punish them. Suicide ganking is viable but costly |
| Security Status | Attacking in lowsec/highsec reduces your security status. Low status makes you kill-on-sight in highsec |
| Wormhole Intel | No local chat in wormholes. You cannot see who is in the system. Creates paranoia and careful play |
| Ship Loss | Ships are permanently destroyed. Insurance covers hull cost but not fitted modules |
| Player Sovereignty | Alliances control nullsec territory. Wars over space involve thousands of players |
| Single Shard | One server for all players. Reputation matters. Actions have lasting consequences |

### Rust

Rust proves that full-loot PvP can sustain a massive playerbase (180k+ concurrent) when designed correctly.

| Aspect | Implementation |
|---|---|
| Full Loot Everywhere | No safe zones in vanilla. Death means losing everything carried |
| Wipe Cycles | Servers wipe on a schedule (weekly/biweekly/monthly). Resets prevent permanent power gaps |
| Base Building | Persistent bases can be raided. Offline raiding is controversial but creates tension |
| Gear Accessibility | Gear is craftable and replaceable. Loss stings but is recoverable |
| Skill-Based Combat | Recoil patterns, bullet drop, positioning matter more than time invested |
| Server Variety | Community servers offer modded rules (PvE, limited PvP, boosted rates). Players choose their experience |
| No Permanent Progression | Progress resets each wipe. New players start on equal footing regularly |

### Dune: Awakening (2025)

MassivelyOP's Best PvP MMO 2025. Successfully blends PvE and PvP audiences.

| Aspect | Implementation |
|---|---|
| Deep Desert PvP | Safe zones near cities, open PvP in the valuable deep desert |
| Resource Motivation | Best resources (spice) only available in dangerous PvP zones |
| Sandstorm Wipes | Periodic storms destroy bases in deep desert, preventing permanent fortification |
| Dual Audience | Core loop satisfies both PvE survival fans and PvP combatants |

---

## 8. Lessons from Failed PvP

### RS3 Wilderness

The most instructive failure in MMO PvP history. RS3 effectively killed its wilderness through a series of well-intentioned but destructive decisions.

| Event | Year | Impact |
|---|---|---|
| Wilderness Removal | 2007 | PvP removed entirely to combat real-world trading. Replaced with Revenants in multi-combat. Massive player exodus |
| Wilderness Return | 2011 | PvP restored after overwhelming player vote. But damage was done -- PvP community had moved on |
| Evolution of Combat | 2012 | Ability-based combat replaced the tick-based system. PvP meta was destroyed overnight |
| Opt-In Wilderness | 2022 | PvP made optional (off by default). Wilderness became a PvE zone. PvP effectively dead |
| Threat System Removal | 2023 | Jagex admitted PvE danger could not replicate the tension of PvP. Removed the replacement system |

**Key Lesson:** You cannot remove PvP from a PvP zone and replace the tension with PvE mechanics. The unpredictability of human opponents is irreplaceable. Once removed, the PvP community disperses and does not return.

### Common PvP Failures Across Games

| Failure | Games Affected | Why It Kills PvP |
|---|---|---|
| Gear Determines Winner | Many MMOs | New players cannot compete regardless of skill. Pay-to-win amplifies this |
| No Risk for Attackers | Games without skull systems | Griefing has no cost. Attackers in cheap gear kill players in expensive gear |
| Forced PvP for PvE Content | Early New World, WoW PvP servers | PvE players resent being forced into PvP. They quit rather than participate |
| No Rewards for PvP | Many MMOs | Without unique incentives, PvP populations dwindle to only griefers |
| Balance Neglect | ESO, WoW at various points | PvP balance patches stop. Meta becomes stale. Competitive players leave |
| Toxic Community Unchecked | Many open-world PvP games | Harassment drives away new players. PvP population self-cannibalizes |
| Arena-Only Focus | WoW post-Cataclysm | Instanced PvP replaces world PvP. The world feels dead |
| Full Loot Without Progression | Various survival MMOs | Players never feel they accomplish anything. Burnout is rapid |

---

## 9. PvP Rewards

Rewards are the reason non-PvP players enter PvP zones. Without them, the wilderness is empty.

### Reward Types

| Type | Description |
|---|---|
| Emblem System | Killing players upgrades an emblem tier. Higher tiers trade for better rewards at a PvP shop |
| Bounty Hunter Points | Assigned targets grant bonus points on kill. Points buy exclusive items |
| PvP-Exclusive Drops | Items that only drop from player kills or PvP zone activities |
| Wilderness Keys | Keys dropped from PvP kills that unlock loot chests at a bank |
| Kill Streak Rewards | Consecutive kills without dying grant escalating bonuses |
| Seasonal Rankings | Periodic leaderboard resets with cosmetic rewards for top performers |
| PvP Currency | Dedicated currency earned through PvP activities, spent at PvP shops |

### PvP Shop Items

| Category | Examples |
|---|---|
| Cosmetics | Skull-themed armor, kill count capes, animated death effects |
| Consumables | Enhanced potions, PvP-specific food, teleport scrolls |
| Utility | Looting bag (extra inventory in wilderness), loot keys, blighted supplies |
| Weapons | PvP-specific weapons with bonuses only active against players |
| Upgrades | Imbues, enchantments, or recolors tied to PvP achievement |

---

## 10. PvP Account Builds

Build diversity is what keeps PvP alive long-term. When every player fights the same way, PvP becomes stale. The combat level formula must allow for meaningful specialization.

### Common Build Archetypes

| Build | Defence Level | Key Stats | Strengths | Combat Level Range |
|---|---|---|---|---|
| 1 Defence Pure | 1 | High Attack, Strength, Ranged, Magic | Maximum KO potential at lowest combat level | 50-85 |
| Void Pure | 42 | Void equipment stats | Void set bonus with low defence | 70-90 |
| Zerker (45 Defence) | 45 | High offence, Barrows gloves access | Best gloves in game, berserker helm, vengeance | 75-100 |
| Rune Pure (40 Defence) | 40 | Rune armor access | Affordable tanking with pure-like offence | 70-95 |
| Piety Pure (70 Defence) | 70 | Piety prayer, barrows armor | Full prayer book, strong defence, high max hit | 100-110 |
| Maxed Main | 99 | All combat 99 | No restrictions, best gear, highest max hit | 126 |
| Tank | 99 Defence | Minimal Attack/Strength | Outlasts opponents, wins by attrition | 80-110 |
| Hybrid | Varies | High Magic + Ranged + Melee | Switches between all three styles mid-fight | Varies |

### Why Build Diversity Matters

| Reason | Explanation |
|---|---|
| Meta Variety | Different builds counter each other. No single build dominates all brackets |
| Replayability | Players create multiple accounts with different builds, multiplying engagement |
| Skill Expression | Low-level accounts reward precise play. Mistakes are punished harder with fewer HP |
| Bracket Activity | Each combat level range has active fights. Not everyone clusters at max level |
| Theory-Crafting | Community develops and shares optimal builds. Creates content outside the game |

---

## 11. PvP Minigames

Structured PvP with defined rules, safe deaths, and matchmaking. These serve as entry points for players who want to learn PvP without risking items. Cross-references minigames.md for template definitions.

### Minigame Catalog

| Minigame | Template | Team Size | Safe Death | Gear Source | Key Mechanic |
|---|---|---|---|---|---|
| Last Man Standing | Battle Royale | 24 FFA | Yes | Provided (randomized chests) | Battle royale with prayer/gear switching |
| Castle Wars | Capture the Flag | 2 teams, up to 64 per side | Yes | Bring your own | Flag capture with barricades, catapults, and tunnels |
| Soul Wars | Objective Defence | 2 teams, variable | Yes | Bring your own | Weaken enemy avatar by collecting soul fragments |
| Clan Wars | Configurable | 2 clans, variable | Configurable | Bring your own | Custom rules set by clan leaders |
| Fight Pits | Free-for-All | Up to 64 | Yes | Bring your own | Last player standing wins |
| Duel Arena | 1v1 Duel | 2 players | Yes | Bring your own or stake | Configurable rule toggles (no prayer, no food, etc.) |
| Bounty Hunter | Open World | 1v1 assigned targets | No (wilderness rules) | Bring your own | Target assignment system with emblem upgrades |
| Rated Arena | 1v1 / 2v2 / 3v3 | Small teams | Yes | Standardized or bring your own | Elo-based matchmaking, seasonal rankings |

### Minigame Reward Structure

| Minigame | Currency | Unique Rewards |
|---|---|---|
| Last Man Standing | Points per placement | Cosmetic upgrades, halos, PvP training |
| Castle Wars | Tickets per game | Gold-trimmed armor (cosmetic), decorative items |
| Soul Wars | Zeal per game | XP rewards, pet chance, imbue scrolls |
| Bounty Hunter | Emblems (tiered) | PvP weapons, supplies, cosmetics |
| Rated Arena | Rating + seasonal points | Seasonal titles, ranked cosmetics, mount skins |

---

## 12. Wilderness Content

Content placed inside PvP zones creates natural tension. PvE players must weigh risk against reward, and PvP players have targets to hunt. This friction is what makes the wilderness feel alive.

### Wilderness Bosses

| Boss | Wilderness Level | Zone Type | Unique Drops | Risk Profile |
|---|---|---|---|---|
| Entry-Level Boss | 10-15 | Singles | Mid-tier unique weapon | Low -- easy to escape, near teleport range |
| Mid-Level Boss Trio | 20-30 | Singles-plus variants | Best-in-slot ring or amulet | Medium -- above standard teleport cutoff |
| Deep Boss | 40-50 | Multi-combat | Rare pet, exclusive cosmetic | High -- no teleports, multi-combat PKers |
| Revenants | 17-40 | Multi-combat cave | Ancient statuettes, PvP weapons | Very high -- hotspot for PKers, entry fee |
| Demi-Boss | 25-35 | Singles | Supply crate, emblem upgrades | Medium -- intended as PvP bait |

### Wilderness Resources

| Resource | Wilderness Level | Advantage Over Safe Alternative |
|---|---|---|
| Prayer Altar | 38 | 50% chance bone is not consumed (effectively 2x XP rate) |
| Best Runecrafting Route | 5-10 (Abyss entry) | Fastest runecrafting method in the game |
| Rare Ore Nodes | 46 | Only free-to-play source of top-tier ore |
| High-Level Fishing | 35 | Best healing food in the game |
| Hunter Creature | 32-36 | Best Hunter XP and GP per hour. Cannot be released in combat |
| Agility Course | 47-56 | Competitive XP rates, no marks of grace |
| Herb Patches | 25-30 | Double yield or unique herbs unavailable elsewhere |

### Wilderness Infrastructure

| Feature | Description |
|---|---|
| Obelisks | Teleport network within the wilderness. Random destination unless upgraded |
| Shortcuts | Agility shortcuts that provide escape routes or faster travel |
| Resource Area | Gated area requiring payment to enter. Best resources but deep wilderness |
| Slayer Master | Wilderness-specific slayer master with higher point rewards and exclusive drops |
| Chest Loot | Locked chests throughout the wilderness requiring keys from various activities |

---

## 13. Open World PvP vs Instanced PvP

Both models have strengths and weaknesses. The best PvP ecosystems use both.

### Comparison

| Aspect | Open World PvP | Instanced PvP |
|---|---|---|
| Tension | High -- always present, unpredictable | Low -- controlled environment |
| Fairness | Variable -- level gaps, gear gaps, numbers advantage | High -- matchmaking, standardized rules |
| Accessibility | Low -- punishing for new players | High -- easy to queue and learn |
| Community | Creates rivalries, stories, emergent gameplay | Creates competitive ladders, esports potential |
| Player Retention | Drives away casual players if unchecked | Retains competitive players long-term |
| World Feel | Makes the world feel alive and dangerous | Instances feel disconnected from the world |
| Griefing Risk | High without proper anti-griefing systems | Low -- matchmaking prevents stomps |
| Reward Design | Risk-based (bring items, lose items) | Point-based (currency, rating) |
| Build Diversity | High -- any build can ambush or escape | Moderate -- meta builds dominate ladders |
| Spectator Value | Limited -- fights happen in remote areas | High -- replays, spectator mode, tournaments |

### Recommendation

| System | Purpose |
|---|---|
| Open World Wilderness | Core risk/reward PvP. The heart of the PvP ecosystem |
| Instanced Arenas | Competitive ranked PvP with matchmaking. Entry point for new PvPers |
| Minigames | Casual structured PvP. Social, low-stakes, rewards-driven |
| Clan Battlefields | Organized group PvP. Scheduled, objective-based |
| PvP Worlds | Full-world PvP for dedicated PKers. Optional server toggle |

---

## 14. Community Health

PvP lives or dies by its community. Toxic environments drive away new players, and without new blood, PvP populations collapse.

### What Makes Players Want to PvP

| Factor | Description |
|---|---|
| Fair Fights | Players return when they feel they lost due to skill, not gear or numbers |
| Achievable Goals | Clear progression (ranks, titles, cosmetics) gives PvP purpose |
| Quick Re-Entry | Fast respawn, easy regear, minimal downtime between fights |
| Build Identity | Players feel ownership over their account build and want to test it |
| Community Recognition | Kill counts, leaderboards, and titles create status |
| Content Updates | Regular PvP updates signal developer commitment |
| Learning Curve | Practice modes (LMS, duels) let players improve without loss |

### What Drives Players Away

| Factor | Description |
|---|---|
| Skull Tricking | Deception that causes unintended item loss. Must be mechanically prevented |
| Rag Killing | Attackers in worthless gear killing players for spite. No risk for the attacker |
| Pile-On Teams | Clans ganking solo players repeatedly. PJ timer is essential |
| Bug Abuse | Exploits that cause unfair deaths. Must be fixed immediately |
| Stale Meta | Same build, same specs, same fights for months. Balance patches needed |
| Developer Neglect | When PvP stops receiving updates, players feel abandoned |
| Wealth Gap | When PvP becomes inaccessible to players without billions in GP |

### Toxicity Management

| System | Description |
|---|---|
| Chat Filtering | Offensive language filtered in PvP zones. Optional toggle |
| Report System | Players can report harassment, skull tricking, and bug abuse |
| Mute Penalties | Escalating mute durations for repeated toxic behavior |
| PvP-Specific Rules | Clear rules about what constitutes griefing vs legitimate PvP |
| Safe Chat Option | Players can disable incoming chat from opponents |
| Kill Message Limits | Rate-limit kill announcement messages to prevent spam |

---

## 15. DM Configuration

Every PvP system is controlled through dungeon master toggles. This allows server operators to create any PvP experience from fully pacifist to hardcore full-loot.

### Zone Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| Wilderness Enabled | On/Off | Off | Enable the progressive danger zone |
| Wilderness Max Level | Integer | 56 | Deepest wilderness level |
| Wilderness Combat Range | Formula | +/- wildy level | How combat brackets scale with depth |
| PvP Worlds | On/Off | Off | Dedicated full-PvP server worlds |
| Safe Zone Guards | On/Off | On | NPC guards that attack skulled players in safe zones |
| Multi-Combat Zones | On/Off | On | Areas where multiple players can attack one target |
| Singles-Plus | On/Off | On | Single combat but PJ timer allows switching opponents |

### Skull Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| Skull System | On/Off | Off | Enable skull-on-attack mechanic |
| Skull Duration | Minutes | 20 | How long the skull persists |
| Items Kept Unskulled | Integer | 3 | Items protected on unskulled death |
| Items Kept Skulled | Integer | 0 | Items protected on skulled death |
| Protect Item Prayer | On/Off | Off | Prayer that protects one additional item |
| Skull Prevention Setting | On/Off | Off | Player toggle to prevent accidental skulling |
| Smite Prayer | On/Off | Off | Prayer that drains opponent prayer to force item loss |

### Combat Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| PvP Prayer Reduction | Percentage | 40% | How much protection prayers block in PvP |
| PvP Damage Cap | Integer | None | Maximum damage per hit in PvP |
| PvP Attack Speed Modifiers | Map | None | Per-weapon speed adjustments for PvP |
| Special Attack in PvP | On/Off | On | Whether special attacks work against players |
| Vengeance Spell | On/Off | Off | Damage reflection spell |
| PvP XP Gain | On/Off | On | Whether players earn combat XP from PvP |
| PvP Set Effect Overrides | Map | None | Per-item set effect modifications in PvP |

### Loot Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| PvP Loot System | Enum | Standard | Standard / Loot Keys / Full Loot / No Loot |
| Loot Keys | On/Off | Off | PvP kills generate a key redeemable at bank |
| Loot Visibility Timer | Seconds | 60 | How long loot is visible only to the killer |
| Untradeable Behavior | Enum | Keep | Keep / Break / Convert to coins / Destroy |
| Food on Death | Enum | Destroy | Destroy / Drop / Keep |
| Emblem System | On/Off | Off | Tiered emblems from PvP kills |
| Kill Streak Tracking | On/Off | Off | Track and reward consecutive kills |

### Anti-Griefing Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| PJ Timer | Seconds | 12 | Delay before another player can attack after combat |
| PJ Timer (PvP Worlds) | Seconds | 10 | Separate PJ timer for dedicated PvP worlds |
| Combat Brackets | On/Off | On | Restrict attacks to similar combat levels |
| Spite Release Prevention | On/Off | On | Prevent releasing items during combat |
| Teleblock | On/Off | Off | Spell that prevents teleportation |
| Teleblock Duration | Seconds | 300 | Full duration of teleblock |
| Logout Timer in Combat | Seconds | 10 | Delay before a player can log out after combat |

### Reward Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| Bounty Hunter | On/Off | Off | Target assignment PvP system |
| PvP Shop | On/Off | Off | Shop that accepts PvP currency |
| PvP Leaderboard | On/Off | Off | Track kills, deaths, KDR, streaks |
| Seasonal Rankings | On/Off | Off | Time-limited competitive seasons |
| Wilderness Slayer | On/Off | Off | PvP-zone slayer master with unique rewards |
| Wilderness Bosses | On/Off | Off | Bosses placed inside PvP zones |
| PvP-Exclusive Drops | On/Off | Off | Items that only drop from PvP activities |
| Kill Count Display | On/Off | Off | Show total PvP kills on player examine |

### Minigame Configuration

| Toggle | Type | Default | Description |
|---|---|---|---|
| Last Man Standing | On/Off | Off | Battle royale minigame |
| Castle Wars | On/Off | Off | Capture the flag minigame |
| Soul Wars | On/Off | Off | Avatar destruction minigame |
| Clan Wars | On/Off | Off | Clan-vs-clan configurable battles |
| Fight Pits | On/Off | Off | Free-for-all last man standing |
| Duel Arena | On/Off | Off | 1v1 consensual duels |
| Rated Arena | On/Off | Off | Elo-based competitive PvP |
| LMS Rewards | Map | Default | Configurable point rewards per placement |

### RSPS Insights

Private servers maintain the most active PvP communities in the RuneScape ecosystem. Key insights from successful RSPS PvP:

| Server Pattern | What Works |
|---|---|
| Instant Gear | Players spawn with or can quickly obtain PvP gear. Zero barrier to entry |
| Active Wilderness | Small map sizes concentrate players. Fights happen constantly |
| Daily Tournaments | Scheduled 1v1 and multi tournaments with prizes keep players coming back |
| Custom PvP Items | Unique weapons and effects not found in the main game create novelty |
| High Engagement Tracking | Discord activity and in-game population monitored for matchmaking |
| Elo Systems | Skill-based matchmaking prevents new players from being farmed |
| Economic Loop | PvP drops feed the economy. Economy funds PvP gear. Self-sustaining cycle |
