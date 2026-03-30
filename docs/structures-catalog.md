# Structures Catalog

Game design document for the Structures Catalog plugin. This system provides a comprehensive catalog of every type of structure, architectural element, and decorative object that can exist in the game world. This is the reference library for the world builder -- every category of thing that can be placed.

---

## Module Toggles

Each toggle enables or disables a subsystem within the structures catalog. DM configures which materials and styles exist in their world.

| Toggle | Description |
|---|---|
| Wall Materials | Which wall materials are available (stone, brick, wood, etc.) |
| Wall Types | Which wall types are available (full, half, fence, bars, etc.) |
| Door Types | Which door types are available (single, double, secret, puzzle, etc.) |
| Window Types | Which window types are available (glass pane, stained glass, arrow slit, etc.) |
| Arch and Column Types | Which architectural column and arch styles are available |
| Stair and Ladder Types | Which vertical movement structures are available |
| Roof Types | Which roof shapes and materials are available |
| Floor Types | Which floor surfaces are available |
| Seating Types | Which seating objects are available |
| Lighting Types | Which light sources are available |
| Decorative Objects | Which decorative items are available (wall, floor, ceiling, garden) |
| Functional Objects | Which crafting stations, storage, and interactive objects are available |
| Natural Objects | Which trees, rocks, plants, and water features are available |
| Structural Elements | Which bridges, fences, and gates are available |
| Custom Materials | DM can define new materials beyond the defaults |
| Material Availability Per Zone | Restrict which materials can be placed in specific zones (stone in mountains, wood in forests) |

---

## 1. Walls

### Wall Materials

| Material | Description | Visual |
|---|---|---|
| Stone | Rough-cut stone blocks, castle-style | Grey, textured |
| Brick | Fired clay bricks in running bond | Red/brown, uniform |
| Wood Plank | Horizontal wooden boards | Brown, grain visible |
| Wood Log | Stacked logs, cabin-style | Round profile, bark |
| Mud/Adobe | Sun-dried clay and straw mix | Tan/brown, rough |
| Marble | Polished stone, decorative | White/veined, smooth |
| Metal | Iron or steel plates or bars | Dark grey, riveted |
| Glass | Transparent panels in frame | Clear/tinted |
| Crystal | Magical crystalline structure | Translucent, glowing |
| Ice | Frozen water blocks | Blue-white, reflective |
| Bone | Constructed from bones | White/yellowed, irregular |
| Living | Grown from vines and roots | Green, organic |
| Sandstone | Desert carved stone | Yellow/tan, worn |
| Obsidian | Volcanic glass | Black, glossy |
| Coral | Underwater grown structure | Pink/orange, irregular |
| Thatch | Woven grass and straw panels | Golden, lightweight |
| Hide/Leather | Stretched animal hides on frame | Brown, tent-style |
| Paper/Screen | Thin translucent panels (Japanese style) | White, rice paper |
| Bamboo | Bundled bamboo stalks | Green/yellow, round |
| Ruined | Any material in decay state | Cracked, mossy, broken |

---

### Wall Types

| Type | Description |
|---|---|
| Full Wall | Floor to ceiling, blocks movement and line of sight |
| Half Wall | Waist height, blocks movement, allows line of sight |
| Fence | Waist height, blocks movement, gaps between slats |
| Railing | Hip height, blocks movement, open design |
| Hedge | Living plant wall, blocks movement |
| Bars/Cage | Metal bars, blocks movement, allows line of sight and projectiles |
| Curtain | Fabric hanging, does NOT block movement, blocks line of sight |
| Barrier | Temporary blockade (barricade, sandbags) |
| Invisible | No visual, blocks movement (for game logic only) |
| Decorative | Visible but does NOT block anything (visual only) |

---

## 2. Doors and Entrances

### Door Types

| Type | Description |
|---|---|
| Single Door | Standard one-panel door |
| Double Door | Two panels opening from center |
| Sliding Door | Slides left or right on track |
| Revolving Door | Rotating panels |
| Gate | Large metal or wood gate, often in walls or fences |
| Portcullis | Vertically sliding metal grate |
| Drawbridge | Lowers over moat or gap |
| Trapdoor | Floor-level, opens to layer below |
| Hatch | Ceiling-level, opens to layer above |
| Archway | Open passage, no actual door (decorative frame) |
| Curtain Doorway | Fabric covering opening |
| Beaded Curtain | Hanging bead strings |
| Secret Door | Disguised as wall, requires discovery |
| Magical Barrier | Energy field blocking passage |
| Toll Gate | Requires payment to pass |
| One-Way | Opens from one side only |
| Breakable | Can be destroyed to pass |
| Puzzle Door | Requires solving a mechanic to open |

---

### Door Materials

| Material | Variants |
|---|---|
| Wood | Pine, oak, mahogany, reinforced |
| Metal | Iron, steel, bronze, gold-trimmed |
| Stone | Carved, sliding |
| Crystal | Solid or translucent |
| Bone | Skeletal construction |
| Vine/Living | Grown organic barrier |
| Ice | Frozen water block |

---

## 3. Windows

### Window Types

| Type | Description |
|---|---|
| Simple Opening | Hole in wall, no glass |
| Glass Pane | Single sheet of glass in frame |
| Multi-Pane | Multiple small glass panels (classic style) |
| Stained Glass | Decorative colored glass (churches, palaces) |
| Arrow Slit | Narrow vertical slot (castles, fortifications) |
| Bay Window | Protruding window alcove |
| Skylight | Ceiling or roof window |
| Rose Window | Circular decorative window (cathedrals) |
| Porthole | Round ship or submarine style |
| Lattice | Diamond-pattern frame, no glass |
| Shutter Window | Solid panels that open and close |
| Barred Window | Window with metal bars (prison style) |
| Balcony Door | Full-height opening to balcony |

---

### Window Accessories

| Accessory | Description |
|---|---|
| Curtains | Open and close, block line of sight when closed |
| Shutters | Solid panels that open and close |
| Flower Box | Exterior decorative planter |
| Window Seat | Interior seating built into bay window |
| Bars | Security bars (can be combined with glass) |
| Blinds | Horizontal slat covering |

---

## 4. Arches and Columns

### Arch Types

| Type | Description |
|---|---|
| Round Arch | Semicircular (Roman style) |
| Pointed Arch | Gothic style |
| Horseshoe Arch | Moorish/Islamic style |
| Flat/Lintel | Straight horizontal top |
| Ogee Arch | S-curve ornamental |
| Trefoil Arch | Three-lobed decorative |
| Tudor Arch | Shallow pointed (English) |
| Catenary Arch | Natural hanging curve |
| Corbel Arch | Stepped stone overhang |

---

### Column Types

| Type | Description |
|---|---|
| Round Column | Cylindrical pillar |
| Square Pillar | Rectangular section |
| Fluted Column | Vertical grooves (Greek/Roman) |
| Twisted Column | Spiral or barley sugar twist |
| Cluster Column | Multiple shafts bundled together |
| Obelisk | Tapered four-sided monument |
| Totem Pole | Carved stacked figures |
| Caryatid | Column shaped as a human figure |
| Tree Trunk | Natural wood column |

---

### Column Parts

| Part | Description |
|---|---|
| Base (Plinth) | Bottom section the column rests on |
| Shaft | Main vertical body of the column |
| Capital | Top section (Doric, Ionic, Corinthian, Tuscan, Composite styles) |

---

## 5. Stairs and Ladders

### Stair Types

| Type | Description |
|---|---|
| Straight | Single flight up or down |
| L-Shaped | Turn at landing |
| U-Shaped | Switchback with landing |
| Spiral | Circular around central column |
| Curved | Sweeping curved flight |
| Floating | No visible support structure |
| Step Ladder | Portable leaning ladder |
| Fixed Ladder | Mounted vertically to wall |
| Rope Ladder | Hanging rope with wood rungs |
| Ramp | Inclined plane (wheelchair accessible, cart access) |
| Escalator | Moving stairs (modern/futuristic settings) |
| Elevator/Lift | Vertical platform (mechanical or magical) |

---

## 6. Roofs

### Roof Types

| Type | Description |
|---|---|
| Gabled | Two sloping sides meeting at ridge |
| Hipped | Slopes on all four sides |
| Half-Gabled | Gabled with clipped or hipped end |
| Flat | Level roof (usable as terrace) |
| Mansard | Four sides each with two slopes |
| Gambrel | Barn-style, two slopes per side |
| Shed | Single slope (lean-to) |
| Dome | Hemispherical |
| Conical | Cone shape (tower roof) |
| Pagoda | Multi-tiered curving |
| Thatched | Straw or reed covering |
| Green | Living roof with vegetation |
| Sawtooth | Repeated ridge-valley pattern (industrial) |

---

### Roof Materials

| Material | Description |
|---|---|
| Thatch | Straw or reed bundles |
| Wood Shingle | Overlapping wood pieces |
| Slate | Natural stone tiles |
| Clay Tile | Fired clay in overlapping rows |
| Metal Sheet | Flat or corrugated metal |
| Stone Slab | Heavy stone tiles |
| Living/Green | Soil and vegetation layer |
| Ice | Frozen water surface |
| Crystal | Translucent magical material |
| Bone | Skeletal construction |
| Glass | Transparent panels (greenhouse style) |

---

### Roof Elements

| Element | Description |
|---|---|
| Ridge | Top line where roof slopes meet |
| Eave | Overhang at roof edge |
| Gutter | Water channel along eave |
| Chimney | Smoke vent from fireplace or furnace |
| Dormer | Window protrusion from roof slope |
| Finial | Decorative ornament at roof peak |
| Weathervane | Directional wind indicator |
| Lightning Rod | Metal spike for electrical discharge |

---

## 7. Floors

### Floor Types

| Type | Description |
|---|---|
| Bare Earth | Dirt or packed ground |
| Wooden Plank | Boards in parallel |
| Parquet | Decorative wood patterns (herringbone, chevron, basketweave) |
| Stone Flag | Large flat stone slabs |
| Tile | Ceramic or porcelain tiles (square, hexagonal, mosaic) |
| Marble | Polished stone (checkerboard, veined) |
| Carpet/Rug | Fabric floor covering (placed over other floor types) |
| Brick | Patterned brick (herringbone, running bond) |
| Cobblestone | Rounded stones in mortar |
| Metal Grate | See-through metal grid |
| Glass | Transparent floor (see layer below) |
| Sand | Loose sand surface |
| Straw | Scattered straw over floor |
| Rush Mat | Woven reed covering |
| Enchanted | Glowing, patterned, magical surface |

---

## 8. Seating

### Seat Types

| Type | Description |
|---|---|
| Stool | Simple seat without back |
| Chair | Standard seat with back |
| Armchair | Chair with armrests |
| Bench | Multi-person backless seat |
| Pew | Church-style bench with back |
| Throne | Ornate high-backed seat |
| Sofa/Couch | Upholstered multi-seat |
| Log Seat | Fallen tree section |
| Rock Seat | Natural boulder |
| Floor Cushion | Seated on ground level |
| Hammock | Suspended fabric seat |
| Swing | Hanging seat on chains or rope |
| Bar Stool | Tall counter-height seat |
| Window Seat | Built into bay window |
| Rocking Chair | Curved base that rocks |

---

## 9. Lighting

### Light Source Types

| Type | Description |
|---|---|
| Torch (wall) | Wall-mounted flaming stick |
| Torch (standing) | Floor-standing torch holder |
| Candle | Single or multi candle on holder |
| Candelabra | Multi-armed candle holder |
| Chandelier | Hanging multi-light fixture |
| Lantern | Enclosed portable light |
| Sconce | Wall-mounted enclosed light |
| Fireplace | Built-in hearth with fire |
| Fire Pit | Open ground fire |
| Brazier | Standing fire bowl |
| Oil Lamp | Wick-based desk or table lamp |
| Crystal Light | Magical glowing crystal |
| Fairy Light | String of small glowing points |
| Magical Orb | Floating glowing sphere |
| Mushroom (glowing) | Bioluminescent fungus |
| Lava Pool | Molten rock light source |
| Sunlight | Natural light through windows and skylights |
| Moonlight | Dim bluish night light |

---

## 10. Decorative Objects

### Wall Decorations

| Object | Description |
|---|---|
| Painting (small) | Small framed artwork |
| Painting (medium) | Medium framed artwork |
| Painting (large) | Large framed artwork |
| Painting (portrait) | Vertical portrait format |
| Painting (landscape) | Horizontal landscape format |
| Tapestry | Woven wall hanging |
| Banner/Flag | Faction, clan, or decorative banner |
| Shield | Mounted display shield |
| Weapon Rack | Wall-mounted swords, axes, bows |
| Mounted Head | Animal or monster trophy |
| Mirror | Reflective wall panel |
| Clock | Wall-mounted timepiece |
| Shelf | Wall shelf with or without items |
| Coat of Arms | Heraldic display |
| Notice Board | Pinnable message board |
| Map | Wall-mounted world or region map |
| Religious Icon | Faith-specific wall display |
| Mask Collection | Decorative mask display |
| Plate Display | Mounted decorative plates |

---

### Floor Decorations

| Object | Description |
|---|---|
| Rug | Rectangular, circular, or runner style |
| Potted Plant | Plant in decorative container |
| Statue (small) | Tabletop-sized figure |
| Statue (medium) | Human-sized figure |
| Statue (large) | Larger than life figure |
| Statue (bust) | Head and shoulders only |
| Fountain | Indoor water feature |
| Vase | With or without flowers |
| Barrel | Wooden storage barrel |
| Crate | Wooden storage crate |
| Sack | Cloth storage sack |
| Chest (decorative) | Ornamental or storage chest |
| Pedestal | Display stand |
| Trophy Case | Glass-fronted display |
| Display Case | Glass-fronted exhibit |
| Aquarium | Fish tank |
| Globe | World model on stand |
| Anchor | Nautical decoration |
| Anvil (decorative) | Non-functional display anvil |

---

### Ceiling Decorations

| Object | Description |
|---|---|
| Hanging Banner | Banner suspended from ceiling |
| Chandelier | Also functions as lighting |
| Hanging Plant | Suspended planter |
| Mobile | Hanging decorative sculpture |
| Spider Web | Natural or decorative web |
| Stalactite | Natural cave formation |
| Hanging Cage | Suspended cage (decorative or functional) |
| Wind Chime | Hanging chime set |
| Hanging Lantern | Also functions as lighting |

---

### Garden and Outdoor Decorations

| Object | Description |
|---|---|
| Sundial | Time-telling garden feature |
| Bird Bath | Stone basin for birds |
| Scarecrow | Field decoration |
| Well | Water-drawing structure |
| Flagpole | Pole with banner or flag |
| Mailbox | Letter collection point |
| Gravestone | Cemetery marker |
| Memorial Plaque | Inscribed commemorative plate |
| Market Stall | Open-air vendor stand |
| Cart/Wagon | Wheeled transport (parked) |
| Hitching Post | Animal tethering point |
| Trough | Animal feeding or watering basin |
| Planter Box | Raised garden bed |
| Garden Gnome | Small garden figure |
| Wind Chime | Outdoor hanging chime |
| Pergola | Open-roofed garden structure |
| Gazebo | Covered open-sided garden structure |
| Arbor | Arched garden entry |
| Fountain (outdoor) | Outdoor water feature |
| Pond | Small body of water |
| Bridge (garden) | Small decorative bridge |
| Path Stepping Stones | Individual flat stones forming a walkway |

---

## 11. Functional Objects

### Crafting Stations

| Object | Description |
|---|---|
| Anvil | Metalworking station |
| Furnace/Smelter | Ore processing station |
| Forge | Advanced metalworking station |
| Workbench | General crafting surface |
| Loom | Textile weaving station |
| Spinning Wheel | Fiber spinning station |
| Pottery Wheel | Clay shaping station |
| Kiln | High-temperature firing station |
| Cooking Range/Stove | Indoor cooking station |
| Campfire/Spit | Outdoor cooking station |
| Brewing Vat | Potion and drink brewing station |
| Tanning Rack | Leather processing station |
| Sawmill | Wood processing station |
| Glassblowing Station | Glass crafting station |
| Sandpit | Sand collection station |
| Well (water source) | Water collection station |
| Herb Table | Herblore preparation surface |
| Enchanting Table | Magical enhancement station |
| Altar | Prayer and magic station |
| Obelisk | Summoning and runecrafting station |
| Crystal Singing Bowl | Crystal crafting station |
| Compost Bin | Organic material processing station |

---

### Storage

| Object | Description |
|---|---|
| Chest (small) | 10-slot storage |
| Chest (medium) | 20-slot storage |
| Chest (large) | 30-slot storage |
| Wardrobe | Clothing and armour storage |
| Bookshelf | Book and scroll storage |
| Armour Stand | Display and store armour sets |
| Weapon Rack | Display and store weapons |
| Tool Shed | Tool storage structure |
| Cupboard | General enclosed storage |
| Dresser | Bedroom storage furniture |
| Nightstand | Small bedside storage |
| Cabinet | Tall enclosed storage |
| Strongbox | Secure small storage |
| Safe | Locked secure storage |
| Display Case | Glass-fronted item display with storage |
| Trophy Shelf | Achievement item display |
| Barrel | Bulk liquid or item storage |
| Crate | Bulk item storage |

---

### Interactive Objects

| Object | Description |
|---|---|
| Lever | Pull to trigger a mechanism |
| Button/Switch | Press to trigger a mechanism |
| Pressure Plate | Step on to trigger a mechanism |
| Tripwire | Walk through to trigger a mechanism |
| Lock (key) | Requires a specific key item to open |
| Combination Lock | Requires a code to open |
| Scale/Balance | Weighing mechanic for puzzles |
| Telescope | View distant locations |
| Microscope | Examine small objects |
| Music Box | Play a tune when activated |
| Piano/Organ | Playable musical instrument |
| Bed | Rest and sleep (restore stats, set respawn) |
| Bathtub | Cosmetic interaction |
| Sink | Water source interaction |
| Toilet | Cosmetic interaction |
| Cooking Pot | Cook food items |
| Cauldron | Brew potions or mix items |

---

## 12. Natural Objects

### Trees

| Type | Description |
|---|---|
| Oak | Standard hardwood, common |
| Willow | Drooping branches, found near water |
| Maple | Broad leaves, temperate regions |
| Yew | Dark conifer, graveyards and ancient sites |
| Magic | Enchanted tree, rare and valuable |
| Teak | Tropical hardwood |
| Mahogany | Dense tropical hardwood |
| Redwood | Massive ancient conifer |
| Palm | Tropical, found on beaches and islands |
| Pine | Conifer, found in cold and mountain regions |
| Birch | White bark, found in temperate forests |
| Dead Tree | Leafless, found in swamps and wastelands |
| Fruit Tree | Apple, banana, papaya, and other harvestable varieties |
| Stump | Remains after a tree is chopped |

---

### Rocks

| Type | Description |
|---|---|
| Mining Rock (copper) | Low-level ore node |
| Mining Rock (tin) | Low-level ore node |
| Mining Rock (iron) | Mid-level ore node |
| Mining Rock (coal) | Mid-level fuel node |
| Mining Rock (mithril) | High-level ore node |
| Mining Rock (adamant) | High-level ore node |
| Mining Rock (rune) | Top-level ore node |
| Mining Rock (amethyst) | Gem ore node |
| Boulder | Large immovable rock (decoration or obstacle) |
| Stepping Stone | Flat stone for crossing water |
| Cliff Face | Vertical rock wall |
| Cave Entrance | Opening in rock leading underground |
| Stalactite/Stalagmite | Cave formations |
| Crystal Formation | Glowing mineral cluster |
| Gem Rock | Multi-gem mining node |

---

### Plants

| Type | Description |
|---|---|
| Bush (berry) | Harvestable berry bush |
| Bush (herb) | Harvestable herb bush |
| Flower | Multiple species, decorative or harvestable |
| Tall Grass | Waist-height grass, may hide items |
| Fern | Forest undergrowth |
| Mushroom | Harvestable fungus, some glowing |
| Cactus | Desert plant, may cause damage on contact |
| Reed/Cattail | Wetland plant, found at water edges |
| Vine | Climbing plant on walls or trees |
| Moss | Surface-covering plant on stone and wood |
| Seaweed | Underwater plant |
| Coral | Underwater growth, decorative or harvestable |

---

### Water Features

| Type | Description |
|---|---|
| Lake | Large body of still water |
| River | Flowing water with current |
| Stream | Small flowing water |
| Waterfall | Vertical water flow between elevations |
| Hot Spring | Warm water with steam (may restore stats) |
| Fountain | Constructed water feature |
| Well | Water access point |
| Swamp/Bog | Shallow murky water with reduced movement |
| Puddle | Small temporary water |
| Ice | Frozen water surface (slippery movement) |

---

## 13. Structural Elements

### Bridge Types

| Type | Description |
|---|---|
| Wooden Bridge | Log or plank bridge |
| Stone Bridge | Arched stone construction |
| Rope Bridge | Suspended rope with plank walkway |
| Drawbridge | Raises and lowers (connects to gate) |
| Stepping Stones | Individual stones across water |
| Ice Bridge | Frozen natural crossing |
| Magical Bridge | Energy or light pathway |
| Chain Bridge | Metal chain suspension |

---

### Fence Types

| Type | Description |
|---|---|
| Picket | Short pointed wooden stakes |
| Post and Rail | Horizontal rails between posts |
| Stone Wall | Low dry-stone wall |
| Iron Wrought | Ornamental metal fence |
| Chain Link | Metal mesh (modern settings) |
| Bamboo | Tied bamboo stalks |
| Hedge | Living plant border |
| Barbed Wire | Hostile barrier (modern settings) |
| Electric | Powered barrier (futuristic settings) |
| Energy Field | Magical barrier |

---

### Gate Types

| Type | Description |
|---|---|
| Garden Gate | Small single panel |
| Farm Gate | Wide livestock gate |
| Castle Gate | Large reinforced double gate |
| Portcullis | Vertical sliding metal grate |
| Drawbridge | Lowers over moat |
| Magical Portal | Energy-based passage |
| Toll Gate | Requires payment to pass |
| Security Gate | Requires key or code to pass |
| Turnstile | One-way rotating gate |

---

## Views

Editor views for browsing and placing structures from the catalog.

| View | Description |
|---|---|
| Structure Browser | Searchable catalog of all placeable structures |
| Material Palette | Browse and select materials for walls, floors, and roofs |
| Style Guide | Reference for which materials pair well together |
| Category Browser | Browse by category (walls, doors, windows, arches, etc.) |
| Preview Renderer | See object in 3D before placing in the world |
