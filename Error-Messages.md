For HTTP 500 error, the following are the common error messages:

| <img width="1650"><div>Message</div> | <img><div>Description</div> | 
| --------------- |------------------|
| `parameter invalid` | Wrong parameter value, such as chip ID, MAC address or advertise type is wrong. |
| `device not found`   | It is possible that this device is disconnected. A GATT call to query the attribute of a disconnected device will return this error. |
| `memory alloc error` | When the Bluetooth chip does not have enough memory to complete the operation it will return this error. |
| `operation timeout` | Each operation has a time-out value, especially those timeconsuming operations, such as connection. When connecting to a device that does not exist, the operation will timeout after the 20s. |
| `chip is not ready` | This error will be reported when sending commands to the chip fails. |
| `chip is busy` | All the GATT commands are mutually exclusive. The continuous call will return this busy error. For example, when calling a connect request before the previous connect request finished, the system will return "chip is busy". |
| `incorrect mode` | Our S Series only supports one role, either master or slave (due to the memory limit). These two roles are different, mainly reflected in the broadcast and scanning. When the router is a slave, it cannot conduct scanning; when the router is the master, it cannot send advertise which is connectable. If you set the unsupported parameters to the chip, the system will return this error. |
| `device not connect` | Same as "device not found" error. |
| `operation not supported` | Reserved for future use. |
| `need pair operation` | Some devices require an operation for pairing after a successful connection. If a GATT function call happens prior to the pairing, the system will return this error. |
| `no resources` | The Bluetooth chip in Cassia routers can store the pair information up to 10 seconds. If you pair too many devices, the system will report this error. |
| `Service Not Found` | Couldn't find a Characteristic inside a Service. |
| `type not supported` | When Bypass scan was set up, the protocol type you specified is not supported by this firmware. |
| `please set bypass params first` | Bypass mode is enabled, but no bypass parameters have been set. |
| `failure` | An error for all other failures not specified yet. |