**NOTE**: For Connect and Pair APIs, avoid calling a new API before the previous API call has finished, otherwise the router will respond with a “chip is busy” error.

For HTTP 500 error, the following are the common error codes:

| <img width="1580"><div>Message</div> | <img><div>Description</div> | 
| --------------- |------------------|
| `parameter invalid` | Wrong parameter value, such as chip ID, MAC address or advertise type is wrong. |
| `device not found`   | It is possible that this device is disconnected. A GATT call to query the attribute of a disconnected device will return this error. |
| `memory alloc error` | When the Bluetooth chip does not have enough memory to complete the operation it will return this error. |
| `operation timeout` | Each operation has a time-out value, especially those timeconsuming operations, such as connection. When connecting to a device that does not exist, the operation will timeout after the 20s. |
| `chip is not ready` | This error will be reported when sending commands to the chip fails. |
| `chip is busy` | The Connect and Pair API are mutually exclusive. If calling a connect request before the previous connect request finishes, the system will return "chip is busy". |
| `incorrect mode` | If calling a connection request is called before the previous connection request for the same device is completed.|
| `operation not supported` | Reserved for future use. |
| `need pair operation` | Some devices require an operation for pairing after a successful connection. If a GATT function call happens prior to the pairing, the system will return this error. <br>From v2.0.3 “Need pair operation” will be placed the following 3 error code,<br>"ATT insufficient Authentication",<br>"ATT insufficient Authorization",<br>"ATT insufficient Encryption",<br>Depending on response message from device side, router will reply one of the above error.|
|`ATT insufficient Authentication`|The attribute requires authentication before it can be read or written|
|`ATT insufficient Authorization`|The attribute requires authorization before it can be read or written|
|`ATT insufficient Encryption`|The Encryption Key Size used for encrypting this link is insufficient|
| `no resources` | The Bluetooth chip in Cassia routers can store the pair information up to 10 seconds. If you pair too many devices, the system will report this error. |
| `Service Not Found` | Couldn't find a Characteristic inside a Service. |
| `type not supported` | When Bypass scan was set up, the protocol type you specified is not supported by this firmware. |
| `please set bypass params first` | Bypass mode is enabled, but no bypass parameters have been set. |
| `failure` | An error for all other failures not specified yet. |
| `device not scan`| Added in v2.x, can not scan the device until connection timeout reached |
| `host disconnect`| manually disconnect device, will report in connection-state SSE reason field|
| `authenticate miss key`| crypt data failed using and existing key, will report in connection-state SSE reason field|