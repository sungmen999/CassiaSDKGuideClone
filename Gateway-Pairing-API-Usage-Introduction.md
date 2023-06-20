This document briefly explains the basic usage of the gateway pairing API through examples.

|Version| Update Notes | Update Date |
|-------| ------- | ------- |
|v0.1.0| Basic Features	 | June 6, 2023 |
|v0.1.1| <ul><li>Typo Fix</li><li>Supplement Document Link</li></ul> | June 19, 2023 |

### 1. Devices Used
|| Central (Gateway) | Peripheral (Device) |
|-------| ------- | ------- |
|Device Type| Bluetooth Gateway | Android Phone |
|Application(Model)| E1000 | nRF Connect(GATT Server) |
|Application(Firmware) Version| 2.1.1.2303082218 | v4.26.1 |

### 2. Pairing Method
The pairing method depends to a certain extent on the IO capabilities of the initiator and the responder, as shown in the figure below:
>- In this example, the pairing is initiated from the perspective of the gateway, so the gateway is the Initiator, and the Android phone is the Responder.
>- The default IO capability of the nRF Connect GATT Server on Android is KeyboardDisplay. We only focus on the last row of the table below, that is, the pairing method when the Initiator is DisplayOnly, DisplayYesNo, KeyboardOnly, NoInputNoOutput, and KeyboardDisplay respectively.
>- For more information on pairing methods and processes, please refer to the following link or the Core Specification.<br /><ul><li>[Bluetooth Pairing Part 1: Pairing Feature Exchange](https://www.bluetooth.com/blog/bluetooth-pairing-part-1-pairing-feature-exchange/)</li><li>[Bluetooth Pairing Part 2: Key Generation Methods](https://www.bluetooth.com/blog/bluetooth-pairing-part-2-key-generation-methods/?utm_campaign=developer&utm_source=internal&utm_medium=blog&utm_content=bluetooth-pairing-part-1-pairing-feature-exchange)</li><li>[Bluetooth Pairing Part 3: Low Energy LegacyPairing Passkey Entry](https://www.bluetooth.com/blog/bluetooth-pairing-passkey-entry/)</li><li>[Bluetooth Pairing Part 4: Bluetooth Low EnergySecure Connections](https://www.bluetooth.com/blog/bluetooth-pairing-part-4/)</li></ul>

[![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/ddd6df2e-5c6f-4945-81c8-3dd0b0dd0c6a)](https://www.bluetooth.com/blog/bluetooth-pairing-part-2-key-generation-methods/?utm_campaign=developer&utm_source=internal&utm_medium=blog&utm_content=bluetooth-pairing-part-1-pairing-feature-exchange)

### 3. Configuration of nRF Connect
#### 3.1 Configuring GATT Service
Please import the GATT service used in the example into the nRF Connect GATT Server.

- File Content
  `PairingGattServerDemo.xml`
  ```xml
  <server-configuration name="PairingGattServerDemo">
    <service name="PairingService" uuid="253d1e52-6ae4-413d-b2cf-9d9ea3b55d74">
        <characteristic name="PairingChar" uuid="253d1e52-6ae4-413d-b2cf-9d9ea3b55d75" value="01">
          <descriptor name="PairingDesc" uuid="253d1e52-6ae4-413d-b2cf-9d9ea3b55d76" value="02">
              <permission name="READ"/>
              <permission name="WRITE"/>
          </descriptor>
          <permission name="READ_ENCRYPTED"/>
          <permission name="WRITE_ENCRYPTED"/>
          <property name="READ"/>
          <property name="WRITE"/>
        </characteristic>
    </service>
  </server-configuration>
  ```
- Process Video

https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/152ade9a-d4c7-420d-9663-9b27ec44b344

#### 3.2 Configure and Start Broadcasting
- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/32412b7a-993f-4784-a45d-d39882093540


#### 3.3 Use Gateway API to Scan and Discover Devices
- Simple Timing Diagram

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/f620abae-c427-46ce-9ba1-25a7fd5ba9a2)

- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/47dc40ec-4866-47a4-92e1-454020b3ffae

- Scanning API Request(SSE)
  ```
  # Use the curl command or open the following URL in a browser
  curl -v 'http://192.168.1.222/gap/nodes?event=1&active=1&filter_name=PairingDemo'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |filter_name| Filter packets with the name "PairingDemo" in the broadcast packet or scan response packet |
    |Other fields	| [Cassia RESTful API - Scan Bluetooth Devices](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#scan-bluetooth-devices) |

### 4. Pairing Process
Next, we will explain the pairing process in turn when the Responder (nRF Connect) IO capability is KeyboardDisplay, and the Initiator has various IO capabilities.
|| DisplayOnly | DisplayYesNo | KeyboardOnly | NoInputNoOutput | KeyboardDisplay |
|-------| ------- | ------- | ------- | ------- |------- |
|Pairing Method	| Passkey Entry<ul><li>Initiator displays</li><li>Responder inputs</li></ul> | Numberic Comparison | Passkey Entry<ul><li>Responder displays</li><li>Initiator inputs</li></ul> | Just Works | Numberic Comparison |

>- <span style="color: red">In this example, please check in advance whether the Android phone retains the pairing information of the gateway during each operation of the pairing process. If so, please manually clear it to prevent affecting the subsequent process</span>
> - For ease of understanding, this example mainly provides a simple explanation of the pairing process using the gateway API and nRF Connect, and does not involve the complete Pairing interaction details of the BLE underlying layer. If necessary, please refer to the Core Specification.
> - To facilitate the demonstration of pairing, the bond parameter of the pairing API is used as 0. Please adjust according to the specific situation when using in actual scenarios.
> - Each time you operate, please use the scanning API to confirm the device MAC address. The address used by Android is of the random type and may change.
> - Each time you operate, check the broadcast name in nRF Connect Devices ADVERTISER. If it has changed, you need to reset it to PairingDemo and rebroadcast.

#### 4.1 Initiator DisplayOnly
- Simple Timing Diagram

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/ecaa243b-04d1-49c4-a81d-0b0aa2eb1c11)

- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/37f23276-98c2-452d-b68f-4fd17e3220ac

- Pairing API Request
  ```
  # InitiatorDisplayOnly.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/49:92:4A:D5:7E:AB/pair/' -H 'Content-Type: application/json' --data-raw '{
      "type": "random",
      "iocapability": "DisplayOnly",
      "timeout": 20000,
      "bond": 0
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |49:92:4A:D5:7E:AB| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pairing API Response
  ```
    {
      "display": "201860",
      "pairingStatus": "Passkey Display Expected",
      "pairingStatusCode": 6
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

#### 4.2 Initiator DisplayYesNo
- Simple Timing Diagram

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/5e56377c-3e04-48d6-a37c-c12174f21e0f)

- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/73188453-232a-4ce3-988d-5c6a3083d261

- Pair API Request
  ```
  # InitiatorDisplayYesNo_Pair.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/49:92:4A:D5:7E:AB/pair/' -H 'Content-Type: application/json' --data-raw '{
      "type": "random",
      "iocapability": "DisplayYesNo",
      "timeout": 20000,
      "bond": 0
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |49:92:4A:D5:7E:AB| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pair API Response
  ```
    {
      "display": "226604",
      "pairingStatus": "Numeric Comparison Expected",
      "pairingStatusCode": 7
    }

  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |


- Pair-Input API Request
  ```
  # InitiatorDisplayYesNo_PairInput.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/49:92:4A:D5:7E:AB/pair-input/' -H 'Content-Type: application/json' --data-raw '{
      "passkey": "1"
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |49:92:4A:D5:7E:AB| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pairing API Response
  ```
    {
      "pairingStatus": "pairingStatus",
      "pairingStatusCode": 1
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

#### 4.3 Initiator KeyboardOnly
- Simple Timing Diagram

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/e1153781-03c9-4611-a881-12aeb450673c)

- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/63ae8b75-0422-48c2-b22f-b938fb21612d

- Pair API Request
  ```
  # InitiatorKeyboardOnly_Pair.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/6E:59:A7:F7:8F:D4/pair/' -H 'Content-Type: application/json' --data-raw '{
      "type": "random",
      "iocapability": "KeyboardOnly",
      "timeout": 20000,
      "bond": 0
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |6E:59:A7:F7:8F:D4| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pair API Response
  ```
    {
      "pairingStatus": "Passkey Input Expected",
      "pairingStatusCode": 5
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |


- Pair-Input API Request
  ```
  # InitiatorKeyboardOnly_PairInput.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/6E:59:A7:F7:8F:D4/pair-input/' -H 'Content-Type: application/json' --data-raw '{
      "passkey": "006762"
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |6E:59:A7:F7:8F:D4| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pairing API Response
  ```
    {
      "pairingStatus": "pairingStatus",
      "pairingStatusCode": 1
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

#### 4.4 Initiator NoInputNoOutput
- Simple Timing Diagram

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/11525645-c2bb-427c-b4ea-338d42ef529f)

- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/97ae2669-483d-4748-bf77-3484938fd53a

- Pair API Request
  ```
  # InitiatorNoInputNoOutput.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/47:BB:AC:A6:88:8C/pair/' -H 'Content-Type: application/json' --data-raw '{
      "type": "random",
      "iocapability": "NoInputNoOutput",
      "timeout": 20000,
      "bond": 0
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |47:BB:AC:A6:88:8C| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pairing API Response
  ```
    {
      "pairingStatus": "pairingStatus",
      "pairingStatusCode": 1
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

#### 4.5 Initiator KeyboardDisplay
- Simple Timing Diagram

![](https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/57e4de02-8257-4543-8bb4-8324376add62)

- Process Video

  https://github.com/CassiaNetworks/CassiaSDKGuide/assets/49749395/30d6c945-6108-4ca8-9ad9-c6eeaf8fbd19

- Pair API Request
  ```
  # InitiatorKeyboardDisplay_Pair.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/47:BB:AC:A6:88:8C/pair/' -H 'Content-Type: application/json' --data-raw '{
      "type": "random",
      "iocapability": "KeyboardDisplay",
      "timeout": 20000,
      "bond": 0
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |47:BB:AC:A6:88:8C| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pair API Response
  ```
    {
      "display": "132056",
      "pairingStatus": "Numeric Comparison Expected",
      "pairingStatusCode": 7
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |


- Pair-Input API Request
  ```
  # InitiatorKeyboardDisplay_PairInput.sh
  set -x
  curl -L -X POST 'http://192.168.1.222/management/nodes/49:92:4A:D5:7E:AB/pair-input/' -H 'Content-Type: application/json' --data-raw '{
      "passkey": "1"
  }'
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |192.168.1.222| Gateway IP Address |
    |49:92:4A:D5:7E:AB| Device MAC Address |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |

- Pairing API Response
  ```
    {
      "pairingStatus": "pairingStatus",
      "pairingStatusCode": 1
    }
  ```
  - Field Description
    |Field Name| Field Description |
    |-------| ------- |
    |Other fields| [Cassia RESTful API - Secure Pairing API](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#secure-pairing-api) |