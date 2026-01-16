# Ping

Packet ID: 2
Bound: Bidirectional
## Description

Packet used to ping a client or a server. When it's received, the destination must reply with a [Pong](Pong.md) packet and the same id.
## Structure

| Name                     | Size       | Description                                                                                       | Optional? |
| ------------------------ | ---------- | ------------------------------------------------------------------------------------------------- | --------- |
| `Null-check`             | Byte       | Bitmask indicating which optional fields are present                                              | No        |
| `Id`                     | Int        | Unique identifier for this ping packet                                                            | No        |
| `Time`                   | Long + Int | Instant which the ping packet was created. If the time is null, this field is filled by 12 zeros. | Yes       |
| `Last ping value raw`    | Int        | TODO                                                                                              | No        |
| `Last ping value direct` | Int        | TODO                                                                                              | No        |
| `Last ping value tick`   | Int        | TODO                                                                                              | No        |
## Null check Bitmask
- `0x01`: `Time` is present