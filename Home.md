This wiki shows developers how to use the Cassia RESTful API to integrate their Bluetooth devices with the Cassia IoT Access Controller (AC) and the Cassia Bluetooth routers.

##Table of Contents
1. Overview
   * Two Set of RESTful APIs
   * Architecture Diagram
   * Server Sent Events
2. Getting Started
   * Access local router
   * Access Cassia router through the Cassia AC
3 RESTful API
   1. Common Parameters
   2. Management APIs
      1. Obtain Cassia router’s configuration
      2. Obtain Cassia router’s status
      3. Monitor Cassia router’s status
      4. Obtain all online routers’ status
      5. Reboot a router remotely
   3. Traffic Related APIs
      1. Scan Bluetooth devices
      2. Filter scanned data based on device MAC, RSSI, name, and UUID
      3. Connect/disconnect to a target device
      4. Discover GATT services and characteristics
      5. Read/write the value of a specific characteristic
      6. Get advertise data
      7. Get device connection status
      8. Receive notification and indication
   4. Positioning APIs
   5. Secure Pairing APIs
      1. Pair Request
      2. Pair-input Request
      3. Un-pair Request
      4. Just Works Example
      5. Passkey Entry Example: Initiator Inputs
      6. LE Legacy Pairing OOB Example
   6. Router Auto-Selection APIs
      1. Router auto-selection
      2. Connect a device
      3. Disconnect a device
   7. SSE Combination APIs
      1. Create combined SSE
      2. Open scan
      3. Close scan
      4. Open notify
      5. Close notify
      6. Open connection-state report
      7. Close connection-state report
      8. Open ap-state report
      9. Close ap-state report
4. Bluetooth Debug Tool
5. Error Messages
Appendix.
   A. Migrate from C1000-2B Firmware to X1000
   B. Sample Code to Get Access Token
