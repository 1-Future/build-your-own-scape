# Minimap and World Map

Game design document for the Minimap and World Map plugin. This system defines how players navigate and understand the game world spatially. Two views: the minimap (always visible HUD element showing the immediate area) and the world map (full-screen overlay showing the entire world).

---

## 1. Minimap

The minimap is a persistent HUD element showing the player's immediate surroundings. It renders a simplified top-down view of the local area.

### Minimap Configuration

| Property | Options | Description |
|---|---|---|
| Position | Top-right, top-left, bottom-right, bottom-left | Screen corner placement. Configurable by player |
| Shape | Square, circle, diamond | Outer border shape of the minimap |
| Size | Small, medium, large, custom pixel value | Adjustable minimap dimensions |
| Rotation | Rotate with camera, fixed north-up | Whether the minimap rotates when the camera rotates or always points north |
| Zoom Level | 1x--5x | Adjustable zoom. Lower values show more area, higher values show more detail |
| Render Style | Match game, simplified, stylized | Match the game world aesthetic, use a clean simplified look, or use a custom stylized theme |
| Opacity | 0--100% | Transparency slider so the minimap does not obscure gameplay |
| Border Style | Default, minimal, ornate, custom | Visual frame around the minimap |

---

## 2. Minimap Icons

Every icon type rendered on the minimap. Each type is independently toggleable by the player.

### Entity Icons

| Icon | Color / Shape | Description |
|---|---|---|
| Player | White dot (centered) | The player's own position. Always centered on the minimap |
| Other Players | White dots | Other players in the area. Optional visibility toggle |
| Friends | Green dots | Players on the friend list |
| Clan Members | Blue dots | Members of the player's clan |
| Enemies / PvP Targets | Red dots | Hostile players in PvP zones |
| NPCs (Quest) | Yellow dot with exclamation | NPCs with available quests |
| NPCs (Shop) | Yellow dot with coin | NPCs that operate shops |
| NPCs (Bank) | Yellow dot with chest | Bank NPCs or bank booths |
| NPCs (Skill) | Yellow dot with star | Skill trainers and tutors |
| NPCs (General) | Yellow dots | All other NPCs |
| Monsters | Red dots | Hostile monsters in the area |

### Object Icons

| Icon | Symbol | Description |
|---|---|---|
| Bank | Chest icon | Bank booth or bank chest locations |
| Altar | Star icon | Prayer altars and recharge points |
| Furnace | Flame icon | Smelting furnaces |
| Anvil | Hammer icon | Smithing anvils |
| Range / Oven | Cooking pot icon | Cooking locations |
| Shop | Coin stack icon | General and specialty shops |
| Fairy Ring | Ring icon | Fairy ring teleport nodes |
| Spirit Tree | Tree icon | Spirit tree teleport nodes |
| Teleport Point | Swirl icon | Generic teleport destinations |
| Agility Shortcut | Arrow icon | Agility shortcut entrances |
| Mining Rock | Pickaxe icon | Mining locations |
| Fishing Spot | Fish icon | Fishing locations |
| Custom Marker | Pin icon (player color) | Player-placed markers |
| Quest Marker | Arrow or star | Active quest step destination |

---

## 3. Minimap Features

Interactive features and overlays on the minimap beyond basic icons.

| Feature | Description |
|---|---|
| Fog of War | Unexplored areas are dark or hidden on the minimap. Reveals as the player walks through new areas. Per-character or per-account persistence |
| Edge Indicators | Directional arrows at the minimap border pointing toward off-screen targets (quest objectives, party members, tracked NPCs) |
| Compass Rose | North indicator on the minimap edge. Always visible regardless of rotation mode |
| Coordinate Display | Current tile coordinates (x, y) shown below or on the minimap. Toggle visibility |
| Zone Name Display | Name of the current area displayed above or below the minimap |
| PvP Zone Boundary | Wilderness level lines and PvP zone borders drawn on the minimap |
| Multi-Combat Indicator | Visual indicator (icon or border tint) when standing in a multi-combat zone |
| Click-to-Move | Click a point on the minimap to walk the character to that location |
| Click-to-Ping | Alt-click the minimap to place a temporary ping visible to party members |
| Death Marker | Skull icon at the location of the player's most recent death |
| Minimap Lock | Lock the minimap to prevent accidental clicks or setting changes |

---

## 4. World Map

A full-screen map overlay showing the entire game world. Opened with a keybind (default: M). The player can continue to see their surroundings but gameplay input is paused or redirected to map interaction.

### World Map Layers

Each layer is independently toggleable.

| Layer | Description |
|---|---|
| Terrain Base | Ground colors derived from tile terrain types. Grass is green, sand is yellow, water is blue, stone is grey |
| Location Labels | Town names, region names, dungeon names, landmark names. Font size scales with zoom |
| Icon Layer | All minimap icon types rendered on the world map with the same toggle controls |
| Transport Overlay | All transportation nodes and routes: teleport destinations, fairy ring codes, boat routes, spirit tree network, agility shortcuts, minecart stations, portals |
| Quest Marker Layer | Quest start locations, active quest step markers, completed quest indicators |
| Player Position | Arrow icon showing the player's current position and facing direction |
| Clan Territory | Highlighted regions claimed by clans. Color-coded per clan |
| Zone Boundaries | PvP zones, level requirement zones, quest-locked zones. Color-coded borders with labels |
| Elevation Contours | Optional topographic lines showing height changes across the terrain |
| Dungeon Maps | Underground and instanced areas accessible via dropdown or layer toggle |

### World Map Controls

| Action | Input | Description |
|---|---|---|
| Pan | Click and drag, arrow keys, WASD | Move the map view |
| Zoom In | Scroll up, + key, pinch out | Zoom closer for more detail |
| Zoom Out | Scroll down, - key, pinch in | Zoom further for broader view |
| Search | Type in search bar | Search for a location, NPC, or object by name. Map jumps to the result |
| Place Pin | Right-click | Place a custom pin with optional label and color |
| Remove Pin | Right-click existing pin | Delete a custom marker |
| Coordinate Grid | Toggle | Overlay a tile coordinate grid on the map |
| Center on Player | Keybind or button | Snap the map view back to the player's current position |
| Close Map | M key, Escape, or close button | Return to gameplay |

---

## 5. Map Data

How the minimap and world map are generated from world data. Maps update automatically as the world is built and modified.

| Data Source | Map Rendering |
|---|---|
| Terrain Tiles | Each tile type maps to a color. Grass tiles render green, water tiles render blue, sand renders tan, stone renders grey. Color palette configurable |
| Wall Edges | Wall edge data renders as dark outlines on the map. Shows building footprints and barriers |
| Room Detection | Enclosed wall areas render as building interiors with a distinct fill color or roof pattern |
| Water Bodies | Contiguous water tiles render as lakes, rivers, or ocean with distinct blue shading |
| Elevation Data | Height values optionally render as contour lines or shaded relief |
| Biome Assignment | Biome metadata applies color tinting to regions (desert = warm, tundra = cool, swamp = muted) |
| Road Paths | Road tiles render as light-colored paths connecting locations |
| Chunk Boundaries | Optional debug overlay showing chunk grid lines |

### Manual Map Painting

| Feature | Description |
|---|---|
| Paint Override | DM can manually paint map colors over auto-generated tiles for a stylized look |
| Label Placement | DM can position and style location labels manually |
| Custom Icons | DM can place custom icons on the map with custom images |
| Map Art | DM can import a hand-drawn or artistic map as the base layer, with auto-generated data overlaid |

---

## 6. Map Sharing

Tools for sharing map data with others.

| Feature | Description |
|---|---|
| Screenshot Map | Capture the current map view as an image file |
| Export as Image | Export the entire world map as a high-resolution image (PNG or JPEG) |
| Share Map Link | Generate a URL that opens the map centered on a specific location. Configurable permissions |
| Public Map | The world map is viewable by anyone via the link, without logging in |
| Private Map | The world map is restricted to server members only |
| Embed Map | Embeddable map widget for external websites or Discord |
| Print Map | Export a print-friendly version with clean borders and legend |

---

## 7. OSRS-Style Map Overlay

Reference map overlay for building. This feature exists in the current OpenScape codebase and is preserved here as a design reference tool.

| Feature | Description |
|---|---|
| Overlay Image | Load an OSRS world map image as a semi-transparent overlay on the game view |
| Draggable | Click and drag the overlay to reposition it over the game world |
| Opacity | Adjustable transparency so it does not obscure the active build |
| Scale | Resize the overlay to match the tile scale of the current world |
| Lock Position | Lock the overlay in place to prevent accidental movement |
| Toggle Visibility | Keybind to show or hide the overlay instantly |
| Use Case | DMs use this as a reference while building worlds that replicate or are inspired by OSRS geography |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `map.minimap` | true | Show the minimap HUD element |
| `map.minimap_click_to_move` | true | Click minimap to walk to location |
| `map.minimap_rotation` | true | Minimap rotates with camera |
| `map.minimap_fog_of_war` | false | Unexplored areas hidden on minimap |
| `map.minimap_edge_indicators` | true | Arrows pointing to off-screen targets |
| `map.minimap_coordinates` | true | Show tile coordinates on minimap |
| `map.minimap_zone_name` | true | Show current zone name |
| `map.minimap_pvp_boundaries` | true | Show PvP zone borders on minimap |
| `map.minimap_multicombat` | true | Multi-combat zone indicator |
| `map.minimap_death_marker` | true | Show skull at last death location |
| `map.icon_players` | true | Show other player dots |
| `map.icon_friends` | true | Show friend dots in green |
| `map.icon_clan` | true | Show clan member dots in blue |
| `map.icon_enemies` | true | Show hostile player dots in red |
| `map.icon_npcs` | true | Show NPC dots |
| `map.icon_monsters` | true | Show monster dots |
| `map.icon_objects` | true | Show object icons (bank, altar, etc.) |
| `map.icon_quest_markers` | true | Show quest destination markers |
| `map.icon_custom_markers` | true | Allow player-placed pins |
| `map.worldmap` | true | Full-screen world map overlay |
| `map.worldmap_search` | true | Search bar on world map |
| `map.worldmap_transport` | true | Transport route overlay on world map |
| `map.worldmap_clan_territory` | false | Clan territory highlighting |
| `map.worldmap_zone_boundaries` | true | Zone border overlay |
| `map.worldmap_elevation` | false | Elevation contour lines |
| `map.worldmap_dungeons` | true | Underground and instance map layers |
| `map.worldmap_coordinate_grid` | false | Tile coordinate grid overlay |
| `map.worldmap_custom_pins` | true | Player-placed pins on world map |
| `map.map_sharing` | true | Map screenshot, export, and share features |
| `map.public_map` | false | Allow non-members to view the world map |
| `map.manual_paint` | true | DM can manually paint map tiles |
| `map.osrs_overlay` | true | OSRS reference map overlay for building |

---

## Views

| View | Description |
|---|---|
| Minimap Settings | Player-facing panel for minimap position, shape, size, rotation, zoom, and opacity |
| Minimap Icon Toggles | Checklist of all icon types with individual show/hide controls |
| World Map | Full-screen pannable, zoomable map with layer toggles and search |
| World Map Layer Panel | Toggle individual map layers on and off |
| Transport Overlay Panel | Filter transport routes by type (fairy ring, spirit tree, boat, teleport, shortcut) |
| Custom Pin Manager | List of all player-placed pins with edit, delete, and jump-to actions |
| Map Export Dialog | Options for exporting the map as image, link, or embed code |
| Map Paint Editor | DM tool for manually painting map tiles, placing labels, and adding custom icons |
| OSRS Overlay Settings | Controls for overlay image, position, opacity, scale, and lock |
