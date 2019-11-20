# Adoption
[Official documentation for UniFi adoption methods](https://help.ubnt.com/hc/en-us/articles/204909754-UniFi-Device-Adoption-Methods-for-Remote-UniFi-Controllers)

## Layer 2 Adoption
UniFi adopts devices within the layer 2 network via an SSH remote command to assign the `authkey` used for [inform encryption](./protocols/inform.md#encryptiondecryption).
The controller will send `/usr/bin/syswrapper.sh set-adopt <inform_url> <authkey>` to the device using the default username and password of `ubnt`:`ubnt`.

## Layer 3 Adoption
Layer 3 adoption works by having a new device reach out with an inform packet to a 'remote' controller over HTTP POST using the default `authkey`.
The new device will show as `Pending Adoption` in the UniFi controller UI until an operator adopts the device.

## Adoption Process
On the first inform packet during adoption the controller will respond with a `setparam` command that will set the device's unique `authkey` as well as other config settings.
The second response will provision the device and contain the device's configuration.