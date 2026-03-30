# Multiplayer Scaling

How the game server architecture scales from 10 players to 10,000+. From single-server hobby projects to production MMO infrastructure.

---

## 1. Server Architecture Models

Five architecture models, each suited to a different stage of growth. A game can start at single server and migrate upward as the player base grows.

| Model | Description | Example |
|---|---|---|
| Single server | One process handles everything -- networking, game logic, persistence. OpenScape current approach. | Private servers, hobby projects |
| Multi-world | Multiple independent server instances. Players pick a world from a list. Each world runs the full game loop. | OSRS (100+ worlds) |
| Sharded | World split into geographic zones, each zone on a different server. Seamless to the player -- zone boundaries are invisible. | WoW (continent-level shards) |
| Megaserver | One logical world. Dynamic instancing per zone based on population. Players are distributed across zone copies automatically. | GW2, ESO |
| Microservices | Separate services for chat, combat, economy, auth. Each service scales independently. Game world servers call into shared services. | Modern MMOs at scale |

---

### Single Server

The starting point. Everything runs in one Node.js process.

| Property | Value |
|---|---|
| Game loop | Single setInterval at tick rate |
| Networking | WebSocket server on one port |
| Persistence | JSON files or SQLite |
| Player cap | 50-200 depending on world complexity |
| Failure mode | Server crash = everyone disconnects |
| Advantages | Simple to run, simple to debug, zero infrastructure |
| Disadvantages | Single point of failure, CPU-bound on one core, no horizontal scaling |

---

### Multi-World

Each world is a fully independent server instance. A login server sits in front and routes players.

| Property | Value |
|---|---|
| World count | Configurable (2 to 200+) |
| World independence | Each world has its own game loop, entity state, and tick clock |
| Shared state | Player accounts, friends list, Grand Exchange, hiscores |
| Character portability | Full -- items, stats, position transfer between worlds |
| Login server | Handles authentication, world list, routing |
| Advantages | Linear horizontal scaling, world failures are isolated |
| Disadvantages | Cross-world features require shared infrastructure |

---

### Sharded

The world is divided into zones. Each zone runs on a separate server. Players crossing zone boundaries are handed off seamlessly.

| Property | Value |
|---|---|
| Zone size | Configurable (region-level or chunk-level) |
| Handoff | Server-to-server entity transfer on boundary crossing |
| Shared state | Player data, inventory, stats stored centrally |
| Edge handling | Entities near zone boundaries are simulated on both servers |
| Advantages | Single continuous world, distributes CPU load geographically |
| Disadvantages | Zone boundary bugs, complex handoff logic, shared state latency |

---

### Megaserver

One world, but zones are dynamically instanced based on population. Players are automatically distributed across zone copies.

| Property | Value |
|---|---|
| Instance creation | Automatic when zone population exceeds threshold |
| Instance merging | Automatic when zone population drops below threshold |
| Player grouping | Party members and friends are placed in the same instance |
| Overflow handling | New arrivals go to least-populated instance |
| Advantages | No world selection, population always feels healthy, no dead servers |
| Disadvantages | Instancing breaks open-world feel, complex grouping logic |

---

### Microservices

Game logic is decomposed into independent services. Each service scales on its own.

| Service | Responsibility |
|---|---|
| Auth service | Login, registration, JWT tokens, account management |
| World service | Game loop, entity simulation, combat, movement |
| Chat service | Public chat, private messages, clan chat, global channels |
| Economy service | Grand Exchange, trade, price tracking |
| Persistence service | Player saves, world state, backups |
| Hiscores service | Leaderboards, rankings, achievements |
| Instance service | Creates and destroys instanced zones |

| Property | Value |
|---|---|
| Communication | Message queue (Redis pub/sub, NATS, or RabbitMQ) between services |
| Scaling | Each service scales independently based on load |
| Deployment | Services can be updated independently without full restart |
| Advantages | Fine-grained scaling, fault isolation, team parallelism |
| Disadvantages | Network overhead, eventual consistency, operational complexity |

---

## 2. World/Server System

The world system governs how players discover, join, and move between server instances.

---

### World List

Browseable list of available worlds. Displayed at login and accessible in-game.

| Column | Description |
|---|---|
| World number | Unique identifier |
| Population | Current player count |
| Ping | Latency from client to world server |
| Location | Server region (US East, EU West, etc.) |
| Type | World type tag (Normal, PvP, Skill Total, etc.) |
| Activity | Featured activity or theme (Minigame spotlight, Boss event) |
| Status | Online, Full, Maintenance |

---

### World Hopping

Switch worlds without logging out. Character transfers fully.

| Property | Value |
|---|---|
| Transfer data | Inventory, equipment, stats, position, quest state |
| Cooldown | Configurable (default: 30 seconds between hops) |
| Combat lockout | Cannot hop while in combat or for X ticks after combat |
| Position handling | Option to spawn at same coordinates or at world spawn point |
| Trade lockout | Configurable delay after hopping before trading is allowed (anti-merching) |

---

### World Types

| Type | Description |
|---|---|
| Normal | Standard gameplay, no restrictions |
| PvP | Open PvP everywhere outside safe zones |
| Skill total | Minimum total level required to join (e.g., 1500+, 2000+) |
| Fresh start | Periodic reset worlds -- everyone starts from scratch |
| Event | Temporary worlds for seasonal events or competitions |
| Ironman | Restricted trading and Grand Exchange access |
| Speedrun | Timer-tracked progression worlds with leaderboards |
| Creative | Building-focused, unlimited resources, no combat |

---

### World Capacity and Queuing

| Setting | Default | Description |
|---|---|---|
| Max players per world | 2000 | Configurable cap |
| Queue enabled | On | When world is full, players wait in line |
| Queue display | Position + estimated wait | Shown to player while waiting |
| Priority queue | Off | Toggle for subscribers or veterans to skip ahead |
| Queue timeout | 5 minutes | Removed from queue after inactivity |
| Overflow redirect | On | Offer to join a less populated world instead of waiting |

---

### Login Server

Central gateway that handles authentication and routes players to world servers.

| Responsibility | Description |
|---|---|
| Authentication | Validates credentials, issues session tokens |
| World registry | Maintains list of active world servers and their status |
| Routing | Directs authenticated players to their chosen world |
| Health checks | Polls world servers, removes unhealthy worlds from the list |
| Rate limiting | Prevents login spam and brute force attempts |
| Announcements | Pushes messages to login screen (maintenance warnings, patch notes) |

---

## 3. Instancing

Instancing creates private copies of zones. Used for player housing, boss encounters, dungeons, and crowd management.

---

### Instance Types

| Type | Description | Example |
|---|---|---|
| Personal | Single-player instance tied to one account | Player-owned house, personal farm |
| Group | Party enters together, isolated from other groups | Raids, dungeons, boss encounters |
| Zone | Multiple copies of the same overworld zone, players distributed | Brighter Shores style -- same area, multiple copies |
| Dynamic | Auto-created when zone population exceeds threshold, auto-merged when empty | Megaserver crowd management |
| Event | Temporary instance for a timed event, destroyed when event ends | World boss spawn, holiday event arena |

---

### Instance Lifecycle

| Phase | Description |
|---|---|
| Creation | Triggered by player action, party entry, or population threshold |
| Population | Players are assigned or choose to enter |
| Active | Game loop runs, entities are simulated |
| Idle detection | Timer starts when last player leaves or goes inactive |
| Cleanup | Instance state is saved (if persistent) and resources are freed |
| Destruction | Instance is removed from memory |

---

### Instance Settings

| Setting | Default | Description |
|---|---|---|
| Creation cost | 0 GP | Gold cost to create an instance (gold sink lever) |
| Inactivity timer | 5 minutes | Time after last player activity before instance closes |
| Max players | Varies by type | Personal = 1, Group = party size, Zone = configurable |
| Persistence | Off | Whether instance state survives server restart |
| Loot rules | Party setting | How drops are distributed in group instances |
| Re-entry | Allowed | Whether players can re-enter after leaving |
| Re-entry window | 2 minutes | Time allowed to re-enter before instance locks |

---

## 4. Player Density Management

Controls how the server and client handle large numbers of players in the same area.

---

### Entity Visibility

| Setting | Default | Description |
|---|---|---|
| Entity draw distance | 256 tiles (4 chunks) | Maximum range at which other players are visible |
| Entity cap per view | 250 | Maximum visible players -- render closest first |
| NPC draw distance | 256 tiles | Maximum range at which NPCs are visible |
| Item draw distance | 128 tiles (2 chunks) | Maximum range at which ground items are visible |
| Projectile draw distance | 256 tiles | Maximum range at which projectiles are visible |

---

### Chat Range

| Channel | Range | Description |
|---|---|---|
| Public | 64 tiles | Only players within range see public chat |
| Shout | 256 tiles | Extended range, rate-limited |
| Clan | Global | Visible to all clan members regardless of distance |
| Private | Global | Direct messages between two players |
| Trade | 32 tiles | Short-range trade chat |
| Global | Global | Server-wide announcements (admin or system only) |

---

### Crowd Management

| Mechanism | Description |
|---|---|
| Entity cap enforcement | When entity cap is reached, furthest entities are culled from view |
| Priority rendering | Party members and friends are always rendered, even beyond cap |
| NPC density scaling | Reduce NPC count in high-population zones to save server resources |
| Resource competition | DM toggle: shared nodes (first-come) vs instanced nodes (everyone gets their own) |
| Auto-instancing threshold | When zone population exceeds X, new arrivals go to a new zone instance |
| Spawn spreading | Distribute new player spawns across multiple locations to prevent bottlenecks |

---

## 5. Network Optimization

Techniques to reduce bandwidth and maintain responsiveness under load.

---

### State Compression

| Technique | Description |
|---|---|
| Delta encoding | Only send what changed since last tick -- position deltas, stat changes |
| Bitfield packing | Encode boolean states (in combat, running, skulled) as bit flags |
| Entity ID reuse | Short IDs for nearby entities, recycled when entities leave view |
| Chunk compression | Tile data sent as base64-encoded binary, not JSON arrays |
| String interning | Common strings (item names, skill names) mapped to integer IDs |

---

### Update Priority

| Entity Distance | Update Frequency | Description |
|---|---|---|
| 0-64 tiles | Every tick | Full state updates for nearby entities |
| 64-128 tiles | Every 2nd tick | Reduced frequency for mid-range entities |
| 128-256 tiles | Every 5th tick | Low-frequency updates for distant entities |
| 256+ tiles | No updates | Entity is culled from client state entirely |

---

### Message Optimization

| Technique | Description |
|---|---|
| Message batching | Bundle multiple small messages into a single WebSocket frame per tick |
| Bandwidth throttling | Detect slow connections, reduce update frequency and detail |
| Client prediction | Client predicts movement locally, server sends corrections when prediction is wrong |
| Input batching | Client buffers inputs and sends one message per tick instead of per keypress |
| Chunk streaming | Only send chunk data for chunks near the player (already in engine) |
| Lazy loading | Defer sending non-critical data (cosmetics, animations) until player is idle |

---

## 6. Database Scaling

How persistence scales from flat files to distributed databases.

---

### Connection Management

| Technique | Description |
|---|---|
| Connection pooling | Reuse database connections instead of opening new ones per query |
| Read replicas | Route read-heavy queries (hiscores, GE price history) to replica databases |
| Write batching | Batch player saves into periodic bulk writes instead of per-action saves |
| Write-ahead log | Log all state changes before applying, enabling crash recovery |

---

### Data Distribution

| Technique | Description |
|---|---|
| Sharded player data | Distribute player records across DB servers by player ID hash |
| Sharded world data | Distribute world/chunk data across DB servers by region |
| Cache layer | Redis or similar for frequently read data -- online players, GE prices, session tokens |
| Event sourcing | Log all game events as immutable records, rebuild state from the event log |
| Hot/cold storage | Active player data in fast storage, inactive accounts archived to cold storage |

---

### Save Strategy by Scale

| Scale | Strategy | Save Frequency |
|---|---|---|
| 1-50 players | JSON files, synchronous writes | On every significant action |
| 50-200 | SQLite, WAL mode | Every 30 seconds + on logout |
| 200-1000 | PostgreSQL, connection pool | Every 60 seconds + on logout |
| 1000-5000 | PostgreSQL + Redis cache | Every 60 seconds, batched writes |
| 5000+ | Sharded PostgreSQL + Redis cluster | Continuous write batching, async saves |

---

## 7. Cross-Server Features

Features that span multiple world servers and require shared infrastructure.

---

### Global Systems

| System | Description |
|---|---|
| Global chat channels | Chat channels visible across all world servers (clan chat, global announcements) |
| Global Grand Exchange | Shared economy -- buy/sell orders work across all worlds |
| Global hiscores | Unified leaderboards across all worlds |
| Global friends list | See which world a friend is on, send messages across worlds |
| Cross-server clans | Clan membership and chat work regardless of which world members are on |
| Global events | World events that run simultaneously on all servers |
| Global mail | Send items and messages to offline players across any world |

---

### Server-to-Server Communication

| Component | Description |
|---|---|
| Message broker | Central pub/sub system (Redis, NATS) for inter-server messaging |
| Event bus | Publish game events (player traded, boss killed) for other servers to react to |
| State sync | Shared state (GE orders, clan rosters, friend statuses) replicated across servers |
| Heartbeat | World servers send periodic health pings to the coordinator |
| Coordinator | Central service that manages world registry, routes cross-server requests |

---

### Cross-Server Data Flow

| Data | Flow | Latency Tolerance |
|---|---|---|
| Chat messages | World -> Broker -> All subscribed worlds | < 200ms |
| GE orders | World -> Economy service -> All worlds | < 1 second |
| Friend status | World -> Coordinator -> Querying world | < 500ms |
| Hiscore updates | World -> Hiscores service (async) | Minutes acceptable |
| Clan updates | World -> Clan service -> All member worlds | < 1 second |
| Global events | Coordinator -> All worlds (broadcast) | < 500ms |

---

## 8. Performance Targets

Hard targets that the server must meet to maintain gameplay quality.

---

### Tick Consistency

| Metric | Target | Description |
|---|---|---|
| Tick duration | 600ms (default) | Must complete all processing within one tick |
| Tick jitter | < 50ms | Maximum deviation from expected tick timing |
| Tick overrun budget | 10% of tick duration | Maximum time a tick can run over before it is flagged |
| Consecutive overruns | 0 | Any consecutive overrun triggers an alert |

---

### Player Count Targets

| Players per World | Requirements |
|---|---|
| 100 | Single server, no optimization needed |
| 500 | Entity culling, proximity filtering, delta encoding |
| 1000 | Full network optimization, connection pooling, write batching |
| 2000 | Dedicated hardware, sharded zones or aggressive instancing |
| 5000+ | Multi-server zone sharding, microservices, load balancing |

---

### Latency Targets

| Context | Target | Description |
|---|---|---|
| PvE gameplay | < 100ms round trip | Acceptable for skilling and PvE combat |
| PvP combat | < 50ms round trip | Required for competitive tick-perfect play |
| Chat delivery | < 200ms | Acceptable for text communication |
| World hop | < 3 seconds | Total time from request to playing on new world |
| Login | < 5 seconds | Total time from credentials to in-game |

---

### Resource Budgets

| Resource | Budget per Tick | Description |
|---|---|---|
| CPU time | < 400ms of 600ms tick | Leave 200ms headroom for OS and GC |
| Database queries | < 50 per tick | Batched, async where possible |
| WebSocket messages out | < 10,000 per tick | Total across all connected players |
| Memory per player | < 50 KB | In-memory state for one connected player |
| Bandwidth per player | < 10 KB/s | Average downstream bandwidth |

---

### Uptime

| Metric | Target |
|---|---|
| Uptime | 99.9% (8.7 hours downtime per year) |
| Planned maintenance window | Weekly, < 30 minutes, rolling (one world at a time) |
| Unplanned downtime recovery | < 5 minutes (auto-restart with state recovery) |
| Data loss window | 0 (write-ahead log ensures no committed state is lost) |

---

## 9. Monitoring and Ops

Observability and operational tooling for running the game in production.

---

### Server Health Dashboard

| Metric | Description |
|---|---|
| CPU usage | Per world server, per service |
| Memory usage | Heap size, RSS, per-player memory |
| Tick timing | Current tick duration, average, 99th percentile |
| Player count | Per world, total across all worlds |
| Entity count | Total simulated entities per world |
| Network throughput | Messages in/out per second, bandwidth per second |
| Database latency | Query time average, 99th percentile |
| Error rate | Errors per minute, by category |

---

### Alerting

| Alert | Trigger | Severity |
|---|---|---|
| Tick overrun | Tick processing exceeds tick duration | Critical |
| World full | World reaches max player capacity | Warning |
| Error spike | Error rate exceeds 10x baseline | Critical |
| Memory pressure | Heap usage exceeds 80% of limit | Warning |
| Database slow | Query p99 exceeds 100ms | Warning |
| World offline | Health check fails for 3 consecutive pings | Critical |
| Disk space | Less than 10% free on any volume | Warning |
| Login failures | Authentication error rate exceeds threshold | Warning |

---

### Operational Procedures

| Procedure | Description |
|---|---|
| Graceful shutdown | Save all player data, warn players 5 minutes in advance, disconnect cleanly |
| Rolling updates | Update one world at a time. Players on updating world are warned and can hop. |
| Auto-scaling | Monitor player demand, spin up new world servers when existing worlds approach capacity |
| Log aggregation | Centralized logging across all world servers and services (structured JSON logs) |
| Backup schedule | Full database backup daily, incremental every hour, write-ahead log continuous |
| Incident response | Auto-restart crashed worlds, page on-call for repeated crashes |

---

### Auto-Scaling Rules

| Trigger | Action |
|---|---|
| All worlds above 80% capacity | Spin up new world server |
| World below 10% capacity for 30 minutes | Offer migration to players, then shut down world |
| Login queue exceeds 100 players | Spin up new world server immediately |
| CPU usage above 90% for 5 minutes | Alert and prepare failover |
| No worlds available in a region | Spin up world in nearest region |

---

## 10. Scale Tiers

What architecture to use at each player count.

| Players | Architecture | Database | Shared State | Estimated Cost |
|---|---|---|---|---|
| 1-50 | Single server, flat JSON files | None (JSON on disk) | N/A (one server) | Free (localhost) |
| 50-200 | Single server, SQLite | SQLite (WAL mode) | N/A (one server) | Cheap VPS ($5-20/mo) |
| 200-1000 | Single server or 2-3 worlds, PostgreSQL | PostgreSQL | Redis for sessions | Dedicated server ($50-100/mo) |
| 1000-5000 | Multi-world (5-10 worlds), PostgreSQL + Redis | PostgreSQL + Redis | Redis pub/sub for cross-world | Multiple servers ($200-500/mo) |
| 5000-20000 | Multi-world (20-50 worlds), sharded DB, load balancer | Sharded PostgreSQL + Redis cluster | Message broker (NATS/Redis Streams) | Cloud auto-scale ($500-2000/mo) |
| 20000+ | Megaserver or microservices, sharded everything, CDN | Distributed DB cluster | Full service mesh | Full cloud infra ($2000+/mo) |

---

### Migration Path

Each tier builds on the previous one. No full rewrites required.

| Transition | What Changes |
|---|---|
| JSON -> SQLite | Replace fs.readFile/writeFile with better-sqlite3 calls. Schema matches JSON structure. |
| SQLite -> PostgreSQL | Replace better-sqlite3 with pg pool. Add connection pooling. Same queries, different driver. |
| Single -> Multi-world | Extract login server. World servers register with coordinator. Add world hop protocol. |
| Multi-world -> Sharded | Split world into zone servers. Add zone boundary handoff. Shared entity state at edges. |
| Add microservices | Extract chat, economy, hiscores into independent services. World servers call via message broker. |
| Add auto-scaling | Containerize world servers. Add orchestrator (Kubernetes or custom). Health-check-driven scaling. |

---

## Module Toggles

Master switches for scaling features. All off by default -- enable as needed.

| Toggle | Default | Description |
|---|---|---|
| World system | Off | Multi-world support with world list and world hopping |
| Instancing | Off | Personal, group, and zone instancing |
| Dynamic instancing | Off | Auto-create and merge zone instances based on population |
| Cross-server features | Off | Global chat, GE, hiscores, friends across worlds |
| Auto-scaling | Off | Automatic world server provisioning based on demand |
| Entity caps | On | Maximum visible entities per client viewport |
| Chunk streaming | On | Only send chunk data for nearby chunks |
| State compression | Off | Delta encoding and bitfield packing for network messages |
| Monitoring | Off | Server health dashboard and alerting |
| Read replicas | Off | Route read queries to replica databases |
| Write batching | Off | Batch player saves into periodic bulk writes |
| Cache layer | Off | Redis cache for frequently accessed data |

---

## Views

UI panels for server operators and dungeon masters.

| View | Description |
|---|---|
| Server dashboard | Real-time health metrics -- CPU, memory, tick timing, player count, error rate per world |
| World list | Admin view of all worlds with controls to start, stop, restart, and configure each world |
| Instance manager | List of active instances with player counts, creation time, and manual close button |
| Performance graphs | Historical charts for tick timing, player count, bandwidth, and database latency |
| Player distribution heatmap | Visual map showing where players are concentrated across the world |
| Log viewer | Searchable, filterable view of aggregated logs across all servers |
| Alert history | Timeline of triggered alerts with status and resolution notes |
