# Camera System

Game design document for the Camera System plugin. This system defines how the player sees the world -- perspective, control, movement, and cinematic behavior. The camera is the lens through which every other system is experienced.

---

## 1. Camera Modes

Each mode defines a distinct perspective. The dungeon master enables whichever modes suit their world. Players switch between enabled modes via keybind or settings menu.

### Isometric

| Property | Value | Description |
|---|---|---|
| Angle | Fixed 45 degrees | Classic OSRS-style fixed overhead angle |
| Pitch Range | Locked | No pitch adjustment |
| Yaw Range | 0, 90, 180, 270 | Snap to 4 cardinal rotations or free rotation (toggle) |
| Zoom Range | 3x--10x tile radius | Adjustable zoom within range |
| Rotation Speed | Instant snap or smooth | Configurable per server |
| Follow Behavior | Lock to player | Camera always centered on player tile |

### Orbital

| Property | Value | Description |
|---|---|---|
| Angle | Variable | Rotate freely around the player character |
| Pitch Range | 10--80 degrees | Adjustable vertical angle, clamped to prevent camera going underground or directly overhead |
| Yaw Range | 0--360 degrees | Full horizontal rotation |
| Zoom Range | 2x--15x tile radius | Closer minimum than isometric for detail work |
| Rotation Speed | Configurable | Degrees per second of mouse drag or stick input |
| Follow Behavior | Lock to player | Camera orbits player position |

### Top-Down

| Property | Value | Description |
|---|---|---|
| Angle | 90 degrees (straight down) | Pure birds-eye view |
| Pitch Range | Locked | No pitch adjustment |
| Yaw Range | 0--360 degrees or locked north | Configurable |
| Zoom Range | 5x--30x tile radius | Wider zoom range for strategy overview |
| Rotation Speed | Configurable | Smooth rotation |
| Follow Behavior | Soft follow or free pan | Camera can drift from player |

### Third Person

| Property | Value | Description |
|---|---|---|
| Angle | Over-shoulder | Camera positioned behind and above the player character |
| Pitch Range | -20--60 degrees | Look slightly below or above the horizon |
| Yaw Range | 0--360 degrees | Full rotation around the player |
| Zoom Range | 1x--5x character height | Close behind to moderate distance |
| Rotation Speed | Configurable | Mouse sensitivity or stick sensitivity |
| Follow Behavior | Lead follow | Camera looks ahead of movement direction |

### First Person

| Property | Value | Description |
|---|---|---|
| Angle | Eye level | Camera at character eye height |
| Pitch Range | -80--80 degrees | Look up or down |
| Yaw Range | 0--360 degrees | Full horizontal look |
| Zoom Range | None | No zoom, fixed FOV (configurable FOV slider) |
| Rotation Speed | Mouse sensitivity | Standard FPS sensitivity curve |
| Follow Behavior | Locked to character head | Camera is the character's eyes |

### Free Cam

| Property | Value | Description |
|---|---|---|
| Angle | Any | No angle constraints |
| Pitch Range | -90--90 degrees | Full vertical range |
| Yaw Range | 0--360 degrees | Full horizontal range |
| Zoom Range | None | Camera flies freely, no zoom concept |
| Rotation Speed | Configurable | WASD fly speed and mouse look speed |
| Follow Behavior | None | Camera is detached from any entity |

### Side-Scrolling

| Property | Value | Description |
|---|---|---|
| Angle | Fixed side view (0 degrees) | 2D platformer perspective |
| Pitch Range | Locked | No pitch adjustment |
| Yaw Range | Locked | No rotation |
| Zoom Range | 3x--12x tile radius | Adjustable zoom |
| Rotation Speed | N/A | No rotation in this mode |
| Follow Behavior | Lock to player horizontal | Camera tracks player X position, configurable Y behavior (snap or smooth) |

### Fixed

| Property | Value | Description |
|---|---|---|
| Angle | Pre-set per room/zone | Resident Evil-style pre-positioned cameras |
| Pitch Range | Locked | Defined per camera placement |
| Yaw Range | Locked | Defined per camera placement |
| Zoom Range | Locked | Defined per camera placement |
| Rotation Speed | N/A | No player-controlled rotation |
| Follow Behavior | None | Camera is fixed in world space, cuts to next camera when player leaves frame |

---

## 2. Camera Controls

Input bindings for camera manipulation. Each control scheme can be enabled or disabled independently. Bindings are remappable.

### Mouse Controls

| Action | Default Binding | Description |
|---|---|---|
| Rotate Camera | Middle-click drag | Hold middle mouse button and drag to rotate horizontally and vertically |
| Zoom In | Scroll up | Zoom camera closer to subject |
| Zoom Out | Scroll down | Zoom camera further from subject |
| Free Look | Right-click drag | Hold right mouse and drag to look around without rotating character |
| Reset Camera | Middle-click | Single middle click to snap camera to default position |

### Keyboard Controls

| Action | Default Binding | Description |
|---|---|---|
| Rotate Left | Left arrow | Rotate camera counterclockwise |
| Rotate Right | Right arrow | Rotate camera clockwise |
| Pitch Up | Up arrow | Tilt camera upward |
| Pitch Down | Down arrow | Tilt camera downward |
| Zoom In | = / + | Zoom camera closer |
| Zoom Out | - | Zoom camera further |
| Reset Camera | Home | Snap to default angle and zoom |
| Toggle Mode | F5 | Cycle through enabled camera modes |

### Touch Controls

| Action | Gesture | Description |
|---|---|---|
| Rotate Camera | Two-finger drag | Rotate camera around subject |
| Zoom | Pinch in / pinch out | Zoom closer or further |
| Pan | Single-finger drag on empty space | Move camera without moving character |
| Reset Camera | Double-tap with two fingers | Snap to default position |

### Controller Controls

| Action | Default Binding | Description |
|---|---|---|
| Rotate Camera | Right stick | Horizontal and vertical camera rotation |
| Zoom In | Right trigger | Hold to zoom closer |
| Zoom Out | Left trigger | Hold to zoom further |
| Reset Camera | Right stick click | Press to snap camera to default |
| Toggle Mode | Select + Right stick click | Cycle through enabled camera modes |

---

## 3. Camera Follow Behavior

How the camera tracks the player or other targets. Each mode has a default follow behavior, but the player or DM can override it.

| Behavior | Description |
|---|---|
| Lock to Player | Camera always centers on the player character. No drift, no lag. Standard for isometric and orbital modes |
| Soft Follow | Camera follows the player but lags behind slightly, creating a cinematic drift. Configurable lag factor (0.0 = instant, 1.0 = maximum drift) |
| Lead Follow | Camera moves ahead of the player's movement direction. Shows more of the world the player is walking toward. Lead distance configurable |
| Group Follow | Camera frames all party members. Zoom adjusts dynamically to keep the entire group visible. Falls back to lock-to-player if solo |
| Target Lock | During combat, camera keeps the active target visible. Camera adjusts angle to frame both player and target. Disengages when combat ends |
| Free Pan | Player can move the camera away from their character. Camera snaps back to player on movement, combat, or after a configurable timeout |

---

## 4. Camera Transitions

How the camera moves from one state to another when modes change, zones change, or scripted events occur.

| Transition Type | Description |
|---|---|
| Snap | Instant change. Camera teleports to new position and angle. No interpolation |
| Smooth Lerp | Camera interpolates from current position to target position over a configurable duration. Default 0.5 seconds |
| Cinematic Pan | Scripted camera movement along a predefined path. Used for cutscenes, zone introductions, boss reveals |
| Zone Transition | Camera adjusts when entering a new area. Indoor zones zoom closer and lower the angle. Outdoor zones zoom wider and raise the angle. Transition speed configurable per zone |

### Transition Properties

| Property | Description |
|---|---|
| Duration | Time in seconds for the transition to complete |
| Easing Curve | Linear, ease-in, ease-out, ease-in-out, custom bezier |
| Interruption | Can the player interrupt a transition by moving or rotating? Configurable per transition type |
| Queue Behavior | If a new transition triggers during an active one: cancel current, queue after, or blend |

---

## 5. Camera Constraints

Limits and collision behavior that prevent the camera from entering invalid states.

| Constraint | Description |
|---|---|
| Min Zoom | Closest the camera can get to the subject. Prevents clipping into character model |
| Max Zoom | Furthest the camera can pull out. Prevents seeing beyond loaded chunks |
| Min Pitch | Lowest angle the camera can tilt. Prevents looking straight up through the floor |
| Max Pitch | Highest angle the camera can tilt. Prevents flipping over the top |
| Rotation Lock | Whether the player can rotate the camera at all. Per-mode toggle |
| Wall Collision | Camera pushes forward when a wall or solid object is between the camera and the player. Prevents seeing through geometry |
| Ceiling Detection | Camera lowers its position when indoors to avoid clipping through the ceiling. Ceiling height read from room data |
| Underwater Camera | When the player is submerged: apply blue color tint, reduce draw distance, add bubble particle effects, optional distortion shader |
| Boundary Clamp | Camera cannot move beyond world boundaries or into unloaded chunks |

---

## 6. Camera Effects

Visual effects applied through the camera. Each effect is independently toggleable by the player and the DM.

### Screen Shake

| Property | Description |
|---|---|
| Trigger | Damage taken, explosions, boss attacks, environmental events |
| Intensity | Configurable magnitude of shake displacement (pixels or world units) |
| Duration | How long the shake lasts in seconds |
| Falloff | Linear or exponential decay of intensity over the duration |
| Direction | Omnidirectional, horizontal only, or vertical only |
| Player Toggle | Players can disable screen shake for accessibility |

### Post-Processing Effects

| Effect | Description |
|---|---|
| Depth of Field | Blur objects far from the focal point. Toggle on/off. Focal distance tracks player or manual |
| Motion Blur | Blur during rapid camera movement. Toggle on/off. Intensity configurable |
| Vignette | Darken the edges of the screen. Intensity configurable. Used for atmosphere or low-HP warning |
| Bloom | Glow on bright light sources. Toggle on/off. Intensity configurable |

### Zone Color Grading

| Zone Type | Color Profile |
|---|---|
| Desert | Warm orange/yellow tint, high contrast |
| Ice / Tundra | Cool blue tint, desaturated |
| Swamp | Green tint, low contrast, slight fog |
| Volcanic | Red/orange tint, high saturation |
| Underground | Dark, desaturated, narrow light radius |
| Forest | Green midtones, soft light |
| Ocean / Beach | Bright, saturated blues and whites |
| Custom | DM defines HSL adjustments per zone |

### Flash Effects

| Flash Type | Description |
|---|---|
| Teleport Flash | White screen flash on teleport arrival. Duration configurable |
| Damage Flash | Red tint flash when taking damage. Intensity scales with damage received |
| Critical Hit Flash | Brief white flash on landing a critical hit |
| Death Flash | Screen fades to black or red on player death |
| Custom Flash | DM triggers custom color and duration flashes via scripting |

### Zoom Pulse

| Trigger | Description |
|---|---|
| Critical Hit | Brief zoom-in and snap-back on landing a critical hit |
| Level Up | Slight zoom-out pulse on level-up with fireworks |
| Boss Phase Change | Zoom pulse to emphasize a boss entering a new phase |
| Rare Drop | Zoom pulse when a rare item drops |
| Custom | DM triggers zoom pulse via scripting with configurable intensity and duration |

---

## 7. Builder Camera

A specialized camera mode for world building. Provides unrestricted movement and additional tools for DMs and builders.

| Feature | Description |
|---|---|
| Free Movement | WASD to fly in the direction the camera faces. No collision, no gravity |
| Vertical Control | Space to ascend, Shift to descend |
| Speed Adjustment | Scroll wheel or keybind to increase or decrease fly speed. Multiple speed presets |
| No Constraints | No zoom limits, no pitch limits, no wall collision. Full freedom |
| Bookmark Positions | Save named camera positions and recall them instantly. Hotkey per bookmark |
| Screenshot Mode | Hide all UI elements. Clean view for screenshots. Keybind toggle |
| Grid Overlay | Toggle tile grid visibility for precision placement |
| Layer Navigation | Jump camera to specific layer heights. Keybind to move up or down one layer |
| Coordinate Display | Always-visible tile coordinate readout at camera center or cursor position |

---

## 8. Spectator Camera

Camera mode for watching other players. Used for tournaments, events, mentoring, and content creation.

| Feature | Description |
|---|---|
| Follow Player | Lock camera to a specific player. Camera uses that player's active camera mode |
| Free Orbit | Orbit freely around the followed player. Independent zoom and angle |
| Switch Target | Cycle between players in a match, event, or world with keybinds |
| Commentator Mode | Smooth cinematic follow with dampened movements. Camera avoids jerky player actions |
| Player List | UI panel showing all players available for spectating with status indicators |
| Delay | Optional configurable delay (seconds) to prevent real-time information abuse in PvP |
| Name Plates | Toggle player names and status indicators visible in spectator view |
| No Interaction | Spectator cannot interact with the world, open doors, pick up items, or be targeted |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `camera.mode_isometric` | true | Classic fixed-angle isometric view |
| `camera.mode_orbital` | true | Free-rotation orbital camera |
| `camera.mode_topdown` | false | Birds-eye top-down view |
| `camera.mode_thirdperson` | false | Over-shoulder third person camera |
| `camera.mode_firstperson` | false | First person perspective |
| `camera.mode_freecam` | true | Detached free-flying camera |
| `camera.mode_sidescrolling` | false | 2D side-scrolling perspective |
| `camera.mode_fixed` | false | Pre-set room-based camera angles |
| `camera.mouse_controls` | true | Mouse-based camera control |
| `camera.keyboard_controls` | true | Keyboard-based camera control |
| `camera.touch_controls` | true | Touch gesture camera control |
| `camera.controller_controls` | true | Gamepad camera control |
| `camera.follow_soft` | true | Cinematic lag follow behavior |
| `camera.follow_lead` | true | Look-ahead follow behavior |
| `camera.follow_group` | true | Party framing follow behavior |
| `camera.follow_targetlock` | true | Combat target lock behavior |
| `camera.follow_freepan` | true | Player can pan camera away from character |
| `camera.transition_smooth` | true | Smooth interpolation between camera states |
| `camera.transition_cinematic` | true | Scripted cinematic camera paths |
| `camera.transition_zone` | true | Automatic camera adjustment on zone entry |
| `camera.wall_collision` | true | Camera pushes forward when blocked by walls |
| `camera.ceiling_detection` | true | Camera lowers indoors to avoid ceiling clipping |
| `camera.underwater_effects` | true | Blue tint and effects when camera is underwater |
| `camera.screen_shake` | true | Screen shake on damage and events |
| `camera.depth_of_field` | false | Background blur effect |
| `camera.motion_blur` | false | Blur during fast camera movement |
| `camera.vignette` | false | Darkened screen edges |
| `camera.bloom` | false | Light source glow effect |
| `camera.zone_color_grading` | true | Per-zone color tint and grading |
| `camera.flash_effects` | true | Screen flash on teleport, damage, and events |
| `camera.zoom_pulse` | true | Brief zoom on critical hits and milestones |
| `camera.builder_camera` | true | Unrestricted camera for world building |
| `camera.spectator_camera` | true | Camera mode for watching other players |
| `camera.spectator_delay` | false | Time delay on spectator feed for anti-cheat |

---

## Views

| View | Description |
|---|---|
| Camera Settings Panel | Player-facing settings for mode selection, sensitivity, zoom range, and effect toggles |
| Camera Keybind Editor | Rebind all camera controls per input device |
| Builder Camera Bookmarks | List of saved camera positions with recall and rename |
| Spectator Player List | Panel showing available players to spectate with status and location |
| Screenshot Gallery | View and manage screenshots taken in screenshot mode |
| Zone Color Grading Editor | DM tool for defining per-zone color profiles with live preview |
| Camera Transition Editor | DM tool for defining cinematic camera paths with keyframes and easing curves |
