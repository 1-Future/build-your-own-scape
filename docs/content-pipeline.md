# Content Pipeline

Game design document for the Content Pipeline plugin. This system covers the end-to-end workflow for creating game content --- from initial idea to live deployment and post-release monitoring. At a studio like Jagex, this process involves dozens of developers, artists, QA testers, and community managers working through structured phases. In our system, the Dungeon Master manages the same pipeline with tooling support and AI assistance. Every subsystem is modular --- Dungeon Masters toggle each feature on or off per world.

---

## Module Toggles

Each toggle enables or disables a subsystem within the content pipeline module. Servers can turn off features they do not need.

| Toggle | Description |
|---|---|
| Content Pipeline System | Core pipeline tracking from idea through release |
| Design Document Templates | Pre-built design document structures for each content type |
| Review Gates | Mandatory approval checkpoints before content advances to the next phase |
| Art Pipeline Tracking | Status tracking for concept art, models, textures, rigging, animation, and VFX |
| Testing Framework | Unit tests, integration tests, playtests, balance tests, and edge case tests |
| Bug Tracker | Bug reporting, priority assignment, fix verification, and regression testing |
| Content Calendar | Quarterly roadmap, monthly milestones, weekly sprints, and daily standups |
| Deployment Automation | Scheduled deployment, staging verification, rollback capability |
| Post-Release Monitoring | Metrics collection, player reception tracking, hotfix pipeline |
| Community Communication Tools | Patch notes, dev blogs, polls, social media, and bug acknowledgment |
| AI-Assisted Content Creation | AI tools for generating design documents, test plans, and balance suggestions |
| Playtest Scheduling | Schedule and manage internal and external playtests |
| Metrics Collection | Track engagement, completion rates, economy impact, and player sentiment |
| Retrospective Tools | Post-release review with structured what-went-well and what-to-improve |

---

## 1. Ideation Phase

### Idea Sources

| Source | Description |
|---|---|
| DM Vision | Core game design direction and world-building goals |
| Player Feedback | Feature requests, bug reports, poll results, community discussion threads |
| Lore Requirements | Story needs driven by the lore bible (faction needs a quest line, god needs a temple) |
| Gameplay Gaps | Missing content identified through analysis (no mid-level fishing spot, no quest for this skill combination) |
| Seasonal | Holiday events, anniversary celebrations, seasonal themed content |
| Competitive | Features successful in other games that fit the world and design philosophy |
| Community Events | Player-submitted content ideas, building competitions, fan art inspiration |
| AI Suggestions | AI analyzes existing content and suggests areas for expansion based on gaps and patterns |

### Idea Entry

| Column | Type | Description |
|---|---|---|
| Idea ID | Integer | Unique identifier |
| Title | String | Short descriptive title |
| Source | Enum | DM Vision, Player Feedback, Lore, Gameplay Gap, Seasonal, Competitive, Community, AI |
| Summary | Text | One paragraph description of the idea |
| Type | Enum | Quest, Boss, Skill Update, Item Set, Area, Minigame, System, Event, Quality of Life |
| Priority | Enum | Critical, High, Medium, Low, Backlog |
| Status | Enum | Proposed, Approved, In Design, In Build, Testing, Staged, Released, Rejected |
| Submitted By | String | Who submitted the idea |
| Date | Date | When the idea was submitted |
| Evaluation | Object | Scores for each evaluation criterion |

### Idea Evaluation Criteria

| Criterion | Question |
|---|---|
| Fun | Is this fun to play? Would the DM want to do this as a player? |
| World Fit | Does this make sense in the established lore and tone? |
| Gap Fill | Does this serve an unmet need in the game? |
| Buildable | Can this be built with available tools, assets, and time? |
| Balanced | Does this break existing systems or create unfair advantages? |
| Maintainable | Will this need constant updates or does it sustain itself? |
| Reusable | Can elements of this be reused in future content? |
| Scope | Is this the right size for the time and resources available? |

---

## 2. Design Phase

### Design Document

Every piece of content gets a design document before building begins.

| Section | Type | Description |
|---|---|---|
| Document ID | Integer | Unique identifier, linked to Idea ID |
| Overview | Text | What is this content? One paragraph summary |
| Goals | Text | What should the player experience? What is the point? |
| Target Audience | Enum | New Players, Mid-Game, End-Game, All, Completionists, PvP, Social |
| Requirements | Object | Existing systems this content depends on (skills, quests, locations, items) |
| Mechanics | Text | Detailed system design with numbers, formulas, and interaction rules |
| Content | Text | Story, dialogue, lore, NPC details for narrative content |
| Assets Needed | Object List | Models, textures, animations, sounds, music with status tracking |
| Rewards | Object | Player rewards (items, XP, access, cosmetics, titles) with balance justification |
| Testing Plan | Text | How this will be tested, edge cases to check, acceptance criteria |
| Timeline | Object | Estimated time for each phase (design, art, build, test, polish) |
| Risk Assessment | Text | What could go wrong, dependencies, technical challenges, contingency plans |
| References | URL List | Links to similar content, inspiration sources, relevant docs |

### Review Gate

Design documents must be reviewed and approved before building begins.

| Reviewer | Checks |
|---|---|
| Lead Designer | Gameplay is fun, mechanics are sound, scope is appropriate |
| Lore Writer | Content is consistent with lore bible, naming follows conventions |
| Technical Lead | Implementation is feasible, no performance concerns, integrates with existing systems |
| Community Manager | Predicts player reception, identifies potential controversy, suggests communication approach |
| Balance Reviewer | Rewards are proportional, no economy-breaking items, no power creep |

All reviewers must approve before the content moves to the build phase. Rejections include specific feedback and a path to resubmission.

---

## 3. Asset Creation Phase

### Art Pipeline

Sequential stages for creating visual and audio assets. Each stage has a status and assignee.

| Stage | Description | Output |
|---|---|---|
| Concept Art | 2D sketches of characters, items, and environments | Reference images |
| 3D Modeling | Build the mesh in Blender or similar tool | 3D model file |
| Texturing | Apply colors, materials, and surface details | Textured model |
| Rigging | Add bones and skeleton for animation | Rigged model |
| Animation | Create movement sequences (walk, attack, idle, emote) | Animation clips |
| VFX | Particle effects, spot animations, visual feedback | VFX assets |
| Audio | Sound effects, music tracks, ambient sounds, voice recording | Audio files |
| Integration | Import into engine, test in-game, adjust scale and positioning | In-engine asset |

### Asset Entry

| Column | Type | Description |
|---|---|---|
| Asset ID | Integer | Unique identifier |
| Document ID | Integer | Foreign key to the design document that requires this asset |
| Name | String | Descriptive asset name |
| Type | Enum | Model, Texture, Animation, VFX, SFX, Music, Voice, UI Element |
| Stage | Enum | Concept, Modeling, Texturing, Rigging, Animation, VFX, Audio, Integration, Complete |
| Assignee | String | Who is working on this asset |
| Due Date | Date | When this asset is needed |
| Notes | Text | Technical requirements, reference links, feedback from review |
| File Path | String | Location of the asset file |

### Content Data Pipeline

Creating the game data that connects assets to gameplay.

| Step | Description |
|---|---|
| Define Properties | Create item, NPC, or object entries in the relevant spreadsheet editor |
| Write Dialogue | Author dialogue trees in the dialogue editor |
| Script Quest Steps | Build quest step sequences in the quest writer |
| Configure Drop Tables | Set up loot tables with items, weights, and conditions |
| Set Up Shops | Configure shop inventories with stock, prices, and restock rules |
| Place in World | Position objects, NPCs, and resources in the world builder |
| Configure Logic | Set up triggers, conditions, state changes, and event handlers |
| Link Everything | Connect quest to NPC to shop to items to locations to dialogue |
| Validate | Run dependency checker to confirm all references resolve |

---

## 4. Build Phase

### Implementation Checklist

Per-content-type step sequences. Each step links to the relevant editor tool.

#### New Item

| Step | Tool | Description |
|---|---|---|
| 1 | Item Editor | Create item entry with all properties (stats, examine text, tradeable, stackable) |
| 2 | Asset Pipeline | Create or assign item model and icon |
| 3 | Drop Table Editor | Add item to relevant monster and skilling drop tables |
| 4 | Shop Editor | Add item to relevant shop inventories if applicable |
| 5 | Lore Writer | Write examine text and any associated in-game books |
| 6 | Test | Verify item appears correctly, stats are right, drops function, shop sells |

#### New NPC

| Step | Tool | Description |
|---|---|---|
| 1 | Character Writer | Create character sheet with personality, backstory, speech pattern |
| 2 | Asset Pipeline | Create or assign NPC model, rig, animate |
| 3 | Dialogue Editor | Write all dialogue trees and conversation options |
| 4 | World Builder | Place NPC in the world with patrol path and spawn rules |
| 5 | Service Config | Configure NPC services (shop, bank, quest start, transportation) |
| 6 | Test | Verify NPC renders, dialogue works, services function, pathing is correct |

#### New Quest

| Step | Tool | Description |
|---|---|---|
| 1 | Quest Writer | Write quest design document with all fields |
| 2 | Dialogue Editor | Write all quest dialogue and branching options |
| 3 | Quest Step Builder | Build step sequence with objectives, conditions, and transitions |
| 4 | World Builder | Place quest-specific NPCs, objects, and area markers |
| 5 | Reward Config | Configure all quest rewards (items, XP, reputation, access) |
| 6 | Requirement Config | Set prerequisites (quests, levels, items, faction standing) |
| 7 | Playtest | Walk through entire quest start to finish |
| 8 | Test | Verify all paths, edge cases, logout recovery, full inventory handling |

#### New Boss

| Step | Tool | Description |
|---|---|---|
| 1 | Boss Designer | Design boss mechanics, phases, attack patterns, and special moves |
| 2 | Asset Pipeline | Create boss model, rig, animate all attacks and transitions |
| 3 | Drop Table Editor | Create boss-specific drop table with unique items |
| 4 | World Builder | Design boss arena with mechanics-relevant terrain |
| 5 | Difficulty Config | Configure difficulty scaling, HP values, damage numbers per mode |
| 6 | Test | Extensive testing of all phases, all difficulty modes, edge cases, death mechanics |

#### New Area

| Step | Tool | Description |
|---|---|---|
| 1 | Settlement Design | Design area layout following settlement design principles |
| 2 | Terrain Painter | Paint terrain with biome-appropriate textures |
| 3 | World Builder | Place buildings, objects, resource nodes, decorations |
| 4 | NPC Placement | Add NPCs with roles, dialogue, services, and patrol routes |
| 5 | Zone Config | Configure zone properties (PvP rules, music, ambient sounds, lighting) |
| 6 | Environmental Storytelling | Apply environmental storytelling checklist |
| 7 | Pathfinding | Test navigation and pathfinding across the area |
| 8 | Test | Verify all NPCs, objects, exits, and zone transitions work correctly |

---

## 5. Testing Phase

### Testing Types

| Type | What | Who | When |
|---|---|---|---|
| Unit Test | Individual mechanic works correctly (drop rate, damage formula, XP calculation) | Developer | During build |
| Integration Test | Systems work together (quest step triggers NPC dialogue, shop sells correct items) | Developer | After build |
| Playtest | Is it fun? Is the difficulty right? Is anything confusing? | Internal team, trusted players | After integration |
| Balance Test | Is the reward appropriate? Does it affect the economy? Is it too easy or too hard? | Designer and economist | After playtest |
| Lore Review | Is it consistent with the world? Does it contradict the lore bible? | Lore writer | After dialogue is written |
| Accessibility Test | Can it be completed with all accessibility features enabled? | Accessibility tester | After build |
| Performance Test | Does it cause lag? Too many entities? Tick time spike? | Developer | After integration |
| Edge Case Test | What happens on logout mid-quest? Full inventory? Death during cutscene? | QA | Final testing |
| Regression Test | Did the new content break anything that was already working? | Automated and QA | After every fix |

### Bug Report Entry

| Column | Type | Description |
|---|---|---|
| Bug ID | Integer | Unique identifier |
| Content ID | Integer | Foreign key to the content item |
| Title | String | Short description of the bug |
| Steps to Reproduce | Text | Exact steps to trigger the bug |
| Expected Behavior | Text | What should happen |
| Actual Behavior | Text | What actually happens |
| Severity | Enum | Critical (blocks play), High (major issue), Medium (annoying), Low (cosmetic) |
| Priority | Enum | Fix Now, Fix Before Release, Fix After Release, Backlog |
| Assignee | String | Who is fixing this bug |
| Status | Enum | Open, In Progress, Fixed, Verified, Closed, Wont Fix |
| Screenshot | File | Visual evidence of the bug |
| Environment | String | Server, client version, operating system, browser |

### Bug Workflow

| Step | Description |
|---|---|
| Report | Tester files bug with all required fields |
| Triage | Lead reviews and assigns severity, priority, and assignee |
| Fix | Developer resolves the issue |
| Verify | Original tester confirms the fix works as expected |
| Regression | Automated tests confirm fix did not break other systems |
| Close | Bug marked as closed after verification passes |

---

## 6. Release Phase

### Pre-Release Checklist

| Step | Description |
|---|---|
| Content Freeze | No more changes to content after the freeze date |
| Final Review | DM or lead approves all content for release |
| Staging Deploy | Deploy to a staging environment that mirrors production |
| Staging Test | Smoke test on staging to catch deployment issues |
| Patch Notes | Write detailed patch notes covering all changes |
| Announcement | Publish teasers, previews, and release date to community |
| Backup | Full world backup before deployment |
| Hotfix Plan | Pre-written contingency plan for critical bugs discovered after launch |

### Deployment

| Step | Description |
|---|---|
| Maintenance Window | Schedule downtime and notify players in advance |
| Deploy | Push content to all worlds and servers |
| Smoke Test | Quick verification that deployed content loads and runs |
| Monitor | Watch error rates, tick timing, server load, and player reports |
| Hotfix Ready | Have the hotfix plan and rollback procedure ready for immediate action |

### Post-Release

| Step | Description |
|---|---|
| Monitor Reception | Track chat sentiment, social media, forum threads, and bug reports |
| Collect Metrics | Measure engagement (players participating), completion rate, average time, economy impact |
| Critical Hotfix | Fix any critical bugs within hours of discovery |
| Balance Adjustment | Tune difficulty, drop rates, and rewards within the first week based on real data |
| Retrospective | Structured review of what went well, what went wrong, and what to improve |
| Update Documentation | Revise design documents and internal docs to reflect final shipped state |

---

## 7. Content Calendar

### Planning Horizons

| Horizon | Scope | Detail Level |
|---|---|---|
| Quarterly Roadmap | Major releases, seasonal themes, milestone goals | High-level titles and dates |
| Monthly Milestones | What ships this month, dependencies between content | Feature list with owners |
| Weekly Sprints | What is being worked on this week, blockers | Task-level assignments |
| Daily Standups | Progress since yesterday, plan for today, blockers | Status updates |

### Content Cadence

| Frequency | Content Type | Examples |
|---|---|---|
| Weekly | Small updates | Bug fixes, balance tweaks, quality-of-life improvements |
| Bi-weekly | Medium content | New items, NPC additions, small standalone quests |
| Monthly | Feature update | New minigame, quest line chapter, skill expansion, new area |
| Quarterly | Major release | New skill, new region, major quest finale, new boss encounter |
| Annually | Expansion | New continent, new game mode, major system overhaul |
| Seasonal | Holiday events | Christmas, Halloween, Easter, anniversary celebrations |

### Calendar Entry

| Column | Type | Description |
|---|---|---|
| Entry ID | Integer | Unique identifier |
| Title | String | Content title |
| Type | Enum | Small Update, Medium Content, Feature, Major Release, Expansion, Holiday |
| Target Date | Date | Planned release date |
| Status | Enum | Planning, In Design, In Build, Testing, Staged, Released, Delayed, Cancelled |
| Dependencies | ID List | Other content entries that must ship first |
| Owner | String | Who is responsible for this content |
| Design Doc | ID | Link to the design document |
| Notes | Text | Additional context, risk flags, scope changes |

---

## 8. Community Communication

### Communication Types

| Type | Frequency | Description |
|---|---|---|
| Patch Notes | Every update | Detailed, honest, clear list of all changes, fixes, and additions |
| Dev Blog | Before major content | Show the design process, share behind-the-scenes, gather early feedback |
| Community Poll | As needed | Let players vote on controversial or preference-driven changes |
| Social Media | Ongoing | Teasers, screenshots, behind-the-scenes clips, community highlights |
| Bug Acknowledgment | As bugs are reported | Tell players the issue is known and being worked on, with estimated timeline |
| Retrospective Post | After major release | Share what went well, what was learned, and what will change next time |
| Roadmap Update | Quarterly | Share upcoming content plans with the community |
| Hotfix Notice | As needed | Announce emergency fixes with explanation of the issue and resolution |

### Patch Notes Template

| Section | Description |
|---|---|
| Summary | One to three sentences describing the theme of this update |
| New Content | List of new quests, items, NPCs, areas, features |
| Improvements | Enhancements to existing systems |
| Balance Changes | Stat adjustments, rate changes, difficulty tuning with reasoning |
| Bug Fixes | List of resolved bugs with brief descriptions |
| Known Issues | Bugs the team is aware of but has not fixed yet |
| Coming Soon | Brief preview of what is next |

---

## 9. AI-Assisted Pipeline Tools

AI tools that accelerate each phase of the content pipeline.

| Tool | Phase | Description |
|---|---|---|
| Idea Scorer | Ideation | AI evaluates an idea against the evaluation criteria and estimates feasibility |
| Design Doc Drafter | Design | AI generates a first-draft design document from a brief idea description |
| Test Plan Generator | Design | AI generates a testing plan based on the design document, including edge cases |
| Balance Simulator | Design | AI simulates the economy and progression impact of proposed rewards and rates |
| Asset List Generator | Design | AI reads the design document and generates a list of all assets needed |
| Dialogue Drafter | Build | AI generates first-draft dialogue for all NPCs in a quest based on character sheets |
| Bug Report Classifier | Testing | AI auto-classifies bug severity and suggests assignee based on affected system |
| Patch Notes Writer | Release | AI generates patch notes from the list of changes, fixes, and additions in the release |
| Sentiment Analyzer | Post-Release | AI analyzes player feedback to identify trends, common complaints, and praise |
| Retrospective Summarizer | Post-Release | AI summarizes playtest feedback and post-release data into actionable insights |

---

## Views

| View | Description |
|---|---|
| Pipeline Dashboard | All content items displayed by status phase with drag-and-drop progression |
| Design Document Editor | Structured form for writing and reviewing design documents |
| Asset Tracking Board | Kanban board showing all assets by stage (concept through integration) |
| Testing Dashboard | Pass/fail status per content item with drill-down to individual test results |
| Bug Tracker | Filterable list of all bugs with status, priority, and assignee |
| Content Calendar | Visual calendar showing planned and released content by date |
| Deployment Log | History of all deployments with status, duration, and any issues |
| Metrics Dashboard | Charts showing engagement, completion rates, economy impact, and player sentiment |
| Community Feedback Aggregator | Combined view of forum posts, social media, and in-game feedback |
| Retrospective Notes | Per-release review documents with action items and follow-up status |
