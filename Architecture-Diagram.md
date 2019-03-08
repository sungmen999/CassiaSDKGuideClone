The Cassia IoT Access Controller (AC) is a powerful IoT network management solution. It
provides RESTful APIs for the business to do data collection, positioning, and security policy
management, enabling the remote control of Cassia Bluetooth routers across the Internet.
You can operate your BLE devices using a set of RESTful APIs, via the Cassia AC and the
Cassia routers. Please see below figure for the Cassia RESTful APIs Working Diagram, using
X1000 as an example.

Figure 4: Cassia RESTful APIs Working Diagram

First, the business application initiates an OAuth authentication request (generated using
developer credentials) to the Cassia AC. Once the authentication succeeds, it will send an
HTTP query to the Cassia AC based on RESTful. Next, the Cassia AC dispatches the query to a
corresponding Bluetooth router via encrypted CAPWAP. The router then executes the query
upon the BLE devices, and passes the result back to the Cassia AC, and then to the business
application.
