# Inform Protocol

UniFi devices communicate to their controller via HTTP POST as `application/x-binary` containing it's current status every ~10 seconds to the `inform URL`.
The `inform URL` defaults to `http://unifi:8080/inform` but may be set to the controller's IP address.
A controller will respond with either a no-op and an interval for the next inform or command for the device to execute.
A UniFi device will execute commands and send another inform packet.
Requests and responses share the same format encoded in big endian byte order.
When reading, inform packets are decrypted before decompression, inverse is true for writing.
There is currently only one payload version. Payload version 1 is JSON content and is the current version.

## Encryption/Decryption
There are two possible ciphers used to encrypt the inform payload.
Although you can send the controller plain-text, it will not respond to non-encrypted payloads.
`AES-CBC` is used, and is the default, if the `1st-bit` is set on the flag, or `AES-GCM` if the `1st-bit` and `4th-bit` are set.
The Initialization Vector is 16 bytes in length and is sent in the header.
The default encryption `authkey` is the MD5 hash of `ubnt` (`ba86f2bbe107c7c57eb5f2690775c712`).
When decrypting `AES-GCM` the first 40 bytes of the packet is used as the additional authenticated data (AAD) and the last 16 bytes of the payload is the tag.

## Compression/Decompression
There are two possible compression modes used at the moment.
The default method is [ZLib](https://en.wikipedia.org/wiki/Zlib) and uses the `2nd-bit` of the flag.
[Snappy](https://en.wikipedia.org/wiki/Snappy_(compression)) is another possible compression method and uses the `3rd-bit`.

## Packet Structure
| Offset | Size | Name |
|---|---|---|
| 0-3 | u32 | Magic Header (`TNBU` or `1414414933`) |
| 4-7 | u32 | Packet Version |
| 8-13 | \[u8; 6\] | Hardware Address |
| 14-15 | u16 | [Flags](#flags) |
| 16-31 | \[u8; 16\] | Initialization Vector for encryption |
| 32-35 | u32 | Payload Version |
| 36-39 | u32 | Payload Length |
| 40-n | \[u8\] | [Payload](./inform-json-payload.md#inform-protocol-json-payload) |

### Flags
| Value | Bits | Name |
|---|---|---|
| `0x01` | `0b0000000000000001` | Encrypted |
| `0x02` | `0b0000000000000010` | ZLibCompressed |
| `0x04` | `0b0000000000000100` | SnappyCompressed |
| `0x08` | `0b0000000000001000` | EncryptedGCM |
