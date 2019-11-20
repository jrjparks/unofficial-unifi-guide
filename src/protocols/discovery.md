# Discovery Protocol

A factory reset UniFi device will send discovery packets as multicast to `233.89.188.1`.
The packets are based on [Type-Length-Value (TLV)](https://en.wikipedia.org/wiki/Type-length-value).
The value is a list of TLVs.

#### Packet Structure
| Offset | Size | Name |
|---|---|---|
| 0 | u8 | Packet version |
| 1 | u8 | Type |
| 2-3 | u16 | Length |
| 4 | \[u8\] | Value |

There are currently two (maybe three?) packet versions. At the moment this document is only covering version 2.

#### Packet Value Types
| Type | Name | Content |
|---|---|---|
| `0x01` | HardwareAddress | \[u8; 6\] |
| `0x02` | IpInfo | HardwareAddress(\[u8; 6\]), IPv4(\[u8; 4\]) |
| `0x03` | FirmwareVersion | String |
| `0x04` | _Unknown_ | _Unknown_ |
| `0x05` | _Unknown_ | _Unknown_ |
| `0x06` | Username | String |
| `0x07` | Salt | \[u8\] |
| `0x08` | Challenge | \[u8\] |
| `0x09` | _Unknown_ | _Unknown_ |
| `0x0A` | Uptime | i64 |
| `0x0B` | Hostname | String |
| `0x0C` | Platform | String |
| `0x0D` | ESSID | String |
| `0x0E` | WMode | i32 |
| `0x0F` | _Unknown_ | _Unknown_ |
| `0x10` | _Unknown_ | String |
| `0x11` | _Unknown_ | _Unknown_ |
| `0x12` | Sequence * | i32 |
| `0x13` | Serial ** | String |
| `0x14` | _Unknown_ | _Unknown_ |
| `0x15` | Model | String |
| `0x16` | MinimumControllerVersion | String |
| `0x17` | IsDefault | bool |
| `0x18` | _Unknown_ | bool |
| `0x19` | _Unknown_ | bool |
| `0x1A` | _Unknown_ | bool |
| `0x1B` | Version | String |
| `0x1C` | _Unknown_ | i32 |
| `0x1D` | _Unknown_ | String |

\* _Sequence is an incrementing number_ \
\** _Serial is usually the hardware address without any separators_
