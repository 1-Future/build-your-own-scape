# Project Philosophy

The guiding principles, process, and vision behind Build Your Own Scape. This document defines HOW we build tools, WHERE we source data, and WHERE this is all going.

---

## 1. The Vision

Today: A dungeon master reads 75 docs, toggles 832 plugins, and builds a world manually with editor tools.

Tomorrow: A person sits down, answers an interview, and an AI builds the entire MMORPG for them. The docs become the AI's training data. The tools become the AI's hands. The plugin registry becomes the AI's decision space.

The end state is: **Interview → Complete MMORPG.**

Everything we build works backward from that vision. Every doc is structured data an AI can parse. Every tool has an API an AI can call. Every toggle is a decision an AI can make. Humans use the same tools today — AI uses them tomorrow.

---

## 2. The Process — FutureFlow + Game2Tools

### FutureFlow (idea to design)
The methodology for going from pain point to plan:

1. **Inspire** — what game/experience inspired this?
2. **Present** — what does it look like in RuneScape?
3. **Gut Check** — does this actually matter? Is it fun?
4. **Explore** — what variations exist across games?
5. **Pain Points** — what's broken or missing in current implementations?
6. **Research** — deep dive into OSRS wiki, RS3 wiki, other sources
7. **Plan** — write the design doc with properties, toggles, formulas
8. **GitHub Issue** — formalize as a trackable task
9. **Execute** — build it
10. **Test** — verify it works and is fun
11. **Iterate** — adjust based on feedback
12. **Document** — update the design docs with what was learned

**Gate rules:**
- Human approves after Step 6 (Research) before Planning
- Human approves after Step 8 (GitHub Issue) before Executing
- Never skip Gut Check and Research

### Game2Tools (design to tool)
The methodology for deriving the editor tool from the game mechanic:

1. **Extract Data Model** — what are the fields, types, constraints?
2. **Identify Context** — Spatial, Temporal, Relational, Numeric, Hierarchical, State Machine, Textual, Visual?
3. **Select Base Tool** — Canvas, Timeline, Spreadsheet, Calculator, Tree, Flowchart, Text Editor, Preview?
4. **Add Context Enhancements** — pickers, sliders, map overlays from secondary contexts
5. **Scale by Volume** — Form (1), List (dozens), Spreadsheet (hundreds), Canvas (thousands)
6. **Add Universal Ops** — undo, copy, search, filter, import/export, preview, test
7. **Add Validation** — required fields, type checking, reference integrity
8. **Build** — implement the derived tool design
9. **Test with Content** — create real game content using the tool
10. **Iterate** — adjust tool based on creator feedback

### Combined Flow
```
Inspiration
    ↓
FutureFlow Steps 1-7 (Research & Design)
    ↓
Game2Tools Steps 1-7 (Tool Derivation)
    ↓
Build (mechanic + tool simultaneously)
    ↓
Test → Iterate → Document
```

---

## 3. Project Conditions

Every tool, system, and doc in this project must satisfy these conditions:

### Granular and Modular
- Every feature is its own toggle. No "all or nothing" packages.
- 832+ plugins, each independently enable/disable.
- A DM can run a world with 5 plugins or 500.
- Removing a plugin never breaks other plugins (graceful degradation).
- Adding a plugin never requires modifying existing plugins.

### Flexible and Intuitive
- JSON hidden behind GUI. A 3-year-old can use the interface.
- Spreadsheet-style editing for data (tables, not code).
- Visual tools for spatial data (paint, not coordinates).
- Flowcharts for logic (drag, not script).
- Every configuration has a sensible default.
- Advanced options exist but are hidden until needed.
- Right-click for power users, left-click for everyone.

### Built for Humans AND AI
- Every tool has a GUI for humans.
- Every tool has an API for AI.
- Every data structure is parseable JSON.
- Every action is scriptable via the game console API.
- The batch() command enables AI to execute sequences of actions.
- AI can read the design docs to understand what to build.
- AI can read the plugin registry to know what's available.
- AI can use Game2Tools to derive what editor tool to create.
- Human and AI use the SAME tools — AI doesn't get a separate system.

### One Database, Many Views
- Same data, different lenses.
- Equipment table viewed by slot, by combat style, by set, by fashion, by skill.
- No data duplication between views.
- Views are just filters and layouts on the same underlying data.

### Everything Optional, Nothing Forced
- Content rating system lets DMs restrict anything.
- Mode system lets DMs create any ruleset.
- Every mechanic has an off switch.
- Defaults are opinionated but overridable.
- The system never judges a DM's choices.

---

## 4. Data Source Priority

When researching mechanics, systems, or content, follow this priority order:

| Priority | Source | When to Use |
|----------|--------|-------------|
| 1 | **OSRS Wiki** | First source for any mechanic. The foundation. If OSRS has it, start here |
| 2 | **RS3 Wiki** | Second source. RS3 often expanded OSRS mechanics — check what they added |
| 3 | **RSPS Community** | Third source. Private servers often innovated where Jagex wouldn't (gambling, custom modes, QoL) |
| 4 | **RuneLite Plugins** | Fourth source. 1735 plugins = 1735 community-identified gaps. Each plugin is a feature request |
| 5 | **Other MMOs** | Fifth source. If OSRS/RS3/RSPS don't cover it, look at what other games do well |
| 6 | **Non-MMO Games** | Sixth source. Sims 4 for building, Apex for pings, Dark Souls for messages. Best ideas come from unexpected places |
| 7 | **Original Design** | Last resort. If no game has solved it, design from first principles |

### Research Checklist (per mechanic)
- [ ] Check OSRS wiki for the mechanic
- [ ] Check RS3 wiki for expansions/differences
- [ ] Check RuneLite plugin hub for community solutions
- [ ] Check if other games solved it better
- [ ] Document what works, what's broken, what's missing
- [ ] Design the system with all sources considered
- [ ] Credit the inspiration without using any IP

---

## 5. Design Principles

### The RuneScape DNA
Even when drawing from other games, the result should FEEL like a -Scape game:

| Principle | Description |
|-----------|-------------|
| Skills ARE the game | Progression through skills is the core loop, not just a leveling system |
| The journey matters | Getting from 1-99 should be an experience, not a chore to skip |
| Everything connects | Mining feeds Smithing feeds Combat feeds Economy. No isolated systems |
| Humor has a place | The world can be funny without undermining serious moments |
| Player expression | Fashion, emotes, housing, pets — let players be themselves |
| Community is content | Bank standing, clan events, trading — social IS gameplay |
| Risk vs reward | The best content has stakes. Risk makes reward meaningful |
| Depth over breadth | Better to have 5 deep skills than 50 shallow ones |
| Accessible floor, high ceiling | Easy to start, lifetime to master. Tick manipulation exists but isn't required |
| Respect the player's time | Don't waste it with tedium. If it's not fun, redesign it |

### Tool Design Principles
| Principle | Description |
|-----------|-------------|
| Show, don't configure | Visual tools over settings panels wherever possible |
| Immediate feedback | See the result of every change instantly (no compile step) |
| Forgiving | Undo everything. Nothing is permanent until published |
| Discoverable | Hover to learn. Click to explore. Right-click for options |
| Consistent | Same interaction patterns across all tools (click to select, drag to place, scroll to zoom) |
| Progressive disclosure | Basic options visible, advanced options hidden behind "More" or held modifier key |
| Templates accelerate | Pre-built starting points for everything. Never start from blank |
| Context-sensitive | Tools show relevant options based on what's selected |
| Batch-friendly | Every action that can be done once can be done on many |
| AI-scriptable | Every GUI action has a corresponding API call |

---

## 6. The Interview → MMORPG Pipeline

The ultimate vision: a person who has never made a game can describe what they want and get a playable MMORPG.

### Phase 1 — The Interview
AI asks questions to understand the DM's vision:

| Category | Example Questions |
|----------|------------------|
| Genre | "What kind of world? Medieval fantasy, sci-fi, modern, mixed?" |
| Tone | "Serious and dark, or funny and lighthearted, or somewhere between?" |
| Scale | "How many players? Small private server or massive public world?" |
| Combat | "Should there be combat? PvP? How important is fighting?" |
| Skills | "What activities should players do? Mining, cooking, fishing? Or programming, hacking, piloting?" |
| Social | "How important is community? Clans? Voice chat? Events?" |
| Economy | "Should there be trading? A marketplace? Real money?" |
| Story | "Is there a main story? What's the world's history?" |
| Building | "Can players build? Houses? Towns? Entire continents?" |
| Risk | "How punishing is death? Lose items? Lose everything? Just respawn?" |
| Content Rating | "Is this for kids, teens, adults, or everyone?" |
| Art Style | "Retro pixel? PS1 low-poly? HD realistic? Anime?" |
| Reference | "What game should this feel most like?" |
| Unique | "What's the ONE thing that makes your world special?" |

### Phase 2 — AI Generates Design
From interview answers, AI:
1. Selects plugins from the 832 registry
2. Configures toggles and defaults per plugin
3. Generates a lore bible draft (creation myth, factions, races)
4. Selects architectural style per zone
5. Generates settlement layouts from templates
6. Populates NPC roster with AI-generated personalities
7. Creates starter quests from narrative templates
8. Configures economy balance (gold sources/sinks)
9. Sets content rating flags
10. Outputs a complete world configuration file

### Phase 3 — Human Refines
DM reviews the AI-generated world:
- Adjust any toggle
- Edit any lore entry
- Move any building
- Rewrite any dialogue
- Add custom content
- The AI did 80% of the work, human does the meaningful 20%

### Phase 4 — Launch
One-click deploy:
- Server spins up with configuration
- World loads with generated terrain and buildings
- NPCs populate with dialogue and schedules
- Economy starts with configured shops and GE
- Players can join immediately

### Phase 5 — Iterate
AI assists ongoing:
- Suggest new content based on player behavior
- Auto-balance economy based on real data
- Generate holiday events on schedule
- Flag potential issues (dead zones, broken quests, unbalanced drops)
- DM makes final calls, AI handles the busywork

---

## 7. Quality Standards

### Documentation Standards
- Every system has a design doc
- Every doc has: overview, properties tables, module toggles, views
- No emojis in docs
- Clean markdown with tables
- Properties have types (Integer, Boolean, Enum, Float, String, Array)
- Toggles have defaults
- Cross-references link related docs
- Plugin audit features tracked separately from core design

### Code Standards (for when building begins)
- Every GUI action has an API equivalent
- Every plugin is independently loadable/unloadable
- Every data structure is JSON-serializable
- Every tool supports undo/redo
- Every editor has a preview/test mode
- Performance budgets per tick (600ms max)
- No hardcoded values — everything configurable
- Comments explain WHY, not WHAT

### Content Standards
- OSRS/RS3 as primary reference, never copy IP
- Mechanics and systems only, no character names, locations, or lore
- Every piece of content tagged with content rating
- AI-generated content always reviewed by human before publishing
- Templates are starting points, not finished products
- Quality over quantity — one deep system beats ten shallow ones

---

## Module Toggles

This doc is a philosophy guide, not a runtime system. No module toggles — these principles apply to everything.

---

## Views

| View | Description |
|------|-------------|
| Project Dashboard | Overview of all docs, completion status, gaps identified |
| Plugin Coverage Map | Which plugins are documented, which need work |
| Data Source Tracker | Which mechanics have been researched from which sources |
| Interview Builder | Create and customize the DM interview question set |
| World Generator | AI-powered world creation from interview answers |
| Quality Checklist | Verify docs and tools meet quality standards |
