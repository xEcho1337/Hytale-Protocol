# Connect

Packet ID: 0
Bound: Client → Server
# Description

Packet used by the client to initiate a connection to the server.
# Structure
### Fixed block (102 bytes)
| Name                     | Size                    | Description                                                                   | Optional? |
| ------------------------ | ----------------------- | ----------------------------------------------------------------------------- | --------- |
| `Null check`             | Byte                    | Bitmask indicating which optional fields are present                          | No        |
| `Protocol hash`          | Fixed ASCII String (64) | Protocol identifier string used to match client and server protocol versions  | No        |
| `Client type`            | Byte                    | Client type enum (`Game = 0`, `Editor = 1`)                                   | No        |
| `UUID`                   | UUID                    | Unique identifier of the connecting client                                    | No        |
| `Language offset`        | Int                     | Offset to the `Language` field in the variable block, or `-1` if absent       | No        |
| `Identity token offset`  | Int                     | Offset to the `IdentityToken` field in the variable block, or `-1` if absent  | No        |
| `Username offset`        | Int                     | Offset to the `Username` field in the variable block                          | No        |
| `Referral data offset`   | Int                     | Offset to the `ReferralData` field in the variable block, or `-1` if absent   | No        |
| `Referral source offset` | Int                     | Offset to the `ReferralSource` field in the variable block, or `-1` if absent | No        |
### Variable block

| Name             | Size            | Description                                               | Optional? |
| ---------------- | --------------- | --------------------------------------------------------- | --------- |
| `Language`       | Var ASCII       | Optional language identifier (max length: 128)            | Yes       |
| `IdentityToken`  | Var UTF-8       | Optional identity/authentication token (max length: 8192) | Yes       |
| `Username`       | Var ASCII       | Username chosen by the client (max length: 16)            | Yes       |
| `ReferralData`   | VarInt + byte[] | Optional referral payload (max length: 4096 bytes)        | Yes       |
| `ReferralSource` | Variable        | Optional referral source address                          | Yes       |
## Null check Bitmask
- `0x01`: `Language` is present    
- `0x02`: `IdentityToken` is present
- `0x04`: `ReferralData` is present
- `0x08`: `ReferralSource` is present