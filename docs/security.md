# Security

Protecting the game from exploits, cheats, and attacks beyond bot detection. This covers the technical security layer.

---

## 1. Client-Server Validation

Never trust the client. All game logic runs server-side. The client is a display layer.

| Validation | Description |
|---|---|
| Movement validation | Server checks pathfinding, rejects impossible moves |
| Combat validation | Server calculates all damage, client only shows the result |
| Inventory validation | Server tracks all items, client is display only |
| XP validation | Server awards XP, client cannot self-award |
| Trade validation | Server verifies both sides of a trade before executing |
| Action rate limiting | Max actions per tick, prevents speed hacks |
| Interaction range | Server checks distance to target before allowing interaction |

---

## 2. Packet Validation

Every message between client and server is validated before processing.

| Check | Description |
|---|---|
| Format validation | Reject malformed JSON or unexpected message structures |
| Size limits | Reject packets exceeding maximum allowed size |
| Sequence checking | Detect replayed or out-of-order packets |
| Rate limiting | Max messages per tick per message type |
| Type whitelisting | Client can only send message types the server expects |
| Checksum verification | Detect packets that have been tampered with in transit |

---

## 3. Anti-Cheat

Detecting and preventing client-side manipulation.

| Detection | Description |
|---|---|
| Speed hack | Movement faster than the maximum possible speed for the player |
| Teleport hack | Position change without a valid path between origin and destination |
| Item duplication | Item count anomalies detected during tick auditing |
| Damage hack | Damage dealt exceeds the calculated maximum for the player's gear and stats |
| Interaction range | Attempting to interact with objects beyond the allowed distance |
| Animation cancel | Actions completing faster than the minimum animation time |
| Auto-clicker | Inhuman click patterns (overlaps with bot detection) |
| Client integrity | Checksum or hash verification of client files on connection |

---

## 4. Account Security

Protecting player accounts from unauthorized access.

| Feature | Description |
|---|---|
| Password hashing | bcrypt or argon2, never plaintext storage |
| Two-factor authentication | Optional or mandatory TOTP-based 2FA |
| Session tokens | JWT or equivalent, with configurable expiration |
| IP anomaly detection | Login from a new country or unusual IP triggers an alert |
| Device fingerprinting | Track known devices per account, flag new ones |
| Brute force protection | Account lockout after configurable number of failed attempts |
| Password strength | Minimum length, complexity requirements (DM configurable) |
| Email verification | Required for sensitive actions (password change, 2FA disable) |
| Account recovery | Secure multi-step recovery flow with identity verification |

---

## 5. Data Security

Protecting game data in transit and at rest.

| Measure | Description |
|---|---|
| Encryption in transit | WSS (TLS) for all WebSocket connections |
| Encryption at rest | Sensitive data encrypted on disk (player credentials, payment tokens) |
| SQL injection prevention | Parameterized queries for all database operations |
| XSS prevention | Sanitize all player-generated content (names, chat, notes) |
| Input sanitization | Reject special characters in names, prevent chat injection |
| Rate limiting | All endpoints rate-limited to prevent abuse |
| DDoS protection | Reverse proxy, connection limits, and rate limiting at the network layer |
| Backup encryption | All backups encrypted with a key the DM controls |

---

## 6. Economy Security

Preventing exploits that target the game economy.

| Protection | Description |
|---|---|
| Transaction atomicity | Trades complete fully or not at all, no partial execution |
| Double-spend prevention | Items and currency locked during pending transactions |
| Race condition protection | GE offers use locking to prevent simultaneous match conflicts |
| Overflow protection | Integer overflow checks on all item and GP quantities |
| Dupe detection | Per-tick item count auditing to catch duplication anomalies |
| Trade logging | All trades logged with timestamps, participants, and items for forensic review |
| Transaction rollback | Ability to reverse exploited transactions and restore prior state |

---

## 7. Privacy

Protecting player personal data and meeting legal requirements.

| Feature | Description |
|---|---|
| Data protection | GDPR-compliant handling of player personal data |
| Data minimization | Only collect information necessary for game operation |
| Right to deletion | Account deletion removes all personal data |
| Data export | Players can download all data associated with their account |
| Cookie/tracking policy | Transparent disclosure of what is tracked and why |
| Age verification | Configurable age gate if required by jurisdiction |
| Payment data | Never stored on the game server, use payment processor tokens |

---

## 8. Module Toggles

Each security system can be independently configured by the DM.

| Module | Default |
|---|---|
| Client validation strictness | Strict |
| Packet rate limiting | On |
| Anti-cheat | On |
| Two-factor authentication | Optional |
| IP anomaly detection | On |
| Device fingerprinting | Off |
| Encryption at rest | Off |
| DDoS protection | On |
| Economy auditing | On |
| Privacy compliance mode | GDPR |

---

## 9. Admin Views

| View | Description |
|---|---|
| Security dashboard | Overview of active threats and system status |
| Threat log | Timestamped log of all detected exploits and attacks |
| Failed login attempts | List of failed logins with IP, account, and timestamp |
| Anomaly alerts | Flagged accounts and sessions requiring review |
| Audit trail | Complete log of all admin and mod actions |
| Compliance report | Summary of privacy compliance status and data handling |
