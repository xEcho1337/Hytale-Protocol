# Disconnect

Packet ID: 1  
Bound: Server → Client *(TODO: check if it's bidirectional)*
## Description

Packet used to immediately terminate a connection. 
## Structure

### Overview

The packet consists of a small fixed block followed by an optional variable-length UTF-8 string.  
The presence of the variable field is controlled by a single-byte null-check bitmask.
### Fixed block (2+ bytes)

| Name         | Size | Description                                          | Optional? |
| ------------ | ---- | ---------------------------------------------------- | --------- |
| `Null check` | Byte | Bitmask indicating which optional fields are present | No        |
| `Type`       | Byte | `DisconnectType`                                     | No        |
### Variable block

| Name     | Size      | Description                                 | Optional |
| -------- | --------- | ------------------------------------------- | -------- |
| `Reason` | Var UTF-8 | Human-readable reason for the disconnection | Yes      |
## Null check Bitmask
- `0x01`: `Reason`is present
## DisconnectType
- Disconnect (0): Normal disconnection
- Crash (1): Disconnection due to an error