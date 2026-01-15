Packet ID: 164
## Description

Packet used to apply knockback to the receving player.
## Structure

| Name           | Size   | Description                               |
| -------------- | ------ | ----------------------------------------- |
| Null check     | Byte   | 0 if the player list is null, 1 otherwise |
| Hit position X | Double | The X component of the hit position       |
| Hit position Y | Double | The Y component of the hit position       |
| Hit position Z | Double | The Z component of the hit position       |
| Knockback X    | Float  | The X component of the velocity change    |
| Knockback Y    | Float  | The Y component of the velocity change    |
| Knockback Z    | Float  | The Z component of the velocity change    |
