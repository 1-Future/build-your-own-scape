# Terrain

Game design document for the Terrain plugin. This system covers the ground layer -- tiles, elevation, biomes, and the fundamental spatial system everything is built on. The world surface that all other systems (buildings, NPCs, monsters, resources) sit on top of.

---

## Module Toggles

Each toggle enables or disables a subsystem within the terrain module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Tile Paradigm Selection | Choose between grid, hex, free movement, room-based, voxel, or hybrid |
| Custom Tile Types | DM can define new tile types beyond the defaults |
| Tile Variant System | Multiple visual variants per tile type (grass1, grass2, grass3) |
| Tile Tinting/Coloring | Hex color override applied on top of tile texture |
| Elevation System | Height values per tile (flat, stepped, smooth, or voxel) |
| Slope Movement | Players can walk up slopes below the cliff threshold |
| Fall Damage | Damage based on height difference when falling |
| Jump Mechanic | Players can clear small gaps or ledges |
| Biome System | Regions with distinct tile palettes, weather, spawns, and effects |
| Biome Transitions | Blending behavior at biome boundaries (hard edge, gradient, mixed) |
| Procedural Generation | Algorithm-based terrain generation from seed |
| Terrain Import/Export | Import height maps or tile maps from external tools |
| Chunk System | World divided into chunks for streaming and storage |
| Layer System | Vertical layers from -1000 to 1000 |
| Speed Modifiers Per Tile | Tile types can speed up or slow down movement |
| Environmental Tile Effects | Poison swamp, cold snow, heat damage from tile type |
| Destructible Tiles | Tiles can be broken or mined by players |
| Tile Growth/Change Over Time | Tiles change state over time (grass grows, erosion) |
| Tile-Based Walkability | Each tile type has a walkable flag |
| Edge-Based Walkability | Only wall edges block movement; tile type is purely visual |

---

## 1. Tile Paradigm

The DM chooses one paradigm for their world. This determines how space is divided and how players move through it.

| Paradigm | Description | Example |
|---|---|---|
| Grid (Square) | Fixed square tiles, player moves tile-to-tile. Floor looks grid-like. Classic -Scape feel | OSRS, Brighter Shores |
| Grid (Hidden) | Square tiles but grid lines not visible. Feels smoother while maintaining tile math | RS3 |
| Hex Grid | Hexagonal tiles. 6 directions instead of 8. Better for tactical/strategy games | Civilization |
| Free Movement | No tiles. Player moves freely in continuous space. Requires different collision system | WoW, FFXIV |
| Room-Based | World divided into discrete rooms connected by exits. Each room is self-contained. Instanced per room | Brighter Shores |
| Voxel | 3D block-based world. Minecraft-style. Full vertical building | Minecraft, Hytale |
| Hybrid | Mix paradigms per zone (overworld = grid, dungeons = rooms, housing = free placement) | Custom |

---

## 2. Tile Types

### Tile Data Model

Every tile type has one row. DM can add custom types beyond the defaults.

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique tile type identifier |
| Name | String | Display name (Grass, Water, Rock, etc.) |
| Texture | String | Texture or appearance reference |
| Walkable | Boolean | Whether players can walk on this tile (ignored if using edge-based walkability) |
| Swim/Wade | Boolean | Water tile behavior -- requires swimming or allows wading |
| Transparent | Boolean | Can see through this tile (glass floor, grate) |
| Destructible | Boolean | Can be broken or mined by players |
| Growth | String | Change behavior over time (grass grows, erosion occurs) |
| Sound | String | Footstep sound effect when walking on this tile |
| Speed Modifier | Float | Movement speed multiplier (1.0 = normal, 0.5 = half speed, 1.5 = fast) |
| Interaction | String | Available interaction (fishing spot, mining node, woodcutting) |
| Variant Count | Integer | How many visual variants exist for this type |
| Tintable | Boolean | Whether player or DM can recolor this tile |

---

### Default Tile Types

| ID | Type | Properties |
|---|---|---|
| 0 | Grass | Walkable, standard speed, multiple green variants |
| 1 | Water | Not walkable (or swim if enabled), fishing spots, blue variants |
| 2 | Tree | Not walkable, woodcutting interaction, blocks line of sight |
| 3 | Path | Walkable, speed bonus, brown/stone variants |
| 4 | Rock | Not walkable, mining interaction, grey variants |
| 5 | Sand | Walkable, speed penalty, beach/desert variants |
| 6 | Wall | Not walkable, blocks LoS (separate from edge walls) |
| 7 | Floor | Walkable, interior surface, wood/stone/tile variants |
| 8 | Door | Walkable when open, closed blocks, interactive |
| 9 | Bridge | Walkable over water, wood/stone variants |
| 10 | Snow | Walkable, speed penalty, cold damage over time |
| 11 | Lava | Not walkable (or damage if walk), fire effect |
| 12 | Swamp | Walkable, speed penalty, poison chance |
| 13 | Ice | Walkable, slide mechanic (continue in direction), no friction |
| 14 | Mud | Walkable, heavy speed penalty, can get stuck |
| 15 | Custom | DM-defined with any property combination |

---

### Tile Variant System

Each tile type can have multiple visual variants (grass1, grass2, grass3, grass4). Variants are assigned per tile position.

| Setting | Description |
|---|---|
| Auto-variant | Random variant selected on placement |
| Manual variant | DM picks a specific variant per tile |
| Tint override | Hex color applied on top of the base texture per tile |

---

## 3. Elevation System

### Height Data Model

| Column | Type | Description |
|---|---|---|
| Height | Float | Per-tile height value (fractional values allow smooth slopes) |
| Rendering | Enum | Flat planes at different heights, or smooth interpolation between neighbors |
| Cliff Threshold | Float | Height difference that creates an impassable cliff wall vs a walkable slope |
| Stair Tile | Boolean | Connects different elevation levels discretely |

---

### Elevation Types

| Type | Description |
|---|---|
| Flat | All tiles same height. Simple 2D feel |
| Stepped | Discrete height levels (0, 1, 2, 3). OSRS-style planes |
| Smooth | Continuous height values. Rolling hills, gradual slopes |
| Voxel | 3D height with overhangs, caves, bridges |

---

### Elevation Tools

Tools available in the world builder for shaping terrain.

| Tool | Description |
|---|---|
| Hill | Raise or lower area with configurable falloff radius |
| Ledge | Create sharp elevation change at boundary |
| Stair | Create walkable steps between two heights |
| Flatten | Level area to a target height |
| Smooth | Average neighboring tile heights to remove jagged edges |
| Plateau | Create flat top with sloped edges |

---

### Movement on Elevation

| Rule | Description |
|---|---|
| Slope walk | Walk up slopes below the cliff threshold |
| Cliff block | Cannot walk up or down cliffs (must use stairs or ladders) |
| Slide | Slide down steep slopes (optional toggle) |
| Fall damage | Damage based on height difference when falling (optional toggle) |
| Jump | Clear small gaps or ledges (optional toggle) |

---

## 4. Biome System

### Biome Data Model

| Column | Type | Description |
|---|---|---|
| ID | Integer | Unique biome identifier |
| Name | String | Display name (Forest, Desert, Arctic, etc.) |
| Tile Palette | Array | Which tile types and variants are common in this biome |
| Ambient Sounds | Array | Background audio loops for the biome |
| Music Tracks | Array | Music that plays while in this biome |
| Weather Patterns | Array | Allowed weather events (links to locations.md weather system) |
| Monster Spawn Table | Array | Which monsters spawn naturally (links to monsters.md) |
| Resource Node Table | Array | Which gathering nodes appear (links to skills-gathering.md) |
| Environmental Effects | Array | Heat damage, cold damage, darkness, poison, etc. |
| Transition Behavior | Enum | How biome blends at boundaries (hard edge, gradient, mixed tiles) |

---

### Default Biomes

| Biome | Description |
|---|---|
| Forest | Temperate woodlands, dense canopy, wood and herb resources |
| Desert | Arid sand or rock, extreme heat, cactus and sandstone resources |
| Arctic/Snow | Frozen landscape, extreme cold, ice fishing and fur trapping |
| Swamp/Marsh | Waterlogged terrain, poison fog, rare herbs and bog ore |
| Volcanic | Lava fields, extreme heat, fire hazards, obsidian and gem resources |
| Cave/Underground | Subterranean, darkness mechanic, ores and fungi |
| Ocean/Coastal | Shoreline and open water, fishing, shells, driftwood |
| Mountain | High elevation, thin air, goats, eagles, rare alpine plants |
| Plains/Grassland | Flat open terrain, farming, livestock, wildflowers |
| Jungle | Dense tropical vegetation, humidity, exotic creatures and plants |
| Tundra | Sparse frozen plains, permafrost, mammoth bones |
| Urban/Town | Built-up area, shops, NPCs, crafting stations |
| Dungeon | Interior hostile area, traps, puzzles, boss encounters |
| Ethereal/Magic | Otherworldly terrain, magic effects, rune resources |
| Void | Empty space, environmental damage, endgame content |
| Custom | DM-defined biome with any property combination |

---

## 5. Chunk System

Inherited from engine-architecture.md. Terrain is stored and streamed in chunks.

| Setting | Value | Description |
|---|---|---|
| Chunk size | 64x64 tiles (configurable) | Fundamental unit of world storage |
| Storage format | Binary (.bin) + JSON sidecar | Tile data in binary, metadata in JSON |
| Loading strategy | Lazy load with memory eviction | Chunks loaded on demand, evicted when player moves away |
| View distance | 3 chunks | Range at which chunk data is streamed to client |
| Streaming protocol | Base64 over WebSocket | Binary chunk data encoded for WebSocket transport |
| Layer system | -1000 to 1000 | Independent flat planes stacked vertically |

---

## 6. Terrain Generation

Optional system for procedural or assisted world creation. DM can use any combination of methods.

### Generation Methods

| Method | Description |
|---|---|
| Hand-painted | DM places every tile manually. Full control, most effort |
| Procedural | Algorithm generates terrain (Perlin noise, diamond-square). Fast, less control |
| Template-based | Pre-built terrain templates stamped into world (forest template, town template) |
| Hybrid | Procedural base with hand-painted details. Best of both |
| Import | Import height maps or tile maps from external tools |

---

### Procedural Generation Settings

| Setting | Type | Description |
|---|---|---|
| Seed | Integer | Deterministic generation from seed value |
| Noise Algorithm | Enum | Perlin, Simplex, or Worley noise |
| Scale | Float | Zoom level of the noise function |
| Octaves | Integer | Layers of detail stacked on top of each other |
| Biome Distribution | Rules | How biomes are assigned to generated regions |
| River/Lake Generation | Boolean | Whether water features are auto-generated |
| Path/Road Generation | Boolean | Whether roads connecting points of interest are auto-generated |
| Structure Placement | Rules | Where towns, dungeons, and ruins are placed in generated terrain |

---

## 7. Walkability System

### Two Approaches

The DM chooses one approach for their world. This determines how movement blocking works.

| Approach | Description |
|---|---|
| Tile-based | Each tile type has a walkable flag. Water = unwalkable, grass = walkable. Simple |
| Edge-based | Tile types are purely visual. Only wall edges (bitmask per tile edge) block movement. More flexible. OpenScape default |

---

### Interaction Between Systems

| System | Interaction |
|---|---|
| Edge-based walls | Wall edges (buildings.md) override tile walkability |
| Elevation cliffs | Cliffs block movement regardless of tile type |
| Doors | Toggle walkability on their edge when opened or closed |
| Placed objects | Objects on tiles can block movement (furniture, barriers) |

---

## Views

Editor and debug views available in the world builder.

| View | Description |
|---|---|
| Tile Type Palette | Browse and select tile types for painting |
| Elevation Heatmap | Color-coded height visualization across the map |
| Biome Map | Color-coded biome regions across the map |
| Chunk Grid Overlay | Shows chunk boundaries on the map |
| Walkability Overlay | Highlights walkable vs blocked tiles |
| Generation Preview | Preview procedural generation before committing |
| Terrain Editor | Full terrain editing interface with all tools |
