This wiki shows developers how to use the Cassia RESTful API to integrate their Bluetooth devices with the Cassia IoT Access Controller (AC) and the Cassia Bluetooth routers.

## Table of Contents

* #### Overview
   * Two Set of RESTful APIs
   * Architecture Diagram
   * Server Sent Events

* #### Getting Started
   * Access local router
   * Access Cassia router through the Cassia AC

* #### RESTful API
   * Common Parameters
   * Management APIs
      * Obtain Cassia router’s configuration
      * Obtain Cassia router’s status
      * Monitor Cassia router’s status
      * Obtain all online routers’ status
      * Reboot a router remotely
   * Traffic Related APIs
      * Scan Bluetooth devices
      * Filter scanned data based on device MAC, RSSI, name, and UUID
      * Connect/disconnect to a target device
      * Discover GATT services and characteristics
      * Read/write the value of a specific characteristic
      * Get advertise data
      * Get device connection status
      * Receive notification and indication
   * Positioning APIs
   * Secure Pairing APIs
      * Pair Request
      * Pair-input Request
      * Un-pair Request
      * Just Works Example
      * Passkey Entry Example: Initiator Inputs
      * LE Legacy Pairing OOB Example
   * Router Auto-Selection APIs
      * Router auto-selection
      * Connect a device
      * Disconnect a device
   * SSE Combination APIs
      * Create combined SSE
      * Open scan
      * Close scan
      * Open notify
      * Close notify
      * Open connection-state report
      * Close connection-state report
      * Open ap-state report
      * Close ap-state report

* #### Bluetooth Debug Tool
* #### Error Messages
* #### Appendix
   a. Migrate from C1000-2B Firmware to X1000
   b. Sample Code to Get Access Token
