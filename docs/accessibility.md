# Accessibility

Game design document for the Accessibility plugin. This system covers making the game playable for everyone regardless of ability. Every feature listed here is an independent toggle that can be enabled or disabled per player.

---

## Module Toggles

Each toggle enables or disables a subsystem within the accessibility module. Every feature is independently toggleable per player.

| Toggle | Description |
|---|---|
| Colorblind Modes | Palette remapping for protanopia, deuteranopia, tritanopia, achromatopsia |
| High Contrast Mode | Increased border contrast and brighter colors |
| Screen Magnifier | Zoom UI elements beyond default scale |
| Large Text Mode | Scale all text up across the entire UI |
| Custom Font Selection | Override default font with dyslexia-friendly or other fonts |
| Reduced Visual Effects | Disable particles, screen shake, flash effects |
| Visual Sound Indicators | On-screen icons for important sounds |
| Closed Captions | NPC dialogue, environmental sounds, and combat sounds as text |
| Mono Audio | Collapse stereo to a single channel |
| Key Remapping | Every action rebindable to any key |
| One-Handed Mode | All essential keys mapped to one side of the keyboard |
| Mouse-Only Mode | All actions accessible via mouse with no keyboard required |
| Keyboard-Only Mode | All actions accessible via keyboard with no mouse required |
| Auto-Move | Hold key to walk continuously instead of click per tile |
| Sticky Keys | Toggle keys instead of hold (sprint, crouch, prayer) |
| Action Queuing | Buffer multiple inputs for sequential execution |
| Auto-Attack | Continue attacking without re-clicking each tick |
| Simplified UI Mode | Hide advanced panels, show only essentials |
| Step-by-Step Quest Guides | Always-on quest objective guides |
| Task Reminders | Reminder of what you were doing last session |
| Text-to-Speech | Read chat, NPC dialogue, and UI elements aloud |
| Speech-to-Text | Voice input transcribed to chat |
| Screen Reader Compatibility | ARIA labels, focus management, logical tab order |
| Adjustable Combat Difficulty | Damage taken multiplier |
| Adjustable Skill Difficulty | Success rate modifier |
| Death Penalty Reduction | Softer death mechanics |
| Extended Timers | More time for timed content |
| Practice Mode | Attempt content without penalty until ready |
| Assisted Combat | Auto-prayer switching, auto-eating (toggle per feature) |

---

## 1. Visual Accessibility

Features that help players with low vision, colorblindness, or visual processing differences.

| Feature | Description |
|---|---|
| Protanopia Mode | Palette remap for red-green colorblindness |
| Deuteranopia Mode | Palette remap for green-red colorblindness |
| Tritanopia Mode | Palette remap for blue-yellow colorblindness |
| Achromatopsia Mode | Palette remap for total colorblindness (greyscale) |
| High Contrast Mode | Increased border contrast and brighter colors across UI and world |
| Screen Magnifier | Zoom in on UI elements beyond the default scale |
| Large Text Mode | Scale all text up across the entire interface |
| Custom Font Selection | Override default font (includes dyslexia-friendly options) |
| Reduced Visual Effects | Disable particles, screen shake, flash effects, weather animations |
| Color-to-Shape | Chat colors and item rarity colors replaced with icons or shapes |
| Item Outline Mode | Thick borders drawn around items on the ground for visibility |
| NPC/Monster Outline Mode | Thick borders drawn around NPCs and monsters |
| Tile Grid Overlay | Always-visible grid lines on terrain |
| Custom UI Colors | Override any UI element color with a custom hex value |

---

## 2. Audio Accessibility

Features that help players who are deaf, hard of hearing, or have auditory processing differences.

| Feature | Description |
|---|---|
| Visual Sound Indicators | On-screen icons for important sounds (hit taken, item drop, chat message, prayer drain) |
| Closed Captions | NPC dialogue, environmental sounds, and combat sounds displayed as text |
| Mono Audio | Collapse stereo to a single channel for single-ear listening |
| Audio Ducking | Lower music volume automatically when dialogue plays |
| Custom Volume Per Category | Independent volume sliders for 10+ sound categories |
| Vibration Feedback | Controller rumble on events (if controller supported) |
| Sound Direction Indicators | On-screen arrows showing direction of spatial sounds |

---

## 3. Motor Accessibility

Features that help players with limited mobility, dexterity, or fine motor control.

| Feature | Description |
|---|---|
| Key Remapping | Every action rebindable to any key or mouse button |
| One-Handed Mode | All essential keys mapped to one side of the keyboard |
| Mouse-Only Mode | All actions accessible via mouse with no keyboard required |
| Keyboard-Only Mode | All actions accessible via keyboard with no mouse required |
| Click-and-Hold | Drag instead of click for movement and interactions |
| Auto-Move | Hold a key to walk continuously instead of click per tile |
| Sticky Keys | Toggle keys instead of hold (sprint, crouch, prayer) |
| Adjustable Double-Click Speed | Wider or narrower timing window for double-click |
| Reduced Precision Requirement | Larger click targets for items, NPCs, and UI elements |
| Adjustable Input Delay Tolerance | Wider timing windows for tick manipulation |
| Action Queuing | Buffer multiple inputs for sequential execution |
| Auto-Attack | Continue attacking without re-clicking each tick |
| Macro Support | Predefined action sequences (toggle, DM approved only) |

---

## 4. Cognitive Accessibility

Features that help players with attention, memory, processing, or learning differences.

| Feature | Description |
|---|---|
| Simplified UI Mode | Hide advanced panels, show only essentials |
| Step-by-Step Quest Guides | Always-on objective list with next action highlighted |
| Task Reminders | On login, show what you were doing last session |
| Clear Objective Markers | Always-visible markers that cannot be missed |
| Reduced Information Overload | Fewer simultaneous notifications on screen |
| Longer Dialogue Display | NPC text stays on screen longer before fading |
| Replay Dialogue | Re-read any NPC conversation from a log |
| Pause Mode | Pause the game in single-player or designated areas (toggle) |
| Reading Level Selector | Choose between simple and detailed descriptions |
| Glossary | In-game dictionary of game terms accessible from any text |

---

## 5. Communication Accessibility

Features that help players who have difficulty with text input, reading, or language.

| Feature | Description |
|---|---|
| Text-to-Speech | Reads chat, NPC dialogue, and UI elements aloud |
| Speech-to-Text | Voice input transcribed to chat |
| Adjustable Text Speed | Controls typewriter effect speed for NPC dialogue |
| Screen Reader Compatibility | ARIA labels, focus management, logical tab order |
| Quick Chat | Pre-scripted messages for players who cannot type quickly |
| Emoji/Reaction-Only Mode | Communicate with icons only |
| Translation | Real-time language translation for chat messages |

---

## 6. Difficulty Accessibility

Features that adjust game difficulty without changing the core experience. DM configures which of these are available.

| Feature | Description |
|---|---|
| Adjustable Combat Difficulty | Damage taken multiplier (0.5x to 2.0x) |
| Adjustable Skill Difficulty | Success rate modifier for gathering, crafting, etc. |
| Death Penalty Reduction | Softer death mechanics (keep more items, shorter respawn) |
| Extended Timers | More time for timed content (puzzles, boss phases, events) |
| Skip Difficult Content | Bypass puzzles or bosses for story progression (toggle) |
| Practice Mode | Attempt any content without penalty until ready |
| Assisted Combat | Auto-prayer switching, auto-eating (toggle per individual feature) |

---

## Presets

Pre-configured accessibility profiles that enable a recommended set of features. Players can start from a preset and customize further.

| Preset | Enabled Features |
|---|---|
| Low Vision | High contrast, large text, screen magnifier, item outlines, NPC outlines, tile grid overlay |
| Colorblind | Colorblind mode (player selects type), color-to-shape, item outlines |
| Motor Impaired | Key remapping, auto-move, sticky keys, action queuing, auto-attack, larger click targets |
| Hearing Impaired | Visual sound indicators, closed captions, mono audio, sound direction indicators |
| Cognitive | Simplified UI, step-by-step guides, task reminders, longer dialogue, pause mode, glossary |
| Full Assist | All features enabled at their most supportive settings |

---

## Views

Player-facing views for configuring accessibility options.

| View | Description |
|---|---|
| Accessibility Settings Panel | All options organized by category (visual, audio, motor, cognitive, communication, difficulty) |
| Feature Preview | See how each option changes the game before enabling it |
| Preset Selector | Choose from pre-configured accessibility profiles |
| Custom Preset Builder | Save your own combination of settings as a named preset |
