<h1 align="center">Bluetooth Device Integration Guide<br> Getting Started by Gateway</h1>

This document briefly introduces the basic usage of Integrating devices through the gateway API through examples. For ease of understanding, we limit the scenario to a debugging scenario where one gateway corresponds to one BLE device.

|Version | Update Description | Update Date |
|-------| ------- | ------- |
|v0.1.0| Basic functions | 2023-07-12 |
|v0.1.1| <ul><li>Add article index</li><li>Adjust Debugger video position</li></ul> | 2023-07-13 |
|v0.1.2| <ul><li>Adjust Debugger section position</li><li>Fix the timing diagram for long connections</li></ul> | 2023-07-17 |

<!--
### Table of Contents

* [1. Bluetooth Debugging Tools](#1-bluetooth-debugging-tools)
* [2. Using the Device](#2-using-the-device)
* [3. Integration Instructions](#3-integration-instructions)
* [4. Device BLE Protocol](#4-device-ble-Protocol)
* [5. Application Scenario Description](#5-application-scenario-description)
* [6. Advertising Data Collection](#6-advertising-data-collection)
  * [6.1 Scanning Device API](#61-scanning-device-api)
  * [6.2 Scanning API Data](#62-Scanning-api-data)
  * [6.3 Data Parsing](#63-data-parsing)
    * [6.3.1 Parsing Instructions](#631-parsing-instructions)
    * [6.3.2 Parsing Code](#632-parsing-code)
  * [6.4 Sample Code](#64-sample-code)
  * [6.5 Sample Video](#65-sample-video)
* [7. Issue Commands to Device](#7-issue-commands-to-device)
  * [7.1 Connect API](#71-connect-api)
  * [7.2 Discover API](#72-discover-api)
  * [7.3 Write API](#73-write-api)
  * [7.4 Disconnect API](#74-disconnect-api)
  * [7.5 Sample Code](#75-sample-code)
  * [7.6 Sample Video](#76-sample-video)
* [8. Long Connection to Retrieve Device Data](#8-long-connection-to-retrieve-device-data)
  * [8.1 Connect API](#81-connect-api)
  * [8.2 Discover API](#82-discover-api)
  * [8.3 Write API](#83-write-api)
  * [8.4 Receive Notification Data](#84-receive-notification-data)
  * [8.5 Sample Code](#85-sample-code)
  * [8.6 Sample Video](#86-sample-video)
* [9. Actual Business Scenarios](#9-actual-business-scenarios)
-->

### 1. Bluetooth Debugging Tools
The HTTP API of the gateway has encapsulated Bluetooth debugging tools to facilitate users to quickly use the gateway API to debug Bluetooth devices via the UI.
- [Bluetooth Debugging Tool Usage](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Bluetooth-Debug-Tool)
- [Bluetooth Tool Link/Download](http://bluetooth.tech/)

https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/fb8f1d1d-4425-428b-ba45-43edb87f9486
<hr style="height: 1px;" />


### 2. Using the Device
|| Master Device (Gateway) | Slave Device (Device) |
|-------| ------- | ------- |
|Device Type| Bluetooth Gateway | Android Phone |
|Application or Product Model| E1000 | [BLE Peripheral Simulator](https://github.com/AcaciaNetworks/ble-test-peripheral-android) |
|Application or Firmware Version	| 2.1.1.2303082218 | [v1.1.3](https://github.com/AcaciaNetworks/ble-test-peripheral-android/releases/tag/v1.3.0) |

### 3. Integration Instructions

BLE device integration, namely operating the following main operations on BLE devices through Cassia's API:

- Discover -> Connect -> Read and Write Data -> Receive Data -> Disconnect
- Fragmentation, data encoding and decoding, encryption and decryption, integrity checking involved in each process need to be implemented according to the specific GATT application protocol of the BLE device

The main API interactions generally involved are as follows:

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/c9367942-59bc-46c4-9993-bcb4835dafaf)


### 4. Slave Device BLE Protocol
https://github.com/AcaciaNetworks/ble-test-peripheral-android

<img src="https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/d7a00598-f706-425d-a088-68747e44d3fa" alt="Image" style="width: 50%;height:50%;">

### 5. Application Scenario Description
Device integration generally falls into the following basic application scenarios, including the commonly used main APIs, which can be quickly familiarized through examples.

|Scenario|Description|Example|
|--|--|--|
|Advertising Data Collection|<ul><li>Parse application data from the device's advertising packet</li></ul>|<ul><li>Heart Rate</li><li>Temperature</li></ul>|
|Issue Commands to Device|<ul><li>Connect to device</li><li>Issue commands to device</li><li>Disconnect</li></ul>|<ul><li>Set Time</li><li>Send Text Messages</li></ul>|
|Short Connection to Retrieve Device Data|<ul><li>Connect to device</li><li>Issue commands to device</li><li>Notification to obtain specified data</li><li>Disconnect</li></ul>|<ul><li>Historical Activity Data</li><li>Historical Sleep Data</li></ul>|
|Long Connection to Retrieve Device Data|<ul><li>Connect to device</li><li>Issue commands to device</li><li>Notification to obtain specified data</li></ul>|<ul><li>Heart Rate Monitoring</li></ul>|
- Short connection and long connection are similar, the following only takes long connection as an example

### 6. Advertising Data Collection

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/d42f44f7-68e1-46f4-9c9a-0037365ea7b0)

#### 6.1 Scanning Device API

- [Scanning API Request (SSE)](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices)

  ```
  # Use the curl command or open the following URL in a browser
  curl -v 'http://10.100.99.117/gap/nodes?event=1&active=1&filter_name=Cassia*'
  ```
  - Data Description
    |Data| Description |
    |-------| ------- |
    |10.100.99.117| Gateway IP Address |
    |Cassia*| <ul><li>Filter devices whose advertising packet name matches Cassia*</li><li>The simulated device advertising name is Cassia Demo APP</li></ul> |

#### 6.2 Scanning API Data
- Advertising Data
    ```
    {
        "bdaddrs": [
            {
                "bdaddr": "72:A2:28:A8:68:68",
                "bdaddrType": "random"
            }
        ],
        "chipId": 0,
        "evtType": 0,
        "rssi": -74,
        "adData": "0201020EFFFFFF1819F99CD17000000000000416372A5107161C2A00000E7E",
        "name": "(unknown)"
    }
    ```
- Scanning Response Data
    ```
    {
        "bdaddrs": [
            {
                "bdaddr": "72:A2:28:A8:68:68",
                "bdaddrType": "random"
            }
        ],
        "scanData": "10094361737369612044656D6F20417070",
        "evtType": 4,
        "rssi": -73,
        "chipId": 0,
        "name": "Cassia Demo App"
    }
    ```
    |Sample Data| Description |
    |-------| ------- |
    |72:A2:28:A8:68:68| Device MAC Address |
    |random| Device MAC Address Type, this parameter will be used when connecting |
    |chipId| Gateway Bluetooth Chip ID used for scanning |
    |evtType| Device Advertising Type |
    |rssi| Advertising Signal Value |
    |adData| Original Advertising Packet Content of the Device |
    |scanData| Original Scanning Response Packet Content of the Device |
    |name| Parsed Device Advertising Packet Name Field, if available |

#### 6.3 Data Parsing
##### 6.3.1 Parsing Instructions
Refer specifically to [APP Advertising Instructions > Services Data](https://github.com/AcaciaNetworks/ble-test-peripheral-android#13-services-data), the data contained in the advertising packet is as follows:

```
0201020EFFFFFF1819F99CD17000000000000416372A5107161C2A00000E7E
```
|Data Type	|Start Index|Type|Endianness | Example|
|--|--|--|--|--|
|Heart Rate|22|uint8|-|<ul><li>Hex: 51</li><li>Type conversion: 0x51</li><li>Heart Rate: 81</li></ul>|
|Temperature|29|uint16|Big-endian|<ul><li>Hex: 0E7E</li><li>Type conversion: 0x0E7E</li><li>Temperature: 37.1</li></ul>|
- The adData/scanData field in Scannig SSE Event Data is a Hex string, which needs to be converted to a byte array.
- According to the device protocol description, parse the data in the advertising packet. The temperature needs to be divided by 100.

##### 6.3.2 Parsing Code
```
# Node.js
let buf = Buffer.from('0201020EFFFFFF1819F99CD17000000000000416372A5107161C2A00000E7E', 'hex');
let heartrate = buf.readUInt8(22);
let temperature = buf.readUInt16BE(29) / 100
```

#### 6.4 Sample Code
```
# Need NodeJS >= v8.0.0
# yarn add eventsource@1.0.7

/**
 * sample for scan BLE devices and parse data
 * use router local API in this example
 * to run the code, you should have a Cassia Router
 */
const EventSource = require('eventsource');
const qs = require('querystring');

/*
 * replace it with your router ip adderss
 * remember switching on local API in setting page
 */
const HOST = 'http://10.100.99.117';

/*
 * scan devices
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices
 * Sever-Sent Event(SSE) is used in scan, connection-state and notify of Cassia RESTful API,
 * SSE spec: https://html.spec.whatwg.org/multipage/server-sent-events.html#the-eventsource-interface
 * API will send ':keep-alive' every 30 seconds in SSE connection for user to check if the connection is active or not.
 * User need to call Cassia RESTful API to reconnect SSE in case that the connection is termincated abnormally, such as keep-alive lost, socket error, network problem, etc.
 * Nodejs library 'eventsource' handle the SSE reconnection automatically. For other lanuages, the reconnection may needs to be handled by users application.
 */
function openScanSse() {
  const query = {
    /*
     * filter devices whose rssi is below -75, and name begins with 'Cassia',
     * there are many other filters, you can find them in document
     * use proper filters can significantly reduce traffic between Router and AC
     */
    filter_rssi: -75,
    filter_name: 'Cassia*',
    /*
     * use active scan, default is passive scan
     * active scan makes devices response with data packet which usually contains device's name
     */
    active: 1
  };
  const url = `${HOST}/gap/nodes?event=1&${qs.encode(query)}`;
  const sse = new EventSource(url);

  sse.on('error', function(error) {
    console.error('open scan sse failed:', error);
  });
  
  /*
   * if scan open successful, it will return like follow:
   * 5.2.1 Data Description
   */
   sse.on('message', function(message) {
    let data = JSON.parse(message.data);

    let adDataHex = data.adData;
    if (!adDataHex) return;
    
    // Parsing broadcast packet data
    let deviceMac = data.bdaddrs[0].bdaddr;    
    let buf = Buffer.from(adDataHex, 'hex');
    let heartrate = buf.readUInt8(22);
    let temperature = buf.readUInt16BE(29) / 100
    console.log({adDataHex, deviceMac, heartrate, temperature});
  });
}

(async () => {
  try {
    openScanSse();
  } catch(ex) {
    console.error('fail:', ex);
  }
})();
```

Running result
```
{ adDataHex: '0201020EFFFFFF1819F99CD17000000000000416372A5407161C2A00000E81',
  deviceMac: '72:A2:28:A8:68:68',
  heartrate: 84,
  temperature: 37.13 }
// ...
```

#### 6.5 Sample Video
https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/0a68e301-1899-4a36-80e9-e266326d9e09

### 7. Sending Instructions to the Device
We use sending messages to the test device as an example, [Alert Notification Service](https://github.com/AcaciaNetworks/ble-test-peripheral-android#24-alert-notification-service)

![WriteCmd](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/54ff45bd-9f9b-4341-9913-83bdcb6bfc8f)

#### 7.1 Connect API
- [Connect API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
    ```
    curl -L -X POST 'http://10.100.99.117/gap/nodes/72:A2:28:A8:68:68/connection' -H 'Content-Type: application/json' --data-raw '{
        "type": "random"
    }'
    ```
    - Field Description
        |Field Name	|Field Description|
        |--|--|
        |10.100.99.117|Gateway IP address|
        |72:A2:28:A8:68:68|Device MAC address<ul><li>Known from [6.2 Scanning API Data](#6.2-Scanning-api-data)</li></ul>|
        |type|Address type<ul><li>random</li><li>Known from [6.2 Scanning API Data](#6.2-Scanning-api-data)</li></ul>|
        |Others|[Connect/Disconnect to a Target Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)|
- [Connect API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
    ```
    OK
    ```
    - Error Code Description
        |Error Code|Description|
        |--|--|
        |OK|Success|
        |Others|[Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages)|
    

#### 7.2 Discover API
- [Discover API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)
    ```
    curl -L -X GET 'http://10.100.99.117/gatt/nodes/72:A2:28:A8:68:68/services/characteristics/descriptors'
    ```
    - Field Description
        |Field Name|Field Description|
        |--|--|
        |72:A2:28:A8:68:68|Device MAC address<ul><li>Known from 6.2 Data Format, Scanning</li></ul>|
        |Others|[Discover API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)|
- [Discover API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)
    - The remaining services are omitted, we only care about the Alert Notification Service UUID here
    ```
    [
        {
            "uuid": "00001811-0000-1000-8000-00805f9b34fb",
            "primary": true,
            "characteristics": [
                {
                    "descriptors": [
                        {
                            "handle": 60,
                            "uuid": "00002a46-0000-1000-8000-00805f9b34fb"
                        },
                        {
                            "handle": 61,
                            "uuid": "00002902-0000-1000-8000-00805f9b34fb"
                        }
                    ],
                    "handle": 60,
                    "properties": 24,
                    "uuid": "00002a46-0000-1000-8000-00805f9b34fb"
                }
            ],
            "handle": 58
        }
    ]
    ```
- Data Description
    |Service Name|Service UUID|Service Handle|
    |--|--|--|
    |Alert Notification Service|00002a46-0000-1000-8000-00805f9b34fb|60|
    
    > - According to the [Alert Notification Service](https://github.com/AcaciaNetworks/ble-test-peripheral-android#24-alert-notification-service)description, we only need to care about the Handle corresponding to its Service UUID (because the current Cassia Write/Read API only supports the handle parameter)
    > - The Handle value here is 60, which is actually obtained from the above API return result
    > - Generally, the Handle value corresponding to the Service UUID of the same type of device is fixed, so it only needs to be obtained once during debugging, and can be directly fixed in the code
    > - If considering the compatibility of different model devices, it can currently be obtained through the Discover API

#### 7.3 Write API
- [Write API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
    ```
    curl -L -X GET 'http://10.100.99.117/gatt/nodes/72:A2:28:A8:68:68/handle/60/value/050148656c6c6f2c2043617373696121'
    ```
    - Field Description
        |Field Name|Field Description|
        |--|--|
        |10.100.99.117|Gateway IP address|
        |72:A2:28:A8:68:68|Peripheral Device MAC address<ul><li>[6.2 Scanning API Data](#6.2-scanning-api-data)</li></ul>|
        |handle|Known from [7.2 Discover API](#7.2-discover-api)|
        |value|[Alert Notification Service](https://github.com/AcaciaNetworks/ble-test-peripheral-android#24-alert-notification-service)<ul><li>050148656c6c6f2c2043617373696121<ul><li>05: categoryId, fixed</li><li>01: Number Of New Alert, fixed</li><li>48656c6c6f2c2043617373696121: utfs, `Hello, Cassia!`</li></ul></li></ul>|
        |Others|[Write API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)|
    - Convert specified string to HEX string
        ```
        # linux bash
        echo -n 'Hello, Cassia!' | xxd -p
        48656c6c6f2c2043617373696121

        # NodeJS
        Buffer.from('Hello, Cassia!').toString('hex')
        ```
- [Write API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
    ```
    OK
    ```
    - Error Code Explanation
        |Error Code|Explanation|
        |--|--|
        |OK|Success|
        |Others|[Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages)|

#### 7.4 Disconnect API
- [Disconnect API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
    ```
    curl -L -X DELETE 'http://10.100.99.117/gap/nodes/72:A2:28:A8:68:68/connection
    ```
    - Field Description
        |Field Name|Field Description|
        |--|--|
        |10.100.99.117|Gateway IP address|
        |72:A2:28:A8:68:68|Peripheral Device MAC address<ul><li>[6.2 Scanning API Data](#6.2-scanning-api-data)</li></ul>|
        |Others|[Connect/Disconnect to a Target Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)|
- [Disconnect API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
    ```
    OK
    ```
    - Error Code Explanation
        |Error Code|Explanation|
        |--|--|
        |OK|Success|
        |Others|[Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages)|

#### 7.5 Sample Code
```
# Need NodeJS >= v8.0.0
# yarn add request

/**
 * connect BLE device, write handle value, disconnect device
 */
const request = require('request');

/*
 * replace it with your router ip adderss
 * remember switching on local API in setting page
 */
const HOST = 'http://10.100.99.117';

/*
 * replace it with your device mac adderss
 */
const DEVICE_MAC = '72:A2:28:A8:68:68';

/*
 * replace it with your device addr type
 */
const DEVICE_ADDR_TYPE = 'random';

/*
 * replace it with your msg
 */
const ALERT_MESSAGE = 'Hello, Cassia!';


const ALERT_SERVICE_UUID = '00002a46-0000-1000-8000-00805f9b34fb';

function req(options) {
  return new Promise((resolve, reject) => {
    request(options, function (error, response) {
      if (error) reject(error);
      else if (response.statusCode !== 200) reject(response.body);
      else resolve(response.body);
    });
  });
}

/*
 * connect one device
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device
 */
function connect(deviceMac, addrType) {
  let options = {
    method: 'POST',
    url: `${HOST}/gap/nodes/${deviceMac}/connection`,
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({timeout: 5000, type: addrType})
  };
  return req(options);
}

/*
 * Read/Write the Value of a Specific Characteristic
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic
 */
function write(deviceMac, handle, value) {
  let options = {
    method: 'GET',
    url: `${HOST}/gatt/nodes/${deviceMac}/handle/${handle}/value/${value}`,
  };
  return req(options);
}

/*
 * Read/Write the Value of a Specific Characteristic
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic
 */
function discover(deviceMac) {
  let options = {
    method: 'GET',
    url: `${HOST}/gatt/nodes/${deviceMac}/services/characteristics/descriptors`,
  };
  return req(options);
}

function getAlertChar(services) {
  for (let index = 0; index < services.length; index++) {
    let service = services[index];
    let characteristics = service.characteristics;
    let alertCharacteristic = characteristics.find(c => c.uuid === ALERT_SERVICE_UUID);
    if (alertCharacteristic) {
      return alertCharacteristic;
    }
  }
  return null;
}

/*
 * disconnect one device
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device
 */
function disconnect(deviceMac) {
  let options = {
    method: 'DELETE',
    url: `${HOST}/gap/nodes/${deviceMac}/connection`,
  };
  return req(options);
}

(async () => {
  try {
    await connect(DEVICE_MAC, DEVICE_ADDR_TYPE);
    console.log('connect device ok:', DEVICE_MAC);

    let servicesJson = await discover(DEVICE_MAC);
    console.log('discover services ok:', servicesJson);
    let services = JSON.parse(servicesJson);
    let alertChar = getAlertChar(services);

    if (alertChar) {
      console.log('found alert char:', JSON.stringify(alertChar));
      let msgHex = Buffer.from(ALERT_MESSAGE).toString('hex');
      msgHex = `0501${msgHex}`;
      await write(DEVICE_MAC, alertChar.handle, msgHex);
      console.log('write alert char ok:', alertChar.handle, msgHex);
    }
    
    await disconnect(DEVICE_MAC);
    console.log('disconnect device ok:', DEVICE_MAC);
  } catch(ex) {
    console.error('fail:', ex);
  }
})();
```

#### 7.6 Sample Video
https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/ee703c76-0303-4632-a086-dd08465de3bd

### 8. Long Connection to Get Device Data
We use the real-time heart rate acquisition of the test device as an example, [Heart Rate Service](https://github.com/AcaciaNetworks/ble-test-peripheral-android#22-heart-rate-service)

![MonitorData](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/b419e79b-a7f4-4fb3-9096-402f5fc7af21)

#### 8.1 Connect API
- [Connect API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
    ```
    curl -L -X POST 'http://10.100.99.117/gap/nodes/72:A2:28:A8:68:68/connection' -H 'Content-Type: application/json' --data-raw '{
        "type": "random"
    }'
    ```
    - Field Description
        |Field Name	|Field Description|
        |--|--|
        |10.100.99.117|Gateway IP address|
        |72:A2:28:A8:68:68|Device MAC address<ul><li>[6.2 Scanning API Data](#6.2-scanning-api-data)</li></ul>|
        |type|Address Type<ul><li>random</li><li>[6.2 Scanning API Data](#6.2-scanning-api-data)</li></ul>|
        |Others|[Connect/Disconnect to a Target Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)|
- [Connect API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
    ```
    OK
    ```
    - Error Code Description
        |Error Code	|Description|
        |--|--|
        |OK|Success|
        |Others|[Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages)|
    

#### 8.2 Discover API
- [Discover API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)
    ```
    curl -L -X GET 'http://10.100.99.117/gatt/nodes/72:A2:28:A8:68:68/services/characteristics/descriptors'
    ```
    - Field Explanation
        |Field Name|Field Explanation|
        |--|--|
        |72:A2:28:A8:68:68|Device MAC address<ul><li>[6.2 Scanning API Data](#6.2-scanning-api-data)</li></ul>|
        |Others|[Discover API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)|
- [Discover API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)
    - Other services are omitted, we only care about the Heart Rate Service UUID (00002a37-0000-1000-8000-00805f9b34fb) here.
    ```
    [
        {
            "uuid": "0000180d-0000-1000-8000-00805f9b34fb",
            "primary": true,
            "characteristics": [
                {
                    "descriptors": [
                        {
                            "handle": 52,
                            "uuid": "00002a37-0000-1000-8000-00805f9b34fb"
                        },
                        {
                            "handle": 53,
                            "uuid": "00002902-0000-1000-8000-00805f9b34fb"
                        }
                    ],
                    "handle": 52,
                    "properties": 16,
                    "uuid": "00002a37-0000-1000-8000-00805f9b34fb"
                }
            ],
            "handle": 50
        }
    ]
    ```
- Data Explanation
    |Service Name|Service UUID	|Service Handle|Data|
    |--|--|--|--|
    |Heart Rate Service|00002a37-0000-1000-8000-00805f9b34fb|52|Actual reported real-time heart rate value|
    |Heart Rate Service CCCD|00002902-0000-1000-8000-00805f9b34fb|53|<ul><li>Write 0100 to control Notification on</li><li>Write 0000 to control Notification off</li></ul>|
    
    > - The UUID of the Client Characteristic Configuration Descriptor (CCCD) is fixed in the Bluetooth specification. Its UUID is `00002902-0000-1000-8000-00805f9b34fb`
    > - Generally, for the same type of device, the Handle value corresponding to its service UUID is fixed, so you only need to get it once during debugging, and it can be fixed in the code.
    > - If considering the compatibility of different model devices, it can currently be obtained through the Discover API. 

#### 8.3 Write API
According to the protocol explained in 7.2, we need to write 0100 to the Heart Rate Service CCCD to control the Notification on.
- [Write API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
    ```
    curl -L -X GET 'http://10.100.99.117/gatt/nodes/72:A2:28:A8:68:68/handle/53/value/0100'
    ```
    - Field Explanation
        |Field Name	|Field Explanation|
        |--|--|
        |10.100.99.117|Gateway IP address|
        |72:A2:28:A8:68:68|Device MAC address<ul><li>[6.2 Scanning API Data](#6.2-scanning-api-data)</li></ul>|
        |handle|7.2 Discover API Heart Rate Service CCCD UUID corresponding Handle value|
        |value|[Heart Rate Service](https://github.com/AcaciaNetworks/ble-test-peripheral-android#heart-rate-service)<ul><li>0100: Turn on notification</li></ul>|
        |Others|[Write API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)|
- [Write API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
    ```
    OK
    ```
    - Error Code Explanation
        |Error Code|Explanation|
        |--|--|
        |OK|Success|
        |Others|[Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages)|

#### 8.4 Receiving Notification Data
- Data Format
    ```
    {
        "value": "08560000",
        "handle": 52,
        "id": "63:B4:C5:46:5C:A2",
        "dataType": "notification"
    }
    ```
    - Field Explanation
        |Field Name|Field Explanation|
        |--|--|
        |value|Notification data|
        |handle|Indicates which business data, here corresponds to Heart Rate|
        |id|Device MAC address|
        |dataType|Indicates data type, notification/indication type|

#### 8.5 Sample Code
```
# Need NodeJS >= v8.0.0
# yarn add eventsource@1.0.7
# yarn add request

/**
 * connect BLE device, open notification, receive notification data
 */
const EventSource = require('eventsource');
const request = require('request');

/*
 * replace it with your router ip adderss
 * remember switching on local API in setting page
 */
const HOST = 'http://10.100.99.117';

/*
 * replace it with your device mac adderss
 */
const DEVICE_MAC = '72:A2:28:A8:68:68';

/*
 * replace it with your device addr type
 */
const DEVICE_ADDR_TYPE = 'random';

const HEART_RATE_UUID = '00002a37-0000-1000-8000-00805f9b34fb';

function req(options) {
  return new Promise((resolve, reject) => {
    request(options, function (error, response) {
      if (error) reject(error);
      else if (response.statusCode !== 200) reject(response.body);
      else resolve(response.body);
    });
  });
}

/*
 * Receive Notification and Indication
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication
 * Sever-Sent Event(SSE) is used in scan, connection-state and notify of Cassia RESTful API,
 * SSE spec: https://html.spec.whatwg.org/multipage/server-sent-events.html#the-eventsource-interface
 * API will send ':keep-alive' every 30 seconds in SSE connection for user to check if the connection is active or not.
 * User need to call Cassia RESTful API to reconnect SSE in case that the connection is termincated abnormally, such as keep-alive lost, socket error, network problem, etc.
 * Nodejs library 'eventsource' handle the SSE reconnection automatically. For other lanuages, the reconnection may needs to be handled by users application.
 */
function openNotifySse() {
  const url = `${HOST}/gatt/nodes`;
  const sse = new EventSource(url);

  sse.on('error', error => {
    console.error('open notify sse failed:', error);
  });
  
  sse.on('message', message => {
    console.log('recevied notify sse message:', message);
    let data = JSON.parse(message.data);
    
    let deviceMac = data.id;
    let valueHex = data.value;
    let valueBuf = Buffer.from(valueHex, 'hex');
    let heartrate = valueBuf.readUInt8(1);
    console.log("heart rate: ", deviceMac, heartrate);
  });
  
  return Promise.resolve(sse);
}

/*
 * connect one device
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device
 */
function connect(deviceMac, addrType) {
  let options = {
    method: 'POST',
    url: `${HOST}/gap/nodes/${deviceMac}/connection`,
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({timeout: 5000, type: addrType, fail_retry_times: 2})
  };
  return req(options);
}

/*
 * Read/Write the Value of a Specific Characteristic
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic
 */
function write(deviceMac, handle, value) {
  let options = {
    method: 'GET',
    url: `${HOST}/gatt/nodes/${deviceMac}/handle/${handle}/value/${value}`,
  };
  return req(options);
}

/*
 * Read/Write the Value of a Specific Characteristic
 * refer: https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic
 */
function discover(deviceMac) {
  let options = {
    method: 'GET',
    url: `${HOST}/gatt/nodes/${deviceMac}/services/characteristics/descriptors`,
  };
  return req(options);
}

function getHandleByUuid(services, uuid, isCCCD=false) {
  for (let index = 0; index < services.length; index++) {
    let service = services[index];
    let characteristics = service.characteristics;
    let characteristic = characteristics.find(c => c.uuid === uuid);
    if (!characteristic) continue;
    if (!isCCCD) return characteristic.handle;
    let descriptors = characteristic.descriptors;
    let descriptor = descriptors.find(d => d.uuid.toLowerCase() === '00002902-0000-1000-8000-00805f9b34fb');
    if (!descriptor) continue;
    return descriptor.handle;
  }
  return 0;
}

(async () => {
  try {
    await openNotifySse();
    console.log('open notify sse ok');

    await connect(DEVICE_MAC, DEVICE_ADDR_TYPE);
    console.log('connect device ok:', DEVICE_MAC);

    let servicesJson = await discover(DEVICE_MAC);
    console.log('discover services ok:', servicesJson);
    let services = JSON.parse(servicesJson);
    let handle = getHandleByUuid(services, HEART_RATE_UUID, true);

    if (!handle) {
      console.log('no heart rate service found');
      await disconnect(DEVICE_MAC);
      console.log('disconnect device ok:', DEVICE_MAC);
      return;
    }

    console.log('found uuid handle ok:', HEART_RATE_UUID, handle);
    let hex = '0100';
    await write(DEVICE_MAC, handle, hex);
    console.log('write alert char ok:', handle, hex);
  } catch(ex) {
    console.error('fail:', ex);
  }
})();
```

#### 8.6 Sample Video
https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/97d32efe-eb7c-4fbc-a926-d4bfa124ab41


### 9.Real Business Scenarios
This document only covers a scenario with one gateway and one BLE device. Actual business scenarios may involve multiple gateways and multiple BLE devices, and the operations involved will be more complex. However, the method of using Cassia's API is consistent, and only needs to be combined and extended for specific business scenarios. Real business scenarios will generally involve the following operations.

|Scenarios|Example|
|--|--|
|Connection Retry|For example, if the device is mobile and the signal is poor, connection fails |
|Reconnection after Disconnection|Maintaining a long connection, such as heart rate monitoring|
|Single Gateway Multiple Device Scenario|Connection speed requirements, concurrent data transmission requirements|
|Multi-Gateway Scenario|Device movement, gateway preference|
|Device Application Protocol|<ul><li>Connection security, handshake, signature, heartbeat required</li><li>Data integrity verification, fragmentation and packaging, encryption and decryption, etc.</li></ul>|

- [Full API Documentation](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API)
- [More Example Codes](https://github.com/CassiaNetworks/CassiaSDKGuide/tree/master/node_examples)