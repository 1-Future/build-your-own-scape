# Settlement Design

A guide for designing towns, cities, and settlements that feel alive, play well, and serve both narrative and gameplay purposes. Covers layout patterns, district zoning, road hierarchy, NPC population, player flow, and pre-ship validation. Sits on top of terrain (terrain.md), uses buildings (buildings.md), populates with NPCs (npcs.md), and connects to shops (shops.md) and transportation (transportation.md).

---

## Module Toggles

Each toggle enables or disables a subsystem within the settlement design module. Servers can turn off features they do not need.

| Toggle | Default | Description |
|---|---|---|
| Settlement Templates | On | Pre-built settlement layouts available to stamp down and customize |
| District System | On | Named districts with distinct visual properties and service rules |
| NPC Population System | On | Automated NPC placement by district type and density rules |
| Settlement Progression | On | Settlements change state based on quest completion, events, and player actions |
| Settlement States | On | Thriving, declining, under attack, rebuilding, festival, and abandoned states |
| Layout Pattern Guides | On | Overlay guides for organic, grid, radial, and other layout patterns |
| Road Hierarchy | On | Different road widths and materials by importance level |
| Landmark System | On | Designated landmark objects used for navigation and orientation |
| Service Proximity Checker | On | Warns the dungeon master if essential services are too far apart |
| Settlement Checklist | On | Pre-ship checklist validation before marking a settlement complete |
| Custom Templates | On | Dungeon masters can save and share their own settlement templates |

---

## 1. Settlement Scale

Scale determines what a settlement can support. Bigger is not always better -- a well-designed hamlet is more memorable than a bloated city full of empty buildings.

| Scale | Buildings | Population | Examples | Typical Services |
|---|---|---|---|---|
| Camp | 1-3 | 2-10 | Hunter camp, bandit hideout, fishing spot | Maybe one vendor, a campfire |
| Hamlet | 4-8 | 10-30 | Crossroads hamlet, farmstead cluster | General store, maybe a well |
| Village | 9-25 | 30-150 | Starting village, fishing village | Bank, general store, a few shops, altar |
| Town | 25-60 | 150-1000 | Regional hub, trade town | Bank, GE, multiple shops, guild, tavern, temple |
| City | 60-200 | 1000-10000 | Capital city, fortress city | Everything -- multiple banks, districts, castle, cathedral |
| Metropolis | 200+ | 10000+ | Imperial capital, mega-city | Multiple towns merged, transport system, sprawling |

### Choosing the Right Scale

Start small. A village with 15 buildings that each have a purpose is better than a city with 100 buildings where 80 are empty filler. Scale up only when:

- The settlement is a major hub on a trade route or at a strategic location
- You have enough quests, NPCs, and services to fill the space
- The world narrative demands it (capital of a kingdom, port city for a continent)

If a building does not have a clear purpose (shop, quest location, housing, atmosphere), cut it. Players notice empty buildings and they make the world feel hollow.

---

## 2. Settlement Layout Patterns

Layout determines how streets and buildings are arranged. Choose based on the settlement's history and geography.

### Organic / Grown

Streets follow old paths. Buildings placed as needed over centuries. Winding roads, irregular blocks, dead ends, narrow alleys. Feels old and lived-in.

| Property | Detail |
|---|---|
| When to use | Medieval towns, old villages, settlements that grew naturally over time |
| Key features | Main street from gate to center, branching side streets, no grid, surprises around corners, market square that happened by accident |
| Navigation feel | Getting lost is part of the charm, but main road should always be findable |
| OSRS parallels | Varrock (organic around the castle), Ardougne, Pollnivneach |

### Grid / Planned

Streets cross at right angles. Regular block shapes. Planned districts. Efficient but can feel sterile without variation.

| Property | Detail |
|---|---|
| When to use | Roman-style cities, military camps, new colonies, utopian settlements |
| Key features | Main avenues (wider), cross streets (narrower), public buildings at intersections, parks at regular intervals |
| Navigation feel | Easy to navigate, predictable, can number blocks for addresses |
| Tip | Break the grid slightly in one or two places to avoid feeling artificial |

### Radial / Hub

Streets radiate outward from a central point. Concentric rings create natural district boundaries.

| Property | Detail |
|---|---|
| When to use | Cities built around a fortress, capital cities, settlements centered on a landmark |
| Key features | Central landmark visible from everywhere, ring road connecting radial streets, inner ring for wealthy/government, outer ring for common/industrial |
| Navigation feel | Central landmark always visible for orientation, easy to know how far from center you are |

### Linear / Road Town

Buildings line both sides of a single main road. One street wide.

| Property | Detail |
|---|---|
| When to use | Trade route towns, mining towns, canyon/valley settlements, bridge towns |
| Key features | Everything on the main road, easy to navigate, one entrance one exit, feels like passing through |
| Navigation feel | Impossible to get lost, but also impossible to explore |
| Tip | Add one or two side alleys for hidden content |

### Fortified / Walled

Settlement enclosed by walls with limited gates. Space is premium inside.

| Property | Detail |
|---|---|
| When to use | Border towns, towns under threat, medieval cities, military outposts |
| Key features | Walls define the boundary, gates funnel traffic, buildings packed tight inside, suburbs/slums grow outside walls, watchtowers at corners |
| Navigation feel | Walls provide constant orientation, gate locations become landmarks |

### Island / Cluster

Buildings clustered in groups with open space between. Multiple small clusters rather than one continuous settlement.

| Property | Detail |
|---|---|
| When to use | Island towns, lakeside villages, trading posts, nomadic camps |
| Key features | Each cluster has a purpose (fishing cluster near water, farming cluster near fields), paths connect clusters, central meeting area |
| Navigation feel | Navigating between clusters, each cluster is a mini-destination |

### Terraced / Hillside

Buildings step up a hillside or cliff, connected by stairs and ramps.

| Property | Detail |
|---|---|
| When to use | Mountain towns, coastal cliffs, canyon walls, volcanic slopes |
| Key features | Best buildings at top (lord's manor, temple), workers at bottom, narrow switchback paths, retaining walls, drainage systems, views from above |
| Navigation feel | Vertical -- elevation tells you what district you are in |

### Underground / Cavern

Settlement carved into rock or built in natural caverns.

| Property | Detail |
|---|---|
| When to use | Dwarven cities, goblin warrens, underground refuges, mining colonies |
| Key features | Vertical layout (levels), mushroom/crystal lighting, underground rivers, heat from forges, sound echoes, limited entrances |
| Navigation feel | Disorienting without good landmarks -- use light color and sound to differentiate areas |

---

## 3. Districts and Zoning

Every settlement above village scale should have distinct districts. Each district has a function, a visual identity, and specific NPCs and services. Players should be able to tell which district they are in by looking around.

### District Types

| District | Function | Key Buildings | Typical NPCs | Visual Feel |
|---|---|---|---|---|
| Market / Commercial | Shopping, trading | Shops, stalls, GE, warehouse | Shopkeepers, merchants, guards | Busy, colorful, signs, awnings, crates |
| Residential (Common) | Worker housing | Small houses, apartments, tenements | Citizens, children, beggars | Modest, clotheslines, gardens, narrow streets |
| Residential (Noble) | Wealthy housing | Manors, townhouses, private gardens | Nobles, servants, private guards | Wide streets, trees, iron fences, clean |
| Civic / Government | Administration | Town hall, courthouse, guard HQ | Mayor, guards, clerks | Imposing stone, flags, open square |
| Religious | Worship | Temple, cathedral, cemetery, shrine | Priests, monks, altar | Peaceful, candles, incense, statues |
| Military | Defense | Barracks, armory, training ground | Guards, soldiers, general | Orderly, weapon racks, drill yard |
| Industrial | Production | Smithy, tannery, sawmill, furnace | Craftsmen, workers | Smoky, loud, functional, raw materials |
| Docks / Harbor | Maritime trade | Warehouses, shipyard, fish market | Sailors, fishermen, dock workers | Salty, ropes, crates, ships |
| Entertainment | Leisure | Tavern, theater, arena, casino | Barkeeps, performers, gamblers | Lively, noisy, lanterns, music |
| Slums | Poverty | Shacks, ruins, sewer entrances | Thieves, beggars, rats | Dark, cramped, broken things |
| Academic | Learning | Library, university, observatory | Scholars, wizards, students | Quiet, books, globes, telescopes |
| Guild | Skill training | Skill-specific buildings | Trainers, masters | Themed to the guild |
| Farm / Agricultural | Food production | Farms, barns, silos, windmill | Farmers, ranchers | Open space, crops, animals, fences |
| Park / Garden | Recreation | Trees, benches, fountain, paths | Musicians, children | Green, peaceful, flowers, birds |
| Graveyard | Death, memorial | Graves, mausoleum, chapel | Priest, ghosts at night | Solemn, quiet, fog, iron gates |
| Underground | Hidden content | Sewers, smuggler caves, black market | Criminals, rats, quest NPCs | Dark, damp, hidden entrances |
| Magic Quarter | Arcane services | Magic shop, enchanting table, rune store | Wizards, enchanters | Glowing, mystical, strange sounds |
| Foreign Quarter | Diplomacy | Embassy, cultural buildings, exotic shops | Foreign NPCs, translators | Different architecture from the rest |

### District Placement Rules

These are guidelines, not hard rules. Break them when you have a good reason.

| Rule | Reasoning |
|---|---|
| Market near the main gate | Visitors find shops first, drives commerce |
| Bank near the market | Players bank constantly, minimize running |
| Industrial downwind from residential | Smell and noise -- tanneries are awful neighbors |
| Noble district on higher ground or near castle | Wealth rises, literally and figuratively |
| Slums near industrial or outside walls | Cheap land, undesirable location |
| Temple central or on highest point | Visible landmark, spiritual authority |
| Docks on waterfront | Self-evident |
| Military near gates and walls | First line of defense |
| Entertainment near market | Foot traffic keeps taverns full |
| Academic away from noise | Libraries need quiet |

### Making Districts Feel Distinct

Districts fail when they all look the same. Differentiate with:

| Element | How to Vary It |
|---|---|
| Road material | Cobblestone in noble, dirt in slums, wooden boardwalk at docks |
| Road width | Wide in market, narrow in residential, cramped in slums |
| Building material | Stone in government, wood in common residential, brick in industrial |
| Building height | Tall in commercial, low in residential, mixed in slums |
| Lighting | Bright in market, warm in entertainment, dim in slums, none in underground |
| Ambient sound | Crowd noise in market, hammering in industrial, waves at docks |
| Music track | Each district should trigger a different track or variation |
| NPC density | High in market, low in residential, patrolling in military |
| Decoration density | Signs and banners in market, flowers in noble, graffiti in slums |

---

## 4. Roads and Pathways

Roads are the circulatory system of a settlement. Bad roads make a settlement frustrating. Good roads make it feel natural.

### Road Hierarchy

| Type | Width (tiles) | Material | Use |
|---|---|---|---|
| Main Avenue | 4-6 | Stone or cobble | Primary through-road, connects gates to center |
| Street | 3-4 | Cobble or packed earth | District roads, connects blocks |
| Alley | 1-2 | Dirt or stone | Shortcuts, back access, atmosphere |
| Path | 1-2 | Dirt or gravel | Trails to outskirts, farm paths |
| Bridge | 2-4 | Stone or wood | Crosses water or gaps |
| Tunnel | 2-3 | Stone | Underground passage |
| Stairs | 1-3 | Stone or wood | Elevation changes |
| Boardwalk | 2-3 | Wood planks | Over water or swamp |

### Road Design Guidelines

| Guideline | Explanation |
|---|---|
| Main roads should be obvious | Wider, different material, lit at night -- players should never wonder which way is "forward" |
| Side streets branch at angles | Perfect 90-degree junctions everywhere looks planned and sterile (unless that is the intent) |
| Dead ends need a reason | A house, a hidden entrance, a garden -- never a dead end that leads to nothing |
| Alleys create discovery | Players who find a shortcut through an alley feel clever -- reward exploration |
| Material changes signal boundaries | Cobble to dirt means entering a poor area; wood planks mean approaching docks |
| Street lights follow importance | Bright on main roads, sparse in alleys, absent in slums |
| Roads narrow into residential | Wide main road narrows into smaller streets as you go deeper into neighborhoods |

---

## 5. Landmarks and Navigation

Players navigate by landmarks, not by coordinates. Every settlement needs visual anchors that tell players where they are and which direction to go.

### Every Settlement Needs

| Landmark Need | Purpose | Example |
|---|---|---|
| Entrance landmark | Tells arriving players "you are in the right place" | Decorated gate, arch, sign, statue |
| Central gathering point | Where players congregate, where NPCs announce things | Fountain, statue, market square, well |
| Highest point | Visible from most of the settlement for orientation | Castle tower, cathedral spire, hilltop |
| Clear sightlines | Can see important buildings from main roads | Bank visible down the main avenue |
| District identity | Know which district you are in by looking around | Different building styles, colors, materials |

### Landmark Types

| Type | Navigation Purpose | Examples |
|---|---|---|
| Monument | Identity, pride, meeting point | Statue, obelisk, memorial, triumphal arch |
| Natural Feature | Geography, wonder, unique identity | Giant tree, waterfall, rock formation, ancient ruin |
| Tall Structure | Orientation from anywhere in town | Tower, spire, minaret, lighthouse |
| Water Feature | Gathering spot, district boundary | Fountain, lake, river, harbor |
| Gate / Entrance | Arrival point, district transition | City gate, arch, bridge, dock |
| Unique Building | Destination, wonder | Colosseum, palace, grand library, enormous forge |

### Navigation Anti-Patterns

Things that make settlements frustrating to navigate:

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Everything looks the same | Players cannot tell where they are | Differentiate districts visually |
| No visible landmarks from main roads | Players lose orientation | Place tall structures or monuments at key intersections |
| Bank hidden behind three turns | Players waste time on basic tasks | Put essential services on or near main roads |
| Maze-like alleys with no payoff | Frustrating, feels like filler | Either add hidden content or simplify the layout |
| Teleport point far from everything | First impression is "where am I and why is the bank so far" | Teleport near center or near bank |

---

## 6. Player Flow and Gameplay

Settlements are gameplay spaces. Design for how players actually use them, not just how they look on a map.

### Essential Service Placement

| Service | Ideal Location | Why |
|---|---|---|
| Bank | Near teleport point AND market | Players bank constantly -- minimize running |
| Grand Exchange | Central, near bank | Highest traffic hub in any settlement |
| Teleport Point | Near entrance or center | First thing players interact with on arrival |
| Altar | Accessible but not blocking traffic | Prayer restore between combat trips |
| Anvil / Furnace | Near bank | Smithing requires constant bank runs |
| Range / Stove | Near bank | Cooking requires constant bank runs |
| Slayer Master | Own building, near teleport | Quick task pickup and return |
| Quest Givers | Spread across town | Gives players a reason to explore every district |
| General Store | Near entrance | New players find it first |
| Farming Patches | Edge of town | Thematically appropriate, does not need bank proximity |

### Player Flow Principles

| Principle | Explanation |
|---|---|
| Bank runs should be short | If a player has to run 30 seconds between bank and anvil, they will hate the settlement. Put crafting stations near banks. |
| Quest NPCs should be findable | Main roads, near landmarks, with overhead indicators. A quest NPC hidden in the back room of a random house is bad design unless that is the quest. |
| Shops should be browseable | Group shops in a market area so players can walk past multiple storefronts. One shop per district is fine for theme but frustrating for shopping. |
| Content should be discoverable | Not everything on the main road. Hide a few things in alleys, behind buildings, underground. Reward players who explore. |
| Districts should feel different | Music changes, lighting changes, NPC types change, road material changes. Crossing into a new district should be noticeable. |
| Travel should be purposeful | Do not put the bank on the opposite side of town from everything else. Every major walk should pass something interesting. |

### Common Player Loops

Design around these common activities. The settlement should make each loop efficient.

| Loop | Steps | Design Implication |
|---|---|---|
| Skilling | Bank > crafting station > bank (repeat) | Crafting stations within 5-10 tiles of bank |
| Quest turn-in | Teleport > quest NPC > teleport out | Quest NPCs near teleport or main road |
| Gear up | Bank > GE > gear swap > teleport to content | Bank, GE, and teleport in a triangle |
| Shopping | Walk through market, compare shops | Shops clustered, not scattered |
| Socializing | Stand at GE or fountain, chat | Central area with space to stand and no obstacles |

---

## 7. NPC Population

NPCs make settlements feel alive. Too few and it feels abandoned. Too many and it feels cluttered. The balance depends on district type and time of day.

### Population Density Guidelines

| Area | NPCs per 10x10 Tile Area | Typical Types |
|---|---|---|
| Market | 5-10 | Shopkeepers, guards, wandering citizens |
| Main Street | 3-5 | Citizens walking, guards patrolling, quest NPCs |
| Residential | 1-3 | Residents in doorways, children, pets |
| Industrial | 2-4 | Workers at stations, foremen |
| Park / Open Space | 0-1 | Occasional walker, musician, couple on bench |
| Slums | 1-3 | Beggars, thieves, rats, hidden quest NPCs |
| Military | 2-4 | Guards, soldiers in training |

### NPC Behavior by Role

Static NPCs (standing behind a counter forever) feel like furniture. Give NPCs behavior.

| Role | Behavior | Notes |
|---|---|---|
| Citizens | Walk between buildings, stop at stalls, sit on benches | Variety of paths, not all the same route |
| Guards | Patrol routes along walls and main streets | Predictable routes so players can learn patterns |
| Shopkeepers | Stay behind counter during business hours | Can close at night if day/night cycle is enabled |
| Children | Run around, chase each other, near residential | Small, fast, chaotic movement |
| Workers | Hammer at anvil, sweep floors, carry crates | Anchored to workstations with animations |
| Tavern NPCs | Sit, drink, brawl occasionally | More active at night |

### Day/Night Population Shifts

If the server has a day/night cycle, NPC behavior should change.

| Time | Changes |
|---|---|
| Dawn | Workers leave houses, market NPCs set up stalls, guards change shift |
| Day | Full population, all shops open, children playing |
| Dusk | Workers head home, tavern fills up, guards light torches |
| Night | Streets mostly empty, tavern peak, more guards, shady NPCs appear |

---

## 8. Settlement Progression

Settlements that never change feel like theme parks. Progression systems make settlements feel like living places.

### How Settlements Change

| Trigger | Change Example |
|---|---|
| Quest completion | New NPC appears, building unlocked, new district opens |
| Skill milestone | Guild access granted, new shop inventory |
| World event | Invasion damage, seasonal decorations, plague aftermath |
| Community goal | New building constructed by collective player effort |
| Player housing | Players build in designated areas, town physically grows |
| Time passing | Construction finishes, ruins decay further, gardens grow |

### Settlement States

A settlement can be in one of several states that affect its appearance, NPC behavior, and available services.

| State | Visual Changes | NPC Changes | Service Changes |
|---|---|---|---|
| Thriving | Clean streets, full shops, flowers | Full population, happy dialogue | All services available |
| Declining | Empty shops, broken objects, overgrown weeds | Fewer NPCs, pessimistic dialogue | Some shops closed, reduced stock |
| Under Attack | Fire damage, barricades, rubble | Military NPCs increase, civilians evacuated | Banks locked down, emergency vendors only |
| Rebuilding | Scaffolding, partial buildings, lumber piles | Construction workers everywhere | Limited services, improving over time |
| Festival | Decorations, lights, banners, special stalls | Extra NPCs, festive dialogue, performers | Temporary vendors, event shops |
| Abandoned | Empty, overgrown, collapsed roofs | No friendly NPCs, monsters may spawn | No services, treasure may be hidden |

---

## 9. Settlement Templates

Pre-built settlement layouts the dungeon master can stamp down and customize. Each template provides a starting point -- not a finished product. Customize building types, NPCs, and district details after placing.

| Template | Scale | Layout Style | Key Features |
|---|---|---|---|
| Starter Village | Village | Organic | Bank, general store, altar, quest giver, 5 houses |
| Fishing Village | Hamlet | Cluster | Dock, fish shop, net repair, 3 houses, beached boat |
| Mining Town | Village | Linear | Mine entrance, furnace, anvil, general store, barracks |
| Trade Town | Town | Grid | Large market, caravanserai, bank, GE, warehouses, inn |
| Castle Town | Town | Radial | Castle center, barracks, market, houses inside walls, farms outside |
| Port City | City | Organic | Harbor, shipyard, fish market, warehouse district, taverns, lighthouse |
| Holy City | City | Radial | Cathedral center, pilgrim facilities, holy shops, monastery, gardens |
| Frontier Outpost | Camp | Fortified | Palisade, tents, campfire, supply chest, watchtower |
| Wizard Tower Complex | Hamlet | Cluster | Central tower, library, enchanting room, garden, apprentice quarters |
| Thieves' Guild Hideout | Underground | Underground | Hidden entrance, planning room, vault, escape routes, bar |
| Elven Forest City | Town | Cluster | Treehouse platforms, rope bridges, nature-integrated, crystal lights |
| Dwarven Underground City | City | Underground | Carved halls, forge district, mine shafts, great hall, mushroom farms |
| Desert Oasis Town | Village | Cluster | Around water source, shade structures, market, camel stable |
| Snow Village | Hamlet | Organic | Warm buildings, fires, fur trader, sled dogs, hot springs |
| Floating Town | Village | Island | Platforms on water, bridges, boat access, lighthouse |
| Ruined City | City | Organic (decayed) | Partially collapsed, monsters in buildings, treasure, ghost NPCs |
| Nomad Camp | Camp | Cluster | Tents, fire pit, animal pens, temporary feel, chief's tent larger |
| Underground Market | Town | Underground | Carved from caves, black market stalls, fight pit, smuggler docks |

---

## 10. Designing for Different Player Types

Different players use settlements differently. A well-designed settlement serves all of them.

| Player Type | What They Want | Design Implication |
|---|---|---|
| Efficiency player | Shortest bank runs, fastest access to services | Tight service cluster, clear main roads |
| Explorer | Hidden alleys, secrets, lore objects, rooftop access | Layered content, rewards for going off the main path |
| Socializer | Places to stand and chat, visible gathering spots | Central square, GE area with open space |
| Quest completionist | Every quest NPC findable, logical quest flow | Quest NPCs on main roads or marked clearly |
| Role-player | Atmosphere, taverns, housing, district immersion | Rich environmental detail, ambient NPCs, places to sit |
| New player | Not getting lost, finding basic services immediately | Clear signage, obvious main road, services near entrance |

---

## 11. Common Mistakes

Mistakes that make settlements feel wrong, and how to fix them.

| Mistake | Why It Fails | Fix |
|---|---|---|
| Too many empty buildings | Settlement feels like a movie set | Cut buildings until every one has purpose |
| Bank far from everything | Players spend more time running than playing | Bank within 10 tiles of crafting and GE |
| No clear main road | Players cannot find anything | One wide, well-lit road from entrance to center |
| Every district looks the same | No sense of place or progress | Vary materials, lighting, road width, NPC types |
| NPCs all stationary | Feels like a wax museum | Give NPCs patrol routes, sitting behavior, work animations |
| Teleport dumps you in a corner | First impression is confusion | Teleport to center or near main services |
| Scale too big for content | Lots of walking, nothing to do | Shrink the settlement or add more content |
| No reason to return | Players visit once and never come back | Unique services, daily NPCs, quest chains, guild headquarters |
| Perfect symmetry everywhere | Feels artificial and sterile | Break symmetry with odd placements, different building sizes |
| Ignoring sound and music | Silent towns feel dead | District-specific music, ambient NPC sounds, environmental audio |

---

## 12. Pre-Ship Checklist

Before marking a settlement as complete, verify every item. The service proximity checker (if enabled) will flag some of these automatically.

| Check | Question |
|---|---|
| Bank access | Can a new player find the bank within 30 seconds of arriving? |
| Teleport access | Is there a teleport point or easy access from the world map? |
| Service clustering | Are essential services (bank, shop, altar) near each other? |
| Road clarity | Do the main roads lead somewhere obvious? |
| Entrance landmark | Is there at least one landmark visible from the entrance? |
| District distinction | Does each district look visually distinct? |
| NPC density | Are there enough NPCs to feel alive but not overcrowded? |
| NPC behavior | Do NPCs move, patrol, or animate -- or are they all stationary? |
| Hidden content | Is there content to discover off the main path (alleys, basements, rooftops)? |
| World purpose | Does the settlement have a reason to exist (trade route, mine, port, defense)? |
| Return value | Are there quests or activities that give players a reason to return? |
| Audio | Does the music or ambient sound change between districts? |
| Accessibility range | Is the settlement useful to both new and experienced players? |
| Top-down view | Does it look good from above (minimap and world map view)? |

---

## Views

Editor and debug views available in the world builder for settlement design.

| View | Description |
|---|---|
| Settlement Overview | Top-down view of entire settlement with district coloring |
| District Map | Color-coded district boundaries with service markers |
| Road Network | Road hierarchy visualization showing width, material, and connectivity |
| Service Heatmap | Where essential services are placed versus where players spend time |
| NPC Density Map | Population density by area, color-coded by count |
| Player Flow Analysis | Predicted player movement patterns based on service placement |
| Settlement Checklist | Interactive pre-ship validation with pass/fail indicators |
| Template Browser | Browse and preview settlement templates before placing |