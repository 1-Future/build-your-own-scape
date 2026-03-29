# Games of Chance

Game design document for the games of chance module. Covers RSPS classics, casino staples, modern gambling games, and community/social formats. Every game, property, and system is independently toggleable.

---

## Game Types

### RSPS Classics

| Game | Description | Players |
|------|-------------|---------|
| Flower Poker | Each player plants 5 flowers. Hands ranked like poker (pair, two pair, three of a kind, full house, four of a kind, five of a kind). Highest hand wins. | 1v1 |
| Dice Bag | Roll 1-100. Over/under a threshold (typically 55). | 1v1 or solo vs house |
| Hot/Cold | Guess whether the next flower color is hot (red, orange, yellow) or cold (blue, purple, white). | 1v1 or solo vs house |
| Boxing | Both players fight fists only, no armor, no food. Last one standing wins the pot. | 1v1 |
| 55x2 | Roll 55 or higher to double your bet. Below 55 loses. | Solo vs house |
| Seal of Approval | Plant flowers and hope for a specific winning combination. Rare combo pays out big. | Solo vs house |

### Casino Games

| Game | Description | Players |
|------|-------------|---------|
| Blackjack | Classic 21. Hit, stand, double, split. Dealer stands on 17. | Solo vs house |
| Poker (Hold'em) | Texas Hold'em with community cards. Standard hand rankings. | 2-9 players |
| Poker (5-Card Draw) | Draw poker. One draw round. Best hand wins. | 2-6 players |
| Roulette | Bet on number, color, odd/even, range. Wheel spins. | Group |
| Slots | Pull lever, match symbols across reels. Paylines determine payout. | Solo |
| Craps | Dice-based. Pass/don't pass, come/don't come, prop bets. | Group |
| Coinflip | 50/50 heads or tails. Winner takes both wagers. | 1v1 |
| Wheel of Fortune | Spin a segmented wheel. Land on multiplier. | Solo vs house |
| Baccarat | Player vs banker hand. Closest to 9 wins. Bet on either or tie. | Solo vs house |
| Keno | Pick numbers from a grid. Random draw. More matches, higher payout. | Solo vs house |

### Modern Games

| Game | Description | Players |
|------|-------------|---------|
| Crash | Multiplier climbs from 1.00x upward. Cash out before it crashes. If you don't cash out in time, you lose your bet. | Group (shared graph) |
| Plinko | Ball drops from top through a field of pegs. Lands in a multiplier slot at the bottom. | Solo |
| Mines | Grid of tiles. Reveal tiles to find gems. Hit a mine and lose everything. Cash out anytime to keep winnings. Fewer mines means lower multipliers. | Solo |
| Limbo | Pick a target multiplier. RNG rolls. If the roll meets or exceeds your target, you win at that multiplier. Higher targets, lower odds. | Solo |
| Cups/Shell Game | Three cups, one hides the prize. Cups shuffle. Pick the right one. | Solo vs house |
| Tower | Climb floors by picking the safe tile on each level. Wrong tile ends the run. Cash out between floors. More tiles per floor means easier odds but lower multiplier per floor. | Solo |
| Hi-Lo | A card is shown. Guess whether the next card is higher or lower. Streak increases payout. | Solo |
| Scratch Cards | Buy a card, scratch to reveal symbols. Match patterns to win. | Solo |

### Community and Social

| Game | Description | Players |
|------|-------------|---------|
| Lottery | Buy tickets at a set price. Random draw at scheduled time. Jackpot split among winners. | Group |
| Raffle | Buy entries. One winner drawn. Single prize. | Group |
| 50/50 | Two players each put up equal stakes. One is randomly chosen to win both. | 1v1 |
| Auction Gamble | Blind bid on a mystery item. Highest bidder wins the item -- could be valuable or worthless. | Group |
| Drop Party Roulette | Items dropped in a zone. Players scramble. Random assignment determines who gets what. | Group |
| Boss Lotto | Players pool GP before a boss kill. If the boss drops a rare, the pot goes to one random contributor. | Group |
| Clan Pot | Clan members contribute GP throughout the week. One random member wins the pot at weekly reset. | Group |
| Bingo | Numbers drawn periodically. First player to complete a line/card wins. | Group |

### Skill-Adjacent

| Game | Description | Players |
|------|-------------|---------|
| Staking Duels | PvP combat with a wager. Both players agree on rules (gear, food, prayer). Winner takes the pot. | 1v1 |
| Race Betting | Bet on which player finishes a skilling task first (e.g., first to 100 ore). | Group |
| Pet Prediction | Bet on which player gets a pet drop first within a session. | Group |
| KC Betting | Over/under on how many kills it takes a player to get a specific rare drop. | Group |
| Skilling Speed Bet | Bet on time to complete a skilling task (e.g., under 3 minutes to fletch 100 bows). | 1v1 or group |

---

## Game Properties

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| Game type | Enum | Pure chance, skill-based, or hybrid |
| Player count | Enum | 1v1, group, solo vs house |
| House edge | Percentage | The mathematical advantage the house holds |
| Provably fair | Boolean | Cryptographic proof of fair outcome (verifiable seed + hash) |
| Odds display | Boolean | Show exact win/loss percentages before the player bets |
| Animated outcome | Boolean | Visual animation for the result (dice roll, card flip, wheel spin) |

### Wagering

| Property | Type | Description |
|----------|------|-------------|
| Wager types | Enum | GP only, items only, or both |
| Min bet | Integer | Minimum wager to enter |
| Max bet | Integer | Maximum wager per round |
| Bet cap (daily) | Integer | Maximum total wagered per day |
| Bet cap (weekly) | Integer | Maximum total wagered per week |
| Anti-addiction cooldown | Duration | Forced delay between consecutive bets |
| Loss limit | Integer | Auto-stop after cumulative losses reach threshold |
| Win limit | Integer | Optional auto-cash-out after cumulative wins reach threshold |

### Payout

| Property | Type | Description |
|----------|------|-------------|
| Payout ratio | Float | Multiplier applied to winning bet (e.g., 2.0x for 50/50 games) |
| Jackpot system | Boolean | Progressive pot that accumulates across rounds |
| Tax on winnings | Percentage | GP removed from winnings as an economy sink |
| Payout timing | Enum | Instant (immediate transfer) or held (delay before release) |

### Per-Game Properties

| Property | Applies To | Type | Description |
|----------|-----------|------|-------------|
| Crash point | Crash | Float | The multiplier at which the round ends |
| Grid size | Mines | Integer | Number of tiles in the grid (e.g., 5x5 = 25) |
| Mine count | Mines | Integer | Number of mines hidden in the grid |
| Tower floors | Tower | Integer | Total floors to climb |
| Tiles per floor | Tower | Integer | Number of tiles to choose from on each floor |
| Ticket price | Lottery, Raffle | Integer | Cost per entry |
| Draw frequency | Lottery, Bingo, Keno | Duration | How often a draw occurs |
| Progressive jackpot | Slots, Lottery | Boolean | Jackpot grows with each round played |
| Jackpot seed | Slots, Lottery | Integer | Starting jackpot amount after a win |
| Jackpot contribution | Slots, Lottery | Percentage | Portion of each bet added to the jackpot pool |
| Cash out mechanic | Crash, Mines, Tower, Hi-Lo | Boolean | Player can exit mid-game to lock in current winnings |
| Auto-play toggle | Slots, Crash, Hi-Lo | Boolean | Automatic repeated play at fixed bet |
| Multiplier display | Crash, Limbo, Plinko | Boolean | Live multiplier shown during play |

---

## Casino and Venue System

| Feature | Description |
|---------|-------------|
| Physical location | Casinos exist as buildings in the game world. Players walk in. |
| NPC dealers | House-run games operated by NPCs. Fixed rules, fixed house edge. |
| Player-run tables | Players can host their own games at their own tables. |
| Host license | Required to run a player table. Granted by a DM or admin. |
| Host rake | Percentage the host takes from each pot. Capped to prevent abuse. |
| Casino entry requirements | Optional. Level, quest, GP minimum, or reputation gate. |
| VIP areas | Restricted zones with higher limits, better odds, or exclusive games. |
| Casino currency | Chips. Players buy in with GP, play with chips, cash out to GP. Chips do not leave the casino. |

---

## Responsible Gambling

| Feature | Description |
|---------|-------------|
| Daily loss limit | Maximum GP a player can lose in a 24-hour period. Configurable per player. |
| Weekly loss limit | Maximum GP a player can lose in a 7-day period. |
| Monthly loss limit | Maximum GP a player can lose in a 30-day period. |
| Self-exclusion | Player voluntarily locks themselves out of all gambling for a set duration. Cannot be reversed early. |
| Cooldown timers | Forced wait between bets or between sessions. |
| Session time alerts | Notification after a configured duration of continuous play. |
| Spending alerts | Notification when spending exceeds a configured threshold. |
| Parental controls | Account-level toggle to disable all gambling. Requires elevated access to re-enable. |
| Addiction warning messages | Periodic messages displayed during play (e.g., "You have been gambling for 2 hours"). |
| Win/loss history | Full log of every bet, outcome, and net result. Always accessible. |
| Profit/loss dashboard | Visual summary of gambling activity over time. Daily, weekly, monthly, all-time. |
| Age restriction | Account flag. Gambling locked unless age requirement is met. |

---

## Anti-Cheat

| Feature | Description |
|---------|-------------|
| Provably fair RNG | Server seed + client seed + nonce. Hash published before the bet. Player can verify after. |
| Host monitoring | Automated checks on player-hosted games for unusual patterns. |
| Collusion detection | Flags accounts that consistently win against the same opponents. |
| Win-rate anomaly detection | Alerts when a player's win rate deviates significantly from expected odds. |
| Seed transparency | Players can view and verify seeds for any past game they participated in. |
| Game logs | Every bet, outcome, payout, and participant recorded permanently. Auditable. |

---

## Module Toggles

Every system and game can be independently enabled or disabled.

### Global Toggles

| Toggle | Scope | Description |
|--------|-------|-------------|
| Games of chance | Global | Master switch for the entire module |
| Pure chance games | Category | Enable/disable all pure chance games |
| Skill-based games | Category | Enable/disable all skill-based games |
| Prediction markets | Category | Enable/disable skill-adjacent betting |
| House games | System | Enable/disable NPC-operated games |
| Player-hosted games | System | Enable/disable player-run tables |

### Per-Game Toggles

Each individual game (Flower Poker, Blackjack, Crash, Lottery, etc.) has its own enable/disable toggle.

### Feature Toggles

| Toggle | Description |
|--------|-------------|
| Host licensing | Require license for player-hosted games |
| Provably fair | Require cryptographic proof on all games |
| Responsible gambling | Enable loss limits, cooldowns, alerts |
| Loss limits | Enforce daily/weekly/monthly loss caps |
| Self-exclusion | Allow players to opt out |
| Casino venues | Enable physical casino locations |
| Casino currency | Enable chip buy-in/cash-out system |
| Jackpot system | Enable progressive jackpots |
| Tax on winnings | Enable GP sink on payouts |
| Item wagering | Allow items as wagers (not just GP) |
| Anti-addiction | Enable cooldowns and session alerts |

---

## Views

| View | Description |
|------|-------------|
| Available games | Browse all enabled games. Filter by type, player count, wager range. |
| Win/loss history | Personal log of all gambling activity. Searchable, filterable. |
| Leaderboard | Opt-in ranking by profit, win streak, or total wagered. |
| Casino directory | List of all casino venues with location, games offered, and host ratings. |
| Host ratings | Player reviews and trust scores for player-hosted tables. |
| Jackpot tracker | Live view of all progressive jackpots and their current values. |
| Economy impact | Admin view. Total GP wagered, total GP removed (tax), net economy effect. |
