# Nature Catalog

Game design document for the Nature Catalog plugin. This system provides a comprehensive catalog of all natural objects, plants, trees, and environmental features available for world building. The DM uses this to populate their world with life and natural beauty. This is the reference library for all flora, fungi, natural formations, and ambient nature.

---

## Module Toggles

Each toggle enables or disables a subsystem within the nature catalog. DM configures which natural elements exist in their world.

| Toggle | Description |
|---|---|
| Trees | All tree types available (deciduous, evergreen, fantasy) |
| Flowers | All flower types available (common and magical) |
| Herbs | Herb and medicinal plant types available |
| Bushes | Bush and shrub types available |
| Ground Cover | Grass, moss, fern, and surface cover types available |
| Vines | Climbing and ground vine types available |
| Mushrooms | All fungi types available |
| Aquatic Plants | Water-based flora available (freshwater, saltwater, fantasy) |
| Cacti | Desert plant types available |
| Crops | Farmable crop types available (grain, vegetable, fruit, industrial) |
| Natural Formations | Rocks, water features, and land features available |
| Seasonal Changes | Flora changes appearance with in-game seasons |
| Fantasy Flora | Magical and fantasy plant types available |
| Wildlife Decoration | Non-interactive ambient animal scenery available |
| Weather Ground Effects | Puddles, frost, fallen leaves, and weather-caused ground states |
| Particle Nature | Falling leaves, fireflies, pollen, and atmospheric particles |
| Custom Flora | DM can define new plant types beyond the defaults |
| Biome-Appropriate Spawning | Flora auto-populates based on biome when placing terrain |

---

## 1. Trees

### Deciduous Trees (lose leaves seasonally)

| Tree | Description | Visual |
|---|---|---|
| Oak | Broad trunk, spreading canopy | Dark brown bark, deep green |
| Maple | Medium trunk, rounded canopy | Brown bark, green/red/orange |
| Willow | Drooping branches, near water | Light bark, trailing green |
| Birch | Thin trunk, white bark | White/silver bark, light green |
| Ash | Tall straight trunk | Grey bark, compound leaves |
| Elm | Vase-shaped canopy | Grey-brown bark, serrated leaves |
| Beech | Smooth bark, dense canopy | Silver-grey bark, dark green |
| Poplar | Tall columnar shape | Light bark, triangular leaves |
| Alder | Medium, near water | Dark bark, rounded leaves |
| Chestnut | Broad canopy, spiny fruit | Brown bark, large leaves |
| Sycamore | Mottled bark, broad canopy | Patchy white/brown bark |
| Linden | Heart-shaped leaves, fragrant | Grey bark, yellow-green |
| Cherry Blossom | Flowering in spring | Dark bark, pink/white blossoms |
| Apple | Fruit-bearing, orchard tree | Brown bark, white flowers |
| Pear | Fruit-bearing, upright shape | Grey-brown bark, white flowers |
| Plum | Fruit-bearing, small | Dark bark, pink flowers |
| Lemon | Citrus, warm climate | Green bark, yellow fruit |
| Lime | Citrus, warm climate | Green bark, green fruit |
| Orange | Citrus, warm climate | Brown bark, orange fruit |
| Banana | Tropical, large leaves | Green pseudostem, no true bark |
| Mango | Tropical fruit tree | Dark bark, broad canopy |
| Avocado | Tropical fruit tree | Grey bark, dense foliage |
| Papaya | Tropical, palm-like | Thin trunk, cluster of fruit |
| Coconut Palm | Tropical, tall curved trunk | Fibrous trunk, fronds |
| Date Palm | Warm climate, tall | Thick trunk, arching fronds |
| Fig | Broad spreading, warm | Light bark, large leaves |
| Olive | Mediterranean, gnarled | Twisted grey trunk, silver-green |
| Walnut | Large, stately | Dark bark, compound leaves |
| Hazel | Small, multi-stemmed | Brown bark, rounded leaves |
| Aspen | Quaking leaves, white bark | White bark, trembling leaves |

---

### Evergreen Trees (keep leaves year-round)

| Tree | Description | Visual |
|---|---|---|
| Pine | Conical shape, long needles | Brown bark, dark green needles |
| Spruce | Dense conical, short needles | Brown bark, blue-green |
| Fir | Symmetrical conical | Smooth grey bark, flat needles |
| Cedar | Spreading branches, aromatic | Reddish bark, scale-like leaves |
| Cypress | Columnar or spreading | Dark bark, feathery foliage |
| Juniper | Small to medium, berries | Shaggy bark, blue-green |
| Yew | Dense, dark, toxic | Red-brown bark, dark needles |
| Redwood | Massive trunk, ancient | Red bark, enormous height |
| Sequoia | Giant, thick trunk | Spongy red bark, massive |
| Holly | Spiny leaves, red berries | Grey bark, glossy green |
| Eucalyptus | Peeling bark, aromatic | Multicolor bark, lance leaves |
| Mahogany | Tropical hardwood, large | Dark red-brown bark |
| Teak | Tropical hardwood, tall | Grey bark, large leaves |
| Ebony | Dense dark wood | Very dark bark, dense canopy |

---

### Magical / Fantasy Trees

| Tree | Description | Visual |
|---|---|---|
| Magic Tree | Glowing leaves, arcane energy | Luminescent canopy, pulsing light |
| Spirit Tree | Sentient, teleportation hub | Face in bark, ethereal glow |
| Bloodwood | Red sap, dark magic associations | Dark bark, red sap dripping |
| Crystal Tree | Prismatic branches, arcane resonance | Translucent crystalline structure |
| Elder Tree | Ancient power, sacred | Enormous gnarled trunk, mossy |
| Dramen Tree | Fairy magic, quest-related | Twisted trunk, faint shimmer |
| Blisterwood | Anti-vampyre properties | Silver-white bark, holy glow |
| Hollow Tree | Portal inside trunk | Enormous hollow trunk, dark interior |
| Living Tree | Moves and speaks | Animated bark, branch-arms |
| Soul Tree | Stores spirits of the dead | Ghostly wisps orbiting, pale bark |
| World Tree | Massive, supports entire ecosystem | Continent-spanning, roots visible for miles |
| Mushroom Tree | Fungal, underground caverns | Giant mushroom cap, spore clouds |
| Void Tree | Corruption, dark energy | Black bark, reality distortion around it |
| Dream Tree | Sleeping beneath grants visions | Silver leaves, mist at base |
| Petrified Tree | Turned to stone, ancient | Grey stone texture, preserved form |

---

### Tree States

| State | Description |
|---|---|
| Healthy | Full canopy, vibrant color, normal appearance |
| Sapling | Young tree, thin trunk, small canopy |
| Stump | Chopped down, flat top, re-grows over time |
| Dead | No leaves, grey bark, branches bare |
| Diseased | Discolored leaves, spots, wilting |
| Burning | On fire, embers, smoke particles |
| Frozen | Ice-covered, blue tint, icicles hanging |
| Overgrown | Covered in vines, moss, thick and wild |
| Autumn | Orange/red/yellow leaves, some falling |
| Flowering | Blossoms visible (cherry blossom, apple, etc.) |
| Fruiting | Fruit visible on branches, ready for harvest |

---

### Tree Properties

| Property | Type | Description |
|---|---|---|
| Tree ID | Integer | Unique identifier |
| Species | Enum | Which tree type from the catalogs above |
| Size | Enum | Sapling, Small, Medium, Large, Ancient/Giant |
| State | Enum | Healthy, Stump, Dead, Diseased, Burning, Frozen, etc. |
| Choppable | Boolean | Can be woodcut (links to woodcutting skill) |
| Level Requirement | Integer | Woodcutting level needed to chop |
| Log Type | Item ID | What log item it produces when chopped |
| Respawn Time | Ticks | Time to regrow after being chopped |
| Bird Nest Chance | Float | Chance of bird nest drop while chopping |
| Canopy Radius | Integer | Shade and visual coverage in tiles |
| Blocks Movement | Boolean | Trunk blocks walking through |
| Blocks Line of Sight | Boolean | Canopy blocks line of sight |
| Seasonal | Boolean | Changes appearance with in-game seasons |
| Fruit | Item ID | Fruit it produces (if fruit tree) |
| Special | String | Unique interaction (spirit tree = teleport, dramen = quest) |

---

## 2. Flowers

### Common Flowers

| Flower | Description | Visual |
|---|---|---|
| Rose | Classic bloom, thorny stem | Red, white, pink, yellow, or black variants |
| Daisy | Simple open bloom | White petals, yellow center |
| Sunflower | Tall, sun-tracking | Yellow petals, brown center, tall stem |
| Tulip | Cup-shaped bloom | Red, yellow, pink, purple variants |
| Daffodil | Spring bloom, trumpet center | Yellow or white, trumpet-shaped |
| Lily | Elegant curved petals | White, tiger-striped, or water varieties |
| Orchid | Exotic, intricate shape | Purple, white, pink, many varieties |
| Lavender | Spike cluster, fragrant | Purple, aromatic |
| Bluebell | Bell-shaped, carpeting | Blue-purple, drooping clusters |
| Snowdrop | Early spring, small | White, drooping, delicate |
| Marigold | Rounded, dense petals | Orange, yellow, gold |
| Nasturtium | Rounded leaves, edible | Orange, red, yellow |
| Chrysanthemum | Dense layered petals | Many colors, spherical |
| Carnation | Ruffled petals, fragrant | Red, pink, white, multicolor |
| Violet | Small, heart-shaped leaves | Purple, blue, white |
| Iris | Tall, blade-like leaves | Purple, blue, yellow, white |
| Peony | Large, ruffly bloom | Pink, white, red, fragrant |
| Poppy | Thin petals, seed pod | Red, orange, papery petals |
| Hibiscus | Tropical, large bloom | Red, pink, orange, large petals |
| Magnolia | Large waxy bloom | White, pink, fragrant |
| Jasmine | Small star-shaped, fragrant | White, intensely aromatic |
| Wisteria | Hanging clusters, vine | Purple, blue, white cascading |
| Foxglove | Tall spike, tubular blooms | Purple, pink, spotted interior |
| Forget-me-not | Tiny clusters | Blue with yellow center |
| Buttercup | Small glossy bloom | Bright yellow, shiny petals |
| Dandelion | Weed, seed puff | Yellow bloom or white seed head |
| Clover | Low carpet, tri-leaf | White or purple tiny flowers |
| Heather | Low shrubby, moor plant | Purple, pink, white |
| Thistle | Spiny, tough | Purple bloom, spiky leaves |
| Protea | Large exotic bloom | Pink, structural, bold shape |
| Bird of Paradise | Tropical, crane-shaped | Orange, blue, dramatic form |
| Lotus | Floating on water | Pink, white, large, sacred |

---

### Magical / Fantasy Flowers

| Flower | Description | Visual |
|---|---|---|
| Moonflower | Glows at night, lunar cycle | White with silver luminescence |
| Firebloom | Warm to touch, embers drift | Red-orange, flickering heat haze |
| Ice Blossom | Frozen, preserved in ice | Blue-white, frost particles |
| Spirit Flower | Ghostly, ethereal | Transparent, faint shimmer |
| Blood Rose | Red thorns, drips crimson | Dark red, viscous droplets |
| Void Orchid | Warps nearby space | Dark purple, space distortion |
| Crystal Petal | Prismatic, chimes in wind | Translucent, rainbow refraction |
| Dream Lily | Inhaling causes drowsiness | Pale blue, mist emanating |
| Poison Nightshade | Deadly toxic, dark beauty | Purple-black, menacing |
| Fairy Ring Mushroom | Circle formation, portal potential | Ring of small mushrooms, faint glow |

---

### Flower Properties

| Property | Type | Description |
|---|---|---|
| Flower ID | Integer | Unique identifier |
| Species | Enum | Which flower type from the catalogs above |
| Color | Hex / Enum | Primary color with variant support |
| Size | Enum | Small, Medium, Large, Giant |
| Pickable | Boolean | Can player pick this flower |
| Regrows | Boolean | Returns after being picked |
| Regrow Time | Ticks | Time to regrow after picking |
| Produces | Item ID | What item you get when picked |
| Farming Level | Integer | Level required to plant (if plantable) |
| Protection Effect | Enum | Protects nearby crops (marigold, white lily) |
| Seasonal | Boolean | Only appears in certain seasons |
| Biome | Enum list | Which biomes this flower grows in naturally |
| Decorative | Boolean | Purely visual, no interaction |
| Walkable | Boolean | Can walk over without blocking movement |
| Fragrance Radius | Integer | Tiles of scent effect (if any gameplay effect) |

---

## 3. Herbs and Medicinal Plants

### Common Herbs

| Herb | Description | Use |
|---|---|---|
| Basil | Leafy, aromatic | Cooking ingredient |
| Mint | Fresh, cooling | Cooking, potion ingredient |
| Rosemary | Needle-like, fragrant | Cooking, aromatic |
| Thyme | Tiny leaves, creeping | Cooking ingredient |
| Sage | Grey-green, fuzzy leaves | Cooking, ceremonial |
| Parsley | Bright green, flat or curly | Cooking garnish |
| Oregano | Small leaves, warm flavor | Cooking ingredient |
| Chamomile | Daisy-like flowers | Tea, calming potion |
| Ginger | Rhizome, knobby root | Cooking, healing potion |
| Garlic | Bulb, pungent | Cooking, anti-vampyre |
| Aloe | Succulent, gel inside | Healing, burn treatment |
| Ginseng | Forked root, medicinal | Healing potion, energy |
| Turmeric | Yellow root, staining | Cooking, healing |
| Cinnamon | Bark strips, aromatic | Cooking, warming potion |
| Vanilla | Vine pod, fragrant | Cooking, luxury ingredient |
| Pepper | Climbing vine, berries | Cooking, spice trade |
| Saffron | Crocus stigma, precious | Cooking, rare dye |
| Cardamom | Pod, aromatic seeds | Cooking, exotic spice |

---

### Game Herbs (RS-inspired naming)

| Herb | Description | Tier |
|---|---|---|
| Guam | Basic healing herb | Beginner |
| Marrentill | Bitter, cleansing herb | Beginner |
| Tarromin | Sharp-scented, defensive | Beginner |
| Harralander | Tough-stemmed, restoration | Low |
| Ranarr | Prized prayer herb | Mid |
| Toadflax | Slimy-stemmed, agility | Mid |
| Irit | Delicate, super restoration | Mid |
| Avantoe | Ranged-boosting herb | Mid |
| Kwuarm | Pungent, strength herb | High |
| Snapdragon | Fast-growing, combat boost | High |
| Cadantine | Thick-leaved, defense | High |
| Lantadyme | Glowing, magic boost | High |
| Dwarf Weed | Stunted, ranging boost | Elite |
| Torstol | Rarest herb, overload | Elite |

---

### Herb Properties

| Property | Type | Description |
|---|---|---|
| Herb ID | Integer | Unique identifier |
| Grimy Version | Item ID | Uncleaned version of the herb |
| Clean Version | Item ID | Cleaned version of the herb |
| Herblore Level to Clean | Integer | Level required to clean grimy herb |
| Used in Potions | Array of Recipe IDs | Which potions this herb is used in |
| Farming Level | Integer | Level required to plant |
| Growth Time | Ticks | Time to grow from seed to harvest |
| Yield | Range | Min-max herbs per harvest |
| Biome | Enum list | Where it grows naturally in the wild |
| Harvestable in Wild | Boolean | Can be picked without farming skill |

---

## 4. Bushes and Shrubs

### Bush Types

| Bush | Description | Visual |
|---|---|---|
| Berry Bush (Redberry) | Thorny, low shrub | Green with red berries |
| Berry Bush (Blueberry) | Dense, rounded | Green with blue berries |
| Berry Bush (Blackberry) | Thorny, sprawling | Dark green with black berries |
| Berry Bush (Raspberry) | Cane-like, thorny | Green with red-pink berries |
| Berry Bush (Gooseberry) | Small, spiny | Light green with pale berries |
| Berry Bush (Elderberry) | Tall, cluster berries | Dark green with purple clusters |
| Rose Bush | Thorny, flowering | Green with colored blooms |
| Hedge (Trimmed) | Rectangular, manicured | Dark green, flat surfaces |
| Hedge (Wild) | Untrimmed, natural shape | Various greens, irregular |
| Hedge (Maze) | Tall, wall-like | Dense green, navigation obstacle |
| Holly Bush | Spiny leaves, red berries | Dark green, glossy |
| Rhododendron | Large flowering shrub | Green with large blooms |
| Azalea | Smaller flowering shrub | Green with colorful flowers |
| Hydrangea | Large flower clusters | Green with blue/pink/white clusters |
| Boxwood | Dense, compact | Dark green, formal shape |
| Laurel | Glossy, dense | Dark green, smooth leaves |
| Juniper Bush | Low-growing, berries | Blue-green, aromatic |
| Sage Bush | Grey-green, aromatic | Silver-green, fuzzy |
| Lavender Bush | Purple flower spikes | Grey-green with purple spikes |
| Bramble | Thorny, tangled, blocks path | Dark green, impenetrable tangle |
| Tumbleweed | Dried, rolls with wind | Brown, spherical, dead |

---

### Fantasy Bushes

| Bush | Description | Visual |
|---|---|---|
| Poison Bush | Touching causes damage | Sickly green, dripping |
| Fire Bush | Constantly smoldering | Orange-red, embers drifting |
| Crystal Bush | Gem-bearing branches | Translucent, prismatic |
| Shadow Bush | Moves when not watched | Dark, shifts position subtly |
| Singing Bush | Emits melody when nearby | Vibrating leaves, musical |
| Trap Bush | Snaps shut on contact | Hinged leaves, hidden teeth |
| Healing Bush | Restores HP when near | Soft green glow, warm |
| Spirit Bush | Ghostly, partially transparent | Transparent, faint shimmer |

---

### Bush Properties

| Property | Type | Description |
|---|---|---|
| Bush ID | Integer | Unique identifier |
| Size | Enum | Small, Medium, Large, Wall-sized (hedge) |
| Produces | Item ID | Berry or item it yields when harvested |
| Harvest Level | Integer | Farming level required to harvest |
| Thorny | Boolean | Damages player who walks into it |
| Thorn Damage | Integer | Damage per contact if thorny |
| Blocks Movement | Boolean | Cannot walk through |
| Trimmable | Boolean | Can be shaped with shears (hedge) |
| Seasonal Fruit | Boolean | Only produces fruit in certain seasons |

---

## 5. Ground Cover and Grass

### Grass Types

| Grass | Description | Visual |
|---|---|---|
| Short Grass | Standard lawn-like ground | Light green, uniform |
| Tall Grass | Hides items and small creatures | Dark green, swaying, waist-high |
| Wild Grass | Uneven, natural | Mixed greens, varied height |
| Meadow Grass | Flowering, mixed species | Green with small flowers |
| Swamp Grass | Sparse, waterlogged | Dark green, wet, patchy |
| Tundra Grass | Short, cold-resistant | Yellow-green, sparse |
| Prairie Grass | Tall, flowing in wind | Golden-brown, wave motion |
| Ornamental Grass | Decorative, planted | Various colors, fountain shape |
| Bamboo Grass | Tall, rapid growth | Green stalks, dense |
| Pampas Grass | Very tall, feathery plumes | Silver-white plumes, tall |
| Wheat Field | Golden crop field | Golden, swaying heads |
| Barley Field | Crop field with awns | Golden, bristled heads |
| Rice Paddy | Flooded field crop | Green in water, terraced |
| Hay | Dried and baled grass | Yellow-brown, stacked bales |

---

### Ground Cover

| Cover | Description | Visual |
|---|---|---|
| Moss (Green) | Soft carpet on stone or soil | Bright green, spongy |
| Moss (Hanging) | Drapes from trees | Grey-green, trailing |
| Moss (Cave) | Damp, underground | Dark green, bioluminescent spots |
| Lichen | Crusty growth on rock surfaces | Grey, yellow, orange, flat |
| Clover Patch | Low carpet, tri-leaf | Green with white/purple flowers |
| Ivy (Ground Creeping) | Low vine, spreading | Dark green, glossy leaves |
| Fern (Small) | Frond plant, shade-loving | Green, curled tips |
| Fern (Large) | Tall fronds, forest floor | Dark green, arching |
| Fern (Tree) | Tropical, trunk-forming | Green fronds on small trunk |
| Bracken | Coarse fern, spreads widely | Brown-green, dry |
| Heather | Low shrubby moorland cover | Purple, pink, white flowers |
| Wildflower Meadow | Mixed flower carpet | Multi-colored, dense |
| Leaf Litter | Autumn leaves on ground | Brown, orange, red, scattered |
| Pine Needles | Fallen conifer needles | Brown-orange, carpet-like |
| Mulch | Shredded bark, garden cover | Dark brown, uniform |
| Gravel | Small loose stones | Grey, brown, crunchy |
| Sand | Fine loose grain | Yellow, tan, white |
| Pebbles | Small rounded stones | Mixed grey, smooth |

---

### Ground Cover Properties

| Property | Type | Description |
|---|---|---|
| Cover ID | Integer | Unique identifier |
| Walkable | Boolean | Always yes for ground cover |
| Speed Modifier | Float | Movement speed change (tall grass = slower, path = faster) |
| Hides Entities | Boolean | Creatures and items hidden in this (tall grass) |
| Seasonal | Boolean | Changes with season (green to brown to snow-covered) |
| Flammable | Boolean | Can catch fire and spread |
| Harvestable | Boolean | Can be cut or collected |
| Sound | Sound ID | Footstep sound when walking on this surface |

---

## 6. Vines and Climbing Plants

### Vine Types

| Vine | Description | Visual |
|---|---|---|
| Ivy | Wall-climbing evergreen | Dark green, woody, clinging |
| Grape Vine | Fruit-bearing, trellis | Green with purple/green grape clusters |
| Wisteria | Hanging flower cascades | Purple, blue, or white draping blooms |
| Morning Glory | Trumpet-shaped flowers | Blue, purple, pink, spiral vine |
| Creeping Vine | Ground-spreading, low | Green, rapidly expanding |
| Jungle Vine | Thick, hanging, swingable | Brown-green, rope-like |
| Thorned Vine | Barbed, damages on contact | Dark green with visible thorns |
| Moss Vine | Damp, cave-dwelling | Grey-green, dripping |
| Root Vine | Thick, exposed tree roots | Brown, gnarled, woody |
| Chain Vine | Strong, supports climbing | Thick, woody, secured |

---

### Fantasy Vines

| Vine | Description | Visual |
|---|---|---|
| Living Vine | Moves autonomously, grabs | Animated, reaching tendrils |
| Blood Vine | Red coloring, drains HP | Crimson, pulsing |
| Crystal Vine | Glowing crystalline nodes | Translucent with bright nodes |
| Shadow Vine | Grows only in darkness | Black, retracts from light |
| Fire Vine | Burning, radiates heat | Orange-red, smoldering |
| Ice Vine | Frozen, brittle | Blue-white, icicle formations |
| Spirit Vine | Transparent, portal potential | Ghostly, faint shimmer |
| Corruption Vine | Spreads disease to nearby flora | Dark purple, oozing |

---

### Vine Properties

| Property | Type | Description |
|---|---|---|
| Vine ID | Integer | Unique identifier |
| Surface | Enum | Wall, ground, ceiling, tree, freestanding |
| Climbable | Boolean | Can player climb this vine |
| Agility Level | Integer | Agility level required to climb |
| Cuttable | Boolean | Can be cut with knife or machete |
| Regrows | Boolean | Returns after being cut |
| Produces | Item ID | Grapes or other item yield |
| Blocks Path | Boolean | Vine blocks movement until cut |
| Swingable | Boolean | Can swing across gap (agility check) |
| Damage on Contact | Integer | Damage from thorned or hazardous vines |

---

## 7. Mushrooms and Fungi

### Mushroom Types

| Mushroom | Description | Visual |
|---|---|---|
| Common Mushroom | Standard edible mushroom | Brown cap, white stem |
| Toadstool | Poisonous, iconic spotted | Red cap with white spots |
| Puffball | Bursts spores when stepped on | White, round, soft |
| Morel | Honeycomb texture, edible | Brown, pitted cap |
| Chanterelle | Funnel-shaped, edible | Golden yellow, wavy |
| Oyster Mushroom | Shelf-like, on trees | White, fan-shaped |
| Shiitake | Broad cap, on wood | Brown, cracked surface |
| Truffle | Underground, rare, prized | Dark brown, lumpy, buried |
| Giant Mushroom | Walkable surface, cave flora | Enormous cap, thick stem |
| Bracket Fungus | Grows on living trees | Shelf-like, brown rings |
| Shelf Fungus | Grows on dead wood | Flat, layered, various colors |

---

### Fantasy Fungi

| Mushroom | Description | Visual |
|---|---|---|
| Glowing Mushroom | Bioluminescent, cave lighting | Blue/green/purple glow, soft light |
| Spore Mushroom | Releases cloud when disturbed | Bulbous, spore particles on contact |
| Shrieking Mushroom | Alarm when approached | Pale, vibrating, emits sound |
| Healing Mushroom | Restores HP when consumed | Soft pink glow, warm |
| Poison Mushroom | Damages when consumed | Sickly green-purple, dripping |
| Fairy Ring | Circle of mushrooms, teleport | Ring formation, faint glow at center |
| Giant Mushroom Tree | Underground forest canopy | Tree-sized, forms cavern ceiling |
| Mind Mushroom | Causes hallucination effect | Swirling colors, psychedelic |
| Bouncy Mushroom | Launches player upward | Springy cap, compressed animation |
| Clock Mushroom | Speeds or slows time nearby | Ticking texture, distortion field |

---

### Mushroom Properties

| Property | Type | Description |
|---|---|---|
| Mushroom ID | Integer | Unique identifier |
| Size | Enum | Tiny, Small, Medium, Large, Giant, Tree-sized |
| Edible | Boolean | Can be eaten safely |
| Poisonous | Boolean | Damages if eaten |
| Pickable | Boolean | Can be harvested |
| Light Radius | Integer | Bioluminescent glow range (0 = none) |
| Light Color | Hex | Color of bioluminescent glow |
| Effect on Contact | Enum | None, Spore Cloud, Bounce, Shriek, Damage |
| Underground Only | Boolean | Only spawns in caves and underground areas |
| Farming Level | Integer | Level required to grow (if plantable) |

---

## 8. Aquatic Plants

### Freshwater Plants

| Plant | Description | Visual |
|---|---|---|
| Water Lily | Floating pads with flowers | Green pads, white/pink flowers |
| Reed | Tall, bank edge growth | Green stalks, feathery tops |
| Cattail | Fuzzy brown seed heads | Tall green with brown spike |
| Seagrass | Submerged, swaying | Green ribbons, underwater |
| Pondweed | Floating and submerged | Small green leaves, tangled |
| Water Hyacinth | Floating, purple flowers | Green rosette, purple bloom |
| Lotus | Sacred floating flower | Large pink/white on pad |
| Papyrus | Tall, ancient writing material | Tall stalks, fan-shaped top |
| Watercress | Edible, stream-growing | Small green leaves, floating |
| Algae | Surface film on still water | Green film, slimy |
| Duckweed | Tiny floating dots | Bright green, covers surface |
| Bulrush | Tall, wetland reed | Brown, cylindrical head |

---

### Saltwater / Ocean Plants

| Plant | Description | Visual |
|---|---|---|
| Kelp | Tall underwater forest | Brown-green, towering fronds |
| Seaweed | Various colors, shore/underwater | Green, brown, red, flowing |
| Coral (Brain) | Rounded, maze-like surface | Tan, brown, grooved |
| Coral (Fan) | Flat, spread like a fan | Purple, red, flat and wide |
| Coral (Staghorn) | Branching like antlers | White, brown, branching |
| Coral (Table) | Flat tabletop shape | Brown, flat horizontal |
| Coral (Tube) | Vertical tubes | Yellow, pink, cylindrical |
| Sea Anemone | Tentacles, colorful | Pink, green, orange, waving |
| Sea Sponge | Porous, irregular shape | Yellow, orange, soft |
| Barnacle | On rocks, ships, piers | White, crusty, clustered |
| Mangrove Root | Above and below waterline | Brown roots, tangled, partial submersion |

---

### Fantasy Aquatic Plants

| Plant | Description | Visual |
|---|---|---|
| Crystal Coral | Prismatic, magical resonance | Translucent, rainbow refraction |
| Blood Kelp | Found in deep dark water | Deep red, swaying, ominous |
| Singing Seaweed | Emits melodic sound | Green-blue, vibrating |
| Pearl Plant | Produces pearls over time | Oyster-like, luminescent |
| Void Algae | Corrupted, dark water | Black-purple film, spreading |
| Bioluminescent Plankton | Glowing water surface | Blue-green glow on water surface |
| Air Bubble Plant | Creates breathable pockets underwater | Green, emits air bubble clusters |

---

## 9. Cacti and Desert Plants

### Cactus Types

| Cactus | Description | Visual |
|---|---|---|
| Saguaro | Tall columnar with arms | Green, ribbed, iconic Western shape |
| Barrel Cactus | Round, squat, ridged | Green, spherical, spiny |
| Prickly Pear | Flat pads, edible fruit | Green pads, red/yellow fruit |
| Organ Pipe | Clustered columns from base | Green, multiple tall columns |
| Cholla | Branching, fuzzy-looking spines | Green, cylindrical segments |
| Hedgehog Cactus | Small, round, clustering | Green, dense spines, small |
| Potato Cactus | Lumpy, harvestable | Green-brown, irregular lumps |

---

### Desert Plants

| Plant | Description | Visual |
|---|---|---|
| Aloe Vera | Succulent, gel inside | Grey-green, thick pointed leaves |
| Desert Rose | Swollen trunk, pink flowers | Thick trunk, pink blooms |
| Tumbleweed | Dried bush, moves with wind | Brown, spherical, rolling |
| Yucca | Sword-like leaves, tall flower | Green spikes, white flower stalk |
| Agave | Rosette of thick leaves | Blue-green, pointed, large |
| Desert Sage | Aromatic, silvery | Silver-green, bushy |
| Mesquite | Thorny, drought-resistant | Brown bark, sparse green |
| Joshua Tree | Branching, iconic desert | Spiky leaves, twisted branches |
| Desert Palm | Oasis tree, fan leaves | Tan trunk, green fan fronds |
| Dead Bush | Dried, brown, lifeless | Brown, bare sticks |

---

## 10. Crops and Agriculture

### Grain

| Crop | Description | Visual |
|---|---|---|
| Wheat | Staple grain, bread-making | Golden stalks, bearded heads |
| Barley | Brewing and animal feed | Golden, long awns |
| Oats | Hardy grain, porridge | Light golden, drooping heads |
| Rice | Flooded paddy crop | Green in water, terraced |
| Corn / Maize | Tall stalked, ear bearing | Tall green, yellow ears |
| Rye | Cold-tolerant grain | Grey-green, slim stalks |
| Millet | Small-grained, drought-resistant | Green, clustered heads |
| Sorghum | Tall, warm-climate grain | Green, bushy seed heads |

---

### Vegetables

| Crop | Description | Visual |
|---|---|---|
| Potato | Underground tuber | Green plant, brown tubers below |
| Onion | Bulb vegetable, layers | Green shoots, brown/white bulb |
| Cabbage | Large leafy head | Green or purple, round head |
| Tomato | Vine fruit, red when ripe | Green vine, red fruit |
| Carrot | Orange root vegetable | Green fronds, orange root below |
| Turnip | Root vegetable, white/purple | Green tops, white-purple root |
| Beet | Red root vegetable | Green-red leaves, dark red root |
| Lettuce | Leafy green, salads | Light green, loose or tight head |
| Pumpkin | Large gourd, orange | Green vine, orange fruit |
| Squash | Various gourd shapes | Green vine, varied fruit shapes |
| Cucumber | Green elongated fruit | Green vine, green fruit |
| Pepper | Bell or hot varieties | Green plant, green/red/yellow fruit |
| Garlic | Pungent bulb | Green shoots, white bulb |
| Leek | Tall onion relative | Long green and white stalk |
| Celery | Crisp stalked vegetable | Pale green, tall stalks |
| Pea | Pod vegetable, climbing | Green vine, green pods |
| Bean | Pod vegetable, many types | Green vine, hanging pods |
| Lentil | Small seed in pod | Low bushy plant, tiny pods |

---

### Fruit

| Crop | Description | Visual |
|---|---|---|
| Apple | Tree fruit, many varieties | Red, green, or yellow on branch |
| Orange | Citrus, tropical/subtropical | Orange spheres on green tree |
| Lemon | Sour citrus | Yellow on green tree |
| Lime | Small green citrus | Green on green tree |
| Banana | Tropical, cluster fruit | Yellow bunch, large leaves |
| Grape | Vine fruit, wine-making | Purple or green clusters |
| Strawberry | Low ground runner | Red, small, green leaves |
| Watermelon | Large ground vine fruit | Green rind, red interior |
| Pineapple | Tropical, spiny crown | Brown-green, crown of leaves |
| Papaya | Tropical tree fruit | Green-orange on palm-like tree |
| Mango | Tropical, sweet | Green-red-yellow on tree |
| Dragonfruit | Exotic cactus fruit | Pink skin, white-black interior |
| Coconut | Palm fruit, hard shell | Brown husk, white interior |
| Fig | Mediterranean, soft | Purple-brown, on twisted tree |
| Date | Desert palm fruit | Brown clusters, on tall palm |
| Pomegranate | Red seeded fruit | Red, hard exterior, red seeds |
| Peach | Stone fruit, fuzzy | Pink-orange, fuzzy skin |
| Plum | Stone fruit, smooth | Purple or red, smooth |
| Cherry | Small stone fruit, clusters | Red, small, paired |
| Pear | Tree fruit, teardrop shape | Green or yellow, smooth |
| Raspberry | Bramble berry, red | Red aggregate berry |
| Blueberry | Bush berry, small | Blue-purple, small, clustered |
| Blackberry | Bramble berry, dark | Black aggregate berry |
| Gooseberry | Bush berry, tart | Green-yellow, translucent |
| Elderberry | Cluster berry, medicinal | Dark purple, tiny clusters |
| Cranberry | Bog berry, tart | Red, small, wet ground |

---

### Industrial Crops

| Crop | Description | Use |
|---|---|---|
| Flax | Blue-flowered fiber plant | Linen cloth, bowstring |
| Cotton | White boll fiber plant | Cloth, bandages |
| Hops | Climbing vine, cone flowers | Brewing beer |
| Tea | Shrub, leaf harvest | Tea brewing, energy |
| Coffee | Shrub, berry/bean harvest | Coffee brewing, energy boost |
| Sugar Cane | Tall grass, sweet stalk | Sugar, rum brewing |
| Tobacco | Broad-leaved, dried | Smoking pipe, trade good |
| Rubber | Tropical tree, latex sap | Rubber items, waterproofing |
| Silk Worm Mulberry | Mulberry tree for silkworms | Silk cloth production |
| Hemp | Tall fibrous plant | Rope, sack, canvas |
| Jute | Tropical fiber plant | Sack cloth, burlap |
| Indigo | Blue dye plant | Blue dye for crafting |
| Woad | Blue dye plant (alternative) | Blue dye, war paint |
| Madder | Red dye root plant | Red dye for crafting |

---

### Crop Properties (shared)

| Property | Type | Description |
|---|---|---|
| Crop ID | Integer | Unique identifier |
| Seed Item | Item ID | Seed required to plant |
| Produce Item | Item ID | What you harvest |
| Farming Level | Integer | Level required to plant |
| Growth Stages | Integer | Number of visual stages before harvestable |
| Growth Time | Ticks | Total time from seed to harvest |
| Yield | Range | Min-max produce per harvest |
| Disease Chance | Float | Chance per growth stage of disease |
| Compost Bonus | Float | Yield increase from applying compost |
| Payment Protection | Item ID + Qty | What to pay NPC gardener to protect crop |
| XP Plant | Float | XP awarded for planting |
| XP Harvest | Float | XP awarded per item harvested |
| Patch Type | Enum | Allotment, Herb, Tree, Bush, Flower, Special |
| Seasons | Enum list | Which seasons this grows in (if seasonal farming enabled) |
| Watering | Boolean | Needs watering (if water system enabled) |

---

## 11. Natural Formations

### Rock Formations

| Formation | Description | Visual |
|---|---|---|
| Boulder | Large standalone rock | Grey, rounded, weathered |
| Rock Pile | Cluster of smaller rocks | Grey, heaped, various sizes |
| Cliff Face | Vertical rock wall | Grey/brown, layered, tall |
| Cave Entrance | Opening in rock face | Dark interior, rocky frame |
| Stalactite | Hanging from cave ceiling | Pointed, mineral deposits, dripping |
| Stalagmite | Rising from cave floor | Pointed, mineral deposits, upward |
| Crystal Formation | Exposed crystal cluster | Prismatic, translucent, glowing |
| Geode | Split rock revealing crystals | Grey exterior, sparkling interior |
| Fossil Rock | Ancient remains in stone | Grey with embedded bone shapes |
| Lava Rock | Cooled volcanic stone | Black, porous, rough |
| Obsidian Outcrop | Volcanic glass exposure | Black, glossy, sharp edges |
| Sandstone | Layered sedimentary rock | Yellow/tan, banded, smooth |
| Limestone | Pale sedimentary rock | White/grey, chalky, karst |
| Granite | Hard igneous rock, speckled | Grey with black/white flecks |
| Marble Vein | Decorative stone vein | White with grey/colored veins |
| Mineral Vein (Copper) | Mineable ore deposit | Green-brown streaks in rock |
| Mineral Vein (Tin) | Mineable ore deposit | Silver-grey streaks in rock |
| Mineral Vein (Iron) | Mineable ore deposit | Red-brown streaks in rock |
| Mineral Vein (Coal) | Mineable fuel deposit | Black streaks in rock |
| Mineral Vein (Mithril) | Mineable fantasy ore | Blue-purple streaks in rock |
| Mineral Vein (Adamant) | Mineable fantasy ore | Dark green streaks in rock |
| Mineral Vein (Rune) | Mineable fantasy ore | Cyan-teal streaks in rock |
| Mineral Vein (Amethyst) | Mineable gem deposit | Purple crystal clusters in rock |

---

### Water Features

| Feature | Description | Visual |
|---|---|---|
| Pond | Small still body of water | Calm surface, reeds at edges |
| Lake | Large still body of water | Expansive, reflective, deep |
| River | Flowing water, current | Moving water, banks, varying width |
| Stream | Small flowing water | Narrow, shallow, gentle flow |
| Creek | Very small flowing water | Thin, rocky bottom visible |
| Waterfall | Vertical water drop | Falling water, mist, rocks, splash pool |
| Hot Spring | Heated natural pool | Steam rising, warm water, mineral color |
| Geyser | Periodic eruption of hot water | Steam vent, periodic spray, mineral buildup |
| Puddle | Small temporary water | Shallow, reflective, after rain |
| Marsh / Bog | Waterlogged ground | Murky water, sparse vegetation, spongy |
| Swamp | Heavily waterlogged area | Dark water, dead trees, thick vegetation |
| Tidal Pool | Coastal trapped water | Clear shallow water, sea life, rocks |
| Whirlpool | Spinning water vortex | Circular current, dangerous, pulling |
| Underground Lake | Subterranean body of water | Dark water, cave ceiling reflection |
| Frozen Lake | Ice-covered body of water | White-blue ice surface, cracks |
| Glacial Melt | Melting ice flow | Clear blue water, ice chunks, cold |

---

### Land Features

| Feature | Description | Visual |
|---|---|---|
| Hill | Gentle elevation rise | Rounded, grassy, gradual slope |
| Valley | Low ground between hills | Flat base, sloping sides, often green |
| Ravine | Narrow steep-sided valley | Deep cut, rocky walls, narrow |
| Canyon | Large deep gorge | Wide, layered rock walls, deep |
| Cliff | Steep vertical drop | Rocky face, exposed layers, sheer |
| Plateau | Flat elevated area | Flat top, steep sides, mesa-like |
| Mesa | Flat-topped isolated hill | Flat top, steep cliff sides, desert |
| Volcano | Cone-shaped, erupts | Dark rock, crater top, smoke/lava |
| Crater | Impact or collapse depression | Round depression, raised rim |
| Sand Dune | Wind-shaped sand mound | Smooth curves, rippled surface |
| Sinkhole | Ground collapse opening | Sudden drop, broken edges, deep |
| Cave | Underground opening/system | Dark entrance, rocky interior |
| Tunnel | Through-passage in rock | Dark, enclosed, connects areas |
| Archway (Natural) | Stone arch formation | Rock arch, erosion-carved |
| Land Bridge | Natural connection over gap | Rock bridge, spans chasm |
| Overhang | Rock jutting over space | Sheltering rock, cliff edge |
| Ledge | Narrow horizontal shelf | Thin walkable surface, cliff face |

---

### Biome-Specific Natural Objects

| Biome | Natural Objects |
|---|---|
| Forest | Fallen log, tree stump, hollow tree, leaf pile, ant hill, bird nest, spider web, fox den |
| Desert | Sand dune, oasis, dry riverbed, bleached bones, cactus skeleton, sandstone arch |
| Arctic | Ice shelf, snowdrift, icicle, frozen waterfall, ice cave, snow-covered rock |
| Swamp | Mud pit, dead tree, floating log, moss mound, gas bubble, quicksand |
| Cave | Stalactite, stalagmite, crystal cluster, underground pool, bat colony, cave painting |
| Coast | Tide pool, driftwood, sea shell pile, sand castle remains, cliff erosion, sea stack |
| Volcanic | Lava flow, obsidian shard, sulfur vent, ash pile, cooling basalt, magma pool |
| Jungle | Giant leaf, thick root, vine tangle, parrot perch, monkey branch, ancient ruin |
| Mountain | Goat trail, eagle nest, wind-carved rock, mountain spring, snow cap, loose scree |
| Underwater | Coral reef, kelp forest, sand floor, shipwreck, treasure chest, giant clam |

---

## 12. Weather and Atmospheric Nature

### Falling Elements (particles)

| Particle | Description | Conditions |
|---|---|---|
| Rain Drops | Falling water, splash on impact | Rain weather |
| Snow Flakes | Falling snow, accumulates | Snow weather |
| Leaves (Autumn) | Falling colored leaves | Autumn season, near trees |
| Petals (Cherry Blossom) | Pink petals drifting | Spring season, near cherry trees |
| Pollen | Yellow floating particles | Spring/summer, near flowers |
| Ash (Volcanic) | Grey falling particles | Near volcanoes, after eruption |
| Sand (Sandstorm) | Blowing sand particles | Desert, storm weather |
| Embers | Floating orange sparks | Near fires, volcanic areas |
| Seeds (Dandelion) | Floating white seed puffs | Near dandelions, wind |
| Spores | Floating mushroom spores | Near mushroom areas |
| Fireflies | Small glowing dots, drifting | Night, forest, warm weather |
| Butterflies | Colorful fluttering | Day, meadow, near flowers |
| Dragonflies | Darting near water | Day, near water bodies |

---

### Ground Effects

| Effect | Description | Trigger |
|---|---|---|
| Puddles | Small water pools on ground | After rain |
| Snow Accumulation | White covering on surfaces | During and after snow |
| Frost | Thin ice crystal layer | Cold mornings |
| Dew Drops | Small water drops on plants | Early morning |
| Fallen Leaves | Leaves scattered on ground | Autumn season |
| Mud | Soft brown ground | After rain on dirt terrain |
| Ice Patches | Frozen puddles, slippery | Freezing weather after rain |
| Flood Water | Rising water level | Heavy/prolonged rain |
| Dried / Cracked Earth | Parched ground with cracks | Drought conditions |

---

## 13. Wildlife Scenery (non-interactive decoration)

### Common Wildlife Decoration

| Wildlife | Description | Location |
|---|---|---|
| Birds (Perched) | Sitting on branches or ledges | Trees, roofs, fences |
| Birds (Flying) | Single bird in flight | Anywhere, sky level |
| Birds (Flock) | Group in formation | Sky, migrating |
| Squirrels | Running up trees, foraging | Forest, parks |
| Rabbits | Hopping near burrows | Meadows, forest edge |
| Deer (Distant) | Grazing in background | Forest clearings, meadows |
| Butterflies | Fluttering near flowers | Meadows, gardens |
| Bees | Buzzing around flowers | Near flowers, apiaries |
| Fish (Jumping) | Leaping from water surface | Rivers, lakes |
| Fish (Swimming) | Visible under clear water | Shallow water, ponds |
| Frogs | Sitting on lily pads, hopping | Near water, wetlands |
| Snails | Slow movement on ground | Wet areas, gardens |
| Ladybugs | Sitting on leaves | Gardens, meadows |
| Spiders (Webs) | Web between structures | Caves, forests, abandoned buildings |
| Ants (Trails) | Line of ants on ground | Forest floor, near food |
| Worms | Emerging from soil | After rain, gardens |

---

### Biome-Specific Wildlife Decoration

| Biome | Wildlife Decoration |
|---|---|
| Forest | Owls, woodpeckers, foxes (tail visible behind bush), hedgehogs |
| Desert | Scorpions, lizards, vultures (circling), snakes (sunning on rocks) |
| Arctic | Penguins, seals, polar bears (distant), snowy owls |
| Swamp | Crocodile eyes (in water), herons, dragonflies, leeches |
| Ocean | Seagulls, crabs, dolphins (jumping), jellyfish, starfish |
| Cave | Bats (hanging, flying), cave spiders, glow worms, blind fish |
| Mountain | Eagles, mountain goats, marmots, hawks |
| Jungle | Parrots, monkeys, snakes, exotic butterflies, toucans |

---

## Views

| View | Description |
|---|---|
| Flora Browser | Searchable catalog of all plants by type, biome, and properties |
| Biome Palette | Pre-selected plants appropriate for each biome, quick-select |
| Seasonal Preview | See how flora looks in each season before placing |
| Farming Reference | All farmable plants with levels, yields, and growth times |
| Decoration Placer | Quick-place decorative natural objects from categorized list |
| Wildlife Preview | Browse ambient wildlife by biome, toggle on/off |
