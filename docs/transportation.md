# Transportation

Game design document for the Transportation plugin. This system covers all methods of moving around the world, from walking to teleportation to mounts. Each method is a standalone module the dungeon master can enable or disable.

---

## 1. Walking / Running

| Mechanic | Value | Description |
|---|---|---|
| Walk Speed | 1 tile/tick | Base movement speed |
| Run Speed | 2 tiles/tick | Double speed, drains run energy |
| Run Energy | 0--10000 | Depletes while running, regenerates while stationary or walking |
| Weight Impact | Formula-based | Heavier inventory drains energy faster |
| Stamina Potions | Duration-based | Reduce drain rate by a configured percentage for a set number of ticks |
| Graceful Outfit | Set bonus | Reduces weight and boosts energy regeneration rate |
| Rest Mechanic | Sit action | Player sits to regenerate energy at an accelerated rate |
| Toggle Run | Keybind / UI | Switch between walking and running |

---

## 2. Teleportation

### Teleport Sources

| Source | Cost | Description |
|---|---|---|
| Spell Teleports | Runes | Standard, Ancient, and Lunar spellbook teleports to fixed destinations |
| Item Teleports | Charges | Jewelry, tablets, and scrolls with limited or unlimited uses |
| Equipment Teleports | Charges or free | Capes, amulets, and other worn items with teleport options |
| NPC Teleports | GP | Pay an NPC to transport you to a destination |

### Teleport Rules

| Rule | Description |
|---|---|
| Wilderness Level Cap | Cannot teleport above a configurable wilderness level (default 20 for standard, 30 for ancient) |
| Teleblock Spell | Target is blocked from teleporting for a duration |
| Area Restrictions | Certain areas block all teleportation |
| Teleport Delay | Cast time in ticks before the player is moved |
| Teleport Animation | Customizable visual effect per teleport method |
| Cooldown | Optional delay between consecutive teleports |

---

## 3. Fairy Rings

| Feature | Description |
|---|---|
| Network | Ring-shaped portals scattered across the world |
| Code System | 3-letter combination (e.g., CKR, AJR) determines the destination |
| Unlock Requirement | Quest completion required to access the network |
| Staff Required | Dramen or Lunar staff must be equipped to use (toggle) |
| Favorites | Save frequently used codes for quick access |
| Recent Log | List of recently used codes |
| Partial Codes | Enter 2 letters to filter possible destinations |
| POH Ring | Players can build a fairy ring in their house for home access |

---

## 4. Spirit Trees

| Feature | Description |
|---|---|
| Network | Living trees that form a teleport network |
| Planting | Requires farming level and spirit tree seed to grow a new node |
| Grow Slots | Limited number of trees a player can have planted (default 5--6) |
| Travel | Teleport between any two grown spirit trees for free |
| Quest Unlock | Initial access gated behind a quest |
| POH Tree | Players can grow a spirit tree in their house garden |

---

## 5. Boat / Ship Travel

| Method | Description |
|---|---|
| Charter Ships | Pay GP to travel fixed routes between ports |
| Player-Owned Ships | Tied to the Sailing skill, player builds and sails their own vessel |
| NPC Ferries | NPC-operated boats between specific points |
| Canoes | Woodcutting level determines canoe type and travel distance downstream |
| Rowing Boats | Manual short-distance crossings |
| Travel Timing | Configurable -- instant arrival or passage of game time during voyage |

---

## 6. Cart / Minecart

| Feature | Description |
|---|---|
| Rail Network | Fixed tracks connecting stations across the world |
| Unlock | Accessed via quest completion or one-time payment |
| Cost After Unlock | Free or recurring fee per trip (configurable) |
| City Routes | Fast travel between major cities and hubs |
| Underground Routes | Minecart networks connecting mine and dungeon areas |

---

## 7. Agility Shortcuts

| Feature | Description |
|---|---|
| Level Requirement | Each shortcut requires a minimum agility level |
| Permanent Access | Once you meet the level, the shortcut is always available |
| Faster Pathing | Shortcuts provide quicker routes between areas compared to walking around |

### Shortcut Types

| Type | Description |
|---|---|
| Pipe Squeeze | Crawl through a narrow pipe |
| Rock Climb | Scale a rock face |
| Stepping Stones | Cross water via stepping stones |
| Log Balance | Walk across a fallen log |
| Rope Swing | Swing across a gap |
| Crevice Squeeze | Slip through a narrow gap in rock |
| Wall Climb | Climb over a wall |
| Gap Jump | Leap across a gap |

---

## 8. Portals

| Feature | Description |
|---|---|
| Fixed Portals | Two-way teleport points placed in the world |
| Attunement | Must visit a portal once to unlock it for future use |
| POH Portals | Portal rooms in player-owned houses link to world portals |
| Boss Portals | Dedicated portals for entering boss instances |
| Minigame Portals | Quick-travel to minigame lobbies |
| Custom Networks | Dungeon master creates custom portal networks linking any locations |

---

## 9. Lodestones / Home Teleport

| Feature | Description |
|---|---|
| Home Teleport | Free teleport to a configured home location |
| Cooldown | Default 30 minutes between uses |
| No Rune Cost | No materials required |
| Multiple Homes | Unlock additional home teleport locations |
| Lodestone Network | RS3-style system -- unlock a lodestone by visiting it, then teleport there for free anytime |

---

## 10. Mounts

Optional rideable creatures or vehicles.

| Feature | Description |
|---|---|
| Speed Boost | Configurable multiplier over base walk/run speed |
| Mount Types | Horse, camel, eagle, or custom creatures |
| Summon / Dismiss | Call or send away mount via keybind or interface |
| Mount Equipment | Saddle, barding, and other mount-specific gear |
| Restrictions | Cannot mount indoors, in combat, or in certain zones |
| Riding Skill | Ties to the Riding skill if enabled on the server |

---

## 11. Flight

Optional aerial travel system.

| Method | Description |
|---|---|
| Eagle Transport | Ride trained eagles between fixed points |
| Magic Carpet | Pay GP to fly between carpet stations |
| Gnome Glider | Glide between gnome outposts |
| Hot Air Balloon | Travel between balloon stations using logs as fuel |
| Custom Flight Paths | Dungeon master defines additional flight routes |
| Flight Mode | Point-to-point (fixed routes) or free flight (full aerial movement) -- toggle |

---

## 12. Swimming

Optional water traversal system.

| Feature | Description |
|---|---|
| Water Crossing | Traverse water tiles without a bridge or boat |
| Speed | Slower than walking by default (configurable) |
| Breath Meter | Limits time spent underwater before taking damage |
| Equipment Restrictions | Heavy armor prevents swimming |
| Diving | Access underwater areas and content |
| Water Current | Directional current pushes the player while swimming |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| `transport.walking_running` | true | Base movement system |
| `transport.run_energy` | true | Energy drain and regen while running |
| `transport.weight_impact` | true | Inventory weight affects energy drain rate |
| `transport.stamina_potions` | true | Potions that reduce run energy drain |
| `transport.rest_mechanic` | false | Sit to regenerate energy faster |
| `transport.spell_teleports` | true | Spellbook-based teleportation |
| `transport.item_teleports` | true | Jewelry, tablet, and scroll teleports |
| `transport.fairy_rings` | true | Fairy ring code-based teleport network |
| `transport.spirit_trees` | true | Living tree teleport network |
| `transport.boats_ships` | true | Water-based travel routes |
| `transport.carts_minecarts` | true | Rail-based fast travel |
| `transport.agility_shortcuts` | true | Level-gated physical shortcuts |
| `transport.portals` | true | Fixed and attunable portal network |
| `transport.lodestones` | false | RS3-style free teleport network |
| `transport.home_teleport` | true | Free cooldown-based home teleport |
| `transport.mounts` | false | Rideable creatures and vehicles |
| `transport.flight` | true | Aerial travel routes |
| `transport.swimming` | false | Water tile traversal |
| `transport.teleport_restrictions` | true | Zone and level-based teleport blocking |
| `transport.teleblock` | true | PvP spell that prevents teleportation |
| `transport.wilderness_teleport_caps` | true | Wilderness level limits on teleporting |
| `transport.teleport_delay` | true | Cast time before teleport completes |

---

## Views

| View | Description |
|---|---|
| World Map Transport Overlay | All transport nodes, routes, and shortcuts displayed on the world map |
| Teleport Directory | Searchable list of all available teleports by source, destination, and cost |
| Fairy Ring Code Browser | Full list of fairy ring codes with destinations and unlock status |
| Spirit Tree Network | Map of all spirit tree locations and growth status |
| Shortcut List | All agility shortcuts sorted by level requirement |
| Transportation Cost Calculator | Estimate GP and material cost for any journey by method |
