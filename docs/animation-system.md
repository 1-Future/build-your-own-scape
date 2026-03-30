# Animation System

How everything moves, reacts, and comes alive. From a character swinging a sword to a flag blowing in the wind to a spell projectile traveling across the screen. This doc covers the technical animation framework -- what types of animation exist, how they sync with the tick system, and what tools the DM/animator uses.

---

## 1. Animation Approaches

DM picks per world or mixes. Different approaches suit different aesthetics, performance budgets, and content creation workflows.

| Approach | Description | Performance | Fidelity | Best For |
|---|---|---|---|---|
| Keyframe (Classic) | Pre-authored poses at specific frames, engine interpolates between them. RuneScape's core approach. Frame data stored per sequence. | Low cost | Depends on frame count | Retro/PS1 aesthetic, low-poly characters, OSRS-style |
| Skeletal | Bones/armature drive mesh deformation. Animations stored as bone transforms per frame. Industry standard. | Medium cost | High | Modern characters, complex creatures, humanoids |
| Procedural | Animations calculated at runtime from rules/physics. Not pre-authored. | Variable | Dynamic, imperfect | Ragdoll death, IK foot placement, chain/rope/tail physics |
| Sprite Sheet | 2D image frames played in sequence. No 3D bones at all. | Very low cost | 2D only | Retro 2D games, UI animations, particle sprites |
| Motion Capture | Real human/animal movement recorded and applied to rig. | High quality source | Very high | Cinematic sequences, premium content |
| Blend Tree | Multiple animations blended based on parameters (speed, direction, slope). Smooth transitions between states. | Medium | Smooth | Movement systems, directional combat |
| Hybrid | Mix approaches per entity. Skeletal body + procedural tail + keyframe facial. | Varies | High | Complex creatures, bosses with many parts |

---

## 2. Animation Categories

### Character Animations

Applied to player and NPC rigs.

| Category | Animations | Typical Count |
|---|---|---|
| Idle | Standing, sitting, resting, leaning, sleeping, AFK (bored fidget) | 5-15 per character |
| Movement | Walk, run, walk backward, strafe left/right, shuffle, crawl, swim, climb, fall, land | 10-20 |
| Combat (Melee) | Slash, stab, crush, kick, punch, block, dodge, parry, riposte. Per weapon type -- sword slash differs from axe slash | 20-50 |
| Combat (Ranged) | Draw bow, fire arrow, throw, reload crossbow, aim. Per weapon type | 10-20 |
| Combat (Magic) | Cast (arms raised), channel (sustained), release. Per spell school | 10-20 |
| Combat (Defend) | Shield block, dodge roll, brace, deflect | 5-10 |
| Death | Fall forward, fall backward, crumble, dissolve, explode, dramatic, comedic | 5-10 |
| Skill/Gathering | Mine (pickaxe swing), chop (axe swing), fish (cast line, reel), cook (stir pot), smith (hammer anvil), craft (hands working), pray (kneel), alch (hands glow) | 1-3 per skill |
| Emote | Wave, bow, dance (multiple types), clap, cry, laugh, think, shrug, yes, no, angry, cheer, beckon, panic, sit, push-up, headbang, salute | 50-200+ |
| Interaction | Open door, climb ladder, pull lever, push button, sit in chair, drink from fountain, read book, talk gesture | 15-30 |
| Eating/Drinking | Eat food, drink potion, eat combo (fast eat) | 3-5 |
| Teleport | Cast departure, travel (spinning/rising), arrival landing | 3-10 per teleport style |
| Status Effects | Poisoned wobble, stunned stars, frozen (ice encased), burning (on fire), sleeping (Zzz), confused | 5-10 |
| Facial | Talk (mouth movement), smile, frown, surprised, angry, scared -- for dialogue close-ups | 5-10 |

### Object Animations

Applied to world objects.

| Category | Examples | Loop |
|---|---|---|
| Mechanical | Windmill blades rotating, waterwheel turning, cog spinning, piston pumping, pendulum swinging, conveyor moving | Loop |
| Environmental | Flag waving, banner fluttering, curtain swaying, rope swinging, hanging sign creaking | Loop |
| Nature | Tree swaying, leaves rustling, flowers bobbing, grass waving, water rippling, fire flickering | Loop |
| Interactive State | Door opening/closing, gate raising/lowering, lever pulling, chest lid opening, drawbridge lowering | Play once per state change |
| Resource Node | Rock cracking (being mined), tree shaking (being chopped), fishing spot splash, farming patch growing | Loop while being interacted with |
| Ambient Life | Birds pecking, squirrels climbing, fish jumping, butterflies fluttering, bees buzzing around flowers | Loop |
| Lighting | Torch flame flicker, candle glow pulse, lantern sway, brazier ember rise, magical orb orbit | Loop |
| Weather | Rain falling, snow drifting, sandstorm particles, leaves blowing, lightning flash | Loop |

### Effect Animations (Spot Anims / VFX)

| Category | Examples | Behavior |
|---|---|---|
| Spell Cast | Hands glow, rune circle appears at feet, energy gathering at staff tip | Plays once on cast tick |
| Projectile | Arrow in flight, spell bolt traveling, thrown axe spinning, cannonball arcing | Travels from source to target over hit delay ticks |
| Spell Impact | Splash on target (fire burst, ice shatter, earth eruption, water splash), heal glow, curse cloud | Plays once on hit tick at target position |
| AoE Ground | Ice barrage zone, poison cloud, fire area, blessed ground, corruption zone | Persists at location for duration |
| Buff/Aura | Stat boost sparkle, protection prayer glow, overload shake, regeneration pulse | Loops while buff active, attached to entity |
| Overhead Icon | Prayer icon, skull, hitpoints bar, combat level | Persistent, follows entity |
| Hit Effects | Hitsplat number (red damage, blue block, green poison, purple heal), blood splash, armor spark | Plays once per hit |
| Death Effects | Bone scatter, soul rising, loot beam, body fade | Plays once on death |
| Celebration | Level-up fireworks, quest complete banner, achievement popup, rare drop beam | Plays once on trigger |
| Environmental FX | Campfire smoke, chimney smoke, waterfall mist, steam vent, lava bubble, magical portal swirl | Loop at position |

---

## 3. Tick Synchronization

All animations must work within the tick system. The tick is the heartbeat -- animations are triggered, synced, and resolved on tick boundaries.

### Animation-to-Tick Mapping

| Event | Tick Behavior |
|---|---|
| Attack animation | Starts on attack tick, plays for weapon speed duration |
| Hit resolution | Damage calculated on attack tick, hitsplat appears after hit delay |
| Eat animation | Starts on eat tick, takes 3 ticks (1.8s) |
| Skill action | Starts on action tick, loops for action duration |
| Movement | Position updates every tick, client interpolates between ticks for smooth movement |
| Prayer toggle | Activates on the tick it is clicked, visual icon appears immediately |
| Death | Triggers on the tick HP reaches 0 |
| Teleport | Cast animation starts, departure after cast time ticks, arrival instant |

### Hit Delay System (Projectile Timing)

| Weapon Type | Formula | Example |
|---|---|---|
| Melee | 0 ticks (instant) | Sword hits immediately |
| Bows/Crossbows | `1 + floor((3 + distance) / 6)` | 5 tiles = 2 ticks delay |
| Thrown weapons | `1 + floor(distance / 6)` | 5 tiles = 1 tick delay |
| Standard Magic | `1 + floor((1 + distance) / 3)` | 5 tiles = 3 ticks delay |
| Special (fixed) | 2 ticks regardless of distance | Some spells, special attacks |

Distance measured in tiles, Chebyshev distance (max of x-diff and y-diff).

### Client Interpolation

| Property | Description |
|---|---|
| Tick rate | Server sends position updates every tick (600ms) |
| Smoothing | Client smoothly interpolates between last position and new position |
| Lerp factor | Configurable (default 0.15 -- smooth but responsive) |
| Animation speed | Adjustable to match tick duration |
| Per-entity tracking | Separate smooth trackers for players vs NPCs vs projectiles |

---

## 4. Rigging and Bones

### Bone System

| Property | Description |
|---|---|
| Bone count limit | Max bones per character rig (RS3 = 200 max, configurable by DM) |
| Bone hierarchy | Parent-child tree (spine to shoulders to arms to hands to fingers) |
| Weight painting | How much each bone influences nearby vertices (smooth deformation) |
| Bone constraints | Limits on rotation/position per bone (elbow cannot bend backward) |
| IK chains | Inverse kinematics -- move hand, arm follows automatically |
| FK chains | Forward kinematics -- rotate shoulder, arm follows down |

### Standard Humanoid Rig

| Chain | Bones |
|---|---|
| Spine | Root, Hips, Spine 1, Spine 2, Spine 3, Neck, Head (+ Jaw for talk animation) |
| Left Leg | Hip, Thigh, Knee, Shin, Ankle, Foot, Toe |
| Right Leg | Hip, Thigh, Knee, Shin, Ankle, Foot, Toe |
| Left Arm | Shoulder, Upper Arm, Elbow, Forearm, Wrist, Hand, Fingers |
| Right Arm | Shoulder, Upper Arm, Elbow, Forearm, Wrist, Hand, Fingers |
| Optional | Cape bones, hair bones, equipment bones (weapon mount, shield mount) |

### Creature Rig Variations

| Creature Type | Rig Modifications |
|---|---|
| Quadruped | 4 legs from hips, horizontal spine, tail chain |
| Bird/Flying | Wings (3-5 bones each), tail fan, talons |
| Snake/Worm | Long spine chain (20+ bones), no limbs |
| Spider/Insect | 6-8 legs, mandibles, abdomen |
| Dragon | Quadruped + wings + tail + jaw (fire breathing) |
| Ghost/Ethereal | Simplified -- floats, no leg IK needed, flowing cloth |
| Multi-headed | Spine branches at neck to multiple head chains |
| Giant/Boss | Same as humanoid but more bones for detail (facial, fingers, toes) |
| Mechanical | Rigid bodies with hinge/piston joints instead of smooth deformation |

### Mount Points

Where equipment attaches to the rig.

| Point | Location | Used For |
|---|---|---|
| Right Hand | Wrist bone | Weapons, tools |
| Left Hand | Wrist bone | Shields, off-hand, lanterns |
| Head | Top of head bone | Helmets, hats, crowns |
| Back | Upper spine | Capes, backpacks, quivers, wings |
| Waist | Hip bone | Belt items, pouches |
| Shoulders | Shoulder bones | Pauldrons, pets perched |
| Feet | Foot bones | Boots (model swap, not mount) |

---

## 5. Animation State Machine

Characters have an animation state that determines which animation plays. States transition based on game events.

### State Transitions

| From State | Trigger | To State |
|---|---|---|
| Idle | Player clicks to move | Walk/Run |
| Walk/Run | Player stops | Idle |
| Idle/Walk | Attack action | Attack (weapon-specific) |
| Attack | Attack animation completes | Idle (or next attack if queued) |
| Any | Eat food | Eat (interrupts current) |
| Any | HP reaches 0 | Death |
| Any | Teleport cast | Teleport Departure |
| Teleport Departure | Cast time complete | Teleport Arrival (at destination) |
| Idle | Skill action | Skill Animation (loops) |
| Skill Animation | Action completes or interrupted | Idle |
| Any | Emote triggered | Emote (plays once, returns to previous) |
| Any | Stunned | Stunned (locked, cannot act) |
| Any | Frozen | Frozen (ice visual, cannot move) |
| Idle | No input for X seconds | AFK Idle (fidget, bored animations) |

### Priority System

Higher priority animations override lower ones.

| Priority | State |
|---|---|
| 1 (highest) | Death |
| 2 | Stun/Freeze (locked states) |
| 3 | Teleport |
| 4 | Combat (attack, defend, special) |
| 5 | Eat/Drink |
| 6 | Skill actions |
| 7 | Emotes |
| 8 | Movement |
| 9 (lowest) | Idle |

### Blending

| Feature | Description |
|---|---|
| Crossfade | Smooth transition between states over configurable frames |
| Upper/lower body split | Separate animations for upper and lower body (attack while running -- upper plays attack, lower plays run) |
| Additive layers | Breathing animation layered on top of any state |
| Override layers | Facial animation plays independently of body |

---

## 6. Projectile System

### Projectile Properties

| Property | Type | Description |
|---|---|---|
| Projectile ID | Integer | Unique identifier |
| Model | Model ID | 3D model of the projectile (arrow, spell bolt, thrown axe) |
| Start Height | Integer | Y offset at source (hand height, staff tip height) |
| End Height | Integer | Y offset at target |
| Speed | Integer | Travel speed (affects arc timing, not hit delay -- hit delay is game logic) |
| Arc | Float | Height of parabolic arc (0 = straight line, higher = more arc) |
| Rotation | Boolean | Does projectile rotate in flight (spinning axe vs straight arrow) |
| Trail Effect | SpotAnim ID | Visual trail behind projectile (magic sparkle, fire trail, smoke) |
| Impact Effect | SpotAnim ID | Visual on hit (splash, explosion, shatter) |
| Sound (launch) | Sound ID | Audio on fire |
| Sound (impact) | Sound ID | Audio on hit |
| Homing | Boolean | Projectile tracks target if they move (magic typically homes, arrows do not) |

### Projectile Types

| Type | Arc | Speed | Rotation | Trail |
|---|---|---|---|---|
| Arrow | Low arc | Fast | No (straight) | None |
| Bolt | Flat | Very fast | No | None |
| Spell (fire) | Flat | Medium | No | Fire trail |
| Spell (ice) | Flat | Medium | No | Ice crystals |
| Thrown axe | Medium arc | Medium | Yes (spinning) | None |
| Cannonball | High arc | Slow | No | Smoke |
| Javelin | Medium arc | Medium | No | None |
| Dart | Flat | Very fast | No | None |
| Chinchompa | Medium arc | Medium | No | Fuse spark |
| Special (magic) | Varies | Varies | Varies | Custom per spell |

### AoE Ground Effects

| Property | Type | Description |
|---|---|---|
| Effect ID | Integer | Unique identifier |
| Shape | Enum | Circle, Square, Cone, Line, Ring |
| Size | Integer | Radius or length in tiles |
| Duration | Ticks | How long it persists |
| Damage per tick | Integer | Ongoing damage while standing in it |
| Visual | SpotAnim ID | The visual effect on the ground |
| Telegraph | Boolean | Shows warning indicator before activating (boss mechanics) |
| Telegraph duration | Ticks | How long warning shows before damage starts |
| Friendly fire | Boolean | Affects the caster's allies |
| Stacking | Boolean | Multiple AoE effects stack damage |

---

## 7. Multi-Model Animation Sync

Complex objects are multiple models animated together.

### Sync Methods

| Method | Description | Example |
|---|---|---|
| Shared Timer | All models reference the same animation clock. Rotate at same speed. | Windmill -- 4 blades on one timer |
| Parent-Child | Child model follows parent's transform + its own animation. | Horse + rider -- rider moves with horse |
| Trigger-Based | One model's animation triggers another's. | Lever pull triggers gate open |
| Physics Chain | Connected models with physics constraints. | Chain links, rope, hanging bridge |
| Bone-Attached | One model mounts to another's bone. | Weapon in hand, pet on shoulder |

### Multi-Part Boss Example

A dragon boss might be composed of:

| Part | Type | Animation Method |
|---|---|---|
| Body | Main model | Skeletal animation, drives state machine |
| Wings | Child model | Bone-attached to body, independent flap cycle |
| Tail | Child model | Chain simulation, procedural physics |
| Jaw | Child model | Bone-attached, separate open/close for fire breath |
| Fire breath | Effect | Projectile/AoE spawned from jaw bone on attack tick |
| Wing gust | Effect | AoE ground effect spawned from wing bones |

All synced through the parent body's animation state machine -- when body enters "fire breath attack" state, jaw opens, fire spawns from jaw bone, wings fold back.

---

## 8. OSRS vs RS3 Animation Differences

| Feature | OSRS | RS3 |
|---|---|---|
| Frame count | ~110,000 frame maps | ~1,000,000 frame maps |
| Bone limit | Low poly, fewer bones | 200 bone max per character |
| Interpolation | Tweening between keyframes | Full skeletal with blend trees |
| Combat animations | 1 animation per attack style | Unique animation per ability (hundreds) |
| Animation overrides | Walk only (recently added) | Full override system -- walk, run, rest, skill, teleport, combat, death |
| Projectiles | Simple sprite/model travel | Complex particle trails + physics |
| Ground effects | Basic spot anims | Full AoE zones with telegraph indicators |
| Facial animation | None | Basic lip sync + expressions |
| Character scale | Fixed | Variable (some bosses scale) |
| Animation smoothing | Client-side toggle (recent) | Built-in, always smooth |
| Ability animations | N/A (no abilities) | Every ability has unique cast + impact animation |
| Channel animations | N/A | Sustained animations during channeled abilities |

DM chooses a fidelity target -- OSRS simplicity or RS3 richness or anywhere between. The animation system supports all levels. A retro world can use 5 combat animations total. A modern world can have unique animations per weapon per ability.

---

## 9. Animation Fidelity Tiers

DM selects an animation tier for their world -- affects how many animations are expected and how smooth they are.

| Tier | Description | Animations Per Character | Bones | Style |
|---|---|---|---|---|
| Retro | Minimal frames, chunky movement, PS1 feel | 10-20 | 10-20 | OSRS / classic |
| Standard | Smooth basics, good variety | 30-60 | 30-50 | RS3 / modern indie |
| High | Fluid movement, weapon-specific, facial | 80-150 | 50-100 | AAA quality |
| Cinematic | Motion captured, full facial, physics cloth | 200+ | 100-200 | Cutscene quality |

Lower tiers = faster to create content, less performance cost, retro aesthetic. Higher tiers = more work per character, higher fidelity, modern feel.

---

## 10. Animation Creation Pipeline

### Workflow

| Step | Description |
|---|---|
| 1. Model | Create the character/object in a 3D tool (Blender, Maya) |
| 2. Rig | Add bones following the standard rig template or a custom layout |
| 3. Skin | Weight paint -- define how the mesh deforms with each bone |
| 4. Animate (Pitch) | Rough key poses, test the concept |
| 5. Animate (Blocking) | Key frames that tell the story of the motion |
| 6. Animate (Splining) | In-between frames, energy, inertia |
| 7. Animate (Polish) | Final 5-10%, all details |
| 8. Export | Convert to engine format (per-frame bone transforms + timing) |
| 9. Register | Assign ID in animation registry, set category, link to entity |
| 10. Configure | Wire into state machine (which state triggers this animation) |
| 11. Test | Verify in-game via play mode or preview tool |

### Animation Editor Tool

| Feature | Description |
|---|---|
| Timeline | Horizontal time axis with keyframe markers |
| Bone selector | Click bone in viewport or list to select |
| Transform gizmo | Move, rotate, scale selected bone |
| Onion skinning | Show ghost frames before/after current frame |
| Playback | Play, pause, scrub, loop, speed control |
| Blend preview | Preview how two animations blend together |
| State machine editor | Flowchart of states and transitions |
| Import | Load from Blender/Maya/FBX/GLTF |
| Export | Save to engine format |
| Animation browser | Search existing animations by category |
| Copy/mirror | Copy animation to opposite side (left hand to right hand) |
| Retarget | Apply one character's animation to a different rig |

---

## 11. Performance Considerations

| Factor | Impact | Mitigation |
|---|---|---|
| Bone count | More bones = more CPU per frame | Cap bones per entity (200 max) |
| Visible entities | Many animated entities on screen | LOD -- reduce animation quality at distance |
| Particle count | Many particles = GPU load | Cap particles, reduce at distance |
| Animation blending | Multiple layers blending = CPU | Limit blend layers (max 3-4) |
| Physics simulation | Ragdoll/chain simulation = CPU | Only simulate nearby entities |
| Shadow casting | Animated entities cast dynamic shadows | Bake shadows for distant/static entities |

### LOD (Level of Detail) for Animation

| Distance | Animation Quality |
|---|---|
| Close (0-5 tiles) | Full quality, all bones, all blend layers |
| Medium (5-15 tiles) | Reduced bones, simpler blend, lower frame rate |
| Far (15-30 tiles) | Key poses only, no blending, minimal bones |
| Very far (30+ tiles) | Billboard sprite or static pose |
| Off-screen | No animation processing |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `animation.approach` | Keyframe | Primary animation method (keyframe, skeletal, procedural, sprite, hybrid) |
| `animation.fidelity_tier` | Standard | Retro, Standard, High, Cinematic |
| `animation.bone_limit` | 200 | Maximum bones per entity |
| `animation.client_interpolation` | true | Smooth movement between ticks |
| `animation.interpolation_factor` | 0.15 | How fast client catches up to server position |
| `animation.smoothing` | true | Tween between keyframes for smoother playback |
| `animation.upper_lower_split` | false | Separate animations for upper and lower body simultaneously |
| `animation.facial` | false | Face bones and expressions |
| `animation.procedural_chains` | true | Physics simulation for tails, capes, chains, ropes |
| `animation.ragdoll_death` | false | Physics-based death animation instead of canned |
| `animation.ik_foot_placement` | false | Feet adjust to terrain slope |
| `animation.projectile_system` | true | Visible projectiles traveling from source to target |
| `animation.projectile_homing` | per weapon | Projectiles track moving targets |
| `animation.aoe_ground_effects` | true | Persistent visual effects on terrain |
| `animation.aoe_telegraph` | true | Warning indicators before AoE activates |
| `animation.hit_delay_formulas` | OSRS default | Distance-based projectile hit timing |
| `animation.lod` | true | Reduce quality at distance |
| `animation.state_machine` | true | State-based animation transitions |
| `animation.blending` | true | Smooth crossfade between states |
| `animation.overrides` | false | Players/DM can override default animations |
| `animation.multi_model_sync` | true | Synchronized animation across multi-part objects |
| `animation.particle_cap` | 500 | Maximum simultaneous particles |
| `animation.emotes` | true | Player-triggered expression animations |
| `animation.afk_idle_variants` | true | Special idle animations after inactivity |
| `animation.mounts` | false | Riding animations (requires mount system) |
| `animation.swim` | false | Water movement (requires swim system) |
| `animation.climb` | false | Vertical movement (requires climb system) |

---

## 12. Cutscene System

Scripted non-interactive sequences used for storytelling, quest progression, and dramatic moments.

### Cutscene Types

| Type | Description |
|---|---|
| In-Engine | Game camera moves, NPCs act out choreography, player watches. Same graphics as gameplay. Most common |
| Player Teleport | Player secretly teleported to a staged area off-screen. Camera focuses on NPC actors. Player character hidden |
| Artwork/Slideshow | Static illustrated artwork panels with voiceover narration. Used for lore dumps and backstory. No 3D rendering |
| Cinematic | Heavily scripted sequences with custom camera paths, special animations, lighting changes, and music |
| Transport | Travel sequences (boat ride, eagle flight, teleport journey). Player frozen during transit animation |
| Hybrid | Mix of in-engine action and artwork panels within the same sequence |

### Cutscene Components

| Component | Description |
|---|---|
| Camera Path | Scripted positions, angles, zoom, pan, dolly, follow-target. Keyframed or spline-based path |
| Camera Speed | Constant speed or variable with easing (slow start, fast middle, slow end) |
| NPC Choreography | Move NPC to position, play animation, face direction, speak line, wait, loop |
| Player Character | Hidden (off-screen), visible but frozen (watching), auto-dialogue (speaks without player input) |
| Player Controls | All disabled during cutscene except Examine. Optional skip button |
| Dialogue | Chat bubbles above characters, text box at bottom, voice acted audio, or combination |
| Music | Cutscene-specific track override. Crossfade from area music to cutscene music and back |
| Environment | Lighting changes, weather changes, objects appear/disappear, terrain modifications, time-of-day shift |
| Letterbox | Optional cinematic widescreen black bars (top and bottom) for dramatic framing |
| Transitions | Fade to black, fade to white, cut, dissolve, wipe between scenes |
| Duration | Measured in ticks. Total length of the sequence |
| Skip Option | Toggle per cutscene -- DM decides if skippable. Hold button to skip (not instant, prevents accidental skip) |
| Replay | Some cutscenes can be re-watched from specific NPCs or objects. Toggle per cutscene |

### Cutscene Scripting (how DM builds a cutscene)

A cutscene is a sequence of timed actions:

| Action | Parameters | Description |
|---|---|---|
| camera_move | x, y, z, duration, easing | Move camera to position over duration |
| camera_look | target_entity or x,y,z, duration | Point camera at target |
| camera_zoom | level, duration | Zoom in or out |
| camera_shake | intensity, duration | Screen shake effect |
| npc_move | npc_id, x, y, speed | Walk NPC to position |
| npc_animate | npc_id, animation_id | Play animation on NPC |
| npc_face | npc_id, direction or target | Turn NPC to face direction |
| npc_speak | npc_id, text, voice_id | NPC says line (chat bubble + optional voice) |
| player_speak | text, voice_id | Player auto-says a line |
| player_hide | boolean | Show or hide player character |
| wait | ticks | Pause before next action |
| music_play | track_id, fade_duration | Change music with crossfade |
| sound_play | sound_id | Play sound effect |
| object_spawn | object_id, x, y | Make object appear |
| object_remove | object_id | Make object disappear |
| lighting_set | color, intensity, duration | Change scene lighting |
| weather_set | type, intensity | Change weather (rain, snow, fog) |
| letterbox | on/off, duration | Toggle cinematic bars |
| fade | color, duration | Fade screen to color (black, white) |
| effect_play | spotanim_id, x, y | Play visual effect at position |
| dialogue_box | text, portrait_id, name | Show dialogue box with portrait |
| artwork_show | image_id, duration | Display artwork panel (slideshow type) |
| branch | condition, scene_id | Conditional branch to different cutscene path |
| end | | End cutscene, return control to player |

### Cutscene Editor Tool

| Feature | Description |
|---|---|
| Timeline | Horizontal time axis with action blocks placed at specific ticks |
| Action palette | Drag-and-drop action blocks onto timeline |
| Camera preview | Live preview of camera path in 3D viewport |
| NPC placement | Position actors in the scene before scripting movement |
| Dialogue editor | Write lines with portrait selection and voice assignment |
| Playback | Play, pause, scrub, loop the cutscene in real-time |
| Branch editor | Visual flowchart for conditional paths |
| Export | Save cutscene as reusable asset |
| Import artwork | Load illustration panels for artwork-type cutscenes |

---

## Views

| View | Description |
|---|---|
| Animation Browser | Search all animations by category, entity, state |
| Animation Editor | Timeline, bone manipulation, keyframe editing |
| State Machine Editor | Visual flowchart of animation states and transitions |
| Projectile Previewer | Test projectile visuals with distance/arc settings |
| AoE Previewer | Preview ground effects with size/shape/duration |
| Blend Tester | Preview how two animations blend together |
| Rig Inspector | View bone hierarchy, weights, constraints |
| Performance Monitor | Bone count, particle count, blend layer count per frame |
| Animation Retargeter | Apply animations from one rig to another |
| LOD Preview | See animation at different distance tiers |
