# Philosophy Extractor

A methodology for extracting the core principles, rules, and DNA from any project, conversation, or system. Turns implicit knowledge into explicit, documented principles that guide all future decisions.

Data Miner extracts DATA. Philosophy Extractor extracts PRINCIPLES.

---

## 1. What Philosophy Extractor Is

Every project has hidden rules — principles that were never written down but govern every decision. Philosophy Extractor makes them explicit.

Examples from this project:
- "Everything is a plugin" — never formally decided, emerged from the conversation
- "OSRS first, then RS3, then other games" — emerged from doing it, then documented as the data source priority
- "A 3-year-old can use it" — said once in passing, became a core UX principle
- "No underwear until 18" — a content decision that revealed the need for a content rating system
- "The character IS the UI" — learned from Sims 4 analysis, became a design principle
- "Built for humans AND AI" — evolved from discussing the Game Console API and batch commands
- "One database, many views" — said as a throwaway line, became an architectural constraint

These principles were IMPLICIT in decisions and conversations. Philosophy Extractor makes them EXPLICIT.

---

## 2. The Extraction Process

### Step 1: Collect Raw Material
Gather all sources of implicit philosophy:
- Conversations (chat logs, meeting notes, design sessions)
- Decisions made (what was chosen and WHY)
- Decisions rejected (what was NOT chosen and WHY NOT)
- Emotional reactions ("that's horrible", "that's perfect", "I love it")
- Repeated patterns (things said or done multiple times = principle)
- Corrections ("no, not that way" = boundary identified)
- Inspirations ("like Sims 4" = reference standard identified)

### Step 2: Identify Patterns
Look for recurring themes across the raw material:

| Pattern Type | Signal | Example from this project |
|-------------|--------|--------------------------|
| Value statement | "We should always..." or "Never..." | "Everything optional, nothing forced" |
| Quality bar | "That's too much" or "That's perfect" | "Simple enough for a 3-year-old" |
| Priority | "This first, then that" | "OSRS wiki first, then RS3, then other games" |
| Boundary | "Not that" or "We don't want..." | "No IP, only mechanics" |
| Reference | "Like [X]" or "Inspired by [X]" | "Sims 4 build mode is the gold standard" |
| Aspiration | "What if..." or "Eventually..." | "Interview -> Complete MMORPG" |
| Frustration | "That's terrible" or "Why would they..." | "RS3 killed PvP by removing the wilderness" |
| Excitement | "I love that" or "That's genius" | "The character IS the UI — genius design" |

### Step 3: Formulate Principles
Turn each pattern into a clear, actionable principle:

**Template:** [Principle Name] — [One sentence description]. Why: [Reason]. How to apply: [Guidance for future decisions].

**Example:**
**Granular Modularity** — Every feature is its own toggle, independently enable/disable. Why: Different worlds need different features. A medieval world doesn't need cybernetics. How to apply: When designing any system, ask "can a DM turn this off without breaking anything else?"

**Example:**
**Mechanics Not IP** — Extract the system design, never the brand assets. Why: Legal safety and creative freedom. The mechanic "edge-based wall collision" is universal; "Lumbridge Castle" is someone else's IP. How to apply: When referencing a game, describe what it DOES, not what it's CALLED.

**Example:**
**Built for Two Audiences** — Every tool has a GUI for humans and an API for AI. Why: Today humans use the tools; tomorrow AI uses the same tools. Building for both now means the AI pipeline is free. How to apply: Every GUI action must have a corresponding API call. No GUI-only features.

### Step 4: Categorize Principles
Group principles by domain:

| Category | What It Governs | Examples from this project |
|----------|----------------|--------------------------|
| Product Philosophy | What the product IS and isn't | "Everything is a plugin", "Interview -> MMORPG" |
| Design Philosophy | How systems are designed | "Skills ARE the game", "Everything connects" |
| UX Philosophy | How interfaces work | "A 3-year-old can use it", "Show don't configure" |
| Data Philosophy | How data is structured | "One database many views", "Every property has a type" |
| Research Philosophy | How knowledge is gathered | "OSRS first then RS3 then others", "Extract don't copy" |
| Content Philosophy | What content is appropriate | Content rating system, age-appropriate defaults |
| Technical Philosophy | How code is written | "Every GUI action has an API", "No hardcoded values" |
| Community Philosophy | How users are treated | "The system never judges a DM's choices" |
| Business Philosophy | How money works | "All cosmetics free", no subscriptions for self-expression |
| Vision Philosophy | Where this is going | "AI uses the same tools tomorrow", "Complete domain coverage" |

### Step 5: Validate
Test each principle against past decisions:
- Does this principle explain decisions already made?
- Would this principle have prevented mistakes that were made?
- Does the team/person agree this IS a principle, not just a one-time choice?
- Is this principle actionable (can someone apply it to a new decision)?
- Does this principle conflict with any other principle? If so, which wins?

### Step 6: Document
Write principles into a living document (like project-philosophy.md):
- Principle name
- One-line statement
- Why this matters
- How to apply
- Examples of applying it correctly
- Examples of violating it (anti-patterns)

---

## 3. Extraction Sources

### From Conversations
| Signal | Extraction |
|--------|-----------|
| "I want..." | Core desire -> product principle |
| "I don't want..." | Boundary -> anti-pattern |
| "That's perfect" | Quality bar -> design principle |
| "That's too much" | Scope boundary -> simplicity principle |
| "Like [reference]" | Inspiration source -> reference standard |
| "Why not..." | Expansion thinking -> vision principle |
| "Let them" | Freedom principle -> user autonomy |
| "If they want that they can" | Optionality -> modularity principle |
| "Anyone can use it" | Accessibility -> UX principle |
| "Built for humans and AI" | Duality -> architecture principle |

### From Decisions
Every decision has an implicit principle:

| Decision Made | Implicit Principle |
|--------------|-------------------|
| OSRS wiki as primary source | "Start with the proven reference" |
| Everything a toggle | "Never force features on users" |
| Docs before code | "Design before building" |
| Studying Sims 4 for build mode | "Learn from the best in class, regardless of genre" |
| Content rating system | "Protect vulnerable users by default" |
| Same tool creates players AND NPCs | "Unify tools where possible" |
| 832 plugins with individual toggles | "Granularity over convenience" |
| Plugin audit of 1605 items | "The community has already identified your gaps" |
| Three-layer architecture (Plugin/Module/Entry) | "Organize at every level of detail" |
| No emojis in docs | "Professional documentation is parseable documentation" |

### From Rejections
What was rejected reveals principles by negative space:

| Rejection | Revealed Principle |
|-----------|-------------------|
| Rejected rigid categories | "Let the DM categorize however they want" |
| Rejected copying IP | "Mechanics only, never brand assets" |
| Rejected separate NPC creator | "Same tool for same data, different config" |
| Rejected underwear for minors | "Age-appropriate defaults are non-negotiable" |
| Rejected surface-level docs | "Field-level data or it's not documented" |
| Rejected single-source research | "Cross-reference 3+ sources minimum" |

### From the Document Structure Itself
The structure of the project reveals unspoken principles:

| Structural Choice | Revealed Principle |
|------------------|-------------------|
| Every doc has "Module Toggles" section | Everything must be configurable |
| Every doc has "Views" section | Same data must be viewable multiple ways |
| Property tables have Type columns | Data must be typed and machine-parseable |
| Plugin registry exists separately from docs | The index of what's available must be independent from the details |
| Plugin audit exists as its own doc | Gap analysis is a first-class deliverable, not a side task |
| Game2Tools exists as a methodology doc | The process of deriving tools is as important as the tools themselves |

---

## 4. Philosophy Extractor Outputs

| Output | Description | Example from this project |
|--------|-------------|--------------------------|
| Principles Document | Named, described, categorized principles | project-philosophy.md with 10 design principles, 10 tool principles |
| Decision Framework | "When faced with X, apply principle Y" | "When adding a feature: make it a toggle. When referencing a game: extract the mechanic." |
| Anti-Patterns List | Things that violate principles (with why and fix) | "Surface Skimming: reading summaries instead of extracting properties" |
| Reference Standards | "For [domain], the gold standard is [reference]" | "For build mode: Sims 4. For skills: OSRS. For social: Discord." |
| Vision Statement | Where the project is heading, extracted from aspirational statements | "Interview -> Complete MMORPG" |
| Quality Bar | Minimum acceptable standard per area | "Every property has a type. Every toggle has a default. Every doc has views." |
| Priority Stack | What matters most, in order | "OSRS -> RS3 -> RSPS -> RuneLite -> Other MMOs -> Non-MMOs -> Original" |
| Boundary Map | What is explicitly out of scope | "No IP. No hardcoded values. No GUI-only features." |

---

## 5. Maintaining Philosophy

Principles aren't static — they evolve:
- New conversations may reveal new principles
- Old principles may be refined or deprecated
- Conflicts between principles must be resolved (document which wins and why)
- Every major decision should reference which principles it follows
- New team members should read the philosophy doc FIRST

### Philosophy Review Triggers
- Before starting any new major feature
- When team members disagree on direction
- When user feedback contradicts current approach
- Quarterly review to check if principles still hold
- When the project scales significantly (what works at 10 docs may not work at 100)

### Conflict Resolution
When two principles conflict, document the resolution:

**Example conflict:** "Granular Modularity" (everything toggleable) vs "Depth over Breadth" (fewer but deeper systems).
**Resolution:** Granularity is about configuration, depth is about design. A deep system can still be toggled on/off. The toggle controls presence; the depth controls quality. Both principles survive.

**Example conflict:** "A 3-year-old can use it" vs "Accessible floor, high ceiling."
**Resolution:** The 3-year-old uses the GUI defaults. The high ceiling is right-click menus, advanced options, formula editors. Simple by default, powerful when needed. Both principles survive.

---

## 6. Combined Methodology: FutureFlow + Data Miner + Philosophy Extractor + Game2Tools

All four work together:

```
Philosophy Extractor
    -> Defines the RULES (principles, boundaries, quality bars)

FutureFlow
    -> Defines the PROCESS (pain point -> research -> plan -> build)

Data Miner
    -> Defines the DATA (exhaustive extraction, gap analysis, completeness)

Game2Tools
    -> Defines the TOOLS (data shape -> context -> editor tool)
```

Flow for a new project:
1. **Philosophy Extractor** — Talk with stakeholder, extract principles from conversation
2. **Data Miner Phase 1** — Map the domain into categories using principles as guide
3. **FutureFlow Steps 1-6** — For each category, research and identify pain points
4. **Data Miner Phase 3-4** — Deep extract data, find gaps
5. **Philosophy Extractor** — Mid-project check: any new principles emerging from the research?
6. **FutureFlow Steps 7-8** — Plan and issue the design docs
7. **Game2Tools** — Derive editor tools from the documented data
8. **FutureFlow Steps 9-12** — Build, test, iterate, document
9. **Philosophy Extractor** — After building, extract new principles learned from implementation
10. **Data Miner Validation** — Are all gaps closed? Any new gaps from building?
11. **Repeat** — Each cycle adds more data, refines principles, improves tools

### What each methodology catches that the others miss

| Methodology | Catches | Misses without the others |
|------------|---------|--------------------------|
| FutureFlow | Pain points, broken things, user needs | Doesn't ensure completeness (only fixes what's broken) |
| Data Miner | Every property, every variant, every gap | Doesn't know which properties MATTER (no principles) |
| Philosophy Extractor | Design rules, quality bars, boundaries | Doesn't produce data or tools (only rules) |
| Game2Tools | Optimal tool per data shape | Doesn't know what data exists (needs Data Miner) or what rules apply (needs Philosophy Extractor) |

Together, they produce a system where:
- The principles say WHAT to build and WHY
- The data says WHAT EXISTS to build from
- The process says HOW to build it
- The tools say WHAT TOOLS to build it with

---

## Module Toggles

Philosophy Extractor is a methodology, not a runtime system. No module toggles.

---

## Views

| View | Description |
|------|-------------|
| Principles Dashboard | All principles organized by category |
| Decision Log | Past decisions with which principles they applied |
| Conflict Resolver | When two principles conflict, shows both and asks for resolution |
| Conversation Analyzer | AI reads conversation and suggests implicit principles |
| Principle Health Check | Are all principles still being followed? |
