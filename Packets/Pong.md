# Pong

Packet ID: 3
Bound: Bidirectional
## Description

Packet used to reply to a [Ping](Ping.md) packet.
## Structure

| Name                | Size       | Description                                                                                       | Optional? |
| ------------------- | ---------- | ------------------------------------------------------------------------------------------------- | --------- |
| `Null-check`        | Byte       | Bitmask indicating which optional fields are present                                              | No        |
| `Id`                | Int        | Unique identifier for this ping packet                                                            | No        |
| `Time`              | Long + Int | Instant which the ping packet was created. If the time is null, this field is filled by 12 zeros. | Yes       |
| `Type`              | Byte       | `PongType`                                                                                        | No        |
| `Packet queue size` | Short      | TODO                                                                                              | No        |
## Null check Bitmask
- `0x01`: `Time` is present
## PongType
- Raw (0): If the packet has been written right after being received
- Direct (1): If the packet has been written and flushed right after being received
- Tick (2): If the packet has been sent during a tick