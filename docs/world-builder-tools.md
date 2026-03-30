# World Builder Tools

Game design document for the World Builder Tools plugin. This system covers the editor suite the dungeon master uses to build the game world. The tools that turn empty terrain into a living world. This is the UX layer on top of terrain.md, buildings.md, npcs.md, and locations.md.

---

## Module Toggles

Each toggle enables or disables a subsystem within the world builder module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Terrain Tools | Tile painting, elevation, biome assignment, procedural generation |
| Building Tools | Wall placement, room tool, doors, windows, roofs, stairs |
| Object Placement | Browse, drag, place, rotate, and configure world objects |
| NPC Placement | Place and configure NPCs, monster spawners, patrol paths |
| Region Tools | Define zones with PvP, combat type, music, weather, access rules |
| Path Tools | Draw roads, rivers, fences, bridges with auto-connect |
| Lighting Mode | Place light sources, set ambient per room or area |
| Test Mode | Switch between builder and player perspective instantly |
| Undo/Redo | Unlimited undo history, checkpoints, branch editing |
| Collaboration | Multi-builder editing, region locking, change tracking, merge |
| Templates/Prefabs | Save, browse, and stamp reusable building templates |
| Community Template Sharing | Upload and download templates from other builders |
| Performance Overlay | Live tile count, object count, entity count, estimated client load |
| Procedural Generation Assist | Noise generator and template stamp for terrain |
| Auto-Connect Paths | Roads and trails automatically connect to nearby paths |
| Snap to Grid | Align placed objects to tile grid (toggle for free placement) |

---

## 1. Editor Modes

The builder switches between modes. Each mode shows a different tool palette and limits what can be placed or edited.

| Mode | Description |
|---|---|
| Terrain Mode | Paint tiles, set elevation, define biomes |
| Building Mode | Place walls, doors, windows, roofs, rooms |
| Object Mode | Place world objects (furniture, stations, decorations, resource nodes) |
| NPC Mode | Place and configure NPCs (shops, quest givers, guards, monsters) |
| Region Mode | Define zones, areas, properties (PvP, combat type, music, weather) |
| Path Mode | Draw roads, trails, fences, rivers |
| Lighting Mode | Place light sources, set ambient per room or area |
| Test Mode | Play the world as a player to test (switch between builder and player instantly) |

---

## 2. Terrain Tools

Tools available in Terrain Mode.

| Tool | Description |
|---|---|
| Pencil | Paint a single tile |
| Brush | Paint an area with configurable radius |
| Bucket Fill | Flood fill all connected tiles of the same type |
| Eyedropper | Sample tile type and color from existing tile |
| Eraser | Reset tile to default type |
| Line Tool | Draw a straight line of tiles between two points |
| Rectangle Fill | Fill a rectangular area with selected tile |
| Circle Fill | Fill a circular area with selected tile |
| Hill Tool | Raise elevation smoothly across an area |
| Ledge Tool | Create a sharp elevation step |
| Stair Tool | Create stepped elevation between two heights |
| Flatten Tool | Level all tiles in area to a target height |
| Smooth Tool | Average elevation across area to reduce rough edges |
| Plateau Tool | Set all tiles in area to the same height with sharp edges |
| Biome Painter | Assign a biome to an area (affects spawns, weather, palette) |
| Texture Variant Selector | Pick which variant of a tile type to paint (grass1, grass2, grass3) |
| Color Tint Picker | Apply a hex color overlay on top of tile texture |
| Noise Generator | Generate procedural terrain from a seed value |
| Template Stamp | Place a pre-built terrain patch from the template library |

---

## 3. Building Tools

Tools available in Building Mode.

| Tool | Description |
|---|---|
| Wall Placement | Click two points to place a wall on the edge between them |
| Wall Line | Drag to draw a straight wall along one axis |
| Wall Rectangle | Drag to create a room outline from four walls |
| Room Tool | Drag to create an enclosed room with auto-generated walls |
| Door Placement | Click a wall edge to place a door |
| Window Placement | Click a wall edge to place a window |
| Wall Type Selector | Choose material, thickness, and breakability for new walls |
| Wall Decoration Panel | Select trim, paintings, shelves, sconces to apply to wall segments |
| Roof Auto-Generate | Detect enclosed room and generate matching roof shape |
| Roof Manual | Place individual roof segments by hand |
| Floor Painter | Override the terrain inside a room with a chosen floor type |
| Ceiling Settings | Set ceiling height and material for a room |
| Stair/Ladder Placement | Connect two layers with stairs or a ladder |

---

## 4. Object Placement

Tools available in Object Mode.

| Tool | Description |
|---|---|
| Object Browser | Search all objects by name, category, or keyword |
| Category Filters | Filter by furniture, stations, decorations, resource nodes, barriers, lighting, signs, mechanical, natural, religious |
| Drag to Place | Click an object in the browser, then click in the world to place it |
| Rotation | Rotate an object before placing (90 degree snap or free rotation) |
| Snap to Grid | Align object to tile grid or switch to free placement (toggle) |
| Elevation Snap | Automatically set object height to match terrain |
| Copy/Paste | Duplicate placed objects |
| Multi-Select | Select an area to move, delete, or copy multiple objects at once |
| Property Panel | Click a placed object to edit its properties (interactions, animations, lock state) |
| Replace | Swap one object type for another across a selected area |

---

## 5. NPC Placement

Tools available in NPC Mode.

| Tool | Description |
|---|---|
| NPC Browser | Search NPCs by type, role, or faction |
| Drag to Place | Click an NPC in the browser, then click in the world to set spawn point |
| Wander Radius | Drag a circle around an NPC to set how far it roams |
| NPC Properties Panel | Configure name, dialogue, shop inventory, services, daily schedule |
| Monster Spawner | Place a spawn point with monster type, count, and respawn rate |
| Patrol Path | Draw waypoints that the NPC follows in sequence |
| Group Spawner | Place a cluster of same or mixed monsters at a spawn area |
| Boss Arena Setup | Define arena bounds, phase triggers, and mechanic zones |

---

## 6. Region Tools

Tools available in Region Mode.

| Tool | Description |
|---|---|
| Area Select (Rectangle) | Select a rectangular zone |
| Area Select (Polygon) | Select a polygon zone by clicking vertices |
| Area Select (Freehand) | Draw a freehand zone boundary |
| Zone Properties Panel | Set name, PvP toggle, combat type, level bracket, weather, music, access requirements |
| Boundary Visualization | Colored outlines showing zone borders on the map |
| Copy Zone Properties | Apply one zone's settings to another zone |
| Transition Settings | Configure how zones blend visually and mechanically at borders |

---

## 7. Path Tools

Tools available in Path Mode.

| Tool | Description |
|---|---|
| Road Painter | Draw roads with automatic edge blending to surrounding terrain |
| River Tool | Draw water paths with configurable flow direction |
| Fence Tool | Draw fence lines with automatic post spacing |
| Bridge Tool | Span gaps with a configurable bridge type |
| Trail Marker | Place waypoint signs along a path |
| Auto-Connect | Roads and trails automatically connect to nearby endpoints |

---

## 8. Undo / Redo

| Feature | Description |
|---|---|
| Unlimited Undo | Revert any number of actions within the current session |
| Redo | Re-apply undone actions |
| History Browser | Scroll through a list of all changes in the session |
| Checkpoint Save | Create a manual save point to revert to at any time |
| Diff View | See what changed since the last save |
| Branch Editing | Save multiple versions of the same area and switch between them |

---

## 9. Collaboration

| Feature | Description |
|---|---|
| Simultaneous Editing | Multiple builders can edit the world at the same time |
| Region Locking | Lock a region to prevent other builders from editing your area |
| Change Tracking | Log of who changed what and when |
| Merge Changes | Combine edits from multiple builders working on the same area |
| Builder Chat | Builder-only chat channel separate from player chat |
| Permission Levels | Junior builders can place objects, senior builders can edit terrain |

---

## 10. Templates and Prefabs

| Feature | Description |
|---|---|
| Save as Template | Select any group of tiles, walls, objects, and NPCs and save as a reusable template |
| Template Library | Browse, search, and categorize saved templates |
| Community Templates | Share templates publicly or download templates from other builders |
| Stamp Template | Place a template into the world with rotation and optional scaling |
| Template Variables | Placeholders that get customized when placed (NPC names, shop stock, sign text) |
| Building Presets | Pre-built common structures: house, shop, castle, tavern, church, mine, farm |

---

## 11. Testing and Preview

| Feature | Description |
|---|---|
| Play Mode | Instantly switch to player perspective in the current area |
| NPC Dialogue Preview | Test NPC conversations without leaving the editor |
| Combat Test | Spawn test monsters and fight them in the editor |
| Path Test | Verify walkability and check pathfinding across the map |
| Performance Overlay | Shows tile count, object count, entity count, estimated client load |

---

## Views

Editor and debug views available in the world builder.

| View | Description |
|---|---|
| Tool Palette | Context-sensitive tool panel for the current editor mode |
| Object Browser | Searchable catalog of all placeable objects |
| NPC Browser | Searchable catalog of all NPCs by type, role, and faction |
| Template Library | Browse and search saved templates and prefabs |
| Property Editor | Edit properties of any selected tile, wall, object, or NPC |
| Region Map | Color-coded overview of all defined zones and boundaries |
| History/Undo Panel | Scrollable list of all actions with undo/redo controls |
| Collaboration Panel | Shows connected builders, locked regions, and change log |
| Performance Monitor | Live stats for tile count, object count, entity count, and load |
