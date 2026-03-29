# Buildings

Game design document for the Buildings plugin. This system covers the architectural layer -- walls, rooms, doors, windows, lighting, and interior design. Sits on top of terrain (terrain.md) and contains NPCs (npcs.md), shops (economy.md), and furniture (player-housing.md).

---

## Module Toggles

Each toggle enables or disables a subsystem within the buildings module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Edge-Based Walls | Walls exist on tile edges with bitmask system |
| Wall Types | Multiple wall types (solid, fence, hedge, glass, bars, curtain) |
| Wall Decoration | Trim, paintings, shelves, sconces on wall segments |
| Window System | Openings in walls with light transmission and LoS rules |
| Door Locking | Doors can be locked with keys or skill requirements |
| Door Types | Special doors (trapdoor, portal, secret, drawbridge) |
| Room Detection | Flood-fill algorithm detects enclosed rooms from walls |
| Room Types | Rooms have functional types that affect allowed content |
| Room Instancing | Private room copy per player or party (Brighter Shores style) |
| Room Capacity Limits | Maximum number of players allowed in a room |
| Ceiling Auto-Generation | Ceilings auto-generated from room bounds |
| Roof System | Exterior roof shapes (gabled, hipped, flat) with materials |
| Lighting System | Light sources with radius, color, and flickering |
| Darkness Mechanic | Areas completely dark without a light source |
| Light Fuel Consumption | Light sources require fuel items to stay lit |
| Interior Floor Override | Room floors override terrain tiles underneath |
| Building Templates/Blueprints | Save and stamp pre-built structures |
| Breakable Walls/Doors/Windows | Architectural elements can be destroyed |
| Climbable Walls | Players can climb certain wall types |
| Line of Sight Blocking | Walls block player and NPC line of sight |
| Sound Blocking | Walls reduce or block proximity chat range |
| Projectile Blocking | Walls block ranged attacks and projectiles |
| Secret Doors | Hidden doors revealed by search or quest completion |

---

## 1. Wall System

### Edge-Based Walls

Walls exist on tile EDGES, not as full tiles. This is the OpenScape default approach.

| Property | Value |
|---|---|
| Bitmask | 6-bit per tile: N(1), E(2), S(4), W(8), DiagNE(16), DiagNW(32) |
| Storage key | layer_x_y |
| Movement blocking | Only wall edges block movement -- tile type is purely visual |
| Capability | Doors on any edge, half-walls, diagonal walls, complex floor plans |

---

### Wall Data Model

Each wall segment has the following properties.

| Column | Type | Description |
|---|---|---|
| Wall ID | String | Key in format layer_x_y |
| Edge | Enum | N, E, S, W, DiagNE, DiagNW |
| Wall Type | Enum | Solid, half-height, fence, railing, hedge, glass, bars, curtain, custom |
| Material | String | Stone, wood, brick, metal, glass, custom |
| Texture | String | Texture ID + variant reference |
| Thickness | Enum | Thin, medium, thick (visual only) |
| Breakable | Boolean | Can be destroyed by players or combat |
| HP | Integer | Hit points if breakable (null if not breakable) |
| Climbable | Boolean | Can players climb over this wall |
| Blocks LoS | Boolean | Blocks line of sight (glass walls do not, solid walls do) |
| Blocks Projectiles | Boolean | Blocks ranged attacks (fences do not, solid walls do) |
| Blocks Sound | Boolean | Affects proximity chat range |

---

### Wall Decoration

Decorations applied per wall segment.

| Decoration | Description |
|---|---|
| Trim/Molding | Top trim (crown molding), bottom trim (baseboard), mid trim (chair rail). Style and color configurable |
| Wallpaper/Paint | Wall surface color or pattern. Per-wall-segment coloring |
| Hanging Items | Paintings, shields, torches, banners, clocks, mounted heads |
| Shelves | Wall-mounted shelves with item display slots |
| Sconces/Lighting | Wall-mounted light sources with radius and color |
| Signs | Text signs on exterior walls (shop signs, warnings, directions) |
| Windows | Cut-through openings (see Windows section) |
| Damage/Wear | Cracks, moss, weathering for aged appearance |

---

### Wall Tools

Tools available in the world builder for placing and editing walls.

| Tool | Description |
|---|---|
| Single Wall | Click two points, wall placed on edge between them |
| Wall Line | Drag to draw a straight wall across multiple tiles |
| Wall Rectangle | Drag to draw a room outline (four walls) |
| Room Tool | Drag to create enclosed room with auto-walls on all edges |
| Wall Eraser | Remove wall segments by clicking |
| Wall Type Picker | Change material or type of existing wall |
| Copy Wall Style | Eyedropper to sample and apply wall properties |

---

## 2. Door System

### Door Data Model

Doors use the same edge position system as walls.

| Column | Type | Description |
|---|---|---|
| Position | String | Same as wall key: layer_x_y + edge |
| Door Type | Enum | Single, double, gate, portcullis, trapdoor, ladder, staircase, archway, custom |
| Material | String | Wood, metal, stone, magic, custom |
| Open State | Boolean | Whether the door is currently open or closed |
| Locked | Boolean | Whether the door is locked |
| Key Item | Integer | Item ID required to unlock (links to items.md) |
| Lockpickable | Boolean | Whether the lock can be picked |
| Lock Difficulty | Integer | Skill level required to pick the lock |
| Auto-Close Timer | Integer | Ticks before door automatically closes (null = stays open) |
| One-Way | Boolean | Can only be opened from one side |
| Quest-Locked | Integer | Quest ID required for access (links to quests.md) |
| Skill-Locked | String | Skill + level required to use (agility shortcut style) |
| Sound Effect | String | Sound played on open and close |

---

### Special Door Types

| Type | Description |
|---|---|
| Trapdoor | Vertical connection to the layer below |
| Ladder | Vertical connection to the layer above |
| Staircase | Multi-tile vertical connection with walk animation |
| Portal | Teleports to a configurable destination |
| Gate | Large double door, may require key or lever to operate |
| Drawbridge | Extends and retracts over water or gap |
| Secret Door | Hidden until discovered (requires search or quest) |
| Breakable Door | Can be destroyed (HP-based, like breakable walls) |

---

## 3. Window System

### Window Data Model

| Column | Type | Description |
|---|---|---|
| Position | String | Which wall edge the window is placed on |
| Window Type | Enum | Simple opening, glass pane, stained glass, arrow slit, bay window, skylight |
| Size | Enum | Small, medium, large, full-wall |
| Openable | Boolean | Can player open and close the window |
| Blocks LoS | Boolean | Always false -- windows always allow line of sight |
| Light Transmission | Boolean | Windows let light into rooms |
| Breakable | Boolean | Glass can shatter (HP-based) |
| Frame Material | String | Wood, stone, metal |

---

### Window Open State Effects

When a window is openable, the open/closed state changes behavior.

| Property | Open | Closed |
|---|---|---|
| Airflow | Yes (affects room temperature) | No |
| Sound Transmission | Full | Reduced |
| Projectile Passage | Yes | Blocked (unless breakable glass shatters) |

---

### Window Accessories

| Accessory | Description |
|---|---|
| Curtains/Blinds | Toggle to block light and line of sight |
| Shutters | Exterior panels that close over the window |
| Flower Box | Decorative planter on the window ledge |
| Bars | Iron bars over the window (blocks entry) |

---

## 4. Room System

### Room Detection

Rooms are detected automatically using flood-fill from wall and door edges. When walls fully enclose an area, it is identified as a room and assigned a room ID. Detection runs per layer.

---

### Room Data Model

| Column | Type | Description |
|---|---|---|
| Room ID | Integer | Unique room identifier (auto-assigned on detection) |
| Room Name | String | Display name (DM or player assigns) |
| Room Type | Enum | Functional type that affects allowed content and NPC behavior |
| Floor Material | String | Override terrain tiles inside the room |
| Ceiling Height | Float | Distance above floor to ceiling |
| Ceiling Material | String | Wood planks, stone, plaster, exposed beams, custom |
| Lighting | Float | Ambient light level inside the room |
| Temperature | Float | Affects player if survival toggle is on |
| Music Override | String | Different music track inside this room |
| Instanced | Boolean | Private room copy per player or party |
| Capacity | Integer | Maximum players allowed in the room (null = unlimited) |
| Indoor | Boolean | Protected from weather, different ambient sounds |

---

### Room Types

Each room type determines what can be placed inside.

| Type | Allowed Content |
|---|---|
| General | Any furniture, NPCs, items |
| Shop | Shop counter, display cases, NPC shopkeeper |
| Bank | Bank booth, deposit box, banker NPC |
| Workshop | Crafting stations, anvil, furnace, workbench |
| Kitchen | Range, sink, larder, table |
| Bedroom | Bed, wardrobe, dresser (rest mechanic) |
| Chapel | Altar, prayer items, holy symbol |
| Combat | Training dummies, weapon racks |
| Dungeon | Traps, puzzles, monsters |
| Throne | Throne, guards, audience chamber |
| Storage | Crates, barrels, chests, shelves |
| Custom | DM defines allowed content |

---

## 5. Ceiling and Roof

### Ceiling Properties

Ceilings are auto-generated from room bounds when the toggle is enabled.

| Column | Type | Description |
|---|---|---|
| Auto-Generated | Boolean | Whether ceiling is created from room bounds |
| Height | Float | Distance above floor |
| Material | String | Wood planks, stone, plaster, exposed beams, custom |
| Visible From Inside | Boolean | Whether the ceiling renders when camera is inside the room |
| Skylight | Boolean | Window in ceiling that lets light in |

---

### Roof Properties

Roofs are the exterior top of a building, visible from outside.

| Column | Type | Description |
|---|---|---|
| Roof Type | Enum | Gabled, hipped, half-gabled, flat |
| Pitch | Float | Angle of roof slope |
| Eave Length | Float | Overhang distance past the walls |
| Rotation | Enum | Which direction the roof ridge faces |
| Material | String | Thatch, wood shingle, slate, tile, metal |
| Color | String | Hex color for roof material |
| Chimney | Boolean | Whether a chimney is present |
| Chimney Position | String | Where the chimney sits on the roof |
| Auto-Hide | Boolean | Roof disappears when camera is inside the building |

---

## 6. Lighting

### Light Source Data Model

| Column | Type | Description |
|---|---|---|
| Light ID | Integer | Unique light source identifier |
| Type | Enum | Torch, candle, lantern, chandelier, fireplace, magic orb, window light, ambient |
| Position | Enum | Wall-mounted, ceiling, floor, held |
| Radius | Integer | Tiles of light emitted |
| Color | String | Light color (warm, cool, hex value) |
| Intensity | Enum | Bright, dim, flickering |
| Flickering | Boolean | Candle/torch-style flicker animation |
| Day/Night Behavior | Enum | Auto-on at night, always on, manual toggle |
| Fuel Item | Integer | Item ID required to keep the light burning (links to items.md, null = no fuel) |

---

### Darkness Mechanic

When the darkness toggle is enabled, areas without a light source are completely dark.

| Rule | Description |
|---|---|
| No light source | Player screen is dark, cannot see surroundings |
| Random damage | Player takes random damage while in darkness (optional) |
| Light radius | Only area within light source radius is visible |
| Portable light | Player can carry light sources (lantern, torch) |
| Light fuel | Carried light sources consume fuel over time (optional) |

---

## 7. Interior Design

### Floor Elements

Placed on the floor inside rooms. Links to player-housing.md furniture system.

| Element | Description |
|---|---|
| Rugs/Carpets | Rectangular, circular, or custom shape floor coverings |
| Floor Patterns | Checkerboard, herringbone, or custom tile patterns |
| Planters/Pots | Decorative plants placed on the floor |
| Floor Items | Vases, statues, barrels, crates |

---

### Multi-Room Structures

Connected rooms form a building. Buildings group related rooms together.

| Property | Description |
|---|---|
| Building ID | Groups rooms into a single structure |
| Exterior Style | Shared material and color for all exterior walls |
| Entrance | Main door designation |
| Floors/Stories | Rooms stacked vertically via the layer system |
| Blueprint | Saveable building template for reuse |

---

## 8. Building Templates and Blueprints

Save and reuse complete building designs.

| Feature | Description |
|---|---|
| Save Template | Capture rooms, walls, doors, windows, and furniture as a template |
| Place Template | Stamp a pre-built structure into the world |
| Template Library | Community-shared building collection |
| Scale | Stretch template to different sizes (optional toggle) |
| Rotate | Place template at different orientations |
| Material Override | Place template but swap all materials |
| DM-Curated | Official building designs approved by the dungeon master |

---

## Views

Editor and debug views available in the world builder.

| View | Description |
|---|---|
| Wall Editor | Place, remove, and configure wall segments |
| Door Manager | List and configure all doors in the world |
| Room List | Detected rooms with their properties and types |
| Window Placement | Place and configure windows on wall segments |
| Lighting Preview | Visualize light source radius and color |
| Blueprint Library | Browse and place saved building templates |
| Building Inspector | Click a building to see all rooms, doors, and walls |
| Floor Plan View | 2D top-down view of building layout |
| Cross-Section View | Vertical slice through a building showing all floors |
