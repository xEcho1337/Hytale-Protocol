# ApplyKnockback

Packet ID: 164
Bound: Server → Client
## Description

Packet sent by the server to apply a velocity change (knockback) to the client.  It optionally includes the world position where the hit occurred, the velocity vector to apply, and the mode used to apply that velocity.
## Structure 

### Fixed block (38 bytes)
| Name           | Size                | Description                                          | Optional? |
| -------------- | ------------------- | ---------------------------------------------------- | --------- |
| `Null check`   | Byte                | Bitmask indicating which optional fields are present | No        |
| `Hit position` | Position (24 bytes) | World-space hit position, or zeroed if not present   | Yes       |
| `Velocity X`   | Float (LE)          | X component of the velocity change                   | No        |
| `Velocity Y`   | Float (LE)          | Y component of the velocity change                   | No        |
| `Velocity Z`   | Float (LE)          | Z component of the velocity change                   | No        |
| `Change type`  | Byte                | `ChangeVelocityType`                                 | No        |
## Null check Bitmask

If the bit is **not set**, the `HitPosition` field is still serialized but filled with **24 zero bytes**.
- `0x01`: `HitPosition` is present
## ChangeVelocityType
- Add (0): Adds the velocity to the current player's velocity
- Set: (1): Overwrites the player's velocity