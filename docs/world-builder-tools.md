# World Builder Tools

The complete editor suite for building game worlds. Inspired by Sims 4's build mode -- the gold standard of building UX -- adapted and expanded for an MMO world builder at a much grander scale.

---

## 1. Universal Tools (always available)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Select | V | Click to select any placed object, wall, floor, or element. Shows property panel |
| Copy | Ctrl+C | Duplicate selected element(s) |
| Paste | Ctrl+V | Place copied element(s) |
| Rotate | R / Mouse wheel | Rotate selected object before or after placement |
| Color/Tint | C | Change the color of selected element (opens color picker) |
| Remove/Delete | Del | Delete selected element |
| Undo | Ctrl+Z | Reverse last action (unlimited history) |
| Redo | Ctrl+Y | Re-apply reversed action |
| Save to Library | Ctrl+S | Save current selection or lot as reusable template |
| Move Building | M | Grab and reposition entire building/lot on the world map |
| Bulldoze Lot | | Clear all objects, walls, floors from a lot (keeps terrain) |
| Bulldoze Terrain | | Flatten terrain on lot back to level 0 |
| Eyedropper | Alt+Click | Sample the properties of clicked element (copies style/color/type for next placement) |
| Multi-Select | Shift+Click | Select multiple elements for batch operations |
| Ruler/Measure | | Measure distance between two points in tiles |

---

## 2. Terrain Tools

### Terrain Painting
Paint surface textures onto tiles using a spray/brush system.

| Subcategory | Examples |
|-------------|---------|
| Grass and Ground Cover | Short grass, tall grass, meadow, clover, moss, leaf litter |
| Flowers | Wildflower patches, daisy clusters, poppy fields, lavender |
| Stone and Pavement | Cobblestone, flagstone, brick path, gravel, marble, slate |
| Dirt and Sand | Packed earth, loose dirt, sand, mud, clay |
| Snow and Ice | Snow cover, frost, ice, packed snow |
| Custom | DM-defined terrain textures |

### Brush Settings
| Setting | Range | Description |
|---------|-------|-------------|
| Brush Size | 1-20 tiles | Radius of the painting area |
| Brush Shape | Circle, Square, Custom | Shape of the brush footprint |
| Opacity/Density | 0-100% | How much texture is applied per stroke (spray density) |
| Randomize | On/Off | Slight random variation in placement for natural look |

### Elevation Tools
| Tool | Description |
|------|-------------|
| Raise Terrain | Creates a smooth conical hill upward. Click and hold to raise higher |
| Lower Terrain | Creates a depression/valley downward. Click and hold to lower deeper |
| Smooth Terrain | Averages the elevation of all tiles within the brush radius. Removes harsh edges |
| Flatten Terrain | Brings terrain back to a target height (default 0). Click a reference point then paint to match that height |

### Elevation Settings
| Setting | Range | Description |
|---------|-------|-------------|
| Brush Size | 1-20 tiles | Radius of the elevation tool |
| Softness | 1-10 | How gradual the slope is. 1 = sharp cliff, 10 = gentle rolling hill |
| Speed | 1-10 | How fast the terrain raises/lowers per tick while holding. 1 = subtle, 10 = dramatic |
| Height Limit | -100 to 100 | Maximum elevation above/below base level |

---

## 3. Water Tools

### Pool Tool
Select a shape (rectangle, circle, custom), tool lowers the terrain within the shape and applies a water surface texture. Adjustable depth. Edge trim around the perimeter with style variants.

### Fountain Tool
Same as pool but RAISED instead of lowered. Creates an elevated water basin. Adjustable height.

### Water Decoration
| Subcategory | Examples |
|-------------|---------|
| Water Surface | Lily pads, floating candles, floating flowers, foam, ripple effects |
| Water Floor | Pebbles, sand, coral, seaweed, treasure, sunken objects |
| Water Edge | Rocks, reeds, cattails, wooden edging, stone trim, plants |
| Water Structures | Fountains (jet, tiered, statue), waterfalls, water wheels, bridges |
| Water Life | Ducks, fish (koi, goldfish, tropical), frogs, turtles, swans, gators |
| Water Texture | Clear blue, murky green, crystal, lava (red/orange), poison (purple), enchanted (glowing) |

### Water Properties
| Property | Type | Description |
|----------|------|-------------|
| Depth | Float | How deep the water is |
| Swimable | Boolean | Can players swim in it |
| Damage | Boolean | Does it hurt (lava, acid, poison) |
| Fishing spot | Boolean | Can fish here |
| Current | Direction + Speed | Water flows in a direction |
| Sound | Sound ID | Water ambience (calm, rushing, bubbling) |
| Transparency | Float | How clear the water is |

---

## 4. Structure Tools

### Wall Tools
| Tool | Description |
|------|-------------|
| Wall Tool | Click two points to place a single wall segment on the edge between tiles |
| Wall Line | Drag to draw a straight wall of any length |
| Wall Rectangle | Drag to draw a rectangular room outline (4 walls) |
| Room Tool | Drag to create an enclosed room with auto-walls. Triggers room detection for ceiling/floor |
| Custom Room Tool | Draw freeform room shape by clicking vertices. Close shape to complete room |
| Diagonal Wall | Place walls at 45-degree angles to break up the grid look |
| Curved Wall | Place curved/arc walls (supports curved-wall-compatible doors and windows) |

### Room Shapes (presets)
| Shape | Description |
|-------|-------------|
| Rectangle | Standard 4-wall room |
| L-Shape | Two rectangles joined at corner |
| T-Shape | Three-way intersection shape |
| U-Shape | Open on one side |
| Octagonal | 8-sided room |
| Circular | Approximated circle from diagonal segments |
| Diagonal Rectangle | Rotated 45 degrees from grid |
| Custom | Freeform vertex placement |

### Half Walls
14 height variants from tiny (knee height) to tall (near ceiling). Each functions as a wall but doesn't reach the ceiling. Can be trimmed. Creates visual separation without fully enclosing. Height presets: Railing (0.3), Quarter (0.5), Counter (0.7), Half (1.0), Three-Quarter (1.5), Tall (2.0), Near-Full (2.5), plus intermediate sizes.

### Platform Tool
Raised floor section within a room. Adjustable height. Creates steps/stages/split-levels. Can be trimmed along edges. Connects to main floor via auto-generated step.

### Basement Tool
Room that extends downward into the terrain. Same room options but generates below ground level. Requires stairs/ladder to access from above.

### Deck Tool
Room that uses fences instead of walls. No ceiling generated. Creates outdoor living spaces (porches, patios, verandas). Flat deck variant = just a floor with no walls or fences at all.

### Foundation
The visible base of a raised building -- the wall between ground and floor level. Has its own texture library:
| Material | Examples |
|----------|---------|
| Stone | Cobblestone, rough stone, cut stone, river rock |
| Brick | Red brick, brown brick, painted brick |
| Concrete | Smooth, exposed aggregate, stamped |
| Wood | Lattice, board, log |
| Stucco | Smooth plaster, textured |

---

## 5. Opening Tools (Doors, Windows, Arches)

### Door Tool
Place doors on walls. Doors auto-size to wall type.

| Filter | Options |
|--------|---------|
| Wall Type | Regular wall doors, Curved wall doors |
| Size | Short, Medium, Tall, Double-wide |
| Style | Hundreds of style variants per category |

### Window Tool
Place windows on walls. Same filtering as doors.

| Filter | Options |
|--------|---------|
| Wall Type | Regular wall, Curved wall |
| Size | Short, Medium, Tall, Full-height (door-window) |
| Style | Hundreds of variants |

### Gate Tool
Place gates on fences (not walls). Gates are outdoor doors.
Styles pair with fence types (iron gate with iron fence, wood gate with wood fence).

### Archway Tool
Open passage through a wall -- no door, just a decorative frame. Multiple arch profiles (round, pointed, flat, ogee).

---

## 6. Vertical Tools (Stairs, Ladders, Columns)

### Stair Tool
| Feature | Description |
|---------|-------------|
| Straight | Single flight up or down |
| L-Turn | Flight with 90-degree landing turn |
| U-Turn | Switchback with 180-degree turn |
| Spiral | Circular around central column |
| Curved | Sweeping curved grand staircase |
| Width | 1-tile, 2-tile, 3-tile wide |
| Styles | Dozens of visual styles |
| Handrail Tool | Add/remove/style handrails on either or both sides. Multiple profiles. Trim for stairs |

### Ladder Tool
Vertical ladders placed against walls. Simple -- style variants only.

### Column Tool
Free-standing or structural columns. Connect with spandrels (decorative panels between columns). Multiple orders: Doric, Ionic, Corinthian, Tuscan, Twisted, Square, Custom. Support gates and create outdoor covered structures (colonnades, pergolas, arcades).

---

## 7. Surface Tools (Textures and Patterns)

### Wall Surfaces
| Category | Description |
|----------|-------------|
| Paint | Solid colors with optional trim accent. Hundreds of colors |
| Wallpaper | Repeating decorative patterns (floral, geometric, stripe, damask) |
| Wainscoting/Moulding | Split wall -- lower panel + upper surface. Adjustable split height |
| Tile | Ceramic, porcelain, mosaic, subway tile. Grid/brick/herringbone layouts |
| Paneling | Wood panels (shaker, raised, flat, beadboard) |
| Masonry | Exposed brick (red, white, brown), stone (fieldstone, river rock, cut) |
| Siding (exterior) | Clapboard, board-and-batten, shingle, stucco, panel, mixed |
| Glass Walls | Full glass in frame. Variants: clear, tinted, frosted, ornamental, hexagonal, multi-panel |

Wall surface property: `{ upper: texture_id, lower: texture_id, split_height: float, split_trim: trim_id }`

### Floor Surfaces
| Category | Examples |
|----------|---------|
| Hardwood | Oak, walnut, cherry, bamboo, maple, reclaimed, parquet, herringbone, chevron |
| Carpet | Plush, berber, shag, patterned, solid colors |
| Tile | Ceramic, porcelain, terra cotta, mosaic, marble, slate, hexagonal |
| Stone | Flagstone, cobblestone, limestone, granite, travertine, sandstone |
| Masonry | Brick (running bond, herringbone, basketweave) |
| Linoleum | Retro patterns, solid, faux-wood, faux-tile |
| Metal | Grating, diamond plate, riveted panels, brushed steel |
| Outdoor | Patio stone, deck boards, gravel, stamped concrete |
| Misc | Custom patterns, themed floors, magical glowing, glass floor |

### Roof Surfaces
| Category | Examples |
|----------|---------|
| Shingle | Asphalt, wood shake, scalloped, weathered |
| Tile | Clay (terracotta), concrete, slate, flat tile |
| Metal | Standing seam, corrugated, copper (patina), tin |
| Thatch | Straw, reed, palm frond |
| Wood | Plank, shingle, bark |
| Glass | Greenhouse panels, skylight sections |
| Living | Green roof with vegetation |
| Specialty | Solar panels, crystal, bone, ice |

### Roof Trim Profiles
| Profile | Description |
|---------|-------------|
| Angled | Simple angled overhang |
| Beveled | Chamfered edge trim |
| Square | Clean right-angle edge |
| Stepped | Stair-stepped profile |
| Angled Out | Flared outward overhang |
| Chamfer | 45-degree cut edge |
| Beveled Out | Outward-angled bevel |
| Ornate | Decorative carved profile |
All with color picker.

---

## 8. Roof Tools

### Roof Shapes
| Shape | Description |
|-------|-------------|
| Gabled | Two sloping sides meeting at ridge |
| Hipped | Slopes on all four sides |
| Half-Gabled | Gabled with clipped/hipped end |
| Hexagonal | Six-sided roof for hex/round rooms |
| Half-Hipped | Hipped with small gable at top |
| Mansard | Four sides, each with two slopes |
| Gambrel | Barn-style, two slopes per side |
| Shed | Single slope lean-to |
| Flat | Level roof (usable as terrace/deck) |
| Dome | Hemispherical |
| Conical | Cone shape for towers |
| Butterfly | V-shape inverted |
| Sawtooth | Repeating ridge-valley (industrial) |

### Roof Properties
| Property | Type | Description |
|----------|------|-------------|
| Pitch | Slider | Angle of slope |
| Overhang | Slider | How far roof extends past walls |
| Height | Slider | Ridge height |
| Surface | Texture | Roof material/pattern |
| Trim | Profile | Edge trim style + color |
| Color | Hex | Trim and accent color |

### Roof Sculptures (objects placed ON the roof)
Chimneys (brick, stone, metal, ornate, industrial), Vents (bathroom, kitchen, attic), Skylights (flat, domed, pyramid), Dormers (gabled, hipped, shed), Weather vanes, Antennas/aerials, AC units, Solar panels, Gargoyles, Roof gardens/planters, Lightning rods, Satellite dishes, Cupolas, Bell towers, Rooftop doors/hatches, Vines/moss (decorative weathering)

---

## 9. Exterior Tools

### Fence Tool
Outdoor wall that doesn't create rooms or ceilings. Pairs with gate tool.
Materials: Wood (picket, split rail, post-and-rail, privacy), Metal (wrought iron, chain link, aluminum), Stone (dry stack, mortared, brick), Hedge (boxwood, privet, tall), Bamboo, Rope, Energy/magic barrier.

### Frieze Tool
Decorative band along the top exterior of walls (between wall top and roof). Stylized molding that adds character to the building exterior.

### Exterior Trim Tool
Protruding ledges, beveled edges, corner trim, window surrounds, door surrounds on the outside of buildings. Makes plain boxes look architectural.

### Foundation Tool
Texture/style for the visible base of raised buildings (between ground and floor).

### Column + Spandrel Tool
Columns with decorative spanning panels between them. Creates arcades, pergolas, covered walkways, grand entrances.

---

## 10. Outdoor Plant Tools

Dedicated tool (not generic object placement) for natural objects. Brush-like placement for clusters.

| Subcategory | Description |
|-------------|-------------|
| Trees | Deciduous, evergreen, tropical, fantasy. Multiple sizes per species. Seasonal variants |
| Shrubs | Shaped/trimmed, natural, cactus, hedge walls, topiary |
| Flowers | Individual stems, flower bushes, flower beds, flower trees, hanging baskets, wildflower patches |
| Rocks | Boulders, rock clusters, decorative stones, river rocks, crystal formations, carved stones |
| Ground Cover | Moss patches, fern clusters, ivy spreads, mulch, gravel beds |

### Plant Placement Modes
| Mode | Description |
|------|-------------|
| Single | Place one plant at a time |
| Brush | Paint clusters of plants with randomized placement within radius |
| Row/Line | Plant in a straight line with configurable spacing |
| Border | Plant along a path or fence automatically |
| Fill | Fill an area with randomized plants of selected types |

---

## 11. Object Palettes

Objects are browseable two ways: **by Room** (context) and **by Function** (purpose). Same objects, different organization -- like looking at a library by genre vs by author.

### By Room (contextual palette)
Each room preset shows only objects appropriate for that space.

| Room | Object Categories |
|------|------------------|
| Kitchen | Cabinets, stoves/ovens, counters, refrigerators, sinks, kitchen appliances, misc decorations, window coverings, lights, indoor plants, barstools, trash cans |
| Bathroom | Toilets, sinks, mirrors, showers, tubs, counters, lights, indoor plants, accents, rugs, paintings/posters |
| Bedroom | Beds, dressers, end tables, mirrors, clocks, lights, paintings, window coverings, indoor plants, ceiling decorations |
| Living Room | Sofas/loveseats, living chairs, coffee tables, accent tables, rugs, television, audio system, fireplaces, sculptures, paintings/posters, indoor plants, lights, window coverings |
| Dining Room | Dining table, dining chairs, bars/bar backs, barstools, lights, indoor plants, misc decorations, wall decorations, paintings/posters, window coverings |
| Study/Office | Desks, desk chairs, computers, bookshelves, end tables, rugs, lights, paintings/posters, window coverings, hobbies and skill items, indoor activities |
| Kids Room | Kids beds, kids chairs, kids desks, instruments, kids decorations, infant toys, stuffed animals, rugs, lights, paintings, clutter, window coverings |
| Outdoors | Outdoor furniture, outdoor cooking, outdoor activities, outdoor lights, sculptures, wall decorations, water decorations, trash cans/mailboxes, life event activities |

### Styled Rooms (pre-built room templates)
Pre-furnished rooms with matching aesthetics. Pick a style, get a complete room.

| Style Category | Examples |
|----------------|---------|
| Modern | Clean lines, neutral colors, minimal |
| Rustic | Wood, warm tones, cozy, handmade feel |
| Traditional | Classic furniture, rich colors, ornate |
| Industrial | Metal, exposed brick, raw |
| Bohemian | Colorful, eclectic, plants, textiles |
| Coastal | Blue/white, beach wood, nautical |
| Gothic | Dark wood, dramatic, ornate |
| Medieval | Stone, torches, heavy wood |
| Elven | Organic, flowing, nature-integrated |
| Dwarven | Stone, metal, geometric, sturdy |
| Futuristic | Sleek, glowing, minimal, tech |
| Custom | DM-defined style packages |

Each styled room is a template: walls + floor + ceiling + furniture + decorations all pre-placed. DM can stamp it, then customize.

### By Function (purpose palette)
Same objects organized by what they DO, not where they go.

| Function | Categories |
|----------|-----------|
| Comfort | Living chairs, dining chairs, desk chairs, sofas, loveseats, beds, outdoor seating, benches, misc seats |
| Surfaces | Coffee tables, dining tables, accent tables, displays, desks, counters, cabinets, shelves, misc surfaces |
| Plumbing | Sinks, toilets, showers, tubs, fountains, wells, water pumps |
| Activities and Skills | Knowledge items, creative items, recreation, active items, outdoor activities, life event items, skill training objects, crafting stations |
| Decorations | Indoor plants, paintings/posters, mirrors, window coverings, sculptures, ceiling decorations, rugs, fireplaces, wall decorations, clutter, misc |
| Kids | Kids decorations, kids furniture, toys, kids activities, infant/toddler items |
| Lighting | Table lamps, floor lamps, outdoor lights, wall lights, ceiling lights, chandeliers, candles, torches, misc/fun lights |
| Electronics | Televisions, computers, audio systems, alarms, misc electronics, communication devices |
| Appliances | Refrigerators, stoves/ovens, kitchen appliances, outdoor cooking, misc (trash cans, vending machines, fans) |
| Storage | Bookshelves, dressers, wardrobes, chests, lockers, cabinets, racks, shelving, shoe racks, bulletin boards |
| Off-the-Grid | Non-powered versions: campfire cooking, candle lighting, manual plumbing (well, bucket), crafting without electricity, survival items |
| Water Features | Open water objects, fountain structures, water emitters, pool objects, pond effects, water styles/textures |

### Object Filters
| Filter | Description |
|--------|-------------|
| By Style | Filter objects that match an architectural style (Modern, Rustic, Gothic, Medieval, etc.) |
| By Color | Filter objects by primary color family |
| By Pack/Set | Filter by content pack or themed set |
| By Material | Filter by primary material (wood, metal, stone, fabric, glass) |
| By Size | Filter by footprint (1x1, 1x2, 2x2, 3x3, etc.) |
| By Price | Filter by in-game value/cost |
| Search | Text search by name or keyword |
| Favorites | Player-bookmarked objects for quick access |
| Recently Used | Last 20 objects placed |

---

## 12. Object Placement Controls

### Placement Modes
| Mode | Description |
|------|-------------|
| Grid Snap | Object snaps to tile grid (default) |
| Half-Grid | Object snaps to half-tile increments |
| Free Placement | Object placed anywhere, no grid (hold Alt) |
| Wall Snap | Object snaps to wall surface (wall hangings, shelves) |
| Ceiling Snap | Object snaps to ceiling (chandeliers, hanging plants) |
| Surface Snap | Object snaps to top of another object (items on tables, shelves) |
| Edge Snap | Object snaps to edge of surface (counter edge, shelf edge) |

### Manipulation
| Action | Control | Description |
|--------|---------|-------------|
| Move | Click + Drag | Reposition placed object |
| Rotate | R or Mouse Wheel | Rotate in 45-degree increments (or free rotate with Alt) |
| Raise/Lower | 9 / 0 keys | Adjust vertical position (for wall objects and shelf items) |
| Scale | Shift + Mouse Wheel | Resize object (if DM allows -- toggle) |
| Lock | L | Lock object in place (prevents accidental moves) |
| Group | Ctrl+G | Group multiple objects (move/copy as unit) |
| Ungroup | Ctrl+Shift+G | Break group back to individual objects |
| Align | A | Align selected objects (left, center, right, distribute evenly) |

---

## 13. Wall Decoration Tool

The wall surface is its own 2D grid. Objects are placed ON the wall just like floor objects are placed on terrain -- but in a vertical plane.

### Wall Object Categories
| Category | Examples |
|----------|---------|
| Paintings/Art | Small, medium, large paintings. Portraits, landscapes, abstract, maps, tapestries |
| Mirrors | Small, medium, large, full-length, ornate, simple |
| Shelves | Floating shelves, bracket shelves, corner shelves (with placeable items on top) |
| Mounted Items | Shields, weapons, antlers, mounted fish, trophy heads, clocks |
| Sconces/Lights | Wall candles, electric sconces, lanterns, picture lights |
| Hanging Plants | Wall-mounted planters, trailing vines, moss frames |
| Signs | Plaques, house numbers, directional signs, menu boards |
| Window Coverings | Curtains, blinds, shutters, valances (placed on windows) |
| Trim/Molding | Crown molding, chair rail, wainscoting panels |
| Coat Hooks | Hooks, racks, pegs for hanging items |
| Bulletin Boards | Pin boards, cork boards, message boards |
| Porch Covers | Awnings, window roofs, sun shades |

### Wall Placement Grid
Wall objects have a vertical grid (like a 2D canvas on the wall face). Objects can be:
- Moved up/down and left/right along the wall
- Centered, left-aligned, or right-aligned
- Stacked vertically (shelf above shelf)
- Placed at any height within the wall bounds

---

## 14. Lot System

A lot is a named rectangular boundary that defines "this is one thing" -- a house, a shop, a park. Everything inside the boundary belongs together.

### Lot Properties
| Property | Type | Description |
|----------|------|-------------|
| Lot ID | Integer | Unique identifier |
| Name | String | "Bob's House", "Market Square", "Town Hall" |
| Bounds | Rectangle | x1,y1 to x2,y2 on a layer |
| Color | Hex | Visual tint on minimap/editor view |
| Type | Enum | Residential, Commercial, Industrial, Park, Farm, Civic, Military, Religious, Custom, Unzoned |
| Owner | Entity | Player, NPC, Clan, DM, Unowned |
| Locked | Boolean | Can't edit without permission |
| Label Visible | Boolean | Show name floating above lot |
| Template | Template ID | If this lot was stamped from a template |
| Value | Integer | In-game property value (for economy) |
| Rent | Integer | Recurring cost to occupy (if rental system enabled) |
| Size Category | Enum | Tiny (4x4), Small (8x8), Medium (16x16), Large (32x32), Mansion (64x64), Custom |

### Lot Operations
| Operation | Description |
|-----------|-------------|
| Create Lot | Draw rectangle to define boundary |
| Resize Lot | Drag edges to expand/shrink |
| Move Lot | Relocate lot (moves all contents with it) |
| Save to Library | Save lot as reusable template |
| Load from Library | Stamp a saved lot template |
| Bulldoze Lot | Clear all objects/walls/floors (keeps terrain) |
| Bulldoze Terrain | Flatten terrain within lot |
| Transfer Ownership | Change who owns the lot |
| Set Permissions | Who can enter, who can edit |
| Merge Lots | Combine adjacent lots into one |
| Split Lot | Divide a lot into smaller lots |

---

## 15. Library and Templates

### Save System
- Save individual objects as favorites
- Save groups of objects as prefabs
- Save rooms (walls + floor + ceiling + furniture) as room templates
- Save entire lots (building + terrain + landscaping) as lot templates
- Save styled room packages (matching furniture sets)
- Name, tag, and categorize all saved items

### Template Library
| Feature | Description |
|---------|-------------|
| Browse | Grid view with thumbnails of all saved templates |
| Search | By name, tag, category, creator |
| Filter | By type (object, room, lot), by style, by size |
| Preview | 3D preview before placing |
| Share | Upload to community library |
| Download | Browse and download community templates |
| Rate | Rate community templates (stars) |
| Version | Templates track creation date and updates |

### Built-in Templates
Pre-built templates shipped with the game for quick world building:
- Starter buildings (house, shop, tavern, temple)
- Room presets per style (modern kitchen, medieval bedroom, elven study)
- Landscape patches (forest clearing, garden, rocky outcrop, pond)
- Settlement templates (hamlet, village, town -- from settlement-design.md)
- Functional buildings (smithy, bank, guild hall, arena)

---

## 16. NPC Placement

Tools for placing and configuring non-player characters in the world.

| Tool | Description |
|------|-------------|
| NPC Browser | Search NPCs by type, role, or faction |
| Drag to Place | Click an NPC in the browser, then click in the world to set spawn point |
| Wander Radius | Drag a circle around an NPC to set how far it roams |
| NPC Properties Panel | Configure name, dialogue, shop inventory, services, daily schedule |
| Monster Spawner | Place a spawn point with monster type, count, and respawn rate |
| Patrol Path | Draw waypoints that the NPC follows in sequence |
| Group Spawner | Place a cluster of same or mixed monsters at a spawn area |
| Boss Arena Setup | Define arena bounds, phase triggers, and mechanic zones |

---

## 17. Region Tools

Define zones with gameplay properties -- PvP rules, combat type, music, weather, access restrictions.

| Tool | Description |
|------|-------------|
| Area Select (Rectangle) | Select a rectangular zone |
| Area Select (Polygon) | Select a polygon zone by clicking vertices |
| Area Select (Freehand) | Draw a freehand zone boundary |
| Zone Properties Panel | Set name, PvP toggle, combat type, level bracket, weather, music, access requirements |
| Boundary Visualization | Colored outlines showing zone borders on the map |
| Copy Zone Properties | Apply one zone's settings to another zone |
| Transition Settings | Configure how zones blend visually and mechanically at borders |

---

## 18. Path Tools

Draw roads, rivers, fences, bridges with automatic connectivity.

| Tool | Description |
|------|-------------|
| Road Painter | Draw roads with automatic edge blending to surrounding terrain |
| River Tool | Draw water paths with configurable flow direction |
| Bridge Tool | Span gaps with a configurable bridge type |
| Trail Marker | Place waypoint signs along a path |
| Auto-Connect | Roads and trails automatically connect to nearby endpoints |

---

## 19. Lighting Mode

| Tool | Description |
|------|-------------|
| Light Source Placement | Place point lights, spotlights, ambient lights |
| Room Ambient | Set ambient light level and color per room |
| Area Ambient | Set ambient light level and color per outdoor area |
| Day/Night Preview | Toggle time of day to see lighting at different hours |
| Shadow Preview | See cast shadows from placed light sources |

---

## 20. Collaboration

### Multi-Builder Editing
| Feature | Description |
|---------|-------------|
| Region Lock | Claim an area -- other builders can't edit your zone |
| Change Tracking | See who changed what, when |
| Builder Chat | Private chat channel for builders only |
| Permission Levels | Junior (objects only), Standard (objects + walls), Senior (terrain + structures), Admin (everything) |
| Merge Changes | Combine edits from multiple builders working on adjacent areas |
| Conflict Resolution | Alert when two builders edit the same area simultaneously |

---

## 21. Testing and Preview

### Play Mode
Instantly switch from builder to player perspective. Walk through what you built. Test doors, stairs, pathfinding, NPC placement. Switch back to builder at any time. Changes in builder mode are live -- no compile/build step.

### Preview Tools
| Tool | Description |
|------|-------------|
| NPC Preview | See how NPCs behave in the space (walking, dialogue) |
| Pathfinding Test | Verify players can walk everywhere they should (and can't where they shouldn't) |
| Combat Test | Spawn test monsters to fight in the area |
| Lighting Preview | Toggle day/night to see how lighting works at different times |
| Performance Overlay | Shows tile count, object count, entity count, estimated client load |
| First Person Walk | Walk through in first person to check scale and feel |
| Minimap Preview | See how the area looks on the minimap |
| Screenshot | Capture current view for sharing |

---

## 22. Conditional Tool Availability

Some tools only appear when prerequisites are met -- reduces clutter and prevents errors.

| Tool | Requires |
|------|----------|
| Wall Trim | Wall must exist first |
| Stair Handrail | Stairs must exist first |
| Gate | Fence must exist first |
| Door/Window | Wall must exist first |
| Roof | Room must be enclosed first |
| Foundation Texture | Building must be raised above terrain |
| Wall Surface | Wall must exist first |
| Floor Surface | Floor/room must exist first |
| Ceiling | Room must be enclosed first |
| Platform Trim | Platform must exist first |
| Half Wall Trim | Half wall must exist first |
| Wall Decoration | Wall must exist first |
| Roof Trim | Roof must exist first |
| Roof Sculpture | Roof must exist first |

---

## 23. Item Requirements for Object Categories

For gameplay objects (not just decorative), items must meet criteria to appear in specific palettes:

| Category | Requirements |
|----------|-------------|
| Crafting Station | Must have: station_type property, at least one recipe that uses this station |
| Resource Node | Must have: skill_type, level_requirement, product_item_id, respawn_time |
| Shop Counter | Must link to a shop_id in shops.md |
| Storage Container | Must have: capacity, allowed_items list |
| Interactive Object | Must have: at least one action in actions[] array |
| Seating | Must have: sit_animation_id, player positions |
| Lighting | Must have: light_radius, light_color |
| Musical | Must have: sound_id or music_track_id |
| Teleporter | Must have: destination coordinates or portal_network_id |
| Quest Object | Must have: quest_id reference, interaction triggers quest step |

---

## Module Toggles

Each toggle enables or disables a subsystem within the world builder module. Servers can turn off features they do not need.

| Toggle | Default | Description |
|--------|---------|-------------|
| Terrain painting | On | Paint surface textures on tiles |
| Elevation tools | On | Raise, lower, smooth, flatten terrain |
| Elevation softness/speed | On | Fine control over elevation tool behavior |
| Water tools | On | Pool, fountain, water decoration |
| Wall tools | On | All wall placement tools |
| Diagonal walls | On | 45-degree wall placement |
| Curved walls | Off | Arc/curved wall support |
| Half walls | On | Partial height wall variants |
| Room presets | On | Pre-shaped room templates |
| Platform tool | On | Raised floor sections |
| Basement tool | On | Below-ground rooms |
| Deck tool | On | Fence-walled outdoor rooms |
| Door/window/gate tools | On | All opening placement tools |
| Stair tool | On | Stair placement with turns |
| Handrail tool | On | Stair handrail decoration |
| Column/spandrel tool | On | Column and arch panel placement |
| Wall surfaces | On | Paint, wallpaper, tile, masonry, siding, glass |
| Wainscoting (split wall) | On | Upper/lower wall surface split |
| Floor surfaces | On | All floor material options |
| Roof tools | On | All roof shape and surface options |
| Roof trim | On | Roof edge profiles |
| Roof sculptures | On | Objects placed on roofs |
| Frieze/exterior trim | On | Building exterior decoration |
| Foundation styles | On | Raised building base textures |
| Fence tool | On | Outdoor wall/boundary placement |
| Outdoor plant tools | On | Tree, shrub, flower, rock placement |
| Plant brush mode | On | Paint clusters of plants |
| Object by room palette | On | Room-based object filtering |
| Object by function palette | On | Function-based object filtering |
| Styled rooms | On | Pre-furnished room templates |
| Object filters | On | Style, color, material, size filters |
| Object free placement | On | Place objects off-grid |
| Object scaling | Off | Resize objects (can break aesthetic) |
| Wall decoration tool | On | 2D wall surface object placement |
| Lot system | On | Named property boundaries |
| Library/templates | On | Save and load reusable designs |
| Community sharing | On | Upload/download community templates |
| NPC placement | On | Place and configure NPCs and spawners |
| Region tools | On | Define zones with gameplay properties |
| Path tools | On | Draw roads, rivers, bridges |
| Lighting mode | On | Place and configure light sources |
| Collaboration | On | Multi-builder editing |
| Play mode testing | On | Switch to player perspective |
| Performance overlay | On | Show rendering load stats |
| Conditional tools | On | Hide tools until prerequisites met |
| Undo/redo | On | Action history (unlimited) |
| Eyedropper | On | Sample existing element properties |
| Procedural generation assist | Off | Noise generator and template stamp for terrain |

---

## Views

Editor and debug views available in the world builder.

| View | Description |
|------|-------------|
| Tool Palette | Organized tool categories with icons |
| Object Browser | Searchable catalog with thumbnails and filters |
| Property Panel | Edit selected element's properties |
| Template Library | Browse, search, preview saved templates |
| Layer Manager | Toggle layer visibility, select active layer |
| Minimap | Overview of current build area |
| Performance Monitor | Real-time tile/object/entity count |
| History Panel | Scrollable undo/redo history |
| Collaboration Panel | Who's editing where, change log |
| Style Guide | Which objects match current zone's architectural style |
| Region Map | Color-coded overview of all defined zones and boundaries |
| NPC Browser | Searchable catalog of all NPCs by type, role, and faction |
