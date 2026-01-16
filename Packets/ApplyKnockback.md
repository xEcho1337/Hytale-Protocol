# ApplyKnockback

Packet ID: 164
Bound: Server → Client
## Description

Packet sent by the server to apply a velocity change (knockback) to the client.  It optionally includes the world position where the hit occurred, the velocity vector to apply, and the mode used to apply that velocity.
## Structure 

### Fixed block (38 bytes)
| Name         | Size                | Description                                          | Optional? |
| ------------ | ------------------- | ---------------------------------------------------- | --------- |
| Null check   | Byte                | Bitmask indicating which optional fields are present | False     |
| Hit position | Position (24 bytes) | World-space hit position, or zeroed if not present   | True      |
| Velocity X   | Float (LE)          | X component of the velocity change                   | False     |
| Velocity Y   | Float (LE)          | Y component of the velocity change                   | False     |
| Velocity Z   | Float (LE)          | Z component of the velocity change                   | False     |
| Change type  | Byte                | Velocity application mode (`0` = Add, `1` = Set)     | False     |
## Null check bitmask

If the bit is **not set**, the `HitPosition` field is still serialized but filled with **24 zero bytes**.
- `0x01` (0000 0001): `HitPosition` is present