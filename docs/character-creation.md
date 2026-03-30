# Character Creation

Game design document for the character creation system. A unified tool that creates both player characters and NPCs using the same interface. The same editor produces player characters, AI companions, traditional NPCs, and ambient townspeople. Inspired by Sims 4's best-in-class character creator, extended for MMO gameplay. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the character creation module. Servers can turn off features they do not need.

| Toggle | Default | Description |
|---|---|---|
| Personality Quiz | Off | Optional quiz before creation that suggests traits, aspirations, and starting path |
| Gender Options | On | Male, Female, Non-binary gender selection |
| Age Stages | On | Infant through Elder age selection |
| Voice Selection | On | Voice type and pitch per age stage |
| Interaction Boundaries | On | Player-set interaction permissions (romance, combat, trade, etc.) |
| Granular Randomizer | On | Per-category randomization toggles on the dice button |
| Direct Manipulation | On | Click-and-drag body part resizing on the 3D model |
| Body Sliders | On | Muscle mass, height, and body fat sliders |
| Face Manipulation | On | Click-and-drag facial feature adjustment |
| Makeup System | On | Full makeup application with categories |
| Tattoo System | On | Body tattoo placement on any body area |
| Body Hair | On | Arm, torso, back, and leg hair density sliders |
| Skin Details | On | Scars, freckles, birthmarks, vitiligo, stretch marks |
| Styled Looks | On | Pre-built outfit packages organized by context |
| Clothing Categories | On | Full wardrobe broken out by tops, bottoms, full body, shoes |
| Fantasy Options | On | Pointed ears, horns, tails, glowing eyes, unusual skin colors |
| Ghost/Undead States | Off | Alternative character states (ghost, vampire, werewolf, etc.) |
| Trait System | On | Aspirations, emotional, hobby, lifestyle, and social traits |
| Preference System | On | Activity, color, music, food, and environment preferences |
| Career/Path System | On | Starting career branching from aspiration |
| Elder Fate Traits | On | Elder-only special traits |
| NPC Creation Mode | On | Same tool creates NPCs with extra entity assignment options |
| AI Behavior Config | On | Trait-based and LLM personality configuration for NPCs |
| Conditional NPC Properties | On | Spawn conditions (quest-gated, time-gated, level-gated) for NPCs |
| Name Generator | On | Random name generation with cultural and style filters |
| Medical/Accessibility Options | On | Prosthetics, mobility aids, hearing aids, braces |

---

## Views

| View | Description |
|---|---|
| 3D Model Viewer | Rotatable, zoomable character preview --- the primary navigation surface |
| Face Editor | Click-to-edit face with subcategory panels |
| Body Editor | Click-to-edit body with subcategory panels |
| Outfit Browser | Searchable clothing catalog with filters |
| Trait Selector | Visual trait picker with descriptions and gameplay effects |
| Preference Panel | Like/dislike toggles per category |
| NPC Config Panel | Entity type, world assignment, AI behavior (NPC creation only) |
| Name Generator | Random name with cultural filters and preview |
| Summary | Full character sheet review before confirming |

---

## 1. Pre-Creation Setup

### Gender Selection

Icons only (no text labels). Three options.

| Option | Description |
|---|---|
| Male | Male body presets, clothing options, voice range |
| Female | Female body presets, clothing options, voice range |
| Non-binary/Custom | Full access to all presets and options from both categories |

Gender affects available body presets, clothing options, and voice range. All cosmetic items remain accessible regardless of selection.

### Age Stage

| Stage | Description | Available Traits | Body Options |
|---|---|---|---|
| Infant | Newborn, carried or crib-bound | Minimal (1--2) | Very limited |
| Toddler | Small child, limited movement | Limited (2--3) | Limited |
| Child | Young, cannot do adult activities | Moderate (3--4) | Moderate |
| Teen | Nearly adult, some restrictions | Most (4--5) | Most |
| Young Adult | Full access, peak physical stats | All | All |
| Adult | Full access, experienced | All | All |
| Elder | Full access, wisdom bonuses, fate traits | All + Elder-only | All + Elder-only |

### Voice

| Property | Description |
|---|---|
| Pitch Slider | Per age stage (voice changes with age) |
| Voice Type | Soft, deep, raspy, bright, gruff, melodic |
| Preview Button | Plays a voice sample for the current selection |

### Personality Quiz

Optional onboarding quiz that suggests traits, aspirations, and starting path based on answers. Toggled off by default.

| Setting | Description |
|---|---|
| Gameplay Impact | DM configures whether quiz results affect gameplay stats |
| Recommendation Only | Quiz suggests traits but player chooses freely |
| Skippable | Player can skip entirely and pick traits manually |

### Interaction Boundaries

Player-defined permission system for NPC and player interactions. Adjustable post-creation in account settings.

| Boundary | Options |
|---|---|
| Romance eligibility | All, Same age range, Specific genders, None |
| Combat eligibility | Open PvP, Consensual only, Never |
| Trade eligibility | Everyone, Friends/Clan, Verified players, Nobody |
| Follow permission | Everyone, Friends, Nobody |
| House entry | Everyone, Friends/Clan, Invited only, Nobody |
| Healing permission | Everyone, Party only, Nobody |
| Message permission | Everyone, Friends, Nobody |
| Age interaction | All ages, Adults only, Same age range |
| Faction rules | All factions, Allied only, Same faction only |
| Reputation threshold | Minimum reputation score to interact (0--100 slider) |

---

## 2. Randomizer

### Dice Button

Single click to randomize the character. Granular toggles control what gets randomized. Players can mix and match --- randomize body but keep chosen hair, randomize face but keep skin tone.

| Toggle | Description |
|---|---|
| All | Randomize everything |
| Body Shape | Randomize muscle, height, proportions |
| Face | Randomize face shape and features |
| Skin Tone | Randomize skin color |
| Hair | Randomize hair style and color |
| Voice | Randomize voice type and pitch |
| Clothing | Randomize outfit |
| Makeup | Randomize makeup |
| Traits | Randomize personality traits |
| Name | Generate random name |

---

## 3. 3D Model Viewer

The character model is the primary navigation surface. All menus are accessed by clicking directly on the model.

### Controls

| Control | Action |
|---|---|
| Left-click drag | Rotate character |
| Scroll wheel | Zoom in/out |
| Hover body part | Highlight indicating clickable zone |
| Click body part | Open that part's editor |

### Click Zones

| Zone | Opens |
|---|---|
| Head/Face | Face Menu (section 4) |
| Any body part (neck through feet) | Body Menu (section 5) |

The character IS the UI. All navigation is through clicking the model.

---

## 4. Face Menu

Opened by clicking the head or face on the 3D model.

### Face

| Subcategory | Description |
|---|---|
| Skin Tones | Color palette --- dozens of natural tones plus fantasy colors (blue, green, grey, red) |
| Face Shapes | Visual grid of preset shapes (round, oval, square, heart, diamond, long) |
| Skin Details | Freckles, moles, birthmarks, wrinkles, scars, blemishes, pores |
| Teeth | Tooth shape, color, gaps, fangs (fantasy), missing teeth |
| Eye Details | Eye shape, color (including fantasy colors), iris patterns, heterochromia, glow (fantasy), pupil shape |
| Nose | Shape presets (button, aquiline, broad, narrow, upturned) |
| Mouth/Lips | Shape, fullness, color |
| Ears | Shape, size, pointed (elf), long (fantasy), pierced |
| Jawline | Shape, width, chin prominence |
| Eyebrows | Shape, thickness, color, arch |

### Direct Face Manipulation

Click and drag any facial feature to adjust in real time.

| Feature | Drag Effect |
|---|---|
| Nose | Resize and reshape |
| Eyes | Wider or narrower spacing |
| Jaw | Widen or narrow |
| Ears | Adjust position and size |
| Forehead | Adjust height |
| Cheekbones | Adjust prominence |
| Chin | Adjust length |

### Hair

| Subcategory | Description |
|---|---|
| Short | Buzz, crew, pixie, bob, fades, undercuts |
| Medium | Shoulder length, layered, shag, wolf cut |
| Long | Straight, wavy, curly, braids, dreadlocks |
| Updo | Bun, ponytail, pinned, crown braid, chignon |
| Facial Hair | Stubble, goatee, full beard, mustache, mutton chops, braided beard |
| Color | Natural palette + fantasy colors + gradient/highlights/ombre |
| Texture | Straight, wavy, curly, coily, kinky |

### Headwear

| Subcategory | Description |
|---|---|
| Brimmed | Sun hats, witch hats, cowboy hats, fedora, wide brim |
| Brimless | Beanie, skullcap, turban, headband, crown, tiara |
| Baseball Caps | Standard, snapback, trucker, fitted |
| Head Decorations | Flowers, feathers, horns (fantasy), antlers, halos, circlets, veils |
| Helmets | Combat helmets (medieval, modern, fantasy) --- cosmetic in creator |
| Hoods | Hooded cloaks, cowls, wraps |

### Face Accessories

| Subcategory | Description |
|---|---|
| Piercings | Ear (lobe, helix, tragus), nose (stud, ring, septum), lip, eyebrow, tongue |
| Glasses | Prescription, sunglasses, monocle, goggles, visor |
| Necklaces | Chains, pendants, chokers, torcs, beaded, layered |
| Medical | Eye patch, hearing aid, breathing apparatus |
| Fantasy | Third eye, face runes, magical markings, ethereal features |

### Makeup

| Subcategory | Description |
|---|---|
| Eyes | Eyeshadow (colors, patterns, blend), liner weight |
| Eyeliner | Wing, cat eye, smudged, dramatic, subtle |
| Eyelashes | Natural, full, dramatic, colored, false lashes |
| Cheeks | Blush (colors, placement), contour, highlight |
| Lips | Lipstick (matte, glossy, ombre), lip liner |
| Face Paint | War paint, tribal markings, festival, sports, cultural designs |
| Fantasy | Glowing marks, runic tattoos, ethereal shimmer, void corruption marks |

---

## 5. Body Menu

Opened by clicking any body part (neck through feet) on the 3D model.

### Direct Body Manipulation

Click and drag any body part. Arrows appear indicating valid drag directions.

| Body Part | Drag Effect |
|---|---|
| Neck | Width (left/right) |
| Shoulders | Width (left/right) |
| Biceps | Size (left/right) |
| Forearms | Size (left/right) |
| Chest | Size (left/right and up/down) |
| Stomach | Size (left/right) |
| Hips | Width (left/right) |
| Thighs | Size (left/right) |
| Calves | Size (left/right) |

### Body Sliders

| Slider | Range | Description |
|---|---|---|
| Muscle Mass | Scrawny to Buff | Small dumbbell icon to large dumbbell icon |
| Height | Short to Tall | Down arrow to up arrow |
| Body Fat | Lean to Heavy | Adjusts overall body fat distribution |

### Body Menu Icons

The body menu uses icon-based navigation. Seven icons along the side panel.

#### Icon 1: Body

| Subcategory | Description |
|---|---|
| Skin Tones | Full color palette (shared with face skin tones) |
| Body Types | Preset body shapes --- thin, athletic, muscular, stocky, heavyset, curvy, lean, bodybuilder |
| Tattoos | Full body tattoo placement --- arms, legs, back, chest, hands, neck. Hundreds of designs |
| Body Hair | Arm hair (density slider), torso/chest hair, back hair, leg hair |
| Body Scars | Scar placement on any body area --- slash, burn, surgical, battle wounds |
| Skin Details | Body freckles, stretch marks, cellulite, vitiligo, body moles |
| Medical | Prosthetics, braces, casts, mobility aids |

#### Icon 2: Styled Looks

Pre-built complete outfits organized by context. Selecting a styled look equips a full outfit at once.

| Context | Description |
|---|---|
| Everyday | Default casual wear |
| Formal | Suits, gowns, dress clothes |
| Athletic | Workout gear, sports uniforms |
| Sleepwear | Pajamas, nightgowns, robes |
| Swimwear | Bathing suits, board shorts, bikinis |
| Cold Weather | Coats, scarves, gloves, boots |
| Hot Weather | Light fabrics, shorts, sandals |
| Party | Night out clothes, flashy outfits |
| Work/Uniform | Job-specific uniforms, armor sets |
| Combat | Battle gear, armor (cosmetic in creator) |
| Ceremonial | Ritual robes, religious vestments |
| Cultural | Region-specific traditional clothing |

#### Icon 3: Tops

Blouses, jackets, t-shirts, sweaters, tank tops, button-ups, sweatshirts, suit jackets, polos, bras/sports bras, swimsuit tops, vests, tunics, robes (upper), armor pieces (chest), cloaks.

#### Icon 4: Full Body

Jumpsuits, long dresses, short dresses, matching sets, outerwear (full), costumes, robes (full), lingerie, aprons, swimsuits (one piece), armor sets (full), uniforms.

#### Icon 5: Bottoms

Pants, skirts, shorts, leggings/tights, jeans, cropped pants, underwear, swimwear (bottom), kilts, sarongs, armor pieces (legs).

#### Icon 6: Body Accessories

Bracelets, gloves, rings, fingernails (shape, length, color, design), toenails (color, design), leggings/stockings, socks, skin effects (glow, shimmer, ethereal --- fantasy), watches, belts, scarves, arm wraps, bandages.

#### Icon 7: Shoes

Sandals, flats, loafers, slippers, heels, wedges, sneakers, lace-ups, boots (ankle, mid, knee-high), combat boots, armored boots, barefoot option, fantasy options (hooves, clawed feet).

---

## 6. Traits and Personality

### Aspirations

Life goal. Player picks one aspiration that defines their character's driving motivation and provides a passive gameplay bonus.

| Aspiration | Description | Gameplay Effect |
|---|---|---|
| Athletic | Physical excellence | Bonus XP in combat, agility, strength |
| Creative | Artistic expression | Bonus XP in crafting, construction, fletching |
| Criminal | Underworld ambition | Bonus XP in thieving, stealth, lockpicking |
| Family | Relationships and home | Bonus to social skill, housing, pet raising |
| Food | Culinary mastery | Bonus XP in cooking, farming, herblore |
| Fortune | Wealth accumulation | Bonus GP from all sources, better shop prices |
| Knowledge | Intellectual pursuit | Bonus XP in magic, runecrafting, prayer |
| Love | Deep connections | Bonus to NPC affinity gain, relationship building |
| Nature | Harmony with the wild | Bonus XP in farming, hunter, woodcutting, fishing |
| Popularity | Social influence | Bonus to clan events, socializing skill, chat reach |

### Career/Path

Each aspiration offers 2--4 career paths. Career affects starting gear, starting location, early quests, NPC introductions, and skill XP bonuses. Career does NOT lock content --- a Fortune character can still train combat.

| Aspiration | Career Options |
|---|---|
| Athletic | Bodybuilder, Athlete, Warrior, Martial Artist |
| Creative | Artist, Musician, Writer, Architect |
| Criminal | Thief, Smuggler, Assassin, Con Artist |
| Family | Caretaker, Homesteader, Breeder, Matchmaker |
| Food | Chef, Brewer, Farmer, Forager |
| Fortune | Merchant, Banker, Treasure Hunter, Gambler |
| Knowledge | Scholar, Wizard, Alchemist, Archaeologist |
| Love | Bard, Diplomat, Counselor, Socialite |
| Nature | Ranger, Druid, Farmer, Beast Tamer |
| Popularity | Herald, Entertainer, Guild Leader, Politician |

### Emotional Traits

Pick 1--3 based on age stage. These define the character's emotional baseline.

| Trait | Description |
|---|---|
| Cheerful | More positive mood baseline, recovers from sadness faster |
| Childish | Enjoys toys and games, lower social expectations |
| Clumsy | Occasional skill failures, endearing to some NPCs |
| Creative | Bonus to art and crafting inspiration |
| Erratic | Mood swings, unpredictable dialogue options |
| Genius | Bonus to knowledge tasks, faster learning |
| Gloomy | Lower mood baseline, deeper emotional dialogue |
| Goofball | Humor-based interactions, joke dialogue options |
| Hot-headed | Faster anger buildup, combat initiative bonus |
| Perfectionist | Higher quality outputs, longer task times |
| Romantic | Bonus to relationship building, poetic dialogue |
| Self-assured | Confidence-based dialogue, resistance to intimidation |
| Brave | Reduced fear effects, bonus in dangerous areas |
| Cautious | Bonus to trap detection, slower movement in unknowns |
| Curious | Bonus to exploration XP, examines everything |
| Stoic | Resistant to mood changes, neutral baseline |
| Impulsive | Faster action speed, occasional unintended actions |
| Patient | Bonus to fishing, crafting, and waiting tasks |
| Stubborn | Resistance to persuasion, bonus to determination checks |
| Adaptable | Faster environment adjustment, versatile bonuses |

### Hobby Traits

Pick 1. Defines the character's primary leisure interest.

| Trait | Description |
|---|---|
| Art Lover | Bonus mood near art, enjoys painting and sculpture |
| Bookworm | Bonus XP from reading, enjoys libraries |
| Foodie | Enhanced taste system, enjoys cooking and dining |
| Geek | Bonus to puzzles and minigames |
| Loves Outdoors | Bonus mood in nature, enjoys camping and exploration |
| Music Lover | Bonus mood near music, enjoys instruments |
| Collector | Bonus to finding rare items, enjoys completing sets |
| Tinkerer | Bonus to repair and invention, enjoys mechanisms |
| Storyteller | Enhanced dialogue options, enjoys lore and history |
| Athlete | Bonus to physical activities, enjoys competition |

### Lifestyle Traits

Pick 1--2. Defines daily habits and behavioral patterns.

| Trait | Description |
|---|---|
| Active | Faster run energy recovery, enjoys movement |
| Glutton | Eats more, gains more from food |
| Kleptomaniac | Occasional steal prompts, bonus to thieving |
| Lazy | Slower task speed, prefers rest |
| Materialistic | Bonus mood from expensive items |
| Neat | Bonus in clean environments, penalty in messy ones |
| Perfectionist | Higher quality outputs, longer task times |
| Slob | No penalty from messy environments |
| Vegetarian | Cannot eat meat, bonus from plant-based food |
| Night Owl | Bonus stats at night, penalty in early morning |
| Early Bird | Bonus stats in morning, penalty at night |
| Minimalist | Bonus from fewer possessions, penalty from clutter |
| Hoarder | Expanded storage tolerance, enjoys collecting |
| Generous | Bonus reputation from giving, enjoys charity |
| Frugal | Better shop prices, enjoys bargains |

### Social Traits

Pick 1. Defines how the character interacts with others.

| Trait | Description |
|---|---|
| Friendly | Faster relationship building, warm dialogue |
| Evil | Dark dialogue options, bonus to intimidation |
| Good | Heroic dialogue options, bonus to healing others |
| Jealous | Negative mood from others' success, competitive |
| Loner | Bonus when alone, penalty in crowds |
| Loyal | Stronger clan and friend bonuses |
| Mean | Insult dialogue options, intimidation bonus |
| Outgoing | Bonus in social situations, enjoys crowds |
| Snob | Bonus mood from luxury, penalty from common items |
| Pacifist | Cannot initiate PvP, bonus to diplomacy |
| Competitive | Bonus in contests, driven by leaderboards |
| Protective | Bonus to guarding and defending others |
| Charismatic | Bonus to persuasion and NPC affinity |
| Suspicious | Bonus to detecting lies, slower trust building |
| Trusting | Faster trust building, vulnerable to deception |

### Preferences

| Category | Options |
|---|---|
| Activities | List of activities the character enjoys or dislikes (toggle like/dislike per activity) |
| Colors | Favorite and disliked colors (affects mood near those colors if mood system enabled) |
| Music Genre | Preferred music styles (affects mood when hearing them) |
| Food | Preferred food types (affects cooking and eating satisfaction) |
| Environment | Preferred biome or setting (bonus stats in favorite environment) |
| Social | Prefers small groups vs crowds, introverted vs extroverted |

### Elder Fate Traits

Available only to Elder age stage characters. Defines how the character approaches their later years.

| Trait | Description |
|---|---|
| Legacy | Focused on what they leave behind --- bonus to mentoring, crafting heirlooms |
| Mentor | Focused on teaching younger characters --- bonus XP when training others |
| Retired | Reduced activity demands, wisdom bonuses to knowledge skills |
| Adventurer | Still active, ignores age penalties --- no stat reduction |
| Spiritual | Prayer and religious bonuses, bonus to holy sites |
| Storyteller | Lore and dialogue bonuses, enhanced NPC conversations |

### Ghost/Undead States

Alternative character states. Toggled off by default at the module level.

| State | Visual Effect | Movement Rules | Interaction Changes | Ability Modifications |
|---|---|---|---|---|
| Living | Standard appearance | Standard movement | Standard | Standard |
| Ghost | Translucent, spectral glow | Walk through walls | Different dialogue options | Immune to physical, weak to magic |
| Undead | Decayed appearance | Standard movement | Feared by some NPCs | Immune to poison, weak to holy |
| Vampire | Pale, fanged | Standard + bat form | Feared/hated by some, loved by others | Night bonus, sun penalty |
| Werewolf | Normal (transforms) | Standard + wolf form | Trust issues with NPCs | Moon-phase dependent |
| Cursed | Dark aura, visual corruption | Standard | NPCs sense the curse | Debuffs with hidden benefits |
| Ethereal | Semi-transparent, floating | Hover movement | Limited physical interaction | Magic bonus, physical penalty |
| Possessed | Glowing eyes, erratic movement | Standard | Unpredictable dialogue | Random ability swaps |

---

## 7. Name and Identity

| Field | Description |
|---|---|
| First Name | Character's given name |
| Last Name | Character's family name |
| Display Name | What shows above the character's head in-game (can differ from first/last) |
| Title | Prefix or suffix earned through gameplay (shown before or after name) |
| Nickname | Informal name visible to friends and clan members |
| Name Generator | Random name button with cultural and style filters (fantasy, medieval, modern, sci-fi) |

---

## 8. NPC/Entity Assignment

This section only appears when creating NPCs. Same tool, extra options at the end. Links to npcs.md for schedule and service systems, monsters.md for combat entity stats.

### Entity Type

| Type | Description |
|---|---|
| Player Character | Standard player account |
| AI Companion | Follows a player, AI-controlled, uses trait-based behavior |
| Traditional NPC | Stands in place, dialogue when talked to, runs a shop or service |
| Wandering NPC | Moves around a radius, has a schedule, ambient life |
| Quest NPC | Gives or progresses quests, may be stationary or wandering |
| Guard NPC | Patrols area, enforces rules, attacks hostiles |
| Ambient NPC | Pure decoration --- walks, sits, adds life to settlements |
| Monster NPC | Combat entity (links to monsters.md for combat stats) |

### World Assignment

| Property | Type | Description |
|---|---|---|
| World/Server | String | Which world this character exists in |
| Starting Location | Coordinates | Spawn coordinates (x, y, layer) |
| Home Lot | Integer | Assigned lot or property (lot ID) |
| Wander Radius | Integer | How far NPC can roam from spawn (tiles) |
| Schedule | Reference | Day/night behavior, weekly routine (links to npcs.md schedule system) |

### AI Behavior

For NPC entity types. Defines how the NPC thinks and acts.

| Property | Type | Description |
|---|---|---|
| AI Mode | Enum | None (player), Scripted (traditional NPC), Trait-Based (uses personality), LLM-Powered (freeform AI) |
| Personality Prompt | String | Auto-generated from traits + preferences + aspiration (fed to LLM if enabled) |
| Behavior Rules | List | What the NPC will and will not do (will not leave town, will not fight, always greets players) |
| Knowledge Scope | List | What the NPC knows about (their shop, their quest, local area, world lore) |
| Memory | Boolean + Integer | Remembers past interactions with players (toggle on/off, depth configurable) |
| Relationship Tracking | Boolean | Tracks affinity per player who interacts with them |
| Combat Behavior | Enum | Aggressive, Defensive, Passive, Flee, Support (if combat-capable) |
| Death Behavior | Enum | Respawn (timer), Permanent death, Ghost form, Replacement NPC spawns |

### Conditional Properties

| Property | Type | Description |
|---|---|---|
| Appears After Quest | Quest ID | Only spawns after specific quest completed |
| Appears During Event | Event ID | Only spawns during world event |
| Appears At Time | Time Range | Only present during day/night/specific hours |
| Level Gated | Integer Range | Only visible to players above or below certain level |
| Faction Gated | Faction ID | Only interactable by specific faction |
| Mode Gated | Mode ID | Only exists in specific game modes |
