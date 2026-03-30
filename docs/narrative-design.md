# Narrative Design

Game design document for the Narrative Design plugin. This system covers story structure, character writing, quest writing craft, environmental storytelling, and authoring tools. The goal is to turn quests from "kill 10 rats" into memorable experiences. At a studio like Jagex, this is the domain of content developers and narrative designers. In our system, the Dungeon Master gets the same toolset with AI assistance. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the narrative design module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Story Structure Templates | Pre-built narrative templates (Hero's Journey, Mystery, Heist) for quest design |
| Character Writing Tools | Character sheets, relationship maps, speech pattern guides, and arc planners |
| Quest Writer | Template-driven quest authoring with beat sheets, step builders, and branching visualizers |
| Environmental Storytelling Checklist | Per-area checklist ensuring every zone tells a visual story |
| Dialogue Editor | Tree-based dialogue authoring with conditions, variables, and preview |
| AI Writing Assistant | LLM-powered tools for drafting dialogue, backstories, and quest scripts |
| Tone Analyzer | AI evaluates whether dialogue matches an NPC's defined personality and speech pattern |
| Consistency Checker | Warns when quest content contradicts lore bible entries |
| Branching Visualizer | Full flowchart view of all possible paths through a quest |
| Playtest Mode | Walk through a quest in-engine to test pacing, flow, and fun |
| Feedback Collector | Players rate quests and leave comments after completion |
| Localization Integration | Dialogue stored as translation keys with auto-generated translation files |
| Voice Acting Notes | Direction notes attached to dialogue lines for voice actors |
| Character Relationship Mapper | Visual graph of NPC connections with labeled relationship types |
| Quest Anti-Pattern Warnings | Flags common quest design mistakes during authoring |

---

## 1. Story Structures

### Quest Narrative Templates

Each template defines a story shape. The Dungeon Master picks a template and the quest writer tool pre-populates beats and structure.

| Template | Structure | Best For |
|---|---|---|
| Hero's Journey | Call to adventure, trials, transformation, return | Epic quest lines, main story arcs |
| Mystery | Discovery, clues, red herrings, revelation | Detective quests, lore-heavy content |
| Rescue | Someone needs saving, journey, obstacles, rescue or failure | Emotional quests, timed urgency |
| Revenge | Wrong committed, hunt, moral choice, confrontation | Dark quests, villain backstory |
| Redemption | Character did wrong, atonement tasks, forgiveness or rejection | NPC development, faction quests |
| Discovery | Explore unknown, piece together history, unlock understanding | Archaeology, exploration, lore |
| Rivalry | Meet rival, compete, escalate, final showdown | PvP-themed PvE, recurring antagonist |
| Heist | Plan, assemble team, execute, escape as things go wrong | Thieving quests, group content |
| Tragedy | Things go well, hubris or mistake, downfall, consequences | Mature quests, world events |
| Comedy | Absurd situation, escalating silliness, punchline resolution | Light quests, holiday events |
| Moral Dilemma | Two valid choices, neither clearly right, consequences either way | Branching quests, faction conflict |
| Coming of Age | Prove yourself, face challenge alone, earn respect | Tutorial, guild initiation |
| War | Sides defined, escalation, battle, aftermath | Large-scale events, faction wars |
| Love Story | Meet, bond, obstacle, reunion or loss | Romance system quests |
| Survival | Stranded or trapped, resource scarcity, endure, escape or adapt | Wilderness content, hardcore modes |

### Pacing Rules

| Rule | Description |
|---|---|
| Start Strong | Open with action or intrigue, never "go talk to the guy across town" |
| Alternate Intensity | Fight, dialogue, puzzle, fight --- vary the tempo |
| Escalate Stakes | Village problem becomes regional threat becomes world-ending danger |
| Breather Moments | Let the player process what happened before the next beat |
| Climax is Hardest | The most dramatic and difficult moment comes at the peak |
| Earned Resolution | The player's own actions resolve the quest, not a convenient miracle |

### Emotional Beats

| Beat | Purpose | Example |
|---|---|---|
| Hook | Grab attention immediately | NPC runs up panicked, explosion in background, mysterious letter arrives |
| Tension | Build anticipation | Timer counting down, enemy growing stronger, ally missing |
| Surprise | Subvert expectations | Ally is the villain, dead NPC is alive, reward is a curse |
| Relief | Release built tension | Safe room after hard dungeon, comic moment after drama |
| Triumph | Player feels powerful and victorious | Boss defeated, town saved, puzzle cracked |
| Loss | Player feels consequence | NPC dies, item destroyed, choice has a cost |
| Wonder | Player feels awe | First sight of a grand city, discovering a hidden world, meeting a god |
| Dread | Player feels danger approaching | Environmental clues, NPC warnings, music shift |

---

## 2. Character Writing

### NPC Depth Tiers

Not every NPC needs a full backstory. Depth is allocated based on narrative importance.

| Tier | Depth | Count in World | Example |
|---|---|---|---|
| Tier 1 Icons | Full backstory, arc, voice, unique personality, central to main story | 5 to 15 | Main villain, mentor figure, love interest |
| Tier 2 Key NPCs | Backstory, personality, quest involvement, memorable traits | 20 to 50 | Quest givers, faction leaders, shop owners with personality |
| Tier 3 Supporting | Brief personality, functional role, a few unique lines | 50 to 200 | Guards with opinions, merchants with quirks, task givers |
| Tier 4 Ambient | Generic but present, category-based dialogue, fill the world | 200 to 1000 | Citizens, wandering NPCs, unnamed guards |
| Tier 5 Silent | No dialogue, purely visual presence | Unlimited | Background NPCs, animals, decorative characters |

### Character Sheet

Used for Tier 1 and Tier 2 NPCs. Each field feeds into dialogue generation, AI persona prompts, and consistency checking.

| Column | Type | Description |
|---|---|---|
| NPC ID | Integer | Foreign key to NPC system |
| Name | String | Full name, titles, and nicknames |
| Role | String | Game role (quest giver, shopkeeper, antagonist, mentor, companion) |
| Race | ID | Race ID from lore bible |
| Age | Integer | Current age and apparent age if different |
| Faction | ID | Faction ID and rank within the faction |
| Location | ID | Location ID where they live or work, with travel patterns |
| Appearance | Text | Physical description, distinctive features, clothing style |
| Personality | String List | Core traits from the trait system (brave, greedy, kind, paranoid, witty) |
| Motivation | Text | What drives them, what they want |
| Fear | Text | What they are afraid of, what would break them |
| Secret | Text | What they hide, what would change everything if revealed |
| Backstory | Text | Key life events, how they became who they are |
| Speech Pattern | Text | How they talk (formal, slang, accent, verbal tics, vocabulary level, sentence length) |
| Relationships | Object List | NPC IDs with relationship type (friend, enemy, family, mentor, rival, lover) |
| Arc | Text | How they change through the story, or whether they remain static |
| Quest Roles | ID List | Quest IDs they appear in and their role in each |
| Voice Direction | Text | Tone, pacing, emotional range for voice actors |
| AI Persona Prompt | Text | Auto-generated from above fields, used for AI freeform dialogue |

### Dialogue Writing Guidelines

| Guideline | Description |
|---|---|
| Distinct Voices | Every NPC should sound different, not all medieval formal |
| Speech Reflects Personality | Nervous characters use short sentences, academics use long words, soldiers are direct |
| NPCs Have Opinions | They comment on other NPCs, factions, world events, and the player |
| NPCs Reference the World | They mention weather, recent events, nearby locations, time of day |
| Important Info First | Critical information in the first dialogue option, not buried three menus deep |
| Humor Is Welcome | Puns, fourth-wall breaks, absurdity are part of the genre tradition |
| Avoid Exposition Dumps | Spread lore across multiple NPCs and conversations |
| Choices Have Consequences | Player dialogue choices produce visible effects, even small ones |
| NPCs Can Push Back | They can disagree, refuse requests, have bad days, lie to the player |
| Read It Aloud | If dialogue sounds unnatural when spoken, rewrite it |

### Character Relationship Map

Visual tool showing all NPC connections as an interactive graph.

| Feature | Description |
|---|---|
| Connection Lines | Lines between NPCs with relationship type label (friend, enemy, family, employer, rival, lover, mentor) |
| Color Coding | Green for positive, red for negative, grey for neutral |
| Click to Inspect | Click a connection to see relationship history and quest involvement |
| Auto-Update | Relationships update automatically when quests change NPC states |
| DM Override | Dungeon Master can manually set or override any relationship |
| Filter by Faction | Show only NPCs belonging to a specific faction |
| Filter by Quest | Show only NPCs involved in a specific quest |

---

## 3. Quest Writing

### Quest Design Document

Every quest gets a design document before any building begins. This document lives alongside the quest definition in the quests system.

| Column | Type | Description |
|---|---|---|
| Quest ID | Integer | Foreign key to quests system |
| Quest Name | String | Title that hints at content without spoiling |
| Series | String | Quest chain this belongs to, or standalone |
| Summary | Text | One paragraph describing what this quest is about |
| Hook | Text | How the player discovers this quest exists |
| Prerequisites | Object | Required quests, levels, items, faction reputation |
| Target Audience | Enum | New Players, Mid-Game, End-Game, Lore Enthusiasts, Completionists |
| Tone | Enum | Serious, Comedic, Dark, Whimsical, Epic, Mysterious, Bittersweet |
| Theme | String | What the quest is really about (loyalty, sacrifice, greed, identity, trust) |
| Stakes | Text | What happens if the player fails or walks away |
| Twist | Text | The surprise and when it hits, if any |
| Branching | Boolean | Whether there are player-driven divergent paths |
| Branch Points | Object List | Decision points with options and resulting paths |
| Consequences | Text | How completing this quest changes the world, NPCs, or player state |
| Rewards | Object | Items, XP, access, reputation, cosmetics, and narrative reward (closure, revelation) |
| Estimated Time | String | How long this quest should take to complete |
| Combat Difficulty | Enum | None, Easy, Medium, Hard, Very Hard, Boss |
| Puzzle Difficulty | Enum | None, Simple, Moderate, Complex, Expert |
| Steps | Object List | Ordered steps linking to the quest step system |
| Dialogue Script | Text | All NPC lines and player options for this quest |
| Test Notes | Text | How to verify the quest works, edge cases to check |

### Quest Anti-Patterns

Common quest design mistakes the system flags during authoring.

| Anti-Pattern | Why It Is Bad | Fix |
|---|---|---|
| Kill X of Y | No story, no engagement, pure grind | Give a reason to kill them, add a twist mid-task |
| Fed-ex Quest | Just running between NPCs with no challenge | Make the journey interesting, add encounters on the way |
| Exposition Dump | NPC talks for five minutes straight | Spread information across multiple NPCs, show instead of tell |
| Arbitrary Gate | Bring me 500 of this random item for no reason | Make requirements logical --- why does this NPC need this item |
| Fake Choice | Player chooses but outcome is identical | If you offer a choice, it must matter, even slightly |
| Plot Armor | NPC cannot die because they are needed later | Let them die, have a contingency NPC or altered storyline |
| Deus Ex Machina | Problem solved by something the player did not earn | Player actions should be what resolves the quest |
| Lore Contradiction | Quest contradicts established world history | Run the lore bible consistency checker before publishing |
| Invisible Progress | Player has no idea how close they are to completion | Show progress indicators, give feedback on each step |
| Backtracking | Player must cross the map repeatedly for no good reason | Consolidate steps or provide fast travel to relevant areas |

### Quest Writing Best Practices

| Practice | Description |
|---|---|
| Start with Why | Why does this quest exist for the player --- what is fun about it |
| Player is the Hero | The player should drive the action, not be a delivery person |
| React to History | NPCs should reference what the player has already done in the world |
| Short Is Fine | Five to fifteen minute quests are valid, not everything needs to be an epic |
| Blind Playtest | Have someone who did not write the quest play through it without guidance |
| Read Aloud | Read all dialogue out loud and rewrite anything that sounds wrong |
| Leave Room | Do not explain everything --- let the player interpret |
| Teach Something | The best quests teach a game mechanic, a lore fact, or a strategy |
| Reward the Curious | Hide optional content for players who explore beyond the critical path |

---

## 4. Environmental Storytelling

Telling stories without dialogue or quests, through the world itself. Every area should have a history that a curious player can piece together from visual clues.

### Techniques

| Technique | Description | Example |
|---|---|---|
| Ruins | Destroyed buildings imply history | Burnt village indicates a past attack, overgrown temple indicates abandoned faith |
| Corpses and Skeletons | Dead bodies tell tales | Skeleton clutching a key means they died trying to escape |
| Notes and Letters | Written fragments in the environment | Love letter in a destroyed house, final journal entry in a cave |
| Item Placement | Objects that suggest events | Overturned cart on road indicates ambush, feast table with rotting food indicates sudden evacuation |
| Architecture | Building style tells cultural history | Old elven columns repurposed in a human wall indicates conquered territory |
| Nature Reclaiming | Overgrowth on structures | Vine-covered castle was long abandoned, maintained garden means someone lives here |
| Graffiti and Carvings | Marks left by people | "Turn back" carved in dungeon wall, faction symbols marking territory |
| Sound Design | Audio tells story | Distant crying, echoing footsteps, creaking chains |
| Lighting | Light and shadow set mood | Dark corners indicate danger, warm light indicates safety, flickering indicates instability |
| Contrast | Juxtaposition creates meaning | Rich palace next to slum indicates inequality, flower growing in battlefield indicates hope |

### Environmental Storytelling Checklist

Applied to every area before it is finalized.

| Question | Purpose |
|---|---|
| What happened here before the player arrived? | Establish area history |
| Who lived, worked, or died here? | Ground the area in character |
| What evidence of that history is visible? | Ensure visual storytelling exists |
| Does the environment match the lore bible? | Consistency check |
| Is there at least one detail that makes a curious player stop and think? | Reward exploration |
| Does the area contrast with adjacent areas in tone or appearance? | Prevent visual monotony |
| Are there signs of current activity or only the past? | Distinguish active from abandoned |
| Would a player returning later notice changes? | Support living world feel |

---

## 5. Writing Tools

### Dialogue Editor

| Feature | Description |
|---|---|
| Tree View | Visual dialogue tree with branching paths and collapsible nodes |
| Character Voice Panel | Select NPC to see their personality, speech pattern, and backstory as reference while writing |
| Variable Insertion | Insert player name, quest state, item names, NPC names, and dynamic values into dialogue text |
| Condition Editor | Set conditions for dialogue branches (quest stage, reputation level, items held, time of day, race) |
| Preview | Read dialogue as the player would see it in the game text box with portraits |
| Spell Check | In-world dictionary that recognizes game-specific terms, race names, location names, and item names |
| Tone Analyzer | AI evaluates whether each line matches the NPC's defined personality and speech pattern |
| Consistency Check | Warns if dialogue references something contradicting the lore bible |
| Voice Acting Notes | Attach direction notes per line (emphasis words, emotion, pacing, pronunciation) |
| Localization Ready | Dialogue stored as translation keys with auto-generated translation template files |
| Bulk Edit | Search and replace across all dialogue trees for a specific NPC or quest |
| History | Full edit history per dialogue tree with diff view and rollback |

### Quest Writer

| Feature | Description |
|---|---|
| Template Selector | Pick a story structure template to pre-populate beats and guidance |
| Beat Sheet | Step-by-step emotional beat outline with guidance prompts for each beat |
| Step Builder | Visual step sequence editor linking to the quest step system in quests.md |
| NPC Assignment | Assign NPCs to quest roles with auto-check for availability and location conflicts |
| Reward Balancer | Input quest difficulty and length, receive suggested reward amounts based on existing quest data |
| Playtest Mode | Walk through the quest in-engine to test flow, pacing, and fun |
| Feedback Collector | After playtest, players rate the quest and leave structured comments |
| Branching Visualizer | Full flowchart showing all possible paths with condition labels on each branch |
| Anti-Pattern Scanner | Flags quest design mistakes from the anti-pattern list during authoring |
| Dependency Checker | Validates that all referenced NPCs, items, locations, and quests exist |

### Character Writer

| Feature | Description |
|---|---|
| Character Sheet Form | Guided form for all Tier 1 and Tier 2 fields with help text and examples |
| Relationship Mapper | Visual graph connecting this NPC to all related NPCs |
| Speech Sample Generator | AI generates five to ten sample dialogue lines matching the NPC's personality and speech pattern |
| Backstory Prompter | Series of guided questions that build a backstory (birthplace, defining moment, biggest regret, current fear) |
| Arc Planner | Define start state, catalyst, change, and end state for character development |
| Consistency Scorer | AI rates how consistent an NPC's behavior is across all quest appearances |
| Name Generator | Suggest names matching the NPC's race naming conventions from the lore bible |

### Lore Writer

| Feature | Description |
|---|---|
| In-World Book Editor | Write books, scrolls, and journals as they will appear in-game with formatting preview |
| Inscription Editor | Write short texts for gravestones, monuments, walls, signs, and plaques |
| Rumor Generator | AI generates NPC gossip lines consistent with current world state and lore |
| History Generator | AI expands brief timeline entries into full historical narratives |
| Mythology Builder | Guided prompts for creating creation myths, deity stories, prophecies, and legends |

### AI Writing Assistant

| Feature | Description |
|---|---|
| Continue Writing | AI continues from where the DM left off in the same style and tone |
| Rewrite in Voice | AI rewrites neutral text in a specific NPC's speech pattern |
| Generate Alternatives | AI provides three to five alternative versions of a line or paragraph |
| Suggest Consequence | Given a player choice, AI suggests realistic narrative consequences |
| Fill Blanks | AI completes partial quest outlines into full scripts with dialogue |
| World-Aware | AI has access to the lore bible and knows all characters, factions, and history for consistent output |
| Tone Matching | AI matches the DM's established tone (dark, comedic, epic) across all generated content |
| Red Flag Checker | AI identifies potentially problematic content (plot holes, anachronisms, contradictions, insensitivity) |
| Dialogue Variation | AI generates five versions of the same information delivered in different NPC voices |
| Scene Description | AI writes area descriptions and environmental storytelling details based on lore and location data |

---

## 6. Narrative Style Presets

The DM picks a narrative style for their world. This affects dialogue tone, AI writing suggestions, template defaults, and the overall feel of all written content. Styles are toggleable and mixable -- a world can be mostly High Fantasy with Comedy zones.

### Style Library

| Style | Tone | Dialogue Feel | Death Handling | Humor | Stakes | Examples |
|---|---|---|---|---|---|---|
| Scape (RS) | Witty, self-aware, serious when it matters | Punny, fourth-wall breaks, NPCs have attitude, examine text is always a joke | Annoying, not devastating | Constant but knows when to stop | Starts low, escalates to world-ending | RuneScape, OSRS |
| Dark Fantasy | Grim, consequential, morally grey | Sparse, heavy, every word counts, silence says more than speech | Permanent, brutal, meaningful | Rare, gallows humor only | Always high, no safe moments | Dark Souls, Game of Thrones, Elden Ring |
| High Fantasy | Epic, heroic, good vs evil, destiny | Grand speeches, prophecies, noble sacrifices, archaic vocabulary | Heroic sacrifice or defeat, never trivial | Light relief between drama, never at expense of stakes | World-saving, chosen one, cosmic | Lord of the Rings, WoW, Elder Scrolls |
| Anime/JRPG | Dramatic, friendship-powered, emotional extremes | Exclamations, power of bonds, dramatic reveals, inner monologue | Dramatic, often reversible, power of friendship revives | Comic relief characters, exaggerated reactions | Escalates from personal to universal | Final Fantasy, Genshin Impact |
| Sci-Fi | Technical, philosophical, exploration | Jargon, logs, data entries, measured and analytical | Clinical, statistical, "acceptable losses" | Dry wit, irony | Survival, discovery, ethical dilemmas | Mass Effect, Star Wars, EVE |
| Horror | Dread, isolation, unreliable | Fragmented, paranoid, contradictory, journal entries, whispers | Sudden, unfair, inevitable | None or deeply uncomfortable | Personal survival, sanity | Bloodborne, Silent Hill, Lovecraft |
| Comedy | Everything is absurd | Sarcasm, wordplay, pop culture, breaking fourth wall constantly | Slapstick, respawn is a joke, death is inconvenient not tragic | The entire point | Low, nothing matters too much | Borderlands, Monkey Island |
| Noir | Cynical, corrupt, narrated | Internal monologue, metaphor-heavy, world-weary, smoky | Cheap, meaningless, everyone dies eventually | Dark, bitter | Personal, small-scale but intense | Disco Elysium, LA Noire |
| Folklore | Ancient, mythological, fate-driven | Formal, parable-like, gods speak plainly, mortals speak reverently | Fated, mythic, transforms into constellation/monument | Trickster gods, ironic twists of fate | Cosmic, gods vs mortals | God of War, Hades |
| Cozy/Slice of Life | Warm, relationships, daily joy | Kind, gentle, encouraging, no conflict in dialogue, gratitude | Doesn't happen, or pet runs away and comes back | Wholesome, gentle humor, puns about farming | None, the world is safe | Stardew Valley, Animal Crossing |
| Western | Frontier justice, lawless, dusty | Drawl, few words, let actions speak, threats are quiet | Duels, hangings, dying in the dirt | Dry, understated, deadpan | Personal honor, survival, revenge | Red Dead Redemption |
| Ghibli | Whimsical, nature, coming-of-age, bittersweet | Gentle wonder, children see magic adults miss, nature speaks | Rare, transformative, beautiful sadness | Warm, character-based, never mean | Growing up, protecting nature, finding yourself | Spirited Away, Ni no Kuni |
| Superhero | Quippy, action, team dynamics | One-liners during combat, banter between allies, villains monologue | Dramatic but reversible, sacrifice plays | Constant quips, pop culture references | City/world level, personal stakes underneath | Marvel, DC |
| Lovecraftian | Cosmic horror, madness, insignificance | Academic degrading to incoherent, forbidden knowledge corrupts, sanity meters | Worse than death, madness, transformation into something wrong | None, humor means you don't understand the horror | Incomprehensible, the threat doesn't even know you exist | Call of Cthulhu, Darkest Dungeon |
| Pirate | Adventure, treasure, freedom | Arrr-peppered, colorful insults, sea shanty references, honor among thieves | Dramatic, walk the plank, buried with treasure | Swashbuckling wit, Jack Sparrow energy | Treasure, freedom, legendary status | Sea of Thieves, Monkey Island |
| Fairy Tale | Simple morals, magical, transformative | "Once upon a time", talking animals, rhyming witches, moral lessons | Rarely permanent, curses can be broken, true love saves | Silly characters, slapstick villains | Good vs evil (simple), save the princess/kingdom | Fable, classic Disney |
| Military | Duty, sacrifice, chain of command | Orders, briefings, call signs, clipped sentences, sir/ma'am | Honored, flag-draped, remembered | Soldier humor, dark and bonding | War, mission success, protecting civilians | Call of Duty campaign, MASH |
| Custom | DM defines all tone parameters | DM writes style guide document | DM defines | DM defines | DM defines | Your world, your rules |

### Style Properties (configurable per world)

| Property | Options | Description |
|---|---|---|
| Humor frequency | None, Rare, Occasional, Frequent, Constant | How often jokes appear in dialogue |
| Humor type | Pun, Sarcasm, Slapstick, Dark, Absurd, Wholesome, Fourth-wall | What kind of jokes |
| Death tone | Trivial, Inconvenient, Serious, Devastating, Permanent | How death is treated narratively |
| Examine text style | Informative, Witty, Lore-heavy, Minimal, Absurd | Tone of item/object examine text |
| NPC formality | Casual, Mixed, Formal, Archaic | How NPCs speak by default |
| Villain complexity | Pure evil, Sympathetic, Morally grey, Tragic, Comedic | Antagonist depth |
| Romance depth | None, Hinted, Light, Moderate, Deep | How far romance goes |
| Player agency tone | Chosen one, Ordinary person, One of many, Anti-hero, Tool of fate | How the narrative frames the player |
| Narrative stakes | Personal, Local, Regional, National, World, Cosmic | Scale of story consequences |
| Fourth wall | Never break, Rare winks, Regular nods, Constant breaks | Self-awareness level |
| Content darkness | Light, Moderate, Dark, Very dark, Unflinching | How dark themes get |

### How Style Affects Tools
When DM selects a style, all writing tools adjust:
- AI assistant writes in that tone
- Quest templates suggest appropriate structures (Horror = Mystery/Survival, Comedy = Absurd)
- Dialogue editor shows tone reference ("this NPC is in a Scape world — add a pun")
- Examine text generator matches style (Scape = "It's a bucket." Dark Fantasy = "A rusted vessel, stained with something darker than water.")
- Character backstory prompts match (Cozy = "What makes them smile?", Noir = "What did they lose?")

---

## Views

| View | Description |
|---|---|
| Quest Writer Workspace | Full quest authoring environment with template, beats, steps, and dialogue in one screen |
| Character Sheet Editor | Form-based NPC editor with all fields, relationship map, and AI tools |
| Dialogue Tree Editor | Visual tree editor with drag-and-drop nodes, condition panels, and preview |
| Environmental Storytelling Checklist | Per-area checklist with pass/fail markers and notes |
| Story Structure Selector | Gallery of narrative templates with descriptions and examples |
| NPC Relationship Graph | Force-directed graph of all NPC connections, filterable by faction or quest |
| Quest Branching Flowchart | Full flowchart view of a quest with all paths and conditions |
| AI Assistant Panel | Side panel for AI writing tools available in any editor view |
| Playtest Mode | In-engine quest walkthrough with annotation tools |
| Feedback Dashboard | Aggregated player ratings and comments per quest |
