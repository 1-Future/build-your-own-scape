# Build Your Own Scape

An open source game design document for building a RuneScape-inspired MMO from scratch. Every system, every mechanic, every property — documented and organized as a plugin-based architecture where everything is optional and toggleable.

This is not a clone of any game. This is a complete blueprint for building your own MMO inspired by the "-Scape" genre, with systems distilled from OSRS, RS3, private servers, and original ideas. No IP is used — only mechanics and systems.

## Philosophy

- **Everything is a plugin.** Dungeon masters toggle what they want.
- **Everything is optional.** A world can have 5 skills or 50. One boss or a hundred.
- **Simple for users.** JSON hidden behind GUI forms. A 3-year-old could use it.
- **Granular control.** Three layers deep: Plugin → Module → Entry.
- **One database, many views.** Same data, different lenses.

## Plugin Architecture

Three layers:
1. **Plugin** — the big category toggle (Equipment, Skills, Quests, etc.)
2. **Module** — the sub-system within a plugin (Worn Equipment, Special Attacks, etc.)
3. **Entry** — the individual thing (Dragon Scimitar, Fire Bolt, Oak Tree, etc.)

## Plugin Directory

### Game Systems
| # | Plugin | Document |
|---|--------|----------|
| 1 | [Equipment](docs/equipment.md) | Worn gear, weapons, armor, sets, auras, upgrades |
| 2 | [Skills](docs/skills.md) | 34+ skills across 6 categories, inspired/futuristic skills |
| 2a | [Skills: Combat](docs/skills-combat.md) | Attack, Strength, Defence, HP, Ranged, Magic, Prayer, Slayer, Summoning, Necromancy |
| 2b | [Skills: Gathering](docs/skills-gathering.md) | Mining, Fishing, Woodcutting, Farming, Hunter, Divination, Archaeology |
| 2c | [Skills: Processing](docs/skills-processing.md) | Cooking, Firemaking, Smithing (smelting), Runecrafting, Thieving |
| 2d | [Skills: Combining](docs/skills-combining.md) | Herblore, Crafting, Fletching, Smithing (anvil), Construction, Invention |
| 2e | [Skills: Activity](docs/skills-activity.md) | Agility, Dungeoneering, Sailing, Socializing, Bank Standing, Pet Raising |
| 3 | [Quests](docs/quests.md) | Quest design, step system, branching, rewards |
| 4 | [Monsters](docs/monsters.md) | Monster stats, types, drop tables, slayer |
| 5 | [Bosses & Raids](docs/bosses-raids.md) | Phases, mechanics, behavior library, raid system |
| 6 | [Items](docs/items.md) | Food, potions, materials, currencies, containers |
| 7 | [Locations](docs/locations.md) | Areas, zones, combat rules, environment |
| 8 | [Tick System](docs/tick-system.md) | Game engine timing, manipulation, action queue |

### Content Systems
| # | Plugin | Document |
|---|--------|----------|
| 9 | [Achievements](docs/achievements.md) | Milestones, tiers, categories |
| 10 | [Collection Log](docs/collection-log.md) | Item tracking, completion, milestones |
| 11 | [Minigames](docs/minigames.md) | Game mode templates, rules, rewards |
| 12 | [Puzzles](docs/puzzles.md) | Escape rooms, logic, player-created |
| 13 | [Dailies](docs/dailies.md) | Repeatable events, timers, streaks |
| 14 | [Games of Chance](docs/games-of-chance.md) | Gambling, casino, provably fair |

### Player Systems
| # | Plugin | Document |
|---|--------|----------|
| 15 | [Account Management](docs/account-management.md) | Profile, security, settings, save states |
| 16 | [Character Overview](docs/character-overview.md) | Stats, progress, milestones, comparison |
| 17 | [Inventory & Bank](docs/inventory-bank.md) | Storage, presets, organization |
| 18 | [Death System](docs/death-system.md) | Death mechanics, graves, reclaim |

### Social Systems
| # | Plugin | Document |
|---|--------|----------|
| 19 | [Clan System](docs/clan-system.md) | Ranks, halls, wars, events, achievements |
| 20 | [Communication](docs/communication.md) | Chat, voice, quick chat, bots, emotes |
| 21 | [Friends & Social](docs/friends-social.md) | Friends list, groups, social features |

### Economy Systems
| # | Plugin | Document |
|---|--------|----------|
| 22 | [Economy](docs/economy.md) | Grand Exchange, trading, shops, sinks, self-correcting |
| 23 | [Monetization](docs/monetization.md) | RWT marketplace, services, microtransactions, P2W meter |

### Governance Systems
| # | Plugin | Document |
|---|--------|----------|
| 24 | [Rules & Moderation](docs/rules-moderation.md) | Rule system, punishments, appeals, mod tools |
| 25 | [Bot Detection](docs/bot-detection.md) | Detection methods, bot scoring, bot policies |
| 26 | [Modes](docs/modes.md) | Ironman, hardcore, leagues, snowflake, custom |
| 27 | [Server Stats & Voting](docs/server-stats-voting.md) | Hiscores, polls, governance, server ranking |
| 28 | [Dialogue](docs/dialogue.md) | NPC conversation, branching, AI freeform |

### Infrastructure Systems
| # | Plugin | Document |
|---|--------|----------|
| 29 | [External Integrations](docs/external-integrations.md) | Webhooks, API, data export, Discord, streaming, IoT |
| 30 | [Music & Audio](docs/music-audio.md) | Playlists, jukebox, per-zone soundtracks, sound effects |
| 31 | [Pet & Companion](docs/pet-companion.md) | Pet raising, naming, transmog, abilities, collection |
| 32 | [Player Housing](docs/player-housing.md) | Rooms, furniture, storage, portals, visitors, trophies |
| 33 | [Engine Architecture](docs/engine-architecture.md) | Networking, chunks, plugins, rendering, pathfinding, multiplayer sync |

### Reference
| Document | Description |
|----------|-------------|
| [Plugin Audit](docs/plugin-audit.md) | 1605 RuneLite plugins analyzed — gaps, new features, coverage |

## Contributing

This is a living document. If you have ideas for mechanics, systems, or improvements, open an issue or submit a PR. The goal is to be the most comprehensive MMO design reference that exists.

## License

This project is open source. Use it to build whatever you want.

## Credits

Inspired by Old School RuneScape, RuneScape 3, the RSPS community, and dozens of other MMOs. No game IP is used — only mechanics and system design patterns.
