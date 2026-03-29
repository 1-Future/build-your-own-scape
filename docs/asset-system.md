# Asset System

Game design document for the Asset System plugin. This system covers the complete visual and data pipeline for all game entities. Every visible thing in the game world is composed from these building blocks --- models, textures, animations, sprites, particles, and audio. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the asset system module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| HD Models | High-detail 3D models vs PS1-style low-poly models |
| Texture Quality | High-resolution textures vs low-resolution indexed textures |
| Particle Density | Full particle effects vs reduced or disabled particles |
| Animation Smoothing | Interpolated animation frames vs raw frame-by-frame playback |
| Ground Decoration Density | Full ground decorations vs reduced or disabled |
| Wall Decoration System | Wall-mounted decorative elements enabled |
| Trim System | Decorative trim and molding along edges enabled |
| Overlay Shape System | Complex overlay shapes for terrain transitions enabled |
| Particle Physics | Gravity, wind, and collision for particles enabled |
| Animated Textures | Flowing water, lava, and other animated surfaces enabled |
| Environment Mapping | Reflective surfaces enabled |
| Custom Model Uploads | Dungeon Masters can upload custom 3D models |
| Custom Texture Uploads | Dungeon Masters can upload custom textures |
| Custom Animation Imports | Dungeon Masters can import custom animation sequences |
| Model LOD | Level of detail scaling by camera distance |
| Sprite Scaling | Sprites scale with camera zoom |
| Particle Limit Cap | Maximum concurrent particles capped at configurable value |
| Shadow Casting | Objects and entities cast shadows |
| Dynamic Lighting | Spot anims and light-emitting objects produce dynamic light |
| Model Recoloring System | Recolor from/to system for model variants |
| Identity Kit System | Player character assembly from body part models |
| Item Ground Model | Items on the ground render as 3D models vs 2D sprites |
| Stack Model Variants | Different models at stack thresholds (coins show pile growth) |
| Seasonal Decorations | Ground decorations and objects change with in-game season or event |

---

## 1. Asset Hierarchy

All visual and audio content falls into one of these categories. Each category feeds into the others --- models reference textures, animations reference models, spot anims reference both.

```
Models (raw 3D meshes)
+-- NPCs (composed of multiple models + identity kit)
+-- Items (inventory icon + ground model + equipped model)
+-- Objects / Locations (world furniture, interactables, resource nodes)
+-- Spot Animations (visual effects attached to entities or positions)
+-- Ground Decorations (flat tile-level visual details)

Textures (surface materials)
+-- Tile Underlays (base terrain color/texture)
+-- Tile Overlays (painted on top of underlay)
+-- Wall Textures (applied to wall edges)
+-- Model Textures (applied to 3D meshes)
+-- Materials (shader properties -- reflectivity, transparency, glow)

Animations / Sequences
+-- Character Animations (walk, run, idle, attack, death, skill)
+-- Object Animations (gate opening, wheel spinning, fire flickering)
+-- Spot Anim Sequences (effect lifecycle -- spawn, play, fade)
+-- Cutscene Sequences (camera paths, choreographed events)

Sprites (2D images)
+-- Interface Icons (skill icons, prayer icons, spell icons)
+-- Item Icons (inventory 2D representation)
+-- Map Icons (minimap symbols)
+-- UI Elements (buttons, borders, backgrounds)
+-- Particles (individual particle sprites for effects)

Audio
+-- Sound Effects (per-action, per-event)
+-- Music Tracks (per-zone, per-event)
+-- Ambient Sounds (environmental audio loops)
+-- Voice Lines (NPC dialogue audio)
```

---

## 2. Models

The fundamental 3D building block. Every visible entity is either a model or composed of multiple models. RuneScape-style models use per-vertex coloring rather than UV-mapped textures for most surfaces, giving the characteristic flat-shaded look.

### Model Properties

| Column | Type | Description |
|---|---|---|
| Model ID | Integer | Unique identifier |
| Vertices | Array | 3D vertex positions (X, Y, Z) |
| Faces | Array | Triangle face definitions referencing vertex indices |
| Vertex Colors | Array | Per-vertex coloring (RS style -- no UV mapping for most models) |
| Texture Faces | Array | Faces that use a texture instead of vertex color |
| Priority | Integer | Render priority / draw order |
| Scale | Float | Model scale multiplier |
| Bounding Box | Coords | Min/max bounds for collision and click detection |
| Translucency | Boolean | Whether model has transparent faces |
| Ambient | Integer | Base light level applied to the model |
| Contrast | Integer | Shadow depth applied to the model |

### Model Composition

Multiple models combine to form complex entities. An NPC is head model + torso model + arms model + legs model + weapon model. Equipment replaces body part models when equipped. This compositional approach means a single arm model can be reused across hundreds of NPCs, and a new helmet only requires one model that slots into the head position.

---

## 3. NPCs (Entity Composition)

NPCs are assembled from multiple models stitched together at runtime. The identity kit system (used for player characters) and the NPC model list (used for monsters and townsfolk) both follow the same compositional pattern. Recoloring allows hundreds of visual variants from a single set of base models.

### NPC Model Assembly

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | Unique NPC definition |
| Model IDs | Array | List of models composing this NPC (head, body, arms, legs, etc.) |
| Idle Animation | Sequence ID | Animation when standing still |
| Walk Animation | Sequence ID | Animation when walking |
| Run Animation | Sequence ID | Animation when running |
| Attack Animation | Sequence ID | Combat attack animation |
| Death Animation | Sequence ID | Death animation |
| Recolor From | Array | Original colors to replace |
| Recolor To | Array | Replacement colors (enables variants from same model) |
| Retexture From | Array | Original textures to replace |
| Retexture To | Array | Replacement textures |
| Size | Integer | Tile footprint (1x1, 2x2, 3x3, etc.) |
| Head Icon | Sprite ID | Overhead icon (prayer, skull, etc.) |
| Render Priority | Boolean | Renders on top of other entities when true |
| Chat Head Models | Array | Model IDs used for the dialogue portrait |

### Identity Kit (Player Character Assembly)

The identity kit defines the body parts available for player character creation. Each kit piece represents one body part slot and contains the models, colorable regions, and chat head data for that slot.

| Column | Type | Description |
|---|---|---|
| IDK ID | Integer | Identity kit piece ID |
| Body Part | Enum | Head, jaw/beard, torso, arms, hands, legs, feet |
| Gender | Enum | Male / Female / Neutral |
| Models | Array | 3D models for this body part |
| Chat Head Models | Array | Models used for the dialogue portrait |
| Recolor Slots | Array | Colorable regions (hair, skin, clothing) |
| Non-Selectable | Boolean | Whether players can choose this in character creation |

---

## 4. Items

Items have up to three visual representations: an inventory icon (2D or 3D-rendered), a ground model (shown when dropped), and an equipped model (replaces a body part when worn). Stack models allow items like coins to visually change as quantity increases.

### Item Visual Properties

| Column | Type | Description |
|---|---|---|
| Item ID | Integer | Unique item definition |
| Inventory Model | Model ID | 3D model rendered in inventory (rotated to produce icon) |
| Inventory Icon | Sprite | 2D pre-rendered icon (alternative to 3D render) |
| Ground Model | Model ID | 3D model shown when item is on the ground |
| Equipped Male Model | Model ID | Model replacing body part when worn (male) |
| Equipped Female Model | Model ID | Model replacing body part when worn (female) |
| Equipped Slot | Enum | Which body part this replaces when worn |
| Model Zoom | Integer | Camera distance for inventory rendering |
| Model Rotation X | Integer | X-axis rotation for inventory rendering |
| Model Rotation Y | Integer | Y-axis rotation for inventory rendering |
| Model Rotation Z | Integer | Z-axis rotation for inventory rendering |
| Model Offset X | Integer | Horizontal position offset for inventory rendering |
| Model Offset Y | Integer | Vertical position offset for inventory rendering |
| Stack Models | Array | Different models at stack thresholds (1, 2, 3, 4, 5+) |
| Noted Model | Model ID | Model for banknote version |
| Placeholder Model | Model ID | Model for bank placeholder |
| Recolor From | Array | Original colors to replace |
| Recolor To | Array | Replacement colors for variants |
| Ground Scale | Float | Size multiplier when on ground |
| Ambient | Integer | Base light level on inventory render |
| Contrast | Integer | Shadow depth on inventory render |

---

## 5. Objects / Locations (Locs)

Everything in the world that is not terrain, NPC, or dropped item. Objects are the world furniture --- doors, trees, anvils, ladders, fences, torches, statues. Each object has a type that determines how it is placed and rendered (wall-mounted, freestanding, ground-level, rooftop).

### Object Types

| Type ID | Name | Description |
|---|---|---|
| 0-3 | Wall | Wall objects placed on tile edges |
| 4-8 | Wall Decoration | Decorations attached to walls (torches, signs, paintings) |
| 9 | Diagonal Wall | Diagonal wall spanning corner to corner |
| 10-11 | Interactive Object | Standard game objects (doors, ladders, anvils, furnaces, chests) |
| 12 | Ground Decoration | Flat decorations on the ground (flowers, mushrooms, footprints) |
| 13-21 | Roof/Edge | Roof pieces and edge decorations |
| 22 | Floor Decoration | Floor-level objects rendered under players |

### Object Properties

| Column | Type | Description |
|---|---|---|
| Object ID | Integer | Unique definition |
| Name | String | Display name |
| Models | Array | One or more model IDs composing this object |
| Model Types | Array | Which object type each model represents |
| Size X | Integer | Tile footprint width |
| Size Y | Integer | Tile footprint depth |
| Interaction Type | Integer | How players interact (0=none, 1=option1, 2=option2, etc.) |
| Actions | Array[5] | Right-click options ("Open", "Search", "Mine", "Chop", "Use") |
| Block Movement | Boolean | Whether this object blocks pathfinding |
| Block Projectile | Boolean | Whether this object blocks ranged/magic attacks |
| Contoured | Boolean | Whether model follows terrain elevation |
| Merge Normals | Boolean | Blend lighting with terrain |
| Animation | Sequence ID | Idle/active animation (spinning wheel, flickering fire) |
| Ambient | Integer | Light emission level |
| Contrast | Integer | Shadow depth |
| Recolor From | Array | Original colors to replace |
| Recolor To | Array | Replacement colors |
| Map Icon | Sprite ID | Icon shown on minimap/world map |
| Rotate | Boolean | Can be rotated on placement |
| Offset X | Integer | Position offset from tile center (horizontal) |
| Offset Y | Integer | Position offset from tile center (vertical) |
| Offset Z | Integer | Position offset from tile center (height) |
| Impassable | Boolean | Blocks player walking through |
| Wall Width | Integer | For wall-type objects, the thickness |
| Var Bits | ID | Game state variable (bit-packed) controlling transforms |
| Var Players | ID | Player-specific variable controlling transforms |
| Transform IDs | Array | Object IDs this can transform into based on game state |
| Support Items | Boolean | Items can be placed on top |
| Decorated | Boolean | Has trim, molding, or embellishment |

### Object Sub-Categories (by function)

| Category | Examples |
|---|---|
| Doors/Gates | Single door, double door, gate, portcullis, trapdoor |
| Crafting Stations | Anvil, furnace, pottery wheel, spinning wheel, loom, range, workbench |
| Storage | Chest, crate, barrel, bookshelf, display case, wardrobe |
| Resource Nodes | Tree, rock, fishing spot (NPC technically), farming patch |
| Transportation | Ladder, staircase, agility obstacle, fairy ring, spirit tree, boat |
| Barriers | Fence, hedge, railing, low wall, rubble |
| Lighting | Torch, lantern, brazier, campfire, chandelier, candelabra |
| Signage | Sign post, notice board, milestone, gravestone |
| Seating | Chair, bench, throne, stool, log seat |
| Decorative | Statue, fountain, pillar, archway, flower pot, painting, rug |
| Mechanical | Lever, button, pressure plate, cog, conveyor, trapdoor |
| Natural | Bush, tall grass, mushroom, vine, boulder, fallen tree, puddle |
| Religious | Altar, shrine, totem, obelisk, gravestone, prayer mat |

---

## 6. Spot Animations (SpotAnims / Graphics / GFX)

Visual effects played on entities or at positions in the world. A spell impact, a teleport column of light, a level-up firework --- these are all spot anims. Each spot anim is a model with an animation sequence, played at a target with optional delay and height offset.

### SpotAnim Definition Properties

| Column | Type | Description |
|---|---|---|
| SpotAnim ID | Integer | Unique identifier |
| Model ID | Integer | 3D model for this effect |
| Animation | Sequence ID | Animation sequence the model plays |
| Rotation | Integer | Model rotation |
| Ambient | Integer | Light emission |
| Contrast | Integer | Shadow depth |
| Recolor From | Array | Original colors to replace |
| Recolor To | Array | Replacement colors |
| Retexture From | Array | Original textures to replace |
| Retexture To | Array | Replacement textures |

### SpotAnim Playback Properties

These are set per use, not per definition. The same spot anim definition can be played at different heights, delays, and targets.

| Column | Type | Description |
|---|---|---|
| Target | Entity or Position | What it is attached to (player, NPC, tile) |
| Height | Integer | Y offset above target |
| Delay | Integer | Client ticks before starting |
| Duration | Integer | How long effect plays |
| Loop | Boolean | Repeat or play once |
| Sound | Sound ID | Audio paired with this effect |

### SpotAnim Categories (by use)

| Category | Examples |
|---|---|
| Combat Impact | Spell hit splashes, melee hit effects, poison bubble, venom drip |
| Spell Cast | Casting glow on player hands, magic rune circles |
| Teleportation | Column of light, portal swirl, vanish/appear poof |
| Prayer | Overhead prayer icon, prayer activation glow, smite drain |
| Buff/Debuff | Overload shake, stat boost sparkle, freeze ice, bind roots |
| Skill | Mining sparks, woodcutting chips, fishing splash, cooking steam |
| Environmental | Campfire flames, torch flicker, waterfall mist, chimney smoke |
| Status | Poison green hits, venom dark green, disease, skull appear |
| Celebration | Level-up fireworks, quest complete banner, rare drop loot beam |
| Emote | Dance sparkles, wave effect, bow flourish |
| Death | Death animation, bone scatter, ghost rise |
| Projectile | Arrow in flight, spell bolt, thrown axe, cannonball |

---

## 7. Ground Decorations

Flat visual elements rendered on the tile surface, below objects and entities. These add environmental detail without affecting gameplay --- flowers, moss, cracks, puddles. They sit at ground level and players walk over them.

### Ground Decoration Properties

| Column | Type | Description |
|---|---|---|
| Decoration ID | Integer | Unique ID |
| Model ID | Integer | Flat or very low-profile 3D model |
| Walkable | Boolean | Can players walk over this |
| Offset | Position | Offset from tile center |
| Animation | Sequence ID | Optional animation (flickering shadow, swaying grass) |
| Seasonal | Boolean | Changes with in-game season/event |

### Ground Decoration Types

Flowers, mushrooms, pebbles, puddles, cracks, moss, fallen leaves, footprints, blood stains, rune circles, trap markers, quest markers, shadows, roots, scattered bones, cobwebs, anthills, small rocks, tire tracks, drag marks.

---

## 8. Wall Decorations

Visual elements attached to wall edges, either on the interior or exterior face. Torches, paintings, banners, mounted trophies. These reference the wall edge they are attached to and can optionally be interactive.

### Wall Decoration Properties

| Column | Type | Description |
|---|---|---|
| Wall Deco ID | Integer | Unique ID |
| Model ID | Integer | 3D model |
| Edge | Enum | Which wall edge it is on (N/E/S/W) |
| Face | Enum | Interior or exterior face of wall |
| Offset From Wall | Float | Distance from wall surface |
| Height | Float | Vertical position on wall |
| Animation | Sequence ID | Optional animation (torch flicker, banner wave) |
| Interactive | Boolean | Can player interact (take painting, light torch) |
| Actions | Array | Right-click options |

### Wall Decoration Types

Torches, sconces, paintings, mounted heads/trophies, shelves, banners, flags, clocks, mirrors, curtains, arrow slits, coat of arms, notice boards, maps, weapon racks, mounted shields, candle holders, herb drying racks, wanted posters, tapestries.

---

## 9. Trim and Molding

Decorative elements along edges --- wall tops, wall bases, roof edges, floor borders. These add architectural detail and distinguish building styles (stone castle vs wooden cabin vs marble temple).

### Wall Trim

| Column | Type | Description |
|---|---|---|
| Trim ID | Integer | Unique ID |
| Style | Enum | Crown molding, baseboard, chair rail, picture rail, dado rail |
| Material | Enum | Wood, stone, plaster, metal, ornate |
| Color | Hex | Trim color |
| Profile | Enum | Flat, rounded, ogee, cove, bead |
| Height | Float | Position on wall |
| Repeat | Boolean | Tiles seamlessly along wall |

### Roof Trim

| Column | Type | Description |
|---|---|---|
| Style | Enum | Eave, ridge, fascia, soffit, gutter, finial |
| Material | Enum | Wood, stone, metal, tile |
| Overhang | Float | How far trim extends past wall |

### Floor Trim

| Column | Type | Description |
|---|---|---|
| Style | Enum | Border, inlay, threshold, transition strip |
| Material | Enum | Wood, stone, tile, metal, carpet |
| Width | Integer | Tiles wide |

---

## 10. Tile Layers (Underlays and Overlays)

The ground surface is built from two layers. The underlay is the base terrain color or texture. The overlay is painted on top and can be shaped to create natural-looking terrain transitions (paths, water edges, cliff borders).

### Underlay (Base Terrain)

| Column | Type | Description |
|---|---|---|
| Underlay ID | Integer | Unique ID |
| Color | RGB | Base terrain color |
| Texture | Texture ID | Optional texture (overrides color) |
| Blend | Boolean | Smooth color blending with neighbors |

### Overlay (Painted on Top)

| Column | Type | Description |
|---|---|---|
| Overlay ID | Integer | Unique ID |
| Color | RGB | Overlay color |
| Texture | Texture ID | Optional texture |
| Secondary Color | RGB | For multi-color overlays |
| Blend | Boolean | Edge blending with adjacent tiles |
| Hide Underlay | Boolean | Completely covers underlay or shows through |
| Shape | Integer | Which tile shape (full, half diagonal, corner, edge strip) |
| Rotation | Integer | Overlay rotation (0-3, quarter turns) |

### Overlay Shapes

Overlays clip to tiles using shapes that create natural terrain transitions:

| Shape | Description |
|---|---|
| Full Tile | Covers entire tile |
| Half Diagonal (NE, NW, SE, SW) | Covers one triangular half of the tile |
| Corner | Small triangle in one corner |
| L-Shape | Three-quarter coverage, one corner cut |
| Edge Strip (N, E, S, W) | Thin strip along one edge |

These shapes, combined with rotation, produce smooth transitions between terrain types --- a dirt path curving through grass, a river bank following natural contours, a cliff edge with exposed rock.

---

## 11. Textures and Materials

Textures provide surface detail beyond flat vertex colors. In the classic RS style, most models use per-vertex coloring with textures reserved for specific surfaces (water, lava, certain floors). The HD/RS3 style adds full material properties --- reflectivity, roughness, emission.

### Texture Properties

| Column | Type | Description |
|---|---|---|
| Texture ID | Integer | Unique ID |
| Sprites | Array | Source sprite(s) composing the texture |
| Size | Integer | Pixel dimensions (typically 64x64 or 128x128) |
| Animation Speed | Integer | For animated textures (water, lava) |
| Animation Direction | Enum | Flow direction for animated textures |

### Material Properties (HD Mode)

| Column | Type | Description |
|---|---|---|
| Material ID | Integer | Unique ID |
| Texture | Texture ID | Base texture |
| Reflectivity | Float | How much light reflects |
| Roughness | Float | Surface smoothness (0 = mirror, 1 = matte) |
| Emissive | Float | Self-illumination intensity |
| Transparency | Float | See-through amount (0 = opaque, 1 = invisible) |
| Flow Speed | Float | For animated materials (water current) |
| Environment Map | Boolean | Reflects environment |

---

## 12. Animations / Sequences

Every moving thing uses a sequence. Walking, attacking, opening a door, a spinning wheel, a flickering fire --- all sequences. A sequence is a list of frames with per-frame timing, optional sound triggers, and loop behavior.

### Sequence Properties

| Column | Type | Description |
|---|---|---|
| Sequence ID | Integer | Unique ID |
| Frame IDs | Array | List of frame references |
| Frame Lengths | Array | Duration per frame (in client ticks) |
| Loop Offset | Integer | Frame to loop back to (-1 = no loop) |
| Priority | Integer | Override priority vs other animations |
| Left Hand Item | Integer | Item model held in left hand during animation |
| Right Hand Item | Integer | Item model held in right hand during animation |
| Stretch | Boolean | Stretch to fit model proportions |
| Force Replay | Boolean | Restart if already playing |
| Sound Effects | Array | Sounds played at specific frames |

### Animation Categories

| Category | Examples |
|---|---|
| Idle | Standing, sitting, resting, sleeping, AFK |
| Movement | Walk, run, shuffle, crawl, swim, climb, jump |
| Combat | Slash, stab, crush, kick, punch, block, dodge, cast, shoot |
| Skill | Mine, chop, fish, cook, smith, fletch, craft, pray, alch |
| Emote | Wave, bow, dance, clap, cry, laugh, think, shrug |
| Death | Player death, NPC death, crumble, fade, explode |
| Interaction | Open door, climb ladder, pull lever, sit on chair |
| Status | Poisoned wobble, stunned stars, frozen ice, burning fire |

---

## 13. Sprites (2D Assets)

Sprites are flat 2D images used for interface elements, item icons, map symbols, and particle textures. They use indexed color with a palette for memory efficiency. Sprites are not placed in the 3D world directly --- they appear in the UI layer or as billboard particles.

### Sprite Properties

| Column | Type | Description |
|---|---|---|
| Sprite ID | Integer | Unique ID |
| Width | Integer | Pixel width |
| Height | Integer | Pixel height |
| Offset X | Integer | Horizontal draw offset |
| Offset Y | Integer | Vertical draw offset |
| Palette | Array | Color palette (indexed color) |
| Pixels | Array | Pixel data referencing palette indices |
| Transparent Color | Integer | Palette index treated as transparent |

### Sprite Categories

| Category | Examples |
|---|---|
| Interface Icons | Skill icons, prayer icons, spell icons, status icons |
| Item Icons | Inventory 2D representations (pre-rendered or live) |
| Map Icons | Minimap symbols (bank, altar, shop, quest, transport) |
| Cursor Sprites | Default cursor, attack cursor, magic cursor, use cursor |
| UI Borders | Window frames, panel edges, scroll bars, tab shapes |
| UI Backgrounds | Panel fills, tooltip backgrounds, chat box backdrop |
| Loading Elements | Loading bar, splash screen, progress indicators |
| Chat Emojis | In-game chat emoticons |
| Particle Sprites | Individual particle images for effects (spark, drop, puff) |

---

## 14. Particles

Particles are lightweight visual effects composed of many small sprites or models emitted from a source. Fire sparks, rain drops, magic sparkles, dust clouds. Each particle emitter defines the behavior --- rate, velocity, lifetime, color over time --- and the renderer handles the rest.

### Particle Emitter Properties

| Column | Type | Description |
|---|---|---|
| Emitter ID | Integer | Unique ID |
| Sprite / Model | ID | Visual for each particle |
| Emit Rate | Float | Particles per tick |
| Lifetime | Integer | Ticks before particle dies |
| Velocity | Vector3 | Initial speed and direction |
| Gravity | Float | Downward acceleration |
| Spread | Float | Random direction variance (cone angle) |
| Scale Start | Float | Particle size at spawn |
| Scale End | Float | Particle size at death |
| Color Start | RGB | Color at spawn |
| Color End | RGB | Color at death |
| Opacity Start | Float | Transparency at spawn |
| Opacity End | Float | Transparency at death |
| Collision | Boolean | Particles bounce off terrain |
| Wind Affected | Boolean | Weather wind pushes particles |

### Particle Types

| Type | Description |
|---|---|
| Fire Sparks | Small bright particles rising from flames |
| Smoke Puffs | Gray/white expanding particles drifting upward |
| Water Droplets | Blue particles splashing or falling |
| Magic Sparkles | Colored glowing particles orbiting or trailing |
| Blood Drops | Red particles on combat impact |
| Dust Clouds | Brown particles kicked up by movement or collapse |
| Snow Flakes | White particles falling from sky |
| Rain Drops | Thin vertical particles falling fast |
| Leaves | Green/brown particles drifting from trees |
| Embers | Orange particles floating up from hot sources |
| Bubbles | Transparent circular particles rising in water |
| Lightning Arcs | Bright white/blue particles in jagged lines |
| Soul Wisps | Pale translucent particles drifting slowly |
| Bone Fragments | White/gray particles scattering on death |

---

## 15. Audio Assets

Sound effects, music, and ambient audio. Every action, event, and zone has associated audio. Sound effects are short one-shot or looping clips tied to specific triggers. Music tracks are longer compositions that play per-zone. Ambient sounds loop continuously based on environment.

### Sound Effect Properties

| Column | Type | Description |
|---|---|---|
| Sound ID | Integer | Unique ID |
| Volume | Integer | Base volume (0-127) |
| Radius | Integer | Tiles before sound fades to silence |
| Pitch Variation | Range | Random pitch range for variety |
| Loop | Boolean | Repeating sound |
| Priority | Integer | Override lower-priority sounds when at channel limit |

### Music Track Properties

| Column | Type | Description |
|---|---|---|
| Track ID | Integer | Unique ID |
| Name | String | Display name shown in music player |
| Duration | Integer | Length in ticks |
| Unlock Zone | Zone ID | Location that unlocks this track |
| Instruments | MIDI Data | Musical data (MIDI-style for runtime synthesis) |

### Ambient Sound Properties

| Column | Type | Description |
|---|---|---|
| Ambient ID | Integer | Unique ID |
| Sound ID | Integer | Sound effect to loop |
| Zone | Zone ID | Area where this ambient plays |
| Volume | Integer | Base volume |
| Fade In | Integer | Ticks to fade in when entering zone |
| Fade Out | Integer | Ticks to fade out when leaving zone |

---

## 16. Asset Pipeline (for Content Creators)

The workflow from raw art asset to in-game entity. Content creators (artists, DMs, modders) follow this pipeline to add new visual content to the game.

### Creation Workflow

1. **Model** --- Create 3D model in Blender or other 3D tool. Use per-vertex coloring for RS style or UV-mapped textures for HD style.
2. **Export** --- Export to engine format (custom binary or GLTF).
3. **Define Properties** --- Fill in the data spreadsheet (item stats, object interactions, NPC dialogue, animation assignments).
4. **Assign Visuals** --- Link textures, colors, and animations to the model.
5. **Compose** --- Assemble into game entity (NPC = models + anims + identity kit; item = inventory model + ground model + equipped model).
6. **Place** --- Position in world (objects) or register in database (items, NPCs).
7. **Test** --- Preview in-game via the asset preview tools.
8. **Iterate** --- Adjust colors, scale, animations, and properties until correct.

### File Formats

| Asset | Format | Notes |
|---|---|---|
| Models | Custom binary or GLTF | Vertices, faces, colors, textures |
| Textures | PNG, custom indexed | Power-of-2 dimensions preferred |
| Sprites | PNG with transparency | Indexed color for memory efficiency |
| Animations | Custom binary or JSON | Frame sequences with timing data |
| Audio | OGG, WAV, MIDI | OGG for sound effects, MIDI for music synthesis |
| Data | JSON | Asset definitions and properties |

---

## Views

All the interface panels for browsing, previewing, and managing assets.

| View | Description |
|---|---|
| Model Browser | Search and filter all models by ID, name, or tag |
| NPC Composer | Assemble NPC from body part models, preview with animations |
| Item Renderer | Preview inventory icon, ground model, and equipped model side by side |
| Object Placer | Browse objects by category and place in world |
| SpotAnim Previewer | Play spot anim effects on a target dummy or at a position |
| Ground Decoration Palette | Browse and paint ground decorations onto tiles |
| Wall Decoration Catalog | Browse and attach decorations to wall edges |
| Trim Editor | Select and apply trim styles to walls, roofs, and floors |
| Texture/Material Browser | Browse textures and materials, preview on sample surfaces |
| Animation Player | Play any sequence on any model, scrub through frames |
| Particle Editor | Create and tune particle emitters with live preview |
| Sprite Sheet Viewer | Browse all sprites by category with search |
| Audio Library | Browse and play all sound effects, music tracks, and ambients |
