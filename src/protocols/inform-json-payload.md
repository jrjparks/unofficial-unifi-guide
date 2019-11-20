# Inform Protocol JSON Payload

\# TODO: Write about the inform JSON payload content, there is a lot here...

### Table of Contents
- [Command Payloads](#command-payloads)

## Command Payloads
There are six command types at the moment.

| Name | Type |
|---|---|
| _type | String: [Inform Command Type](#inform-command-type) |
| use_alert | bool |
| device_id | String |
| datetime | String |
| time | u64 |
| cmd | String |
| [mgmt_cfg](../config/management.md) | String |
| [system_cfg](../config/system.md) | String |
| md5sum | String |
| url | String |
| version | String |
| interval | u64 |
| server_time_in_utc | String |

### Inform Command Type

| Type | Description |
|---|---|
| noop | Do nothing |
| setparam | Update a config ([mgmt_cfg](../config/management.md) / [system_cfg](../config/system.md)) |
| upgrade | Upgrade the device using the firmware file located at `url` |
| reboot | Reboot the device |
| cmd | Run a `cmd` |
| setdefault | Factory reset the device |
