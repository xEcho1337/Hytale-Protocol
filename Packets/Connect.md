# Connect

Packet ID: 0
Bound: Client → Server
# Description

Packet used by the client to initiate a connection to the server.  It contains protocol identification data, client metadata, authentication-related information, and optional referral details.
# Structure
### Fixed block (102 bytes)
| Name                   | Size                    | Description                                                                   | Optional? |
| ---------------------- | ----------------------- | ----------------------------------------------------------------------------- | --------- |
| Null check             | Byte                    | Bitmask indicating which optional fields are present                          | False     |
| Protocol hash          | Fixed ASCII String (64) | Protocol identifier string used to match client and server protocol versions  | False     |
| Client type            | Byte                    | Client type enum (`Game = 0`, `Editor = 1`)                                   | False     |
| UUID                   | UUID (16)               | Unique identifier of the connecting client                                    | False     |
| Language offset        | Int32 (LE)              | Offset to the `Language` field in the variable block, or `-1` if absent       | False     |
| Identity token offset  | Int32 (LE)              | Offset to the `IdentityToken` field in the variable block, or `-1` if absent  | False     |
| Username offset        | Int32 (LE)              | Offset to the `Username` field in the variable block                          | False     |
| Referral data offset   | Int32 (LE)              | Offset to the `ReferralData` field in the variable block, or `-1` if absent   | False     |
| Referral source offset | Int32 (LE)              | Offset to the `ReferralSource` field in the variable block, or `-1` if absent | False     |
### Variable block

Fields in the variable block are not stored in a fixed order semantically, but are written sequentially during serialization.  Each field is accessed through its corresponding offset.

| Name           | Size                  | Description                                               | Optional? |
| -------------- | --------------------- | --------------------------------------------------------- | --------- |
| Language       | VarInt + ASCII string | Optional language identifier (max length: 128)            | True      |
| IdentityToken  | VarInt + UTF-8 string | Optional identity/authentication token (max length: 8192) | True      |
| Username       | VarInt + ASCII string | Username chosen by the client (max length: 16)            | True      |
| ReferralData   | VarInt + byte[]       | Optional referral payload (max length: 4096 bytes)        | True      |
| ReferralSource | Variable              | Optional referral source address                          | True      |
## Null check bitmask

Each bit in the `Null check` byte determines whether the corresponding optional field is present.
- `0x01` (0000 0001): `Language` is present    
- `0x02` (0000 0010): `IdentityToken` is present
- `0x04` (0000 0100): `ReferralData` is present
- `0x08` (0000 1000): `ReferralSource` is present