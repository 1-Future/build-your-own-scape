# Data Persistence

How game state is saved, loaded, backed up, and migrated. The invisible infrastructure that keeps the world alive between server restarts.

---

## 1. Data Categories

What needs to be saved.

| Category | Contents |
|---|---|
| Player Data | Stats, bank, equipment, position, friends list, achievements, quest progress, settings, appearance, emotes unlocked, collection log |
| World Data | Tiles, walls, doors, heights, objects, NPCs, lots, roofs, variants, chunk modifications |
| Economy Data | Grand Exchange offers, trade history, money supply, price history, tax records |
| Clan Data | Members, ranks, coffer balance, events, achievements, clan hall state |
| System Data | Server configuration, plugin states, mod logs, ban lists, uptime records |

---

## 2. Storage Approaches

DM picks the storage backend based on world size and technical comfort.

| Approach | Description | Pros | Cons |
|---|---|---|---|
| Flat Files (JSON/Binary) | Simple file-per-entity storage. Current OpenScape approach | No dependencies, easy to inspect, portable | No query support, slow at scale, no concurrent access |
| SQLite | Embedded single-file relational database | Structured queries, zero configuration, single file backup | Single-writer limitation, not ideal for high concurrency |
| PostgreSQL / MySQL | Full relational database server | Scalable, concurrent access, ACID transactions, production-grade | Requires separate server, more complex setup |
| MongoDB | Document-oriented database | Flexible schema, good for player profiles, JSON-native | Weaker transactions, eventual consistency concerns |
| Redis | In-memory key-value store | Extremely fast reads, good for sessions and leaderboards | Volatile by default, not a primary data store |
| Hybrid | Redis cache + PostgreSQL persistence | Fast reads from cache, durable writes to database | Most complex setup, two systems to maintain |

### Scale Recommendations

| Scale | Players | Recommendation |
|---|---|---|
| Hobby | 1-50 | Flat JSON files |
| Small | 50-500 | SQLite |
| Medium | 500-5000 | PostgreSQL |
| Large | 5000+ | PostgreSQL + Redis cache |

---

## 3. Save Strategy

| Strategy | Description |
|---|---|
| Auto-Save Interval | Periodic save on a configurable timer (default 30 seconds) |
| Save-on-Change | Save immediately when critical data changes (trades, deaths, level-ups, item transfers) |
| Periodic Full Save | Full world snapshot at longer intervals (default 5 minutes) |
| Incremental Save | Only save what changed since the last save (delta saves) |
| Transaction Logging | Write-ahead log recording every state change for crash recovery |

| Setting | Description |
|---|---|
| Auto-Save Frequency | Configurable interval in seconds |
| Critical Events List | Which events trigger an immediate save |
| Full Save Interval | How often a complete world snapshot is written |
| WAL Mode | Enable or disable write-ahead logging |
| Save Queue | Asynchronous save queue to avoid blocking the game loop |

---

## 4. Backup System

| Feature | Description |
|---|---|
| Automatic Backups | Scheduled backups at configurable intervals (hourly, daily, weekly) |
| Backup Retention | Keep the last X backups, automatically delete older ones |
| Off-Site Backup | Push backups to cloud storage (S3, Google Cloud Storage, etc.) |
| Backup Verification | Periodically test-restore from a backup to verify integrity |
| Point-in-Time Recovery | Restore to any moment using transaction logs combined with the nearest backup |
| Manual Backup Trigger | DM can force a backup at any time from the dashboard |
| Backup Naming | Timestamped backup files with world name and version |

| Setting | Description |
|---|---|
| Backup Interval | How often automatic backups run |
| Retention Count | Number of backups to keep before pruning |
| Off-Site Destination | Cloud storage bucket or remote path |
| Verification Schedule | How often backup test-restores run |
| Compression | Whether backups are compressed (gzip, zstd) |

---

## 5. Data Migration

| Feature | Description |
|---|---|
| Version Migrations | When the data schema changes between updates, migration scripts transform old format to new format |
| Migration Scripts | Ordered, versioned scripts that run automatically on server start if data version is behind |
| Backward Compatibility | New server versions can read data from previous versions |
| Export / Import | Move an entire world between servers using a standardized export format |
| Cross-Version Compatibility | Transform data from older game versions (OSRS-era format) to the current schema |
| Dry Run | Test a migration without applying changes to verify it will succeed |

---

## 6. Data Integrity

| Feature | Description |
|---|---|
| Checksums | Verify data files have not been corrupted using hash comparison |
| Foreign Key Validation | Ensure every reference is valid (item in bank references a real item ID, quest progress references a real quest) |
| Orphan Detection | Find data entries that reference deleted entities (items referencing removed item definitions) |
| Duplicate Detection | Identify impossible duplicates that indicate dupe exploits (same unique item ID in two banks) |
| Audit Trail | Log all data modifications with timestamp, source (player action, admin command, system), and before/after values |
| Constraint Enforcement | Validate data against defined rules (no negative GP, no impossible stat values, no items exceeding stack limits) |

---

## 7. Chunk Streaming and Persistence

Chunks are the spatial unit of world data. See engine-architecture.md for chunk dimensions and structure.

| Feature | Description |
|---|---|
| Binary Storage | Chunks saved as binary files for compact storage and fast read/write |
| Lazy Loading | Chunks loaded from disk only when a player approaches the area |
| Memory Eviction | Chunks with no nearby players are unloaded from memory after a configurable timeout |
| Dirty Flag | Only chunks that have been modified since last save are written to disk |
| Compression | Stored chunks are compressed to reduce disk usage (configurable algorithm) |
| Chunk Indexing | Index file maps chunk coordinates to file locations for fast lookup |
| Concurrent Access | Multiple players in the same chunk share the in-memory copy, writes are serialized |

---

## 8. Player Data Portability

| Feature | Description |
|---|---|
| Account Export | Player downloads their full character data (stats, bank, achievements, appearance) as a portable file. Supports GDPR compliance |
| Account Import | Bring a character from another server by importing an export file. DM controls whether imports are allowed and what data is accepted |
| Cross-Server Play | Same account used on multiple servers (if a platform-level account system is enabled). Character data syncs or forks per server |
| Character Transfer | Move a character from one DM world to another. Both servers must agree on the transfer. Configurable restrictions on what transfers (items, stats, quests) |

| Setting | Description |
|---|---|
| Allow Export | Whether players can export their data |
| Allow Import | Whether the server accepts imported characters |
| Import Restrictions | Which data fields are accepted on import (stats only, stats + bank, everything) |
| Transfer Approval | Whether character transfers require DM approval |
| Cross-Server Mode | Off, Read-Only (view characters from other servers), or Full Sync |

---

## 9. Disaster Recovery

| Scenario | Recovery Method |
|---|---|
| Server Crash | Resume from last auto-save plus replay of the write-ahead transaction log to recover changes since last save |
| World Rollback | Restore the entire world to a previous backup. All changes after the backup point are lost |
| Player Rollback | Restore an individual player to a previous saved state without affecting the rest of the world |
| Data Corruption | Detect corrupted files via checksum mismatch, replace with last known good version from backup |
| Failover | Automatic switch to a standby backup server if the primary crashes (requires redundant infrastructure) |

| Setting | Description |
|---|---|
| Transaction Log Retention | How long write-ahead logs are kept before pruning |
| Rollback Granularity | How many player save states are kept for individual rollback |
| Corruption Alert | Notify DM immediately when a checksum mismatch is detected |
| Failover Mode | Off, Manual (DM triggers switch), or Automatic |
| Recovery Testing | Schedule periodic recovery drills to verify the process works |

---

## Module Toggles

| Toggle | Default |
|---|---|
| Storage Backend Selection | Off |
| Auto-Save | Off |
| Save-on-Change | Off |
| Transaction Logging | Off |
| Automatic Backups | Off |
| Off-Site Backups | Off |
| Backup Verification | Off |
| Chunk Compression | Off |
| Data Export (Player) | Off |
| Data Import (Player) | Off |
| Cross-Server Play | Off |
| Character Transfer | Off |
| Integrity Checks | Off |
| Audit Trail | Off |
| Failover | Off |
| Auto-Migration | Off |

---

## Views

| View | Description |
|---|---|
| Backup Dashboard | List of all backups with timestamps, sizes, and restore buttons |
| Data Health Monitor | Real-time status of save operations, last save time, pending writes, errors |
| Migration Status | Current data version, available migrations, migration history and results |
| Storage Usage | Disk space used by world data, player data, backups, and transaction logs |
| Integrity Report | Results of the latest integrity check -- checksums, orphans, duplicates, constraint violations |
| Audit Log | Searchable log of all data modifications with filters by player, action type, and time range |
| Recovery Console | DM tool for triggering rollbacks, restoring backups, and replaying transaction logs |
