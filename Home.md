This wiki shows developers how to use the Cassia RESTful API to integrate their Bluetooth devices with the Cassia IoT Access Controller (AC) and the Cassia Bluetooth routers.

##Table of Contents
1. Overview
   1.1. Two Set of RESTful APIs
   1.2. Architecture Diagram
   1.3. Server Sent Events
2. Getting Started
   2.1. Access local router
   2.2. Access Cassia router through the Cassia AC
3 RESTful API
   3.1. Common Parameters
   3.2. Management APIs
      3.2.1. Obtain Cassia router’s configuration
      3.2.2. Obtain Cassia router’s status
      3.2.3. Monitor Cassia router’s status
      3.2.4. Obtain all online routers’ status
      3.2.5. Reboot a router remotely
   3.3. Traffic Related APIs
      3.3.1. Scan Bluetooth devices
      3.3.2. Filter scanned data based on device MAC, RSSI, name, and UUID
      3.3.3. Connect/disconnect to a target device
      3.3.4. Discover GATT services and characteristics
      3.3.5. Read/write the value of a specific characteristic
      3.3.6. Get advertise data
      3.3.7. Get device connection status
      3.3.8. Receive notification and indication
   3.4. Positioning APIs
   3.5. Secure Pairing APIs
      3.5.1. Pair Request
      3.5.2. Pair-input Request
      3.5.3. Un-pair Request
      3.5.4. Just Works Example
      3.5.5. Passkey Entry Example: Initiator Inputs
      3.5.6. LE Legacy Pairing OOB Example
   3.6. Router Auto-Selection APIs
      3.6.1. Router auto-selection
      3.6.2. Connect a device
      3.6.3. Disconnect a device
   3.7. SSE Combination APIs
      3.7.1. Create combined SSE
      3.7.2. Open scan
      3.7.3. Close scan
      3.7.4. Open notify
      3.7.5. Close notify
      3.7.6. Open connection-state report
      3.7.7. Close connection-state report
      3.7.8. Open ap-state report
      3.7.9. Close ap-state report
4. Bluetooth Debug Tool
5. Error Messages
Appendix.
   A. Migrate from C1000-2B Firmware to X1000
   B. Sample Code to Get Access Token
