[comment]: # (Comments in this Markdown file are based on this: https://stackoverflow.com/questions/4823468/comments-in-markdown)

This wiki shows developers how to use the Cassia RESTful API to integrate their Bluetooth devices with the Cassia IoT Access Controller (AC) and the Cassia Bluetooth routers.

[comment]: # (Change the release date after this guide is completed.)
Release date: Nov 12th, 2018

## Table of Contents

* #### Overview
   * Two Set of RESTful APIs
   * Architecture Diagram
   * Server Sent Events

* #### Getting Started
   * Access Local Router
   * Access Cassia Router through the Cassia AC

* #### RESTful API
   * Common Parameters
   * Management APIs
      * Obtain Cassia Router’s Configuration
      * Obtain Cassia Router’s Status
      * Monitor Cassia Router’s Status
      * Obtain All Online Routers’ Status
      * Reboot a Router Remotely
   * Traffic Related APIs
      * Scan Bluetooth Devices
      * Filter Scanned Data based on Device MAC, RSSI, Name, and UUID
      * Connect/Disconnect to a Target Device
      * Discover GATT Services and Characteristics
      * Read/Write the Value of a Specific Characteristic
      * Get Advertise Data
      * Get Device Connection Status
      * Receive Notification and Indication
   * Positioning APIs
   * Secure Pairing APIs
      * Pair Request
      * Pair-Input Request
      * Unpair Request
      * Just Works Example
      * Passkey Entry Example: Initiator Inputs
      * LE Legacy Pairing OOB Example
   * Router Auto-Selection APIs
      * Router Auto-Selection
      * Connect a Device
      * Disconnect a Device
   * SSE Combination APIs
      * Create Combined SSE
      * Open Scan
      * Close Scan
      * Open Notify
      * Close Notify
      * Open Connection-State Report
      * Close Connection-State Report
      * Open AP-State Report
      * Close AP-State Report

* #### Bluetooth Debug Tool
* #### Error Messages
* #### Appendix
   * Migrate from C1000-2B Firmware to X1000
   * Sample Code to Get Access Token
