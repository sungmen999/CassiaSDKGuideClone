The Cassia Bluetooth Router is the world’s first long-range Bluetooth router designed for
enterprise deployments, enabling seamless coverage of any size and scale. It extends
Bluetooth range up to 1,000 feet (open space, line-of-sight), and enables remote control of
multiple Bluetooth Low Energy (BLE) devices without requiring any changes to the Bluetooth
devices.

The Cassia RESTful APIs were developed to enable third-party developers and device
manufacturers to utilize the Bluetooth routing and extended range capabilities of the Cassia
router while using their Cloud services to connect and control multiple BLE devices per
router simultaneously. Furthermore, the Cassia RESTful APIs are designed to integrate
directly into your application and server using an HTTP/HTTPS-based communication
protocol, which provides programming language flexibility (C#, Node.js, Java, and any other
languages you prefer). This document helps you to use the Cassia RESTful APIs and its
associated services.

The Cassia RESTful APIs provide the following functions:
  * Connect and control your BLE devices.
  * Support three modes: Scanning, Connecting, Broadcasting.
  * Write/read data to/from the BLE device.
  * Read data as notification/indication events from the BLE device.

### [Two Sets of RESTful APIs](#two-sets-of-restful-apis)
Cassia provides two sets of RESTful APIs that enable BLE device interaction with Cassia
routers:
  * APIs on the local router (where the application is usually on the same network as the
router)
  * APIs through Cassia’s IoT Access Controller (Cassia AC).
The below APIs are only available through the Cassia AC. Except for these APIs, the two set
of RESTful APIs are the same and will give the same result. In this document, we use the API
through the Cassia AC as an example.
  * Positioning APIs (chapter 6.4)
  * Obtain Cassia router’s status (chapter 6.2.2)
  * Monitor Cassia router’s status APIs (chapter 6.2.3)
  * Obtain all online routers’ status (chapter 6.2.4)
  * Router auto-selection (chapter 6.6, introduced in firmware 1.3)
  * SSE Combination (chapter 6.7, introduced in firmware 1.3)

**NOTE**:
  * The RESTful APIs through Cassia AC includes “/api” after {your AC domain}. It is not
needed for the RESTful APIs on the local router.
  * The RESTful APIs through Cassia AC includes “mac=<mac>” to identify which router is
used. It is not needed for the RESTful APIs on the local router.
  * For firmware 1.2 or below, if you want to use RESTful APIs on the local routers, you
need to turn on Local RESTful API in AC console or router console. Please see below
screenshots.

<br />

![Figure 1](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f1.png)

**Figure 1: (v1.2) Turn on Local RESTful API in AC Console**

<br />

![Figure 2](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f2.png)

**Figure 2: (v1.2) Turn on Local RESTful API in Router Console**

<br />

In firmware 1.3, if the router is configured as Standalone Mode, the local RESTful API
will be automatically turned on. If the router is configured as AC Managed Mode, the
local RESTful API will be turned off by default (customer can enable the local RESTful
API from AC console). Please see below screenshots.

<br />

![Figure 3](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f3.png)

**Figure 3: (v1.3) Configuration of Router Mode on Router Console**

<br />

### [Architecture Diagram](#architecture-diagram)
The Cassia IoT Access Controller (AC) is a powerful IoT network management solution. It
provides RESTful APIs for the business to do data collection, positioning, and security policy
management, enabling the remote control of Cassia Bluetooth routers across the Internet.

You can operate your BLE devices using a set of RESTful APIs, via the Cassia AC and the
Cassia routers. Please see below figure for the Cassia RESTful APIs Working Diagram, using
X1000 as an example.

<br />

![Figure 4](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f4.png)

**Figure 4: Cassia RESTful APIs Working Diagram**

<br />

First, the business application initiates an OAuth authentication request (generated using
developer credentials) to the Cassia AC. Once the authentication succeeds, it will send an
HTTP query to the Cassia AC based on RESTful. Next, the Cassia AC dispatches the query to a
corresponding Bluetooth router via encrypted CAPWAP. The router then executes the query
upon the BLE devices, and passes the result back to the Cassia AC, and then to the business
application.

### [Server Sent Events](#server-sent-events)
SSE is a technology where a browser receives automatic updates from a server via an HTTP
connection. The SSE API is standardized as a part of HTML5 by the W3C. SSE is used to send
message updates or continuous data streams to a browser client. It needs to be manually
terminated, otherwise, it will keep on running until an error occurs.

Six RESTful APIs are using SSE:
  - [scan](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices)
  - [get device connection status](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#get-device-connection-status)
  - [receive indication and notification](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  - [get RSSI report for BLE connections](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#get-rssi-for-ble-connection-v20-and-above) (firmware v2.0 and above)
  - [monitor Cassia router’s status](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#monitor-cassia-routers-status-through-ac) (through AC)
  - [create combined SSE](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#create-combined-sse) (firmware 1.3 and above)

Each SSE response starts with “data:”. When debugging, you can input the URL of an SSE
into a web browser, then you will see the SSE output from the web browser.

In the program, an SSE request won’t return any data if you call the interface like a normal
HTTP request, because a normal HTTP request only returns output when it finishes. In
addition, when calling an SSE, you should monitor this thread. If it is interrupted by an error
or any unexpected incident, you can restart it.

**NOTE**: If you use tools like CURL for HTTP request, the tool will return data when the HTTP
request ends. However, SSE API which NEVER ends and sends data in a stream, so it will
hang the page. You should add the following snippet (use scan as an example):

#### PHP
```php
if ($stream = fopen($url_for_scan, 'r')) {
  while(($line=fgets($stream))!== false){
    echo $line;
  }
}
```
