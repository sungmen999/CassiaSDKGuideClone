This document briefly describes the main process of performing DFU on BLE devices using the Nordic SDK through the BLE gateway's RESTful API.

|Version| Update Notes | Update Date |
|-------| ------- | ------- |
|v0.1.0| Basic Features	 | June 16, 2023 |
|v0.1.1| <ul><li>Typo Fix</li></ul> | June 17, 2023 |

> - This document briefly describes the main process of performing DFU on BLE devices using the Nordic SDK through the BLE gateway's RESTful API.
> - The basic successful main process of DFU has been handled, and no additional processing has been done for the exception process for the time being
> - Specific use may need to be adjusted according to the actual situation of the device, please feedback in time if there are any problems.

|  | Secure DFU | Legacy DFU | 
| ------- | ------- | ------- |
| DFU Control Point | 8EC90001-F315-4F60-9FB8-838830DAEA50 | 00001531-1212-EFDE-1523-785FEABCD123 |
| DFU Packet | 8EC90002-F315-4F60-9FB8-838830DAEA50 | 00001532-1212-EFDE-1523-785FEABCD123 |
| Doc Link | [DFU Protocol](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2) | [Transport layers](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk5.v11.0.0%2Fbledfu_transport.html&cp=9_5_13_4_2_3_4) |

### 1. File/Directory Description
| File/Directory | Description |
| ------- | ------- |
| doc | Example documentation(this wiki) |
| script | [Example script](https://github.com/CassiaNetworks/CassiaSDKGuide/blob/master/doc_attachments/Brief%20Guide%20to%20Secure%20DFU%20via%20Gateway%20RESTful%20API/nordic-dfu_v0.1.0.zip) |
| script/index.js | Main code of the example script |
| script/firmware | Device firmware used by the example script |
| script/log/ok.log | Log of successful execution of the example script |

### 2. Test Devices

|| Central(Gateway) | Peripheral(Device) |
|-------| ------- | ------- |
|Device Type| E1000 | Nordic Development Board |
|Application Version| 2.1.1.2303082218 | [./script/firmware/blinky_1.0.1_dfu_package](https://github.com/CassiaNetworks/CassiaSDKGuide/blob/master/doc_attachments/Brief%20Guide%20to%20Secure%20DFU%20via%20Gateway%20RESTful%20API/nordic-dfu_v0.1.0.zip)<ul><li>blinky_pca10056.dat</li><li>blinky_pca10056.bin</li></ul> |
|Address Information| IP: 10.100.99.117 | <ul><li>MAC: EB:2E:AF:D8:49:DC</li><li>Addr Type: random</li></ul> |

### 3. Device BLE Data
#### 3.1 Advertising Packet
```
{
    "bdaddrs": [
        {
            "bdaddr": "EB:2E:AF:D8:49:DC",
            "bdaddrType": "random"
        }
    ],
    "chipId": 0,
    "evtType": 0,
    "rssi": -30,
    "adData": "020106030259FE080944667554617267",
    "name": "DfuTarg"
}
```
- Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |random| Device address type |
    |name| Device broadcast name |
    |Other fields| [Cassia RESTful API - Scan Bluetooth Devices](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices) |
#### 3.2 Scan Response Packet
```
{
    "bdaddrs": [
        {
            "bdaddr": "EB:2E:AF:D8:49:DC",
            "bdaddrType": "random"
        }
    ],
    "scanData": "",
    "evtType": 4,
    "rssi": -29,
    "chipId": 0,
    "name": "(unknown)"
}
```
#### 3.3 GATT Service And characteristics
```
[
    // Others are omitted ...
    {
        "uuid": "0000fe59-0000-1000-8000-00805f9b34fb",
        "primary": true,
        "characteristics": [
            {
                "descriptors": [
                    {
                        "handle": 13,
                        "uuid": "8ec90002-f315-4f60-9fb8-838830daea50"
                    }
                ],
                "handle": 13,
                "properties": 4,
                "uuid": "8ec90002-f315-4f60-9fb8-838830daea50"
            },
            {
                "descriptors": [
                    {
                        "handle": 15,
                        "uuid": "8ec90001-f315-4f60-9fb8-838830daea50"
                    },
                    {
                        "handle": 16,
                        "uuid": "00002902-0000-1000-8000-00805f9b34fb"
                    }
                ],
                "handle": 15,
                "properties": 24,
                "uuid": "8ec90001-f315-4f60-9fb8-838830daea50"
            }
        ],
        "handle": 11
    }
]
```
- Example data explanation

    |UUID| Handle | Characteristic name | Required properties |
    |-------| ------- | ------- | ------- | 
    |8ec90001-f315-4f60-9fb8-838830daea50| 15 |  DFU Control Point | Write, Notify |
    |00002902-0000-1000-8000-00805f9b34fb| 16 | DFU Control Point CCCD |  |
    |8ec90002-f315-4f60-9fb8-838830daea50| 13 | DFU Packet | WriteWithoutResponse, Notify |


### 4. Script Usage

> - The script is a [NodeJS script](https://github.com/CassiaNetworks/CassiaSDKGuide/blob/master/doc_attachments/Brief%20Guide%20to%20Secure%20DFU%20via%20Gateway%20RESTful%20API/nordic-dfu_v0.1.0.zip). If Node is not installed, please install and use Node v10.0 or higher. For more details, refer to [Nodejs](https://nodejs.org/en/docs/guides/getting-started-guide)
> - If the script is executed normally, it will complete the entire DFU process. If an error occurs midway, it will print error information.
> - [Nordic DFU Protocol](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)
> - [Nordic DFU BLE Transport](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)

```
node index.js -g 10.100.99.117 -m EB:2E:AF:D8:49:DC -d ./firmware/blinky_1.0.1_dfu_package/blinky_pca10056.dat -b ./firmware/blinky_1.0.1_dfu_package/blinky_pca10056.bin -p 244
```
- Parameter Description
    | Parameter Name | Parameter Description |
    |-------| ------- |
    |-h| Usage description |
    |-g| Gateway IP address |
    |-m| Device MAC address |
    |-t| Device address type, <ul><li>public or random, default is random</li><li>Fill in according to the specific device information scanned</li></ul> |
    |-d| Device firmware dat file path |
    |-b| Device firmware bin file path |
    |-p| The size of the package written to the upgrade firmware each time, generally use 244 or 20, please fill in according to the device support situation |

### 5. Overall DFU Process
The following diagram mainly shows several important steps in the overall DFU process, including several important commands during the DFU process and the response to the commands.
> - <span style="color: red">Only the main normal interaction process is displayed. For more specific commands and responses, please refer to the following links:
> - [Nordic DFU Protocol](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)
> - [Nordic DFU BLE Transport](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/1692fe38-ae2b-48b1-aa3b-c0f08e6bf695)


### 6. Detailed Process
#### 6.1 Scan for Devices
Use the gateway API to scan and discover devices

- [Scanning API Request(SSE)](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices)
  ```
  # Use curl command or open the following URL in a browser
  curl -v 'http://10.100.99.117/gap/nodes?event=1&active=1&filter_name=DfuTarg'
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |DfuTarg| Filter broadcast packets with the name `DfuTarg` |

#### 6.2 Open Device Notification Data
- [Notification API Request(SSE)](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  # Use curl command or open the following URL in a browser
  curl -v 'http://10.100.99.117/gatt/nodes?event=1'
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |


#### 6.3 Connect to the Device
- [Connect API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device)
  ```
  curl -L -X POST 'http://10.100.99.117/gap/nodes/EB:2E:AF:D8:49:DC/connection' -H 'Content-Type: application/json' --data-raw '{
      "type": "random"
  }'
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |type| Device MAC address type |

- Connect API Response
  ```
    OK
  ```
#### 6.4 Retrieve Device Services
- [Services And Characteristics API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#discover-all-services-characteristics-and-descriptors-all-at-once)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/services/characteristics/descriptors'
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |

- Services And Characteristics API Response
  ```
  Refer to section 2.3 Service And characteristics
  ```

#### 6.5 Enable DFU Control Point Notification
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/16/value/0100' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 16| DFU Control Point CCCD Handle |
    |0100| Enable DFU Control Point CCCD |

- Write Handle API Response
  ```
    OK
  ```

#### 6.6 DFU-dat File
> - <span style="color: red">Only the main normal interaction process is shown. For more specific commands and responses, please refer to the following link:
> - [Nordic DFU Protocol](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)
> - [Nordic DFU BLE Transport](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/b6d731b0-f5ee-4e39-8872-d095552e4b84)

##### 6.6.1 [Select](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_5#lib_dfu_transport_op_select)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/0601' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |0601| <ul><li>06: [Select Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_5#lib_dfu_transport_op_select)</li><li>01: Object Type: Command</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600601000200000000000000000000",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600601000200000000000000000000| <ul><li>60: DFU Request Response</li><li>06: [Select Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_5#lib_dfu_transport_op_select)</li><li>01: Success</li><li>00200000: max_size</li><li>00000000: offset</li><li>00000000: crc32</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.6.2 [Set Receipt Notification](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_2#lib_dfu_transport_op_receipt_notif_set)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/020000' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |020000| <ul><li>02: [Set Receipt Notification](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_2#lib_dfu_transport_op_receipt_notif_set)</li><li>0000: If set to 0, then the CRC response is never sent after Write request.</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600601000200000000000000000000",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600601000200000000000000000000| <ul><li>60: DFU Request Response</li><li>06: [Set Receipt Notification](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_2#lib_dfu_transport_op_receipt_notif_set)</li><li>01: Success</li><li>00200000: max_size</li><li>00000000: offset</li><li>00000000: crc32</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.6.3 [Create Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_1#lib_dfu_transport_op_create)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/01018c000000' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |01018c000000| <ul><li>01: [Create Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_1#lib_dfu_transport_op_create)</li><li>01: Object Type: Command</li><li>8c000000: dat File Size.(LSB)</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600101",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600101| <ul><li>60: DFU Request Response</li><li>01: [Create Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_1#lib_dfu_transport_op_create)</li><li>01: Success</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.6.4 Write dat File Content
> - <span style="color: red">The file content is split into packets according to the -m parameter, and the parameter is set according to the device support situation.</span>

  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/1289010a430801123f08914e10341a0100200028003000389c0d42240803122090eb5b3217c2e7dc9a53a9dd07d454fdc29d153c56a0145945b07733232f4452480052040801120010001a40415e36ab6e8ec711df8ee89a03d102adae04268bacb46da5f65719ea27b1f82c4f6147f91e904b90805756564209562199f20626fc3391482b29340d34ed3531' 
  ```

##### 6.6.5 [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)
> - <span style="color: red">Note: The -c parameter enables CRC32 check by default. You can set whether to enable it according to specific needs.</span>
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/03' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |03| <ul><li>03: [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "6003018C000000B89584A5",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |6003018C000000B89584A5| <ul><li>60: DFU Request Response</li><li>03: [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)</li><li>01: Success</li><li>8C000000: offset</li><li>B89584A5: CRC32</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.6.6 [Execute Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_4#lib_dfu_transport_op_execute)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/04' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |04| <ul><li>04: [Execute Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_4#lib_dfu_transport_op_execute)</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600401",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600401| <ul><li>60: DFU Request Response</li><li>04: [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)</li><li>01: Success</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |


#### 6.7 DFU bin File

> - <span style="color: red">Only the main normal interaction process is shown here. For more specific commands and responses, please refer to the following link:
> - [Nordic DFU Protocol](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)
> - [Nordic DFU BLE Transport](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_dfu_transport.html)

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/92dfccea-7c16-48bd-a92c-5e93480ac127)

##### 6.7.1 [Select](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_5#lib_dfu_transport_op_select)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/0602' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |0602| <ul><li>06: [Select Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_5#lib_dfu_transport_op_select)</li><li>02: Object Type: Data</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600601001000000000000000000000",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600601001000000000000000000000| <ul><li>60: DFU Request Response</li><li>06: [Select Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_5#lib_dfu_transport_op_select)</li><li>01: Success</li><li>00100000: max_size</li><li>00000000: offset</li><li>00000000: crc32</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.7.2 [Set Receipt Notification](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_2#lib_dfu_transport_op_receipt_notif_set)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/020000' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |020000| <ul><li>02: [Set Receipt Notification](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_2#lib_dfu_transport_op_receipt_notif_set)</li><li>0000: If set to 0, then the CRC response is never sent after Write request.</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600601000200000000000000000000",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600601000200000000000000000000| <ul><li>60: DFU Request Response</li><li>06: [Set Receipt Notification](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_2#lib_dfu_transport_op_receipt_notif_set)</li><li>01: Success</li><li>00200000: max_size</li><li>00000000: offset</li><li>00000000: crc32</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.7.3 [Create Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_1#lib_dfu_transport_op_create)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/01029c060000' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |01029c060000| <ul><li>01: [Create Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_1#lib_dfu_transport_op_create)</li><li>02: Object Type Data</li><li>9c060000: bin File Size.(LSB)</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600101",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600101| <ul><li>60: DFU Request Response</li><li>01: [Create Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_1#lib_dfu_transport_op_create)</li><li>01: Success</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.7.4 Write bin File Content
> - <span style="color: red">The file content is split into packets according to the -m parameter, and the parameter is set according to the device support situation.</span>

  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/00000420757302005d7302005f730200617302006373020065730200000000000000000000000000000000006773020069730200000000006b7302006d7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f73020000000000000000006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302006f7302000000000000000000'
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/6f730200000000006f73020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000'
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/000000000000000000000000000000000000000000000000354936480a1a02d0072291438d46344934480a1a06d00722914381f30988022282f3148830483149314a00f044f831483149324a00f03ff831483249324a00f03af832483249334a00f035f832483349334a00f030f833483349344a00f02bf833483449344a00f026f834483449002200f02cf833483449002200f027f833483349091a082902db002202604160314a90471f481f49884205d00268043003b4904703bcf7e700208646ec4600200021294a9047fee7884207d0521a05d0037801300b700131013af9d17047884202d002700130fae7704700000420'
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/00e003200000042000000420987602000080002004800020d0730200d07302008c7602009876020000800020008000208c7602008c7602008c7602008c7602008c7602008c7602008c7602008c760200967602009c7602000480002004800020048000200480002004800020048000200480002004a000209d7302008d740200fee7fee7fee7fee7fee7fee7fee7fee7fee7fee700f002b8fff7fcbf0f484ff00701884385460e4880470e48016841f470010160bff34f8fbff36f8ffff732bf09480a490a4a884207d0521a05d0037801300b700131013af9d1704700000420d174020088ed00e08c7602000080002000800020'
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/074b1b5c1f2b86bf064903f01f034ff0a041012202fa03f3c1f80835704700bf8c7602000003005008b50020fff7e8ff0120fff7e5ff0220fff7e2ffbde808400320fff7ddbf00000a4b185c1f288abf094b4ff0a04300f01f00d3f804250121814021ea02000a40c3f80805c3f80c25704700bf8c76020000030050c20710b504460cd54ff0a0430322c3f83427c3f83827c3f83c27c3f84027fff7c5ffa3070ad54ff0a0430c22c3f82c27c3f83027c3f86027c3f8642710bd000008b50120074cfff7dbff44f001040020fff7bcff4ff4fa754ff47a40a047013dfad1f4e7907602004ff08053d3f83021082a03bfd3f83401'
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/b0fa80f0400900207047000008b54ff080430022c3f80c21c3f81021c3f838254ff0805203f54043d2f80414c3f82015d2f80814c3f82415d2f80c14c3f82815d2f81014c3f82c15d2f81414c3f83015d2f81814c3f83415d2f81c14c3f84015d2f82014c3f84415d2f82414c3f84815d2f82814c3f84c15d2f82c14c3f85015d2f83014c3f85415d2f83414c3f86015d2f83814c3f86415d2f83c14c3f86815d2f84014c3f86c15d2f84424c3f87025fff79eff18b13b4b3b4ac3f88c26fff797ff18b1394bfb22c3f81825fff790ff70b14ff080414ff08053d1f8e42ed3f8583222f00f0203f00f031343c1f8e43efff77eff'
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/13/value/20b12e4b4ff40072c3f840264ff08043d3f80024d20744bf6ff00102c3f80024274ad2f8883043f47003c2f88830bff34f8fbff36f8f4ff01023d3f80022002a03dbd3f80432002b2eda1e4b0122c3f80425d3f80024002afbd04ff010221221c2f80012d3f80024002afbd04ff010231222c3f80422134bd3f80024002afbd00022c3f80425d3f80024002afbd0bff34f8f0b490c4bca6802f4e0621343cb60bff34f8f00bffde7084b094a1a6008bd005000404881030000f000400090024000ed00e000e001400400fa05008000200090d0030d0e0f100338fdd87047ffff0090d003'
  ```

##### 6.7.5 [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)
> - <span style="color: red">Note: The -c parameter enables CRC32 check by default. You can set whether to enable it according to specific needs.</span>
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/03' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |03| <ul><li>03: [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "6003019C06000060F768A3",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |6003019C06000060F768A3| <ul><li>60: DFU Request Response</li><li>03: [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)</li><li>01: Success</li><li>9C060000: offset</li><li>60F768A3: CRC32</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |

##### 6.7.6 [Execute Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_4#lib_dfu_transport_op_execute)
- [Write Handle API Request](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
  curl -L -X GET 'http://10.100.99.117/gatt/nodes/EB:2E:AF:D8:49:DC/handle/15/value/04' 
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |10.100.99.117| Gateway IP address |
    |EB:2E:AF:D8:49:DC| Device MAC address |
    |handle 15| DFU Control Point Handle |
    |04| <ul><li>04: [Execute Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_4#lib_dfu_transport_op_execute)</li></ul> |

- [Write Handle API Response](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#readwrite-the-value-of-a-specific-characteristic)
  ```
    OK
  ```

- Wait Response in [Notification Data](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#receive-notification-and-indication)
  ```
  {
      "value": "600401",
      "handle": 15,
      "id": "EB:2E:AF:D8:49:DC",
      "dataType": "notification"
  }
  ```
  - Example data explanation
    |Example Data| Example data explanation |
    |-------| ------- |
    |600401| <ul><li>60: DFU Request Response</li><li>04: [CRC Request](https://infocenter.nordicsemi.com/topic/sdk_nrf5_v17.1.0/lib_dfu_transport.html?cp=9_1_3_5_2_0_3#lib_dfu_transport_op_crc)</li><li>01: Success</li></ul> |
    |handle 15| DFU Control Point Handle |
    |EB:2E:AF:D8:49:DC| Device MAC |
