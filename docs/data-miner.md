# Data Miner

A systematic methodology for exhaustive data extraction when building complete systems. While FutureFlow solves specific problems and Game2Tools derives editor tools, Data Miner ensures you've captured EVERY piece of data needed to build a complete system — leaving no stone unturned.

This is what turns "I want to build an MMORPG" into 76 docs, 30,000+ lines, and 832 individually toggleable plugins.

---

## 1. What Data Miner Is

FutureFlow is REACTIVE — starts from a pain point.
Data Miner is PROACTIVE — starts from a domain and exhaustively maps every corner.

FutureFlow asks: "What's broken and how do we fix it?"
Data Miner asks: "What EXISTS and how do we capture ALL of it?"

Data Miner is for projects where:
- The scope is massive (entire game, entire platform, entire industry)
- Missing one system means the product feels incomplete
- The data already exists in reference implementations (OSRS, RS3, Sims 4)
- You're building tools that need to handle EVERY case, not just common ones
- AI will eventually consume the data, so completeness matters

---

## 2. The Data Miner Process

### Phase 1: Domain Mapping
Map the entire domain into top-level categories. Don't go deep yet — just identify the major areas.

Process:
1. Start with the reference implementation (for MMORPG = RuneScape)
2. List every major system visible to the player (equipment, skills, quests, combat, etc.)
3. List every major system invisible to the player (tick system, networking, persistence)
4. List every tool the developer uses (world builder, DM dashboard, content pipeline)
5. List every support system (security, accessibility, localization, content rating)
6. Cross-reference with competitors (RS3, other MMOs) for systems the primary reference is missing
7. Result: a table of contents — every category that needs a doc

**How it worked in this project:** The OSRS Wiki has 6 major navigation categories (Equipment, Skills, Quests, Bestiary, Calculators, Minigames). Those became the first 6 docs. Then RS3 added systems OSRS doesn't have (Invention, Necromancy, Auras). Then asking "what does a player interact with that isn't a skill or quest?" revealed NPCs, Shops, Economy, Communication, Clans. Then "what does the world need?" revealed Terrain, Buildings, Locations, Transportation. Then "what does a server operator need?" revealed Modes, Bot Detection, Rules, Moderation, Server Stats. Then "what does a builder need?" revealed World Builder Tools, DM Dashboard, Asset System, Engine Architecture. Then adjacent domains added Architectural Styles, Nature Catalog, Music/Audio. The original 6 categories expanded to 76 docs covering everything from combat formulas to content rating systems.

### Phase 2: Source Identification
For each category, identify ALL data sources in priority order.

| Priority | Source Type | Method | Example from this project |
|----------|-----------|--------|--------------------------|
| 1 | Primary reference wiki | WebFetch wiki pages, extract every property | OSRS Wiki — every skill page, every equipment slot, every monster |
| 2 | Secondary reference wiki | Same process, look for additions/differences | RS3 Wiki — abilities, necromancy, invention, auras |
| 3 | Community tools/plugins | Scrape plugin lists, categorize each one | RuneLite Plugin Hub — 1605 plugins scraped, each categorized |
| 4 | Community discussions | Search forums for opinions, complaints, wishlists | Reddit, MMORPG forums — what players want, what's broken |
| 5 | Competitor analysis | Study how other products solve the same problems | Sims 4 builder mode, Albion Online PvP, Apex Legends pings |
| 6 | Expert content | Developer interviews, GDC talks, behind-the-scenes | Jagex dev diaries — animation pipeline, bone limits, art team process |
| 7 | Adjacent domains | Import knowledge from related fields | Architecture (building styles), Urban Planning (settlement design), Botany (nature catalog) |
| 8 | Original ideation | Design from first principles when no reference exists | Socializing skill, Bank Standing skill, AI NPC personality system |

### Phase 3: Deep Extraction
For each category, go DEEP. Not surface-level summaries — actual properties, formulas, data types, edge cases.

Extraction checklist per topic:
- [ ] What properties does this thing have? (fields, types, constraints)
- [ ] What formulas/calculations drive it? (XP curves, damage formulas, success rates)
- [ ] What are ALL the variants? (every weapon type, every tree species, every door style)
- [ ] What are the toggles? (what can be turned on/off by the DM?)
- [ ] What are the relationships? (this references that, these conflict, those stack)
- [ ] What are the edge cases? (what happens at 0? at max? when two things collide?)
- [ ] What's the UI/UX? (how does the player interact with this? how does the DM configure it?)
- [ ] What's missing from the reference? (what SHOULD exist but doesn't?)
- [ ] What did competitors add? (features the reference implementation lacks)
- [ ] What would the community add? (RuneLite plugins = community feature requests)

**Example — Equipment deep extraction:**
Surface level: "Players wear equipment in slots." That's useless.
Deep extraction: 15 equipment slots (head, cape, neck, ammo, weapon, shield, body, legs, hands, feet, ring, quiver, pocket, sigil, aura). Each slot has a worn_id, equip_requirements (skill levels), combat_bonuses (stab_attack, slash_attack, crush_attack, magic_attack, range_attack + 5 defense bonuses + strength, ranged_strength, magic_damage, prayer), weight_kg, is_two_handed, attack_speed_ticks, special_attack_cost, degradation_charges, augmented (boolean for Invention), set_id (references equipment sets). That's extractable data an AI can use to build the system.

### Phase 4: Gap Analysis
After extracting everything from known sources, systematically find what's missing.

Methods:
1. **Plugin/mod audit** — every third-party addition is a gap in the base product (we did this with 1605 RuneLite plugins, found ~180 new features across 28 docs plus 4 entirely new docs needed)
2. **Competitor diff** — features in Game B that don't exist in Game A
3. **Cross-domain import** — knowledge from outside gaming (architecture, botany, music theory, urban planning)
4. **Player journey mapping** — walk through the entire player experience start to finish, note every moment something is missing
5. **AI gap detection** — feed the existing docs to AI, ask "what's missing?"
6. **Category matrix** — cross every system with every other system, check for missing intersections (skill x equipment = skilling outfits? equipment x location = area-restricted gear?)

**How the plugin audit worked in this project:**
1605 RuneLite plugins were scraped from GitHub. Each was categorized:
- ~850 COVERED (already in docs)
- ~180 NEW FEATURE (gaps — features that should exist but no doc covers)
- ~250 RENDERING (visual/client-side only, not game design)
- ~120 MEME/JOKE (novelty plugins, not useful for design)
- ~205 OSRS-SPECIFIC (tied to OSRS IP, not generalizable)

The ~180 NEW FEATURE plugins revealed gaps across all 28 existing docs AND identified 4 entirely new docs needed: External Integrations, Music/Audio, Pet/Companion, Player Housing. Each plugin that addresses the same gap = evidence that gap matters.

### Phase 5: Compilation
Organize extracted data into structured, parseable documents.

Document structure (proven pattern from this project):
1. Title and overview
2. Module toggles (everything configurable)
3. Numbered sections with property tables
4. Linked spreadsheets (related data in separate tables connected by ID)
5. Module toggles summary (all toggles with defaults)
6. Views (how users browse the data)

Data format rules:
- Every property has a Type (Integer, Boolean, Enum, Float, String, Array)
- Every toggle has a Default value
- Every relationship is a foreign key (item_id references items table)
- Every enum lists all valid values
- Tables use | Column | Type | Description | format
- No emojis, clean markdown throughout

**Three-layer architecture (proven in this project):**
1. Plugin — the big category toggle (Equipment, Skills, Quests, etc.)
2. Module — the sub-system within a plugin (Worn Equipment, Special Attacks, etc.)
3. Entry — the individual thing (Dragon Scimitar, Fire Bolt, Oak Tree, etc.)

This produced 832 individually toggleable plugins across 43 categories in the plugin registry.

### Phase 6: Validation
Verify completeness and accuracy.

1. **Cross-reference check** — does every foreign key point to something that exists?
2. **Coverage check** — does every player-visible feature have a corresponding doc?
3. **Toggleability check** — can every feature be independently enabled/disabled?
4. **AI parsability check** — can an AI read this doc and know what to build?
5. **Tool derivability check** — can Game2Tools derive the editor tool from this data?
6. **Peer review** — have someone unfamiliar with the project read the docs and identify confusion
7. **Plugin audit reconciliation** — have all gaps from the plugin audit been addressed in the docs?

---

## 3. Data Mining Techniques

### Wiki Crawling
Systematically visit every page in a wiki, extracting structured data.

Process:
1. Start at the category/index page
2. Visit every subcategory
3. For each page, extract: properties, formulas, relationships, variants
4. Cross-reference pages that mention each other
5. Build a dependency graph (what references what)

**Practical example:** For the Skills system, start at the OSRS Wiki "Skills" category page. Visit each of the 23 skills. For each skill: extract level requirements per action, XP per action, success rate formulas, tool requirements, resource types, product types, training methods, milestones, and related quests. Then visit the RS3 Wiki for the same 23 skills plus RS3-exclusive skills (Invention, Necromancy, Divination, Archaeology). Extract what RS3 added that OSRS lacks. This produced 6 skill sub-docs spanning gathering, processing, combining, activity, combat, and inspired/futuristic concepts.

### Plugin/Mod Auditing
Analyze every third-party modification to identify gaps.

Process:
1. Get the complete list (we got 1605 from RuneLite GitHub)
2. Categorize each: COVERED (already in docs), NEW FEATURE (gap), RENDERING (not game design), MEME (not useful), OSRS-SPECIFIC (not generalizable)
3. Group NEW FEATURES by which doc they should be added to
4. Prioritize by how many plugins address the same gap (many plugins = big gap)
5. For entirely uncovered areas, create new docs (4 new docs emerged from the audit)

**Why this works:** Every community-made plugin is a feature request backed by implementation effort. If 15 different plugins all add variants of "loot tracking," that's strong evidence your loot system doc is missing a tracking subsystem. The community has already done your gap analysis — you just need to read their work.

### Competitor Feature Mapping
Study other products feature-by-feature.

Process:
1. Play/study the competitor product
2. List every feature/mechanic you encounter
3. Map each to your existing docs: exists, partially exists, missing
4. For missing features: should we add it? Does it fit our domain?
5. Extract the MECHANIC, not the IP (we take "context-aware ping system" not "Apex Legends")

**Examples from this project:**
- Sims 4: Extracted the build mode interaction model (drag-to-draw walls, room tool, auto-roofing). This became the world builder tools doc.
- Apex Legends: Extracted the context-aware ping system (ping an item = different call than ping a location). This informed the communication doc.
- Dark Souls: Extracted the asynchronous message system (leave messages for other players). This became a feature in the social systems.

### Real-World Domain Import
Import knowledge from fields outside gaming.

Domains imported in this project:

| Domain | What We Got |
|--------|------------|
| Architecture | 60+ historical styles (Tudor, Gothic, Baroque, etc.), building types, architectural elements, construction methods |
| Urban Planning | Settlement layout patterns, district zoning, road hierarchy, public vs private space |
| Botany | 200+ plant species, growth patterns, biome distribution, seasonal behavior |
| Music Theory | Track composition, dynamic layers, mood-based scoring, instrument categories |
| Military Science | Fortification types, siege weapons, chain of command, defensive structures |
| Maritime | Ship components, dock objects, navigation instruments, port facilities |
| Religion Studies | Altar types, worship practices, pantheon structures, temple architecture |
| Culinary Arts | Kitchen objects, cooking methods, food types, preparation stations |
| Textile Arts | Fabric types, clothing construction, fashion elements, dye systems |
| Sociology | Community dynamics, communication patterns, honor systems, social hierarchies |

**Why this matters:** Game design docs that only reference other games produce derivative content. Importing real-world domain knowledge produces depth that feels authentic. A building system informed by actual architectural history has 60+ distinct styles instead of "medieval, modern, futuristic."

### Exhaustive Enumeration
When you find a category, list EVERY possible entry — not just common ones.

**Example — Door types:**
Don't stop at "wooden door, metal door." Go to: single, double, sliding, revolving, gate, portcullis, drawbridge, trapdoor, hatch, archway, curtain, beaded curtain, secret, magical barrier, toll gate, one-way, breakable, puzzle door. 18 types. THAT's exhaustive.

**Example — Lighting sources:**
Don't stop at "torch, lantern." Go to: candle, candelabra, torch (wall), torch (ground), lantern, oil lamp, chandelier, firepit, brazier, glowing crystal, bioluminescent plant, magical orb, lava glow, moonlight shaft, fairy light, campfire, forge glow, enchanted window, stained glass light, lighthouse beam. 20 types.

**Pattern:** For any category, ask "what else?" until you genuinely can't think of more, then ask "what would a different culture/era/genre add?" and you'll find 5 more. Then ask "what would a child think of?" and "what would appear in a fantasy novel?" and you'll find 5 more.

---

## 4. Data Miner Outputs

What Data Miner produces for any domain:

| Output | Description | Example from this project |
|--------|-------------|--------------------------|
| Category Map | Top-level table of contents | 76 docs covering every system |
| Property Tables | Per-system field definitions | Equipment has 100+ properties per item |
| Formula Registry | Mathematical relationships | DPS = hit_chance x (max_hit / 2) / speed |
| Variant Catalogs | Exhaustive lists of every type | 200+ plants, 60+ architecture styles, 18 door types |
| Toggle Registry | Every configurable option | 832 independently toggleable plugins across 43 categories |
| Relationship Graph | How systems reference each other | Equipment -> Skills -> Quests -> Monsters -> Drops -> Equipment (cycle) |
| Gap Report | What's missing and where to find it | Plugin audit: ~180 gaps across 28 docs, 4 new docs needed |
| Source Index | Where each piece of data came from | OSRS Wiki, RS3 Wiki, RuneLite, Sims 4, real-world architecture |
| Tool Specifications | What editor tools are needed (via Game2Tools) | 50+ tool types derived from data contexts |
| AI Training Data | Structured docs an AI can parse to build the system | 76 docs x 30k lines = comprehensive training set |

---

## 5. Scaling Data Miner

| Project Scale | Categories | Docs | Lines | Sources | Time |
|--------------|-----------|------|-------|---------|------|
| Small Feature | 1-3 | 1-3 | 100-500 | 1-2 wikis | Hours |
| Medium System | 5-10 | 5-15 | 1k-5k | 3-5 sources | Days |
| Large Platform | 15-30 | 20-40 | 5k-15k | 5-10 sources | Weeks |
| Complete Domain (MMORPG) | 40-80 | 50-100 | 15k-50k | 10-20 sources | Months |
| Industry Atlas | 100+ | 100+ | 50k+ | 20+ sources | Ongoing |

This project: Complete Domain scale — 76 docs, 30k+ lines, 15+ sources, built over multiple intensive sessions.

---

## 6. Data Miner Anti-Patterns

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| Surface Skimming | Reading summaries instead of digging into actual properties | Always extract field-level data, not just descriptions |
| Single Source | Only using one reference implementation | Cross-reference 3+ sources minimum |
| Copying IP | Taking character names, location names, brand terms | Extract MECHANICS only — "edge-based wall system" not "Varrock wall" |
| Analysis Paralysis | Researching forever, never documenting | Time-box research per category. Good enough > perfect |
| Undocumented Extraction | Researching but not writing it down in structured format | Write the doc AS you research, not after |
| Missing Toggles | Documenting features but not making them configurable | Every feature gets a toggle. No exceptions |
| Ignoring Edge Cases | Only documenting the common case | Ask "what happens at 0? at max? when two conflict?" |
| Skipping Adjacent Domains | Only looking at games for game design data | Architecture, botany, sociology, music — import from everywhere |
| Flat Structure | Dumping everything into one giant doc | Three layers: Plugin -> Module -> Entry. Separate docs per major system |
| No Validation Loop | Writing docs and never checking them against each other | Cross-reference check: every foreign key must point to something real |

---

## 7. The Data Miner Cycle

Data Miner is not a one-pass process. It cycles:

```
Domain Map (what exists?)
    |
    v
Source Identification (where is the data?)
    |
    v
Deep Extraction (pull out every property)
    |
    v
Compilation (structure into parseable docs)
    |
    v
Gap Analysis (what's missing?)
    |
    v
Back to Source Identification (find sources for gaps)
    |
    v
Deep Extraction again (extract gap data)
    |
    v
Compilation again (add to docs)
    |
    v
Validation (is it complete? is it correct?)
    |
    v
If gaps remain: cycle again
    If complete: done
```

In this project, the cycle ran multiple times:
1. First pass: OSRS Wiki -> 28 docs covering core game systems
2. Second pass: RS3 Wiki -> expanded skills, added abilities, necromancy, invention
3. Third pass: RuneLite plugins -> 1605 audited, ~180 gaps found, 4 new docs created
4. Fourth pass: Sims 4 + real-world domains -> world builder tools, architectural styles, nature catalog
5. Fifth pass: Infrastructure gap -> engine architecture, asset system, security, accessibility, localization
6. Result: 76 docs, 30k+ lines, 832 plugins

Each cycle added depth and breadth. Each cycle found things the previous cycle missed.

---

## Module Toggles

Data Miner is a methodology, not a runtime system. No module toggles.

---

## Views

| View | Description |
|------|-------------|
| Domain Map | Visual overview of all categories and their completion status |
| Source Tracker | Which sources have been consulted per category |
| Gap Heatmap | Categories with the most identified gaps |
| Extraction Progress | Percentage of identified data that has been documented |
| Relationship Graph | Visual web of cross-references between docs |
| Completeness Score | Overall project completeness rating |
