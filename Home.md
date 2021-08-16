# Cassia SDK Implementation Guide

[comment]: # (Comments in this Markdown file are based on this: https://stackoverflow.com/questions/4823468/comments-in-markdown)

This guide shows developers how to use the Cassia RESTful API to integrate their Bluetooth devices with the Cassia IoT Access Controller (AC) and the Cassia Bluetooth routers.

---
**NEWS**:

v2.0 has been launched! Here are some new API features:
* [Enhanced Scan Filter](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#enhanced-scan-filter-v20-and-above) (v2.0 and above)
* [RSSI API for Current BLE Connection RSSI](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#get-rssi-for-ble-connection-v20-and-above) (v2.0 and above)
* [Numeric Comparison Pairing](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#numeric-comparison-example) (v2.0 and above)
* [Security OOB Pairing](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#security-oob-example) (v2.0 and above)
* [Batch Connection API for Faster Mulitple Connection Requests](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#batch-connectdisconnect-to-a-target-device-v20-and-above) (v2.0 and above)
---

<br>

The SDK guide is updated time to time to reflect the latest features. Please view the GitHub Wiki page revisions or contact [Cassia Support](https://www.cassianetworks.com/support/) if you have any questions.

If you would like to verify how the SDK API works, please use the [Cassia Bluetooth Debug Tool](http://www.bluetooth.tech/debugger).

For more information about the Cassia Bluetooth Debug Tool, please visit:
<br>[Bluetooth Debug Tool wiki page](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Bluetooth-Debug-Tool).

We are looking for any edit suggestions or corrections. Please contact us at: <br />
[https://www.cassianetworks.com/support](https://www.cassianetworks.com/support/)

## Table of Contents

__[Home](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki)__
<details><summary><strong>Overview</strong></summary>

   * [Cassia Router Overview](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Cassia-Router-Overview)
   * [Two Set of RESTful APIs](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Cassia-Router-Overview#two-sets-of-restful-apis)
   * [Architecture Diagram](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Cassia-Router-Overview#architecture-diagram)
   * [Server-Sent Events](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Cassia-Router-Overview#server-sent-events)

</details>
<details><summary><strong>Getting Started</strong></summary>

* [How to Get Started](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Getting-Started)
* [Access Local Router](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Getting-Started#access-local-router)
* [Access Cassia Router through the Cassia AC](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Getting-Started#access-cassia-router-through-the-cassia-ac)

</details>
<details><summary><strong>RESTful API</strong></summary>

   * <div><a href="https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API">Overview of RESTful API</a></div>
   * <div><a href="https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#common-parameters">Common Parameters</a></div>
   * <details><summary><strong>Management API</strong></summary>

     * [Obtain Cassia Router’s Configuration](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#obtain-cassia-routers-configuration)
     * [Obtain Cassia Router’s Status (Through AC)](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#obtain-cassia-routers-status-through-ac)
     * [Monitor Cassia Router’s Status (Through AC)](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#monitor-cassia-routers-status-through-ac)
     * [Obtain All Online Routers’ Status (Through AC)](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#obtain-all-online-routers-status-through-ac)
     * [Reboot a Router Remotely](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#reboot-a-router-remotely)
     </details>
   
   * <details><summary><strong>Traffic Related API</strong></summary>

     * [Scan Bluetooth Devices](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices)
     * [Filter Scanned Data based on Device MAC, RSSI, Name, and UUID](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#filter-scanned-data-based-on-device-mac-rssi-name-and-uuid)
     * [Enhanced Scan Filter](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#enhanced-scan-filter-v20-and-above) (v2.0 and above)
     * [Connect/Disconnect to a Target Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
     * [Discover GATT Services and Characteristics](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-gatt-services-and-characteristics)
     * [Read/Write the Value of a Specific Characteristic](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
     * [Send Advertise Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#Send-advertise-data)
     * [Get Device Connection Status](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#get-device-connection-status)
     * [Receive Notification and Indication](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
     * [Get RSSI for BLE Connection](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#get-rssi-for-ble-connection-v20-and-above) (v2.0 and above)
     </details>
   * <div><a href="https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#positioning-api">Positioning API</a></div>
   * <details><summary><strong>Secure Pairing API</strong></summary>

     * [Overview of Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api)
     * [Pair Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#pair-request)
     * [Pair-Input Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#pair-input-request)
     * [Unpair Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#unpair-request)
     * [Just Works Example](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#just-works-example)
     * [Passkey Entry Example: Initiator Inputs](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#passkey-entry-example-initiator-inputs)
     * [LE Legacy Pairing OOB Example](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#le-legacy-pairing-oob-example)
     * [Numeric Comparison Example](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#numeric-comparison-example) (v2.0 and above)
     * [Security OOB Example](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#security-oob-example) (v2.0 and above)
     </details>
   * <details><summary><strong>Router Auto-Selection API</strong></summary>
   
     * [Overview of Router Auto-Selection API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#router-auto-selection-api)
     * [Router Auto-Selection](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#router-auto-selection)
     * [Connect a Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connect-a-device)
     * [Disconnect a Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#disconnect-a-device)
     </details>
   * <details><summary><strong>SSE Combination API</strong></summary>
   
     * [Overview of SSE Combination API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#sse-combination-api)
     * [Create Combined SSE](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#create-combined-sse)
     * [Open Scan](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#open-scan)
     * [Close Scan](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#close-scan)
     * [Open Notify](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#open-notify)
     * [Close Notify](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#close-notify)
     * [Open Connection-State Report](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#open-connection-state-report)
     * [Close Connection-State Report](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#close-connection-state-report)
     * [Open AP-State Report](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#open-ap-state-report)
     * [Close AP-State Report](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#close-ap-state-report)
     </details>
</details>

__[Bluetooth Debug Tool](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Bluetooth-Debug-Tool)__

__[Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages)__

__[Postman Setup for Cassia RESTful API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Postman-Setup-for-Cassia-RESTful-API)__

__[Getting Started with Mosquitto MQTT Broker](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Getting-Starting-with-Mosquitto-MQTT-Broker)__

<details><summary><strong>Extras</strong></summary>
   
   * [Migrate from C1000-2B Firmware to X1000](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Migrate-from-C1000-2B-Firmware-to-X1000)
   * [Sample Code to Get Access Token](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Sample-Code-to-Get-Access-Token)
   * [Sample Code to Update Access Token](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Sample-Code-to-Update-Access-Token)
</details>