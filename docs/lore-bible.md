# Lore Bible

Game design document for the Lore Bible plugin. This system covers world history, mythology, factions, religions, races, languages, and cultural details --- the canonical reference for everything about the game world. Every NPC, quest, and piece of environmental storytelling draws from this document. At a studio like Jagex, this is maintained by lore developers. In our system, the Dungeon Master builds it with structured editors and AI tools. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the lore bible module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Lore Bible System | Core lore storage with wiki-style entries for all world knowledge |
| Timeline | Chronological world history divided into ages and eras |
| Faction System | Organizations with goals, territory, ranks, and inter-faction relationships |
| Religion System | Gods, pantheons, belief systems, holy sites, and clergy |
| Race System | Playable and non-playable races with culture, homeland, and naming conventions |
| Language System | In-world languages with scripts, phrase books, and translation rules |
| Cultural Details | Customs, traditions, food, festivals, and daily life per culture |
| Lore Delivery Books | Readable in-game books, scrolls, and journals |
| Lore Delivery Environmental | Ruins, corpses, carvings, and architecture that tell stories without words |
| Lore Delivery Dialogue | NPCs reference history, share rumors, and tell faction-biased stories |
| Lore Delivery Examine | Brief lore snippets when examining objects in the world |
| AI Lore Tools | AI-powered generators for world building, backstories, names, and gap analysis |
| Player-Facing Wiki | Auto-generated player wiki from lore entries marked as discoverable |
| Spoiler Control | Mark lore entries as hidden until the player discovers them in-game |
| Consistency Checker | Warns when new lore contradicts existing entries |
| Collaboration | Multiple writers edit simultaneously with change tracking |
| Templates | Pre-built lore structures for creation myths, factions, deities, and races |

---

## 1. Creation Myth

Every world needs an origin story. The creation myth defines what existed before, how the world came to be, and what evidence of that origin remains.

### Creation Myth Entry

| Column | Type | Description |
|---|---|---|
| Myth ID | Integer | Unique identifier |
| Name | String | Title of the creation myth (e.g. "The Sundering", "The First Word") |
| Summary | Text | One paragraph overview of how the world was created |
| Full Text | Text | Complete narrative of the creation myth |
| Creator | String | Who or what created the world (gods, science, accident, dream, unknown) |
| Template | Enum | Divine Creation, Big Bang, Simulation, Dream of a Sleeping God, Cosmic Accident, Ancient War Aftermath, Cycle of Rebirth, Custom |
| Pre-Existence | Text | What existed before the world was created |
| Time Ago | String | How long ago creation occurred (e.g. "10,000 years", "Before time itself") |
| Evidence | Text List | What remains in the world as proof (ruins, artifacts, ancient texts, cosmic phenomena) |
| Disputed | Boolean | Whether different cultures tell different versions of this myth |
| Variants | ID List | Alternative creation myth IDs told by different factions or races |

---

## 2. Timeline

Chronological history of the world divided into ages or eras. Every major event is a node that can be linked to quests, locations, NPCs, and factions.

### Ages and Eras

| Column | Type | Description |
|---|---|---|
| Era ID | Integer | Unique identifier |
| Name | String | Display name (e.g. "First Age", "Age of Ruin", "The Long Night") |
| Start Year | Integer | In-world year the era begins |
| End Year | Integer | In-world year the era ends, or null if current era |
| Description | Text | Summary of what defines this era |
| Dominant Faction | ID | Faction that held the most power during this era |
| Defining Event | ID | The event that started or defines this era |
| Color | Hex | Color used in timeline visualizer for this era |

### Timeline Events

| Column | Type | Description |
|---|---|---|
| Event ID | Integer | Unique identifier |
| Era ID | Integer | Foreign key to the era this event belongs to |
| Year | Integer | In-world year the event occurred |
| Name | String | Event title (e.g. "The Battle of Ashenvale", "Discovery of Runecrafting") |
| Description | Text | What happened and why it mattered |
| Factions Involved | ID List | Factions that participated in or were affected by this event |
| Locations Involved | ID List | Where this event took place |
| NPCs Involved | ID List | Key characters involved |
| Consequences | Text | What changed in the world as a result |
| Discoverable | Boolean | Whether players can learn about this event in-game |
| Discovery Method | Enum | Quest, Book, NPC Dialogue, Environmental, Examine, None |
| Quest Link | ID | Quest that reveals or relates to this event |

### Timeline Types

| Type | Description |
|---|---|
| Linear | One authoritative history accepted by all cultures |
| Branching | Player choices created divergent timelines with different histories |
| Cyclical | History repeats in cycles --- ages echo previous ages |
| Fragmented | History is lost, disputed, or unreliable --- different factions remember differently |

---

## 3. World Geography

Continents, regions, and oceans that form the physical world. Geography shapes culture --- island people become sailors, mountain people become miners, desert people become traders.

### Regions

| Column | Type | Description |
|---|---|---|
| Region ID | Integer | Unique identifier |
| Name | String | Display name |
| Type | Enum | Continent, Island, Peninsula, Valley, Mountain Range, Desert, Forest, Swamp, Tundra, Ocean, Underworld, Floating Island, Pocket Dimension |
| Climate | Enum | Tropical, Arid, Temperate, Continental, Polar, Magical, Cursed |
| Dominant Faction | ID | Faction that controls or inhabits this region |
| History | Text | How this region came to be and what has happened here |
| Resources | String List | Natural resources available (ores, timber, fish, magical crystals) |
| Dangers | String List | Threats specific to this region (monsters, weather, terrain hazards) |
| Cultural Identity | Text | How the region shapes its inhabitants |
| Neighbor Regions | ID List | Adjacent regions and their relationships |
| Map Coordinates | Object | Position on the world map |

---

## 4. Races and Species

Playable and non-playable races. Each race has a homeland, culture, naming conventions, and visual aesthetic.

### Race Entry

| Column | Type | Description |
|---|---|---|
| Race ID | Integer | Unique identifier |
| Name | String | Race name |
| Playable | Boolean | Whether players can choose this race |
| Physical Description | Text | General appearance, height, build, distinguishing features |
| Homeland | ID | Region ID where this race originates |
| Lifespan | String | Average lifespan (e.g. "80 years", "Immortal", "500 years") |
| Strengths | String List | Racial advantages (magic affinity, night vision, fire resistance) |
| Weaknesses | String List | Racial disadvantages (sunlight sensitivity, iron allergy, short memory) |
| Government | Enum | Monarchy, Democracy, Tribal Council, Theocracy, Anarchy, Hive Mind, Elder Council, None |
| Religion | ID | Primary deity or pantheon worshipped |
| Architecture Style | String | Building aesthetic (crystal spires, underground halls, treehouses, mud brick) |
| Clothing Style | String | Fashion aesthetic (robes, furs, armor, minimal, ornate) |
| Art Style | String | Cultural art (tapestry, sculpture, song, rune carving, body paint) |
| Language | ID | Primary language spoken |
| Creation Myth | Text | How this race believes it came to exist |

### Naming Conventions

| Column | Type | Description |
|---|---|---|
| Race ID | Integer | Foreign key to race entry |
| Pattern | String | Naming structure (e.g. "First Last", "Clan-First", "Single Name + Title") |
| Syllable Rules | String | Common sounds and syllable patterns (e.g. "harsh consonants, short vowels") |
| Example Names | String List | Ten to twenty sample names for reference |
| Forbidden Names | String List | Names or sounds considered taboo in this culture |

### Template Races

| Template | Description |
|---|---|
| Humans | Versatile, everywhere, short-lived but adaptable, diverse cultures |
| Elves | Ancient, magical, forest-dwelling, long-lived, connection to nature |
| Dwarves | Underground, smithing, sturdy, mountain halls, clan loyalty |
| Orcs | Tribal, combat-focused, survival instinct, honor through strength |
| Gnomes | Inventive, small, clever, mechanical aptitude, curiosity |
| Undead | Risen, cursed, necromantic origin, feared by the living |
| Merfolk | Aquatic, coral cities, tidal magic, trade with coastal races |
| Fae | Magical, trickster, nature spirits, unpredictable, otherworldly |
| Dragon-kin | Scaled, fire or elemental breath, hoarding instinct, ancient pride |
| Custom | Dungeon Master defines all properties from scratch |

---

## 5. Factions and Organizations

Groups with goals, power, territory, and internal structure. Factions drive conflict and give players loyalty systems with tangible rewards.

### Faction Entry

| Column | Type | Description |
|---|---|---|
| Faction ID | Integer | Unique identifier |
| Name | String | Faction name |
| Motto | String | Creed or slogan |
| Leader | ID | NPC ID of the current leader |
| Headquarters | ID | Location ID of the faction base |
| Territory | ID List | Region IDs the faction controls or claims |
| Goals | Text | What the faction wants to achieve |
| Methods | Text | How they pursue their goals (diplomacy, war, trade, espionage, religion) |
| Allies | ID List | Allied faction IDs |
| Enemies | ID List | Hostile faction IDs |
| Membership Requirements | Text | How someone joins (birth, initiation, purchase, invitation, trial) |
| Ranks | Object List | Ordered list of ranks with names, privileges, and promotion requirements |
| History | Text | Founding story and major events |
| Current Conflicts | Text | Active disputes, wars, or tensions |

### Faction Reputation

| Column | Type | Description |
|---|---|---|
| Faction ID | Integer | Foreign key to faction entry |
| Reputation Tiers | Object List | Named tiers from hostile to exalted with point thresholds |
| Earn Methods | Text List | Actions that raise reputation (quests, kills, donations, wearing faction gear) |
| Lose Methods | Text List | Actions that lower reputation (killing members, helping enemies, theft) |
| Tier Rewards | Object List | Per-tier unlocks (shop access, equipment, titles, areas, abilities) |

### Faction Relationship Matrix

A table showing every faction's stance toward every other faction. Changes dynamically based on quest outcomes and world events.

| Relationship | Description |
|---|---|
| Allied | Fight together, share resources, mutual defense pacts |
| Trade Partner | Neutral but economically connected |
| Neutral | No strong opinion, coexist without conflict |
| Rival | Competitive tension but not open hostility |
| Hostile | Active dislike, skirmishes, espionage |
| At War | Open armed conflict |
| Unknown | No contact or awareness of each other |

---

## 6. Religion and Mythology

Gods, pantheons, and belief systems. Deities can be active forces in the world or distant myths depending on the Dungeon Master's preference.

### Deity Entry

| Column | Type | Description |
|---|---|---|
| Deity ID | Integer | Unique identifier |
| Name | String | Deity name and any titles |
| Domain | String List | Areas of influence (war, nature, death, love, knowledge, chaos, crafting) |
| Alignment | Enum | Good, Neutral, Evil, Chaotic, Lawful, Unknown, Beyond Morality |
| Symbol | String | Description of the deity's holy symbol |
| Followers | ID List | Faction and race IDs that worship this deity |
| Holy Sites | ID List | Location IDs of temples, shrines, and sacred grounds |
| Clergy | ID List | NPC IDs of priests, paladins, and religious leaders |
| Prayers | Object List | Named prayers with effects (blessings, buffs, curses) |
| Creation Role | Text | What this deity created or did during the creation of the world |
| Relationships | Object List | Connections to other deities (ally, enemy, lover, rival, parent, child) |
| Current State | Enum | Active, Dormant, Dead, Imprisoned, Unknown, Ascended, Banished |
| Lore Entries | ID List | Lore bible entries about this deity |

### Pantheon Structure

| Column | Type | Description |
|---|---|---|
| Pantheon ID | Integer | Unique identifier |
| Name | String | Pantheon name |
| Structure | Enum | Hierarchy (one supreme god), Council (gods are equal), Dualism (two opposing forces), Animism (spirits in everything), Monotheism (one god) |
| Deities | ID List | All deity IDs in this pantheon |
| Origin Story | Text | How the pantheon formed |
| Worshipped By | ID List | Races and factions that follow this pantheon |

---

## 7. Magic System Lore

How magic works in the world --- not game mechanics (see skills-combat.md) but the in-world explanation for why magic exists and who can use it.

### Magic System

| Column | Type | Description |
|---|---|---|
| System ID | Integer | Unique identifier |
| Source | Enum | Gods, Nature, Cosmic Energy, Technology, Blood, Souls, Leylines, Ancient Artifacts, Innate, Unknown |
| Access | Enum | Everyone, Trained Only, Born With It, Racially Innate, Gifted by Deity, Stolen, Purchased |
| Schools | Object List | Named magical traditions (elemental, divine, arcane, necromantic, druidic, psionic, runic) with descriptions |
| Taboos | Object List | Forbidden magic types with description and consequences for using them |
| History | Text | Golden age, catastrophe, current state of magic in the world |
| Relationship to Technology | Enum | Complementary, Opposing, Independent, Technology IS Magic, Magic Replaces Technology |
| Cost | Text | What using magic costs (mana, health, lifespan, sanity, reagents, nothing) |
| Visibility | Text | What magic looks like when cast (runes, light, sound, invisible, element-specific) |

---

## 8. Languages

In-world languages spoken by different races and factions. Languages can be purely decorative or functionally important.

### Language Entry

| Column | Type | Description |
|---|---|---|
| Language ID | Integer | Unique identifier |
| Name | String | Language name |
| Speakers | ID List | Race and faction IDs that speak this language |
| Script | String | Writing system description (runic, flowing script, pictographic, no written form) |
| Complexity | Enum | Simple, Moderate, Complex, Alien |
| Related Languages | ID List | Languages that share roots or are mutually intelligible |
| Common Phrases | Object List | Phrase, translation, and usage context pairs |
| Implementation | Enum | Flavor Only (mentioned but not rendered), Visual (different font), Functional (players must learn to read or understand) |
| Learnable | Boolean | Whether players can learn this language as a skill |
| Learn Method | String | How players acquire the language (quest, NPC tutor, book, immersion, racial default) |

---

## 9. Cultural Details

Customs, traditions, and daily life that make races and factions feel like living cultures rather than stat blocks.

### Culture Entry

| Column | Type | Description |
|---|---|---|
| Culture ID | Integer | Unique identifier |
| Race or Faction | ID | Race ID or Faction ID this culture belongs to |
| Greetings | String List | How members greet each other and outsiders |
| Food | String List | Common foods, delicacies, and taboo foods |
| Marriage Customs | Text | How partnerships are formed and celebrated |
| Death Rituals | Text | How the dead are honored (burial, cremation, sky burial, soul capture) |
| Coming of Age | Text | How young members earn adult status |
| Festivals | Object List | Named holidays with dates, traditions, and significance |
| Taboos | String List | Things considered unforgivable or deeply offensive |
| Art Forms | String List | Dominant creative expressions (music, sculpture, weaving, tattoo, dance) |
| Sports and Games | String List | Popular games and competitions |
| Attitude Toward Outsiders | Enum | Welcoming, Cautious, Hostile, Indifferent, Curious, Fearful |
| Gender Roles | Text | Cultural expectations around gender, if any |
| Economy Style | Enum | Currency, Barter, Gift Economy, Communal, Tribute, Mixed |
| Technology Level | Enum | Stone Age, Bronze Age, Iron Age, Medieval, Renaissance, Industrial, Magical, Mixed |

---

## 10. Lore Delivery Methods

How players discover lore in-game. Each method is independently toggleable.

| Method | Description |
|---|---|
| Books and Scrolls | Readable items found in bookshelves, dungeons, and quest rewards |
| NPC Dialogue | NPCs reference history, tell stories, share rumors based on their faction perspective |
| Environmental | Ruins, battle sites, graveyards, and ancient carvings tell stories without words |
| Quest Narrative | Quests reveal lore through gameplay and story beats |
| Cutscenes | Cinematic lore reveals at key story moments |
| Examine Text | Brief lore snippets when examining objects in the world |
| Loading Screens | Lore facts displayed during loading transitions |
| Museum or Gallery | Dedicated in-game space for collected lore artifacts and exhibits |
| Audio Logs | Discoverable voice recordings for modern or sci-fi worlds |
| Cave Paintings | Prehistoric storytelling through art on cave and dungeon walls |
| Inscriptions | Text carved into monuments, gravestones, walls, and pillars |
| Oral Tradition | NPCs tell the same story differently based on their faction or race |

---

## 11. Lore Bible Editor Tool

The authoring interface for building and maintaining the lore bible.

| Feature | Description |
|---|---|
| Wiki-Style Editor | Rich text with headers, links between entries, inline images, and tables |
| Cross-Reference | Click any faction, character, or location name to jump to its entry |
| Timeline View | Visual horizontal timeline with draggable event nodes, zoom by era, color-coded by faction |
| Relationship Graph | Interactive web showing faction and character relationships with labeled connections |
| Consistency Checker | Warns when new lore contradicts existing entries (date conflicts, dead characters appearing alive, location mismatches) |
| Export | Export lore bible as PDF, wiki pages, or in-game readable book items |
| Player-Facing Wiki | Auto-generate a player wiki from lore entries marked as discoverable |
| Spoiler Control | Mark entries as hidden until the player discovers them in-game through quests, books, or exploration |
| Version History | Track changes to lore over time with diff view and revert capability |
| Collaboration | Multiple writers edit simultaneously with change tracking and conflict resolution |
| Templates | Pre-built entry structures for creation myths, factions, deities, races, and regions |
| Search | Full-text search across all lore entries with filters by category, era, and faction |
| Tagging | Tag entries with keywords for cross-cutting queries (e.g. all entries tagged "undead") |

---

## 12. AI-Powered Lore Tools

AI assistants that help the Dungeon Master build, maintain, and expand the lore bible. All generated content is presented as drafts for the DM to review and refine.

| Tool | Description |
|---|---|
| World Generator | Input high-level parameters (genre, number of factions, number of gods, years of history) and generate a complete lore bible draft |
| Character Backstory Generator | Input race, faction, role, and personality traits to generate a backstory consistent with existing lore |
| Name Generator | Generate character, place, and faction names consistent with a culture's naming conventions and phonetic rules |
| Lore Gap Finder | Analyze the lore bible and identify missing pieces (factions without creation stories, regions without history, undefined relationships) |
| Conflict Generator | Suggest potential conflicts between factions based on their goals, territory, and relationships to seed quest ideas |
| Cultural Detail Filler | Generate customs, traditions, food, clothing, and festival details for a culture based on its environment and values |
| In-World Text Writer | Write books, inscriptions, letters, and journals in the voice of specific characters or cultures using lore bible context |
| Translation Generator | Create fictional language fragments consistent with a culture's established phonetic aesthetic |
| Rumor Mill | Generate NPC gossip and rumors based on current world events, recent player actions, and lore bible entries |
| Prophecy Writer | Generate cryptic prophecies that hint at future content the DM is planning, using existing lore symbolism |
| Timeline Expander | Take a brief event description and expand it into a full historical narrative with causes, participants, and consequences |
| Faction Manifesto Writer | Generate founding documents, creeds, and propaganda materials for a faction based on its goals and values |

---

## Views

| View | Description |
|---|---|
| Lore Bible Editor | Wiki-style entry editor with sidebar navigation by category |
| Timeline View | Horizontal scrollable timeline with events plotted by year and era |
| Faction Relationship Graph | Interactive force-directed graph of all factions with labeled relationship lines |
| World Map Overlay | World map with lore entries pinned to their geographic locations |
| Character Directory | Searchable list of all named characters with role, faction, and status |
| Deity Pantheon View | Visual pantheon layout showing deity relationships and domains |
| Language Reference | Side-by-side comparison of language phrase books and scripts |
| Cultural Comparison Table | Matrix comparing cultural traits across all races and factions |
| Lore Gap Report | Dashboard showing completeness of lore bible with flagged missing entries |
| Player Discovery Tracker | Per-player view of which lore entries they have discovered in-game |
