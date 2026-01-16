# Status

Packet ID: 10
Bound: Server → Client
## Description

Packet used to obtain a server's informations.
## Structure

### Fixed block (X bytes)
| Name           | Size | Description                                                         | Optional? |
| -------------- | ---- | ------------------------------------------------------------------- | --------- |
| `Null check`   | Byte | Bitmask indicating which optional fields are present                | No        |
| `Player count` | Int  | Number of players inside the server                                 | No        |
| `Max players`  | Int  | Maximum number of allowed players                                   | No        |
| `Name offset`  | Int  | Offset to the `Name` field in the variable block, or `-1` if absent | No        |
| `Motd offset`  | Int  | Offset to the `Motd` field in the variable block, or `-1` if absent | No        |
### Variable block

| Name   | Size      | Description                              | Optional? |
| ------ | --------- | ---------------------------------------- | --------- |
| `Name` | Var UTF-8 | Optional server's name (max length: 128) | Yes       |
| `Motd` | Var UTF-8 | Optional Motd text (max length: 512)     | Yes       |
## Null check Bitmask
- `0x01`: `Name` is present    
- `0x02`: `Motd` is present