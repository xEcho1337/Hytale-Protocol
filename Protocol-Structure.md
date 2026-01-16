# Protocol Structure

The Hytale protocol is a framed binary protocol implemented using Netty as the networking framework and transported over **QUIC** as the underlying transport layer.

Each packet is serialized into a binary payload buffer, optionally compressed, and then wrapped inside a fixed frame structure before being sent over the network.
## Netty Pipeline

Hytale uses Netty to manage connections and packet processing. The following pipeline represents the logical order of handlers involved in packet transmission and reception:

| Name                 | Class                  | Description                                                    | Optional |
| -------------------- | ---------------------- | -------------------------------------------------------------- | -------- |
| `timeOut`            | `ReadTimeoutHandler`   | Disconnects the client if no data is received within a timeout | No       |
| `packetDecoder`      | `PacketDecoder`        | Reads framed packets and invokes protocol decoding             | No       |
| `rateLimit`          | `RateLimitHandler`     | Applies rate limiting to incoming packets                      | Yes      |
| `packetEncoder`      | `PacketEncoder`        | Encodes packets into framed binary form                        | No       |
| `packetArrayEncoder` | `PacketArrayEncoder`   | Encodes arrays or batches of packets into sequential frames    | No       |
| `logger`             | `LoggingHandler`       | Logs packet-related activity                                   | Yes      |
| `handler`            | `PlayerChannelHandler` | Core packet handling and dispatch logic                        | No       |
| `N/A`                | `ExceptionHandler`     | Centralized exception handling                                 | No       |
## Packet Framing

Each packet transmitted over the wire is encoded using the following structure:

| Name           | Size    | Byte Order    | Description                                           |
| -------------- | ------- | ------------- | ----------------------------------------------------- |
| `Frame length` | 4 Bytes | Little Endian | Length of the payload section in bytes                |
| `Packet ID`    | 4 Bytes | Little Endian | Identifier of the packet type                         |
| `Payload`      | N Bytes | N/A           | Serialized (and optionally compressed) packet payload |
> [!info] **Note**
> Frame Length specifies the size of the payload only.
> The Frame Length **does not include** the length field itself and the packet ID field.

## Null-check Bitmask

In the majority of packets, you might find as the first bit a **null-check byte**. It's a bitmask used to determinate whether an optional field is present or not.
## Compression & Size constrains

Compression is packet-specific and determined by the packet registry. Hytale uses Zstd to compress packets.
### Compression rules

A packet payload is compressed if and only if:
- the packet is marked as compressed in the registry    
- the serialized payload size is greater than zero
### Size limits

- Maximum uncompressed payload size: **defined per packet**
- Maximum compressed payload size: `1,677,721,600` bytes
- Decompressed payload size is validated against `info.maxSize()`

The compression level is configurable via the JVM property:

```
hytale.protocol.compressionLevel
```

If not specified, `Zstd.defaultCompressionLevel()` is used.
## Data types

All multi-byte numeric primitives are encoded using **little-endian byte order**, unless explicitly stated otherwise.

| Type        | Size     | Notes                                  |
| ----------- | -------- | -------------------------------------- |
| Byte        | 1        | Unsigned semantics vary by context     |
| Int (LE)    | 4        | Little-endian                          |
| Float (LE)  | 4        | IEEE 754                               |
| Half (LE)   | 2        | IEEE 754 half-precision                |
| UUID        | 16       | Two consecutive longs                  |
| VarInt      | 1–5      | Variable-length integer                |
| Fixed ASCII | Variable | Fixed length ASCII string, null-padded |
| Fixed UTF-8 | Variable | Fixed length UTF-8 string, null-padded |
| Var ASCII   | Variable | VarInt length + ASCII bytes            |
| Var UTF-8   | Variable | VarInt length + UTF-8 bytes            |
