Here are common parameters for RESTful API:
  * mac: the mac address of a Cassia router (e.g. CC:1B:E0:E0:24:B4)
  * node: the mac address of a BLE device (e.g. EF:F3:CF:F0:8B:81)
  * handle: after you find the device services, based on the device’s Bluetooth profile, you
can identify its corresponding handle index in the UUID (e.g. 37)
  * value: the value written into the handle
  * chip (optional): 0 or 1, indicates which chip of the Cassia router is used for scan and
connect. By default, the router will pick up the chip automatically based on an internal
algorithm. S Series routers only support chip 0, X1000/E1000/C1000 supports 0 and 1.