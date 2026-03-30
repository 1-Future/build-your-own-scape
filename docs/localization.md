# Localization

Multi-language support and cultural adaptation. Making the game accessible to players worldwide.

---

## 1. Language System

Core configuration for which languages the game supports.

| Setting | Description |
|---|---|
| Default language | The language used when no preference is set (configured by DM) |
| Supported languages | DM enables which languages are available for the server |
| Player preference | Each player selects their language in account settings |
| Fallback language | If a translation is missing, display this language instead (usually English) |
| Language detection | Auto-detect preferred language from browser or OS locale on first connection |

---

## 2. Text Translation

All game text is stored as translation keys, not raw strings. Each language has its own translation file (JSON or YAML).

### Translation Categories

| Category | Description |
|---|---|
| UI labels | Buttons, menus, tooltips, headers |
| NPC dialogue | All NPC conversation text |
| Quest text | Quest names, descriptions, objectives, completion messages |
| Item names | Display names for all items |
| Item descriptions | Examine text and flavor text for all items |
| Achievement text | Achievement names, descriptions, and unlock messages |
| System messages | Server messages, notifications, warnings |
| Error messages | Client and server error text |
| Tutorial text | All onboarding and tutorial instructions |
| Help text | In-game help pages and guides |
| Skill names | Display names for all skills |
| Spell names | Display names for all spells and abilities |
| Prayer names | Display names for all prayers |

### Translation Workflow

| Step | Description |
|---|---|
| English master file | All text authored in English as the source of truth |
| Community translation | Players or volunteers submit translations for their language |
| DM review | DM or designated reviewer approves submitted translations |
| Publish | Approved translations go live for that language |
| Fallback | Missing translations display the fallback language text |
| Completeness tracker | Dashboard shows percentage translated per language |

---

## 3. Dynamic Text

Text that changes at runtime and requires locale-aware formatting.

| Feature | Description |
|---|---|
| Player names | Not translated, displayed as entered by the player |
| Chat messages | Not translated by default (real-time chat translation available as a plugin) |
| Number formatting | Locale-aware separators (1,000 vs 1.000 vs 1 000) |
| Date/time formatting | Locale-aware display (MM/DD/YYYY vs DD/MM/YYYY vs YYYY-MM-DD) |
| Currency display | Locale-appropriate symbols and placement |
| Pluralization | Language-specific plural rules (1 item vs 2 items vs 5 items) |
| Gender-aware text | Masculine/feminine/neutral forms for languages that require it |
| Variable substitution | Template strings with named variables ("You need {count} more {item_name}") |

---

## 4. Right-to-Left (RTL) Support

Full support for RTL languages including Arabic, Hebrew, and Urdu.

| Feature | Description |
|---|---|
| UI mirroring | Menus, panels, and layouts mirror to right-aligned |
| Text alignment | Text right-aligned in RTL mode |
| Chat input direction | Input field switches to RTL for RTL languages |
| Mixed content | LTR content within RTL text handled correctly (and vice versa) |
| Number direction | Numbers remain LTR within RTL text |
| Scrollbar position | Scrollbars move to the left side in RTL mode |

---

## 5. Cultural Adaptation

Adjustments beyond translation to respect regional differences.

| Adaptation | Description |
|---|---|
| Date formats | Region-appropriate date display |
| Number formats | Region-appropriate number separators and decimal marks |
| Color meanings | Awareness that color associations vary by culture (red is not universally danger) |
| Icon/symbol review | Ensure icons and symbols are appropriate across cultures |
| Content sensitivity | Some content may need regional variants to avoid offense |
| Name length limits | CJK characters occupy different widths than Latin, UI must accommodate both |

---

## 6. Asset Localization

Non-text assets that require language-specific versions.

| Asset Type | Description |
|---|---|
| Textures with text | Signs, books, scrolls, and other in-world text need localized texture variants |
| Audio/voice acting | Separate voice files per language (if voice acting is used) |
| Font support | CJK characters need appropriate fonts, Arabic needs connected script rendering |
| UI layout | German and Finnish text is often longer than English, buttons and panels must flex |

---

## 7. Module Toggles

Each localization feature can be independently enabled or disabled by the DM.

| Module | Default |
|---|---|
| Multi-language support | Off |
| Community translation | Off |
| Auto-detect language | On |
| RTL support | Off |
| Locale-aware formatting | On |
| Chat translation plugin | Off |
| Voice acting per language | Off |
| Font per language | Off |
| Cultural adaptation | Off |

---

## 8. Admin Views

| View | Description |
|---|---|
| Translation dashboard | Completeness percentage per language, recently updated keys |
| Translation editor | Side-by-side view of original text and translation for editing |
| Missing translations | List of untranslated keys per language, sorted by priority |
| Community submissions | Pending translation submissions awaiting DM review |
| Language preference stats | Distribution of player language preferences |
