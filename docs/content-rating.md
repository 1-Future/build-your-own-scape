# Content Rating

Age-appropriate content filtering system. The DM sets a rating for their world -- everything above that rating is hidden, not removed. Not censorship -- age-appropriate defaults with full DM control.

---

## 1. Rating Tiers

| Rating | Age | Description |
|---|---|---|
| E (Everyone) | All ages | Safe for children. No violence, no adult themes, no complex social topics |
| E10+ | 10+ | Mild cartoon combat, basic social features |
| T (Teen) | 13+ | Standard combat with mild effects, basic romance (friendship+), taverns, mild risk |
| M (Mature) | 17+ | Full combat with blood, named alcohol, gambling, romance, full character options |
| AO (Adults Only) | 18+ | Everything unlocked -- underwear/revealing clothing, full adult themes, unrestricted chat, full gambling, all content flags |
| Custom | DM sets | Toggle every individual content flag independently |

---

## 2. Content Flags

Each flag is independently toggleable. DMs can use a rating tier as a base and override individual flags.

### Violence

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Combat (any) | Off | On (cartoon) | On | On | On |
| Hitsplats (damage numbers) | Off | On | On | On | On |
| Blood effects | Off | Off | Off | On | On |
| Gore/dismemberment | Off | Off | Off | Off | On |
| Death animation (dramatic) | Off | Off | On | On | On |
| Death animation (simple poof) | On | On | On | On | On |
| Skull/bone decorations | Off | On | On | On | On |
| Weapon visibility | Simple | Simple | Full | Full | Full |
| Monster horror level | Cute only | Mild | Moderate | Full | Full |
| PvP combat | Off | Off | On (consensual) | On | On |

### Substances

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Alcohol references | Off | Off | "Drinks" (unnamed) | Named (ale, wine, beer) | Full |
| Brewery/brewing skill | Off | Off | "Beverage making" | Full | Full |
| Smoking/pipes | Off | Off | Off | Decorative only | Full |
| Potions (named as drugs) | "Tonics" | "Tonics" | "Potions" | Full | Full |
| Tavern/pub | "Gathering hall" | "Gathering hall" | "Tavern" | Full | Full |

### Gambling

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Games of chance | Off | Off | Off | On (no real money) | Full |
| Staking/wagering GP | Off | Off | Off | On | Full |
| Casino venues | Off | Off | Off | On | Full |
| Loot boxes | Off | Off | Off | On (odds shown) | Full |
| RWT marketplace | Off | Off | Off | Off | On |

### Character Creation

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Gender options | Male/Female only | Male/Female only | Male/Female/Custom | Full | Full |
| Pronouns field | Hidden | Hidden | Hidden | On | On |
| Body manipulation (drag resize) | Off | Limited | On | On | On |
| Underwear/revealing clothing | Off | Off | Off | Off | On (18+ only) |
| Base clothing layer removable | No (always clothed) | No | No | No | Yes |
| Suggestive clothing | Off | Off | Off | On | On |
| Tattoos | Off | Off | On (mild) | On | On |
| Piercings | Off | On (ears only) | On | On | On |
| Makeup | Off | On (subtle) | On | On | On |
| Fantasy body mods (horns, tails) | On | On | On | On | On |
| Scars/wounds on character | Off | Off | On (mild) | On | On |
| Ghost/undead state | Off | Off | On | On | On |

### Social and Romance

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Romance/dating | Off | Off | Friendship+ (hand holding, compliments) | On (kissing, dating) | Full |
| Marriage system | Off | Off | Off | On | On |
| Flirting dialogue | Off | Off | Off | On | On |
| NPC relationship system | Friendship only | Friendship only | Friendship + admiration | Full affinity system | Full |
| Player housing cohabitation | Off | Off | Friends can visit | Roommates/partners | Full |

### Chat and Communication

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Free text chat | Off (quick chat only) | On (filtered) | On (filtered) | On (light filter) | On (no filter available) |
| Profanity filter | Forced on (strict) | Forced on (strict) | Forced on (moderate) | On (toggleable) | Off (optional) |
| Voice chat | Off | Off | On (filtered) | On | On |
| Private messaging | Off | On (logged) | On | On | On |
| Custom emoji | Off | Off | On (curated) | On | On |
| GIF sharing | Off | Off | Off | On (filtered) | On |

### Economy

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Trading with players | Off | Limited | On | On | On |
| Grand Exchange | Off | On (limited) | On | On | On |
| Real money transactions | Off | Off | Off | Off | On (with parental controls) |
| Service marketplace | Off | Off | Off | Off | On |
| Account sharing/RBAC | Off | Off | Off | Off | On |

### World Content

| Flag | E | E10+ | T | M | AO |
|---|---|---|---|---|---|
| Graveyard/cemetery | Off | On (peaceful) | On | On | On |
| Dungeon/prison | Off | On (mild) | On | On | On |
| Undead/skeleton NPCs | Off | On (cartoon) | On | On | On |
| Demonic/infernal themes | Off | Off | Off | On | On |
| Religious themes (altars, prayer) | Simplified | On (neutral) | On | On | On |
| Slavery/oppression themes in quests | Off | Off | Off | On (handled sensitively) | On |
| Dark/horror areas | Off | Off | On (mild) | On | On |
| Jump scares | Off | Off | Off | On (toggle) | On |

---

## 3. Age Verification

| Method | Description |
|---|---|
| Self-declared | Player selects age range on account creation (honor system) |
| Parental account | Parent creates child account with restrictions from their own account |
| Date of birth | Enters DOB, system calculates age, locks rating |
| External verification | Third-party age verification service (for AO content) |
| No verification | DM trusts players (default for private servers) |

### Parental Controls

- Parent account links to child account
- Parent sets content rating (overrides DM default if stricter)
- Parent sets playtime limits (daily/weekly hours)
- Parent sets spending limits (if monetization enabled)
- Parent approves friend requests
- Parent can view child's chat log
- Parent can restrict specific features individually
- Parent receives weekly activity report
- Parent can lock settings (child cannot change rating)

---

## 4. Content Tagging System

Every piece of content in the game gets tagged with content flags.

### Asset Tags

| Asset Type | Taggable Properties |
|---|---|
| Items | Violence level, substance reference, suggestiveness, age-appropriateness |
| NPCs | Theme (dark, horror, adult), dialogue content rating, visual horror level |
| Objects | Theme, violence association (weapon racks, torture devices, prison cells) |
| Quests | Violence, romance, dark themes, complexity, substance references |
| Locations | Violence zone, adult theme, horror level, restricted area |
| Music | Mood (dark, scary, romantic), lyric content if any |
| Dialogue | Language level, romance, threats, substance references |
| Emotes | Suggestiveness, violence |
| Clothing | Suggestiveness level (0-5), body exposure amount |
| Chat messages | Auto-filtered by profanity/content filter matching rating |

### Auto-Filtering

When DM sets world rating, the system automatically:

- Hides items tagged above the rating
- Replaces NPC dialogue with age-appropriate alternatives
- Swaps violent animations for mild ones
- Renames substances ("ale" becomes "drink")
- Removes revealing clothing from character creation
- Activates chat filter at appropriate strictness
- Hides locations/quests above the rating
- Replaces gore effects with cartoon effects

---

## 5. DM Content Management

### Rating Dashboard

- Current world rating displayed prominently
- Content flag overview (how many assets per rating tier)
- Untagged content report (content that has not been rated yet)
- Override log (DM manually overriding auto-tags)
- Player age distribution (how many players at each age -- if verified)

### Per-Content Override

DM can override any auto-tag:

- "This quest is tagged M but I think it is fine for T -- override to T"
- "This item is tagged E but in my world context it is M -- override to M"
- Override requires justification note (audit trail)

### Custom Rating Creation

DM can create a custom rating between tiers:

- "My world is Teen but I want gambling and named alcohol"
- Select base tier, then toggle individual flags on/off
- Save as named custom rating ("My World Rating")
- Apply to world

---

## 6. Player-Side Controls

### Personal Content Filter

Even within the world's rating, individual players can self-restrict:

- "I am on an 18+ server but I do not want to see blood" -- personal blood toggle off
- "I am fine with combat but hide gambling content" -- personal gambling toggle off
- Personal filter can only be stricter than world rating, never looser

### Content Warnings

- Optional content warnings before entering areas with specific themes
- "This area contains dark/horror themes. Continue?"
- Toggleable per content type
- DM can mark specific quests/areas with mandatory warnings

### Report Inappropriate Content

- Players can flag content they believe is mistagged
- Report goes to DM moderation queue
- Community-driven content rating improvement

---

## 7. Platform-Level Enforcement

### OpenScape Platform Rules

- Worlds with AO content must be marked as AO on server browser
- Server browser filters by rating (parents can hide AO servers)
- P2W meter (from monetization.md) displayed alongside content rating
- Platform can audit worlds for rating accuracy
- Misrated worlds can be flagged and forced to correct

### Legal Compliance

| Regulation | How System Supports It |
|---|---|
| COPPA (US, under 13) | Parental consent required, chat restrictions, no personal data collection |
| GDPR (EU) | Data minimization, right to deletion, parental controls for minors |
| PEGI / ESRB | Content tagging maps to standard rating systems |
| Regional restrictions | Content flags can map to region-specific requirements |
| Gambling regulations | Gambling content clearly gated behind M/AO with odds disclosure |

---

## Module Toggles

| Toggle | Default | Description |
|---|---|---|
| Content rating system | On | Master toggle for the entire system |
| Age verification | Self-declared | Method of verifying player age |
| Parental controls | On | Parent account management for child accounts |
| Content tagging | On | Assets tagged with content flags |
| Auto-filtering | On | System auto-hides content above world rating |
| Personal content filter | On | Players can self-restrict below world rating |
| Content warnings | On | Warnings before entering themed areas |
| Report system | On | Players can flag mistagged content |
| Platform enforcement | On | OpenScape platform audits world ratings |
| Chat filter strictness | Per rating | Profanity filter level tied to content rating |
| Violence level | Per rating | Combat visual intensity |
| Substance references | Per rating | How alcohol/drugs are named or hidden |
| Gambling access | Per rating | Games of chance availability |
| Romance features | Per rating | Relationship system depth |
| Clothing restrictions | Per rating | What clothing is available in character creator |
| Underwear/revealing | 18+ only | Base clothing layer mandatory under 18 |

---

## Views

| View | Description |
|---|---|
| Rating dashboard | Current world rating with flag overview |
| Content tagger | Tag assets with content flags |
| Untagged report | List of content missing rating tags |
| Override log | History of DM rating overrides |
| Parental control panel | Parent manages child account settings |
| Player content settings | Personal filter toggles |
| Content warning editor | Configure per-area content warnings |
| Platform compliance | Check world against platform rules and legal requirements |
