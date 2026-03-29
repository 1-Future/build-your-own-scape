# Engine Architecture

Reference architecture from the working OpenScape implementation. Documents proven patterns and technical decisions for building a -Scape game engine.

---

## 1. Networking

### WebSocket Protocol

| Property | Value |
|---|---|
| Transport | Raw WebSocket on configurable port (default: 2222) |
| Message format | JSON (JSON.stringify/parse for all messages) |
| Binary exception | Chunk tile data transmitted as base64-encoded binary |
| Reconnect strategy | Auto-reconnect on client with 2-second retry |
| Authentication | None in base implementation (pluggable -- auth is a plugin concern) |

---

### Message Types

All communication uses typed JSON messages. Plugins can register additional message types.

| Direction | Type | Description |
|---|---|---|
| Client to Server | set_name | Set player display name |
| Client to Server | move | Request pathfinding to x,y |
| Client to Server | chat | Send chat message |
| Client to Server | paint | Paint tile at position |
| Client to Server | wall | Place or remove wall edge |
| Client to Server | door | Place or remove door edge |
| Client to Server | height | Set tile elevation |
| Client to Server | bucket | Flood-fill area |
| Server to Client | state | Entity positions and states (every tick) |
| Server to Client | chunk | Chunk tile data (base64 + variants) |
| Server to Client | chat | Chat message |
| Server to Client | wall_update | Wall change notification |
| Server to Client | door_update | Door change notification |
| Server to Client | online_list | Connected players |
| Bidirectional | plugin-defined | Plugins register custom message handlers |

---

### Proximity Filtering

| Setting | Value | Description |
|---|---|---|
| Entity view distance | 4 chunks (256 tiles) | Only sends entity state for nearby entities |
| Chunk view distance | 3 chunks | Range at which chunk data is streamed to client |
| Chunk tracking | sentChunks Set per player | Tracks which chunks each player has received |
| Eviction | On player movement | Chunk tracking cleared when player moves away |

---

## 2. World Data Format

### Tile System

15 base tile types. CUSTOM tiles use arbitrary hex color.

| Index | Type | Index | Type |
|---|---|---|---|
| 0 | GRASS | 8 | DOOR |
| 1 | WATER | 9 | BRIDGE |
| 2 | TREE | 10 | FISH_SPOT |
| 3 | PATH | 11 | FLOWER |
| 4 | ROCK | 12 | BUSH |
| 5 | SAND | 13 | DARK_GRASS |
| 6 | WALL | 14 | CUSTOM |
| 7 | FLOOR | | |

**Key design principle:** tile types are purely visual. Only wall edges block movement.

---

### Chunk Format

| Property | Value |
|---|---|
| Chunk size | 64x64 tiles |
| Storage format | Binary .bin (Uint8Array) + JSON color sidecar |
| Naming convention | cx_cy.bin for layer 0, Lx_cx_cy.bin for other layers |
| Memory management | Lazy load from disk, cache in memory Map, evict after 60 seconds idle |
| Transfer format | Base64 over WebSocket with attached variant and color data |

---

### Edge-Based Wall System

Walls exist on tile edges, not as full tiles.

| Property | Value |
|---|---|
| Bitmask | 6-bit per tile |
| North | Bit 0 (value 1) |
| East | Bit 1 (value 2) |
| South | Bit 2 (value 4) |
| West | Bit 3 (value 8) |
| Diagonal NE | Bit 4 (value 16) |
| Diagonal NW | Bit 5 (value 32) |

- Doors use the same bitmask format stored in a separate data file.
- Only wall edges block pathfinding. Tile type never blocks movement.
- Diagonal walls split tile rendering into two triangles (half-paint).

---

### Layer System

| Property | Value |
|---|---|
| Range | -1000 to 1000 |
| Independence | Each layer is an independent flat plane |
| Per-layer data | Walls, doors, heights, roofs, variants |
| Transitions | Stairs and ladders connect layers (discrete, not ramps) |

---

### Per-Tile Data

| Data | Storage | Format |
|---|---|---|
| Tile type | chunks/*.bin | Uint8Array index |
| Tile color | chunks/*.json | Hex color per index |
| Tile variant | variants.json | Integer variant number |
| Wall edges | walls.json | 6-bit bitmask |
| Door edges | doors.json | 6-bit bitmask |
| Wall texture | wall_textures.json | "type_variant" string |
| Height / elevation | heights.json | Float (fractional for slopes) |
| Roof | roofs.json | type, pitch, eave, rotation |
| Custom name | names.json | String label |

---

### Persistence

All data stored as flat JSON files. No database.

| Setting | Value |
|---|---|
| Auto-save interval | 30 seconds |
| Data files | walls.json, doors.json, heights.json, positions.json, appearances.json, friends.json, variants.json, wall_textures.json, lots.json, roofs.json, names.json |
| Plugin data | data/plugins/{pluginId}.json |

---

## 3. Plugin Architecture

### Dual-Sided Pattern

Plugins contain both server and client code in a single file using a universal module pattern (IIFE that works as both CommonJS and browser global).

```
exports.meta = { id, name, version, depends: [] }
exports.server = { api: {}, init(engine) {} }
exports.client = { api: {}, init(engine) {} }
```

---

### Plugin Loading

| Step | Description |
|---|---|
| Configuration | plugins.json lists enabled plugin IDs in load order |
| Server loading | Server loads plugins/{id}/plugin.js |
| Client injection | Client plugins injected via HTML placeholder |
| Dependencies | depends array enables dependency checking |

---

### Server Engine API

Methods exposed to plugins on the server side.

| Method | Description |
|---|---|
| tileAt(x, y, layer) | Get tile type at position |
| setTile(x, y, layer, type, color) | Set tile at position |
| isWalkable(x, y, layer) | Check if tile is walkable |
| findPath(from, to, layer) | A* pathfinding between points |
| schedule(atTick, priority, key, fn) | Schedule action at future tick |
| cancelScheduled(key) | Cancel scheduled action by key |
| getTick() | Get current tick number |
| onTick(fn) | Register per-tick callback |
| onMessage(type, fn) | Register message handler |
| addXp(player, skill, amount) | Grant skill XP |
| addItem(player, itemId, amount) | Add item to inventory |
| broadcast(msg) | Send to all connected players |
| send(player, msg) | Send to specific player |
| sendChat(player, msg) | Send chat message to player |
| loadData(pluginId) | Load plugin JSON data |
| saveData(pluginId, data) | Save plugin JSON data |
| on(event, fn) / emit(event, data) | Event bus |

---

### Client Engine API

Methods exposed to plugins on the client side.

| Method | Description |
|---|---|
| registerTool(toolDef) | Register a build mode tool |
| registerBuildTab(tabDef) | Register a build panel tab |
| registerPanel(panelDef) | Register a UI panel |
| registerKeybind(keybindDef) | Register a keyboard shortcut |
| onRender(fn) | Register per-frame render callback |
| send(msg) | Send message to server |
| onMessage(type, fn) | Register server message handler |
| tileAt(x, y) | Get local tile data |
| getWallEdge(x, y, layer) | Get wall bitmask |
| setLocalTile(x, y, type, color) | Set tile locally (immediate feedback) |
| showChat(msg) | Display chat message locally |

---

### Plugin Data Persistence

| Property | Value |
|---|---|
| Storage path | data/plugins/{id}.json |
| Load | engine.loadData() |
| Save | engine.saveData() |
| Auto-save | Included in global 30-second save interval |

---

## 4. Rendering

### Dual Render Modes

| Mode | Technology | Description |
|---|---|---|
| 2D | Canvas 2D | Tile-by-tile rendering with procedural textures |
| 3D | Three.js r128 | Full 3D with geometry built from tile, wall, and height data |

---

### PS1 Procedural Textures

| Property | Value |
|---|---|
| Count | ~77 textures generated at runtime |
| Resolution | 16x16 pixels each |
| Categories | grass, water, tree, path, rock, sand, wall, floor, door, bridge, fish_spot, flower, bush, dark_grass, custom |
| Variants | Multiple per type (grass x4, water x3, etc.) |
| Atlas | Built dynamically for 3D mode |
| Tint system | Color tinting applied via canvas manipulation |

---

### 3D Rendering

| Element | Method |
|---|---|
| Floors | Textured quads from tile data with height displacement |
| Walls | Vertical planes from edge bitmasks |
| Roofs | Generated from roof data (type, pitch, eave, rotation) |
| NPCs and objects | GLTF model loading |
| Camera | Orbital with angle, pitch, distance controls |
| Free-roam | WASD camera mode |

---

### Coordinate System

| Property | Value |
|---|---|
| Y-flip | Game Y increases northward, screen Y increases downward |
| Camera follow | Smooth interpolation (lerp factor 0.15) |
| Panning | Middle-click drag |
| Zoom | Scroll wheel |

---

## 5. Pathfinding

### A* Implementation

| Property | Value |
|---|---|
| Directions | 8 (cardinal + diagonal) |
| Max distance | 200 tiles |
| Max search nodes | 2000 |
| Diagonal movement | Allowed if no wall edges block |
| Wall checking | Checks both source and destination tile edges |
| Runs on | Server-side |
| Cardinal cost | 1.0 |
| Diagonal cost | 1.414 |

---

## 6. Multiplayer Sync

### State Broadcasting

| Property | Value |
|---|---|
| Frequency | Every tick (600ms) |
| Payload | Position, name, appearance, combat info for nearby entities |
| Interpolation | Smooth lerp on client (factor 0.15) |
| Scope | Separate smooth position trackers for players and NPCs |

---

### Tile Change Sync

- Tile, wall, door, and height changes broadcast only to players who have that chunk loaded.
- Uses sentChunks Set per player to track delivery.

---

### Player Lifecycle

| Event | Behavior |
|---|---|
| Connect | Assign sequential ID, send online list |
| Set name | Load position and appearance, broadcast join |
| Move | Server pathfinds, streams new chunks if needed |
| Disconnect | Save position, broadcast leave |
| Reconnect | Resume from last saved position |

---

## 7. Game Console API

### MCP / AI Interface

Exposed as window.game object for programmatic control.

| Command | Description |
|---|---|
| game.move(x, y) | Move player to position |
| game.teleport(x, y) | Instant teleport |
| game.pos() | Get current position |
| game.setName(name) | Change display name |
| game.layer(n) | Switch layer |
| game.paintTile(x, y, type, color) | Paint a tile |
| game.fillArea(x1, y1, x2, y2, type, color) | Fill rectangular area |
| game.placeWall(x1, y1, x2, y2) | Place wall segment |
| game.wallLine(x1, y1, x2, y2) | Draw wall line |
| game.wallRect(x1, y1, x2, y2) | Draw wall rectangle |
| game.setHeight(x, y, h) | Set tile elevation |
| game.placeRoof(type, pitch, eave, rotation) | Place roof |
| game.tileAt(x, y) | Query tile data |
| game.screenshot() | Capture screenshot |
| game.batch(commands[]) | Execute array of commands |

The batch() method enables AI-driven world building. Send an array of paint, wall, and height commands in one call.

---

## 8. UI Panel System

### Panel Customization

| Property | Options |
|---|---|
| Corner style | Round, Square, Chamfer (6 variants) |
| Edge profile | Flat, Bevel, Inset, Ridge, Groove (6 variants) |
| Border weight | None, Thin, Medium, Thick |
| Shadow / depth | None, Subtle, Lifted, Floating |
| Glow effect | None, Accent, Neon, Soft |
| Glass mode | Transparency / glass-morphism toggle |
| Stickers | Decorative sticker overlays |
| Color | Per-panel color, gradient, or texture background |
| Opacity | Per-panel transparency |
| Label | Rename any panel |

---

### Panel Behaviors

| Behavior | Description |
|---|---|
| Clone | Any panel can be duplicated as standalone floating window |
| Pop-out | Individual tabs can be extracted from parent panel |
| Drag and resize | All panels freely movable and resizable |
| Taskbar | Shows all panels as minimize/restore buttons |
| Build mode | Replaces taskbar with build tool tabs |
| Snap grid | Anchor grid system for panel snapping |

---

## 9. Character System

### Character Creation

| Property | Value |
|---|---|
| Body types | A (male) / B (female) |
| Body parts | 7 slots: head, jaw, torso, arms, hands, legs, feet |
| Color slots | 5: hair, torso, legs, feet, skin |
| Preview | 3D rotatable model (Three.js) |
| Persistence | Server-side by player name |
| Changeable | Yes, post-creation |

---

## 10. Deterministic Systems

### Seeded RNG

| Property | Value |
|---|---|
| Formula | seed * 16807 % 2147483647 |
| Purpose | Reproducible world generation |
| Loot rolls | Consistent when seed is known |
| Fairness | Provably fair -- share seed post-roll to verify |

---

### Tick Determinism

| Property | Value |
|---|---|
| Tick rate | 600ms |
| Execution order | Priority queue ensures consistent order |
| Deduplication | Key-based -- scheduling with same key replaces previous entry |

---

## Module Toggles

Master switches that enable or disable entire subsystems.

| Toggle | Default | Description |
|---|---|---|
| WebSocket protocol | On | JSON messaging over WebSocket |
| Binary chunk streaming | On | Base64 chunk transfer |
| Proximity filtering | On | Only sync nearby entities |
| Edge-based walls | On | Walls on edges versus full tiles |
| Layer system | On | Multiple vertical layers |
| Dual render mode | On | 2D + 3D toggle |
| PS1 textures | On | Procedural texture generation |
| A* pathfinding | On | Server-side pathfinding |
| Plugin dual-side pattern | On | Combined server and client plugins |
| Game console API | On | MCP / AI programmatic control |
| Panel customization | On | Full UI theming |
| Seeded RNG | On | Deterministic random numbers |
| Auto-save | On | 30-second save interval |
| Chunk eviction | On | Memory management for idle chunks |

---

## Views

| View | Description |
|---|---|
| Network diagnostics | WebSocket connection status and message throughput |
| Chunk loading map | Visual display of loaded and pending chunks |
| Plugin manager | Enable, disable, and configure plugins |
| Console / REPL | In-browser command console for game API |
| Panel editor | Visual panel customization interface |
| Render mode toggle | Switch between 2D and 3D rendering |
| Pathfinding debug overlay | Visual display of A* search nodes and final path |
