# Game2Tools

A systematic framework for deriving development tools from game mechanics. Given any game mechanic, this framework produces the editor tool needed to build, configure, and manage that mechanic.

The pipeline: **Game Mechanic → Data Model → Context Analysis → Tool Derivation → Editor Implementation**

---

## 1. The Problem

Every game mechanic needs two things:
1. A **runtime system** that executes the mechanic during gameplay
2. An **editor tool** that lets creators build, configure, and iterate on the mechanic

Most game development focuses on #1 and improvises #2. Game2Tools systematizes #2 — so that for any mechanic, the optimal editor tool can be derived rather than guessed.

---

## 2. Step 1 — Extract the Data Model

Every mechanic is data. Before designing a tool, define what data the mechanic needs.

| Mechanic | Data Model |
|----------|-----------|
| Walls | `{ x, y, layer, edge: bitmask, material: enum, breakable: bool, hp: int }` |
| NPC | `{ id, name, type, dialogue_id, shop_id, spawn: {x,y}, wander_radius, schedule }` |
| Quest Step | `{ step_id, type: enum, instruction: string, npc_id, conditions: [], rewards: [] }` |
| Drop Table | `{ monster_id, item_id, quantity: range, rate: fraction, condition: string }` |
| XP Curve | `{ formula: string, level_cap: int, base_xp: int, growth_rate: float }` |
| Boss Phase | `{ phase_num, trigger: condition, attacks: [], spawns: [], arena_changes: [] }` |
| Dialogue | `{ node_id, npc_line: string, options: [{ text, next_node, condition }] }` |
| Recipe | `{ inputs: [{item, qty}], tool: item_id, station: object_id, output: [{item, qty}], xp: float }` |
| Tile | `{ x, y, layer, type: int, color: hex, variant: int, height: float }` |
| Animation | `{ seq_id, frames: [{frame_id, duration}], loop: int, priority: int }` |
| Music Track | `{ track_id, name, unlock_zone, duration, instruments: midi_data }` |
| Achievement | `{ id, name, condition: expression, tier: enum, points: int, reward: item_id }` |
| Spawn Point | `{ monster_id, x, y, layer, count: int, respawn_ticks: int, wander_radius: int }` |
| Shop Stock | `{ shop_id, item_id, quantity: int, price: int, restock_rate: int, currency: enum }` |
| Event | `{ id, name, type: enum, start: condition, duration: time, rewards: [], scaling: bool }` |

### Data Model Checklist

For any mechanic, answer:
- What are the fields?
- What are the types? (string, int, float, enum, boolean, array, reference to another entity)
- Which fields are required vs optional?
- What are the constraints? (min/max, valid enums, foreign key references)
- What is the default state?
- How many instances will exist? (1, dozens, hundreds, thousands, millions)

---

## 3. Step 2 — Identify the Context

Data context determines what kind of interaction the editor needs. Most mechanics have a primary context and sometimes a secondary.

| Context | Definition | Signal |
|---------|-----------|--------|
| Spatial | Data has a position in the game world (x, y, layer) | Fields include coordinates, radius, area, zone |
| Temporal | Data has sequence, timing, or schedule | Fields include order, duration, delay, frame, tick |
| Relational | Data references other entities | Fields include foreign IDs (item_id, npc_id, quest_id) |
| Numeric | Data is primarily numbers, formulas, curves | Fields are mostly int/float with mathematical relationships |
| Hierarchical | Data has parent-child or category structure | Fields include parent_id, category, tier, depth |
| State Machine | Data has discrete states with transition rules | Fields include state, condition, next_state, trigger |
| Textual | Data is primarily written content | Fields are mostly strings (dialogue, descriptions, lore) |
| Visual | Data defines appearance | Fields include model_id, color, texture, animation |

### Context Matrix (mechanic → primary context)

| Mechanic | Primary | Secondary |
|----------|---------|-----------|
| Tiles | Spatial | Visual |
| Walls | Spatial | Relational |
| Objects | Spatial | Relational |
| NPCs | Spatial | Relational, State Machine |
| Monsters | Spatial | Numeric |
| Spawn Points | Spatial | Numeric |
| Quest Steps | Temporal | Relational, Textual |
| Animations | Temporal | Visual |
| Event Schedule | Temporal | Relational |
| Dialogue Trees | Hierarchical | Textual, State Machine |
| Skill Trees | Hierarchical | Numeric |
| Boss Phases | State Machine | Temporal, Spatial |
| Door States | State Machine | Spatial |
| Drop Tables | Relational | Numeric |
| Recipes | Relational | Numeric |
| Shop Stock | Relational | Numeric |
| Achievements | Relational | Numeric |
| XP Curves | Numeric | — |
| Damage Formulas | Numeric | — |
| Drop Rates | Numeric | Relational |
| NPC Dialogue | Textual | Hierarchical |
| Quest Text | Textual | Temporal |
| Item Descriptions | Textual | — |
| Lore/Books | Textual | — |
| Models | Visual | — |
| Textures | Visual | — |
| Particle Effects | Visual | Temporal |
| Music | Visual (audio) | Temporal |

---

## 4. Step 3 — Derive the Tool Type

The primary context determines the base tool. The secondary context adds specialized features on top.

### Base Tool Per Context

| Context | Tool Type | Core Interaction |
|---------|-----------|-----------------|
| Spatial | **Canvas/Map Editor** | Click to place, drag to draw, paint with brush |
| Temporal | **Timeline Editor** | Drag to reorder, set duration, scrub playback |
| Relational | **Spreadsheet + Picker** | Table rows with dropdown/search references to other entities |
| Numeric | **Calculator + Graph** | Input formula, see curve/output, slider adjustment |
| Hierarchical | **Tree Editor** | Expand/collapse nodes, drag to reparent, add children |
| State Machine | **Flowchart Editor** | Place nodes, draw arrows, set conditions on transitions |
| Textual | **Text Editor** | Rich text input, variable insertion, preview rendering |
| Visual | **Preview + Property Panel** | 3D/2D preview with property sliders and color pickers |

### Secondary Context Enhancements

| Secondary Context | What It Adds To The Base Tool |
|-------------------|-------------------------------|
| + Spatial | Minimap preview, "show on world map" toggle, coordinate picker |
| + Temporal | Timeline bar below main editor, playback/preview button |
| + Relational | Dropdown pickers referencing other entity tables, link visualization |
| + Numeric | Inline calculator, range sliders, formula field with live preview |
| + Hierarchical | Breadcrumb navigation, collapsible sections, category sidebar |
| + State Machine | State indicator badge, transition arrows overlay |
| + Textual | Text preview panel, variable autocomplete, character counter |
| + Visual | Thumbnail preview, color swatch, model/texture picker |

### Derivation Examples

**Walls** (Spatial + Relational):
- Base: Canvas editor (paint walls on map)
- Enhancement: Material dropdown (references material table), property panel for breakable/HP
- Result: **Wall painter with edge-snapping, material selector, and property panel**

**Quest Steps** (Temporal + Relational + Textual):
- Base: Timeline editor (reorder steps)
- Enhancement: Entity pickers (NPC, item, location references), text fields for instructions
- Result: **Step list with drag-reorder, NPC/item pickers, instruction text editor, condition builder**

**Boss Phases** (State Machine + Temporal + Spatial):
- Base: Flowchart editor (phase nodes with transitions)
- Enhancement: Timeline per phase (attack sequence), arena map (spatial zones)
- Result: **Phase flowchart with per-phase attack timeline and arena zone painter**

**Drop Tables** (Relational + Numeric):
- Base: Spreadsheet (rows of item drops)
- Enhancement: Item picker dropdown, rate calculator, expected value preview
- Result: **Spreadsheet with item search, fraction input for rates, simulated loot preview**

**XP Curves** (Numeric):
- Base: Calculator with graph
- Enhancement: None needed
- Result: **Formula input field, XP-per-level graph, total XP display, level comparison slider**

**Dialogue Trees** (Hierarchical + Textual + State Machine):
- Base: Tree editor (nodes with children)
- Enhancement: Text input per node, condition editor on branches, flowchart preview
- Result: **Visual dialogue tree with node text editor, branch conditions, and conversation preview/playback**

---

## 5. Step 4 — Determine Operations by Volume

How many instances of this data will exist determines the editing operations needed.

| Volume | Count | Operations | UI Pattern |
|--------|-------|------------|-----------|
| Singleton | 1 | Edit form | Single panel with fields |
| Few | 2-10 | Add, edit, delete, reorder | Vertical list with inline editing |
| Dozens | 10-100 | Add, edit, delete, search, filter | Scrollable list with search bar |
| Hundreds | 100-1000 | Batch edit, sort, filter, import/export | Spreadsheet/table with column headers |
| Thousands | 1000-10000 | Brush/paint, area select, batch operations | Canvas with tools palette |
| Massive | 10000+ | Procedural generation, chunk streaming, LOD | Map editor with zoom levels and streaming |

### Volume Examples

| Mechanic | Typical Volume | Tool Complexity |
|----------|---------------|-----------------|
| XP Curve | Singleton | Simple form |
| Boss Phases | Few (3-5) | Node editor |
| Quest Steps | Dozens (10-50 per quest) | List with drag reorder |
| Shop Stock Items | Dozens per shop, hundreds total | Spreadsheet |
| Drop Table Entries | Hundreds | Spreadsheet with filters |
| NPC Spawns | Hundreds | Map painter with spawn markers |
| Achievements | Hundreds | Spreadsheet with category tabs |
| Dialogue Nodes | Hundreds per NPC, thousands total | Tree editor with search |
| Recipes | Hundreds to thousands | Spreadsheet with chain visualizer |
| Items | Thousands | Spreadsheet with category filters |
| Tiles | Millions | Canvas brush painter with chunks |
| Walls | Tens of thousands | Canvas line/rectangle tools |
| Objects | Thousands | Canvas object placer with browser |

---

## 6. Step 5 — Add Universal Operations

Every tool gets these regardless of context or volume.

### Editing Operations

| Operation | Description |
|-----------|-------------|
| Undo | Reverse last action |
| Redo | Re-apply reversed action |
| Copy | Duplicate selection to clipboard |
| Paste | Place clipboard contents |
| Duplicate | Copy + paste in one action |
| Delete | Remove selected entries |
| Select All | Select everything in current view |
| Multi-Select | Hold shift/ctrl to select multiple |
| Search | Find entries by name, ID, or property |
| Filter | Show only entries matching criteria |
| Sort | Order entries by any column/property |

### Data Operations

| Operation | Description |
|-----------|-------------|
| Import | Load data from file (JSON, CSV) |
| Export | Save data to file |
| Validate | Check data integrity (missing references, invalid values) |
| Diff | Compare current state to previous save |
| Merge | Combine edits from multiple editors |
| Template Save | Save current selection as reusable template |
| Template Load | Stamp template into current context |

### Preview Operations

| Operation | Description |
|-----------|-------------|
| Preview | See how the data looks in-game without full launch |
| Test | Play the mechanic in isolation (test a boss fight, test a quest) |
| Simulate | Run the mechanic without player input (simulate 1000 drop rolls) |
| Compare | Side-by-side comparison of two configurations |

---

## 7. The Game2Tools Formula

For any game mechanic, derive the editor tool:

```
Tool = BaseToolType(PrimaryContext)
     + Enhancements(SecondaryContexts)
     + Operations(Volume)
     + UniversalOps
```

### Quick Reference Card

1. **What's the data?** → List the fields, types, constraints
2. **Where does it live?** → Spatial, Temporal, Relational, Numeric, Hierarchical, State Machine, Textual, Visual
3. **What's the base tool?** → Canvas, Timeline, Spreadsheet, Calculator, Tree, Flowchart, Text Editor, Preview
4. **What references other data?** → Add pickers/dropdowns for linked entities
5. **How much data?** → Singleton=form, Dozens=list, Hundreds=spreadsheet, Thousands=canvas
6. **Add universal ops** → Undo, copy, search, filter, import/export, preview, test

---

## 8. Applied — Every Build Your Own Scape Mechanic

Applying Game2Tools to every major system in the project:

### World Building Tools

| Mechanic | Data | Context | Volume | Base Tool | Result |
|----------|------|---------|--------|-----------|--------|
| Terrain tiles | type, color, variant | Spatial | Millions | Canvas | Tile painter with brush, bucket, eyedropper |
| Elevation | height per tile | Spatial + Numeric | Millions | Canvas | Height painter with hill, ledge, smooth tools |
| Walls | edge bitmask, material | Spatial | Thousands | Canvas | Wall drawer with line, rectangle, room tools |
| Doors | edge, type, locked | Spatial + State Machine | Hundreds | Canvas + Property | Door placer with state toggle and lock config |
| Windows | edge, type, openable | Spatial + State Machine | Hundreds | Canvas + Property | Window placer with open/close and accessory config |
| Roofs | type, pitch, material | Spatial + Numeric | Hundreds | Canvas + Slider | Roof generator with shape/pitch/material selectors |
| Objects | position, type, interactions | Spatial + Relational | Thousands | Canvas + Browser | Object browser with drag-to-place and property panel |
| Lighting | position, radius, color | Spatial + Numeric | Hundreds | Canvas + Slider | Light placer with radius preview and color picker |
| Biomes | area, properties | Spatial | Dozens | Canvas + Property | Biome painter with zone boundary and property panel |
| Zones/Regions | area, rules | Spatial + Relational | Dozens | Canvas + Form | Region painter with rule configuration panel |

### Entity Tools

| Mechanic | Data | Context | Volume | Base Tool | Result |
|----------|------|---------|--------|-----------|--------|
| NPC placement | position, type, properties | Spatial + Relational | Hundreds | Canvas + Form | NPC browser with drag-to-place and config panel |
| Monster spawns | position, type, count, rate | Spatial + Numeric | Hundreds | Canvas + Slider | Spawn point placer with count/rate sliders |
| NPC dialogue | tree of lines and options | Hierarchical + Textual | Thousands | Tree + Text | Dialogue tree editor with conversation preview |
| NPC schedules | time, location, behavior | Temporal + Spatial | Dozens | Timeline + Map | Schedule timeline with map waypoints |
| Patrol paths | waypoint sequence | Spatial + Temporal | Dozens | Canvas + Timeline | Waypoint path drawer with speed/pause config |

### Content Tools

| Mechanic | Data | Context | Volume | Base Tool | Result |
|----------|------|---------|--------|-----------|--------|
| Quest steps | ordered actions with conditions | Temporal + Relational | Dozens per quest | Timeline + Picker | Step list with drag-reorder and entity pickers |
| Quest requirements | skill levels, items, quests | Relational + Numeric | Dozens | Form + Picker | Requirement form with skill/item/quest dropdowns |
| Quest rewards | XP, items, unlocks | Relational + Numeric | Dozens | Form + Picker | Reward form with item/skill pickers and amounts |
| Boss phases | state transitions with attacks | State Machine + Temporal | Few per boss | Flowchart + Timeline | Phase node editor with attack sequence per phase |
| Boss attacks | damage, AoE, conditions | Numeric + Spatial | Dozens per boss | Spreadsheet + Preview | Attack table with AoE preview on arena map |
| Puzzles | objects, states, triggers | State Machine + Spatial | Dozens per puzzle | Flowchart + Canvas | Trigger chain editor with spatial object placement |
| Minigame rules | conditions, scoring, phases | State Machine + Numeric | Dozens | Form + Flowchart | Rule builder with phase flow and scoring config |
| Events | schedule, rewards, scaling | Temporal + Numeric | Dozens | Calendar + Form | Event calendar with config panel per event |

### Item and Economy Tools

| Mechanic | Data | Context | Volume | Base Tool | Result |
|----------|------|---------|--------|-----------|--------|
| Items | properties, stats, categories | Relational | Thousands | Spreadsheet | Item database with category tabs and stat columns |
| Equipment stats | bonuses per slot | Numeric + Relational | Thousands | Spreadsheet | Equipment table with bonus columns and slot filter |
| Drop tables | item, rate, quantity | Relational + Numeric | Thousands | Spreadsheet | Drop table editor with rate calculator and loot sim |
| Recipes | inputs, outputs, requirements | Relational + Numeric | Hundreds | Spreadsheet + Graph | Recipe table with chain visualizer |
| Shop stock | items, prices, restock | Relational + Numeric | Hundreds | Spreadsheet | Shop editor with price preview and restock config |
| XP curves | formula, level data | Numeric | Singleton | Calculator + Graph | Formula editor with level-by-level graph |
| Damage formulas | effective level, max hit | Numeric | Singleton | Calculator | DPS calculator with input sliders and output display |
| Economy balance | sources, sinks, rates | Numeric + Temporal | Singleton | Dashboard + Graph | Economy health dashboard with flow visualization |

### Skill Tools

| Mechanic | Data | Context | Volume | Base Tool | Result |
|----------|------|---------|--------|-----------|--------|
| Skill list | name, category, properties | Hierarchical | Dozens | Tree + Form | Skill tree with per-skill config panel |
| Training methods | XP rates, requirements | Relational + Numeric | Hundreds | Spreadsheet | Method comparison table with rate calculator |
| Resource nodes | type, location, yield | Spatial + Numeric | Thousands | Canvas + Slider | Node placer with yield/respawn config |
| Slayer tasks | monster, count, weight | Relational + Numeric | Hundreds | Spreadsheet | Task table with weight distribution preview |
| Prayer list | name, effect, drain | Hierarchical + Numeric | Dozens | List + Slider | Prayer list with drain calculator |

### Player and Social Tools

| Mechanic | Data | Context | Volume | Base Tool | Result |
|----------|------|---------|--------|-----------|--------|
| Achievements | conditions, tiers, rewards | Relational + Numeric | Hundreds | Spreadsheet | Achievement table with condition builder |
| Collection log | items, sources, categories | Relational | Thousands | Spreadsheet | Log editor with source linking and category tabs |
| Clan settings | ranks, permissions, toggles | Hierarchical | Dozens | Tree + Form | Rank hierarchy editor with permission checkboxes |
| Clan events | type, schedule, rewards | Temporal + Relational | Dozens | Calendar + Form | Event scheduler with type templates |
| Chat channels | rules, permissions | Hierarchical | Dozens | List + Form | Channel list with permission config |
| Modes | rules, restrictions | Relational | Dozens | Form + Comparison | Mode builder with toggle matrix and side-by-side compare |

---

## 9. Tool Composition Patterns

Common patterns that emerge across many tools:

### The Browser-Placer Pattern (Spatial data)
1. **Browser panel** — searchable catalog of things to place (objects, NPCs, tiles)
2. **Canvas** — the world map where you click to place
3. **Property panel** — edit selected placed thing's properties
4. Used by: object placer, NPC placer, spawn placer, light placer

### The Spreadsheet-Picker Pattern (Relational data)
1. **Table** — rows of data with sortable/filterable columns
2. **Picker dropdowns** — cells that reference other tables (item picker, NPC picker)
3. **Inline editing** — edit cells directly in the table
4. Used by: drop tables, recipes, shop stock, achievements, slayer tasks

### The Flowchart-Detail Pattern (State Machine data)
1. **Node graph** — visual nodes with arrows showing transitions
2. **Detail panel** — click a node to edit its properties
3. **Condition editor** — configure what triggers each transition
4. Used by: boss phases, dialogue trees, puzzle logic, quest state flow

### The Timeline-Preview Pattern (Temporal data)
1. **Timeline bar** — horizontal time axis with draggable entries
2. **Preview window** — shows what the result looks like at the scrubbed time
3. **Playback controls** — play, pause, scrub, loop
4. Used by: animations, event schedules, quest step sequences, music tracks

### The Form-Graph Pattern (Numeric data)
1. **Input form** — formula field, sliders, number inputs
2. **Output graph** — visual curve, bar chart, or calculation result
3. **Comparison mode** — overlay two configurations to see differences
4. Used by: XP curves, damage formulas, drop rate analysis, economy dashboards

### The Tree-Text Pattern (Hierarchical + Textual data)
1. **Tree view** — expandable/collapsible node hierarchy
2. **Text editor** — rich text input for selected node's content
3. **Preview** — rendered preview of how text appears in-game
4. Used by: dialogue trees, quest text, lore books, help system content

---

## 10. Validation Rules

Every tool should validate data before saving:

| Validation Type | Description | Example |
|-----------------|-------------|---------|
| Required fields | All mandatory fields have values | Item name cannot be empty |
| Type checking | Values match expected types | Level must be integer, not string |
| Range checking | Numbers within valid bounds | Drop rate between 0 and 1 |
| Reference integrity | Foreign keys point to existing entities | Recipe input item_id must exist in items table |
| Uniqueness | IDs and names are unique within scope | No two items share the same ID |
| Circular reference | No infinite loops in references | Quest A requires Quest B requires Quest A |
| Completeness | Related data sets are complete | Every NPC with a shop_id has a corresponding shop entry |
| Balance warnings | Soft warnings for potentially unbalanced values | Drop rate of 1/1 (guaranteed) flagged as unusual |

---

## 11. From Mechanic to Shipping

The full pipeline from game mechanic idea to usable editor tool:

1. **Define the mechanic** — what does it do for the player?
2. **Extract the data model** — what fields, types, constraints?
3. **Identify context** — spatial, temporal, relational, numeric, hierarchical, state machine, textual, visual?
4. **Determine volume** — singleton, dozens, hundreds, thousands, millions?
5. **Select base tool** — canvas, timeline, spreadsheet, calculator, tree, flowchart, text, preview?
6. **Add context enhancements** — pickers, sliders, preview, map overlay?
7. **Add volume operations** — form, list, table, brush/paint?
8. **Add universal ops** — undo, copy, search, import/export, preview, test?
9. **Add validation** — required fields, type checking, reference integrity?
10. **Build the tool** — implement the derived design
11. **Test with content** — create real game content using the tool
12. **Iterate** — adjust based on creator feedback

---

## Module Toggles

Game2Tools is a methodology, not a runtime system. No module toggles — this doc is a reference framework applied during development.

---

## Views

| View | Description |
|------|-------------|
| Mechanic Analyzer | Input a data model, get recommended tool type |
| Context Classifier | Tag a mechanic with its contexts |
| Tool Derivation Worksheet | Step-by-step walkthrough from mechanic to tool spec |
| Pattern Library | Browse reusable tool composition patterns |
| Validation Rule Builder | Configure validation rules for a data model |
