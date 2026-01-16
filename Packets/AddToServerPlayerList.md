# AddToServerPlayerList

Packet ID: 224
Bound: Server -> Client
## Description

Packet used to add multiple players to the player list.

> [!info] Notes
> The maximum amount of players that can be added at once is 4096000
## Structure

| Name       | Size                                               | Description                                       |
| ---------- | -------------------------------------------------- | ------------------------------------------------- |
| Null check | Byte                                               | 0 if the player list is null, 1 otherwise         |
| Length     | Varint                                             | The length of the player list                     |
| Players    | [ServerPlayerListPlayer[]](ServerPlayerListPlayer) | A list of ServerPlayerListPlayer to be serialized |
