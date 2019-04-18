Most of the Bluetooth GAP/GATT operations are exposed in RESTful APIs. The signatures of
those APIs are fully compliant with Bluetooth SIG’s Internet Working Group RESTful API
specification.

## Common Parameters
Here are common parameters for the RESTful API:

| Parameter | Description |
|-----------|-------------|
| `mac` |  The MAC address of a Cassia router (e.g. CC:1B:E0:E0:24:B4). |
| `node` | The MAC address of a BLE device (e.g. EF:F3:CF:F0:8B:81). |
| `handle` | After you find the device services, based on the device’s Bluetooth profile, you can identify its corresponding handle index in the UUID (e.g. 37). |
| `value` | The value written into the handle. |
| `chip` | (Optional) 0 or 1, indicates which chip of the Cassia router is used for scan and connect. By default, the router will pick up the chip automatically based on an internal algorithm. S Series routers only support chip 0, X1000/E1000/C1000 supports 0 and 1. |
<br />

## Management API
### Obtain Cassia Router’s Configuration
You can use the API below to obtain the configuration of a router, including its IP address,
model, version, etc.

AC Managed:
```
GET http://{your AC domain}/api/cassia/info?mac=<hubmac>
```
Local:
```
GET http://{router ip}/cassia/info
```
The return result is a JSON object.
<details><summary><strong>Configuration Response Example</strong></summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "capwap-uplinkmac": "CC:1B:E0:E7:FD:84",
    "cpu": {
        "used": 158,
        "total": 2184
    },
    "mem": {
        "used": 103508,
        "total": 225716
    },
    "capwap-state": "7\n",
    "timeconf": {
        "now": "2018-09-07 09:56:42",
        "ntp2": "stdtime.gov.hk",
        "auto": true,
        "ntp1": "time.nist.gov"
    },
    "timezone": "Asia\/Shanghai",
    "chipinfo": {
        "1": {
            "adv_en": "0",
            "ant": "0",
            "max": 11,
            "scan_type": "0",
            "ver": "7",
            "addr": "CC:EB:E0:19:88:1F",
            "scan_en": "0",
            "status": "Idle",
            "speed": {
                "rx": 0,
                "tx": 0
            },
            "id": "1"
        },
        "0": {
            "adv_en": "0",
            "ant": "0",
            "max": 11,
            "scan_type": "0",
            "ver": "7",
            "addr": "CC:EB:E0:19:88:1E",
            "scan_en": "0",
            "status": "Idle",
            "speed": {
                "rx": 0,
                "tx": 0
            },
            "id": "0"
        }
    },
    "conn_params": {
        "type": 0,
        "latency": 0,
        "conn_max_intval": 30,
        "supvtimeout": 1000,
        "conn_min_intval": 7.5,
        "scan_window": 30,
        "scan_intval": 60
    },
    "mqtt_stat": {
        "e": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        "d": 0,
        "s": 0,
        "f": 0
    },
    "wireless": {
        "proto": "static",
        "dns": "",
        "dns2": "",
        "netmask": "255.255.255.0",
        "ip": "192.168.40.1",
        "password": "cassia-E7FD84",
        "speed": {
            "rx": 0,
            "tx": 0
        },
        "iface": {
            "mac": "CC:1B:E0:E7:FD:85",
            "mtu": "1500",
            "netmask": "255.255.255.0",
            "ip": "192.168.40.1",
            "tx": "428",
            "name": "wlan0",
            "metric": "0",
            "rx": "0",
            "bcast": "192.168.40.255"
        },
        "country": "US",
        "gateway": "",
        "mode": "ap",
        "ssid": "cassia-E7FD84",
        "dns1": ""
    },
    "sshlogin": "1",
    "ble_power": 20,
    "sqs_stat": {
        "e": [0, 0, 0, 0, 0, 0],
        "d": 0,
        "s": 0,
        "f": 0
    },
    "fat": "0",
    "mac": "CC:1B:E0:E7:FD:84",
    "container": {
        "disk": {
            "used": "1.0G",
            "total": "2.3G"
        },
        "kernel": "Ubuntu 16.04.3 LTS\n",
        "iface": {
            "mac": "FE:EB:E0:BE:E0:62",
            "mtu": "1500",
            "tx": "636",
            "name": "vethBU791K",
            "metric": "0",
            "rx": "568"
        },
        "cpu": {
            "used": 205,
            "total": 2204
        },
        "status_code": 3,
        "process": "USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND\nroot 1 0.0 0.4 2612 1056 pts\/0 Ss+ 01:55 0:00 \/bin\/bash\/root\/start.sh\nroot 71 0.0 0.3 7980 848 ? Ss 01:55 0:00 \/usr\/sbin\/sshd\nroot 86 7.1 10.5118496 23852 ? Ssl 01:55 0:04 PM2 v2.10.1: God Daemon (\/root\/.pm2)\nroot 99 0.0 0.6 27081360 pts\/0 S+ 01:55 0:00 \/bin\/bash\nroot 133 0.0 0.4 4740 1124 ? R 01:56 0:00 psaux\n",
        "apps": {},
        "speed": {
            "rx": 0,
            "tx": 0
        },
        "mem": {
            "used": 52350976,
            "total": 134217728
        },
        "status": "running"
    },
    "capwapuplink": "wired",
    "ac": {
        "port": "5246,5247",
        "user": "",
        "control_port": "5246",
        "data_port": "5247",
        "force_network": "1",
        "address": ""
    },
    "capwap-ip": "168.168.20.154\n",
    "https": "0",
    "dongle": {
        "keepalive": "",
        "ifname": "ppp0",
        "dialnumber": "*99#",
        "service": "umts",
        "defaultroute": "",
        "username": "",
        "pincode": "",
        "apn": "3gnet",
        "metric": "5",
        "proto": "3g",
        "dns": "",
        "device": "\/dev\/ttyUSB0",
        "maxwait": "",
        "password": "",
        "ipv6": "",
        "type": "none",
        "demand": "",
        "peerdns": ""
    },
    "scan": {
        "one_scan_time": "300",
        "scan_interval": "15",
        "scan_window": "10"
    },
    "wired": {
        "duplex": "full",
        "proto": "dhcp",
        "speed": {
            "rx": 0.381,
            "tx": 0.2376
        },
        "iface": {
            "mac": "CC:1B:E0:E7:FD:84",
            "mtu": "1500",
            "netmask": "255.255.255.0",
            "ip": "168.168.20.30",
            "tx": "27746",
            "name": "eth0",
            "metric": "1",
            "rx": "31793",
            "bcast": "168.168.20.255"
        },
        "trans_speed": "100"
    },
    "chip-params": "1",
    "version": "1.3.0.1807030130",
    "local-api": "0",
    "start": {
        "bypass": {
            "use": "mqtt"
        }
    },
    "capwap-runtime": 54,
    "uptime": "94",
    "model": "X1000"
}
```

</details>
<br />

### Obtain Cassia Router’s Status (Through AC)
You might use the API below to obtain the status of a router, either online or offline.<br />
**NOTE**: This API is only available through Cassia AC.
```
GET http://{your AC domain}/api/cassia/hubs/<hubmac>
```
The return result is a JSON object.
<details><summary><strong>[Online] Status Response Example</strong></summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "_id": "5a9497eeadc22500524e27e5",
    "id": "CC:1B:E0:E0:DF:80",
    "mac": "CC:1B:E0:E0:DF:80",
    "name": "NewBootloader",
    "group": "SJCLab",
    "status": "online",
    "model": "E1000",
    "version": "1.2.1.1803121427",
    "position": "",
    "time": 1519687662258,
    "ip": "96.64.240.30",
    "localip": "192.168.0.106",
    "uptime": 807183,
    "offline_time": 1523468797,
    "online_time": 1523468804,
    "update_status": "update_ok",
    "update_reason": "",
    "update_version": "1.2.1.1803121427",
    "update_progress": 100,
    "groupcolor": "undefined"
}
```

</details>
<details><summary><strong>[Offline] Status Response Example</strong></summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "_id": "5a9f5bb26d48ab005290b45f",
    "id": "CC:1B:E0:E0:61:9C",
    "mac": "CC:1B:E0:E0:61:9C",
    "name": "CassiaRouter",
    "group": "",
    "status": "offline",
    "model": "C1000",
    "version": "1.2.2.1801101456",
    "position": "",
    "time": 1520393138130,
    "ip": "73.202.248.99",
    "localip": "192.168.1.106",
    "uptime": 1708,
    "offline_time": 1520893570,
    "online_time": 1520891832,
    "update_status": "update_ok",
    "update_reason": "",
    "update_version": "",
    "update_progress": 0
}
```

</details>
<br />

### Monitor Cassia Router’s Status (Through AC)
You can use this API to monitor the status of a router continuously.

**NOTE**: This API is a Server-Sent Events (SSE) API and is only available through Cassia AC.
```
GET http://{your AC domain}/api/cassia/hubStatus
```
The return result is a JSON object.
<details><summary><strong>[Online] Monitor Status Response Example</strong></summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "model": "X1000",
    "ip": "96.64.240.30",
    "mac": "CC:1B:E0:E0:98:50",
    "version": "1.1.1.1710261111",
    "uptime": 0,
    "user": "tester",
    "localip": "192.168.0.105",
    "whitelist": true,
    "status": "online"
}
```

</details>
<details><summary><strong>[Offline] Monitor Status Response Example</strong></summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

data:
{
    "mac": " CC:1B:E0:E0:98:50",
    "status": "offline"
}
```

</details>
<br />

### Obtain All Online Routers’ Status (Through AC)
**NOTE**: This API is only available through Cassia AC.
```
GET http://{your AC domain}/api/cassia/hubs
```
The return result is an array of JSON objects.
<details><summary><strong>Status Response Example</strong></summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

[{
        "_id": "5a9497eeadc22500524e27e5",
        "id": "CC:1B:E0:E0:DF:80",
        "mac": "CC:1B:E0:E0:DF:80",
        "name": "New Bootloader",
        "group": "ExampleLab",
        "status": "online",
        "model": "E1000",
        "version": "1.4.0.1901300130",
        "position": "",
        "time": "1519687662258",
        "ip": "96.64.240.30",
        "localip": "192.168.0.106",
        "uptime": 801523,
        "offline_time": 1523468797,
        "online_time": 1523468804,
        "update_status": "update_ok",
        "update_reason": "",
        "update_version": "1.4.0.1901300130",
        "update_progress": 100,
        "groupcolor": "undefined"
    },
    {
        "_id": "1a3447egadc24570524a27d1",
        "id": "CC:1B:E0:E0:FA:52",
        "mac": "CC:1B:E0:E0:FA:52",
        "name": "New Bootloader",
        "group": "AnotherLab",
        "status": "online",
        "model": "X1000",
        "version": "1.4.0.1901300130",
        "position": "",
        "time": "1519687662258",
        "ip": "62.134.231.112",
        "localip": "192.168.0.96",
        "uptime": 800215,
        "offline_time": 1523461324,
        "online_time": 1523467810,
        "update_status": "update_ok",
        "update_reason": "",
        "update_version": "1.4.0.1901300130",
        "update_progress": 100,
        "groupcolor": "undefined"
    }
]
```
</details>
<br />

### Reboot a Router Remotely
AC Managed:
```
GET http://{your AC domain}/api/cassia/reboot?mac=<hubmac>
```
Local:
```
GET http://{router ip}/cassia/reboot
```
<br />

## Traffic Related API
### Scan Bluetooth Devices
To use the router to scan Bluetooth Low Energy (BLE) devices through your AC:

```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>
```

This API is a Server-Sent Events (SSE) which will be running continuously. Please check
figure 5 for response example.
Here are more optional parameters:

| Parameter | Description |
|-----------|-------------|
| `active`  | (Optional): 0 or 1, 0 indicates passive scanning and 1 active scanning. If you don't specify, by default Cassia routers will perform passive scanning. |
| `filter_duplicates` | (Optional): 0 or 1, turn on/off to filter duplicated records. Default is 0. |

### Filter Scanned Data based on Device MAC, RSSI, Name, and UUID
**NOTE**: Customer can add several filters with format (<filter1>,< filter2>, … , < filterX>). Wildcards are not supported.

This is a useful API which can significantly reduce the amount of traffic sent from the router to the server.

```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_mac=<mac1>,<mac2>, … , <macX>
```

Customer can also filter out devices based on its RSSI level, e.g. filter out devices who’s RSSI value is weaker than a certain value.

```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_rssi=<rssi1>,<rssi2>, … , <rssiX>
```

In addition, customer can filter out a device based on its service UUID and name inside its advertise packet.
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_uuid=<uuid1>,<uuid2>, … , <uuidX>
```
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_name=<name1>,<name2>, … , <nameX>
```

**NOTE**: In order to filter UUID or name from advertise packets, the device should include the corresponding types in advertise packets:
```
#define EIR_UUID16_SOME 0x02 /* 16-bit UUID, more available */
#define EIR_UUID16_ALL 0x03 /* 16-bit UUID, all listed */
#define EIR_UUID32_SOME 0x04 /* 32-bit UUID, more available */
#define EIR_UUID32_ALL 0x05 /* 32-bit UUID, all listed */
#define EIR_UUID128_SOME 0x06 /* 128-bit UUID, more available */
#define EIR_UUID128_ALL 0x07 /* 128-bit UUID, all listed */
#define EIR_NAME_SHORT 0x08 /* shortened local name */
#define EIR_NAME_COMPLETE 0x09 /* complete local name */
```

### Connect/Disconnect to a Target Device
To use the router to connect to specific BLE devices using Cassia AC:
```
POST http://{your AC domain}/api/gap/nodes/<node>/connection?mac=<hubmac>
```
We have added a few parameters in release 1.2 for this API:

| Parameter | Description |
|-----------|-------------|
| `type`    | (Mandatory): the BLE device’s address type, either public or random. |
| `timeout` | (Optional): in ms, the connection request will timeout if it can’t be finished within this time. The default timeout is set to 20,000ms. For S Series, the timeout value can’t be configured, while for X1000/E1000/C1000, this parameter is configurable, the minimum value is 200ms. |
| `auto`    | (Optional): 0 or 1, indicates whether or not the BLE device will be automatically reconnected after it is disconnected unexpectedly. Return value: 200 for success, 500 for error. The default value is 0. |

Here is an example for access the router from the local network (no “/api” and “mac=<mac>”):
```
curl -X POST -H "content-type: application/json" -d '{"timeout":"1000","type":"public","auto":"1"}' 'http://172.16.10.6/gap/nodes/CC:1B:E0:E8:09:2B/connection'
```
<details><summary>Response Example</summary>

```
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```

</details>
<br />

To disconnect:
```
DELETE http://{your AC domain}/api/gap/nodes/<node>/connection?mac=<hubmac>
```
<details><summary>Response Example</summary>

```
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```

</details>
<br />

Get the device list connected to a router:
```
GET http://{your AC domain}/api/gap/nodes?connection_state=connected&mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "nodes": [{
        "type": "random",
        "bdaddrs": {
            "bdaddr": "EF:A3:E6:94:CD:2D",
            "bdaddType": "random"
        },
        "chipId": 0,
        "handle": "",
        "name": "",
        "connectionState": "connected",
        "id": "EF:A3:E6:94:CD:2D"
    }]
}
```

</details>
<br />


### Discover GATT Services and Characteristics
#### Discover all services:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services?mac=<hubmac>
```

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

[{
    "uuid": "00001800-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 1
}, {
    "uuid": "00001801-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 8
}, {
    "uuid": "0000fd00-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 9
}, {
    "uuid": "0000180d-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 20
}, {
    "uuid": "0000180f-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 26
}, {
    "uuid": "0000180a-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 30
}, {
    "uuid": "00001802-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 43
}, {
    "uuid": "00001803-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 46
}]
```

</details>
<br />

#### Discover all characteristics:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/characteristics?mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

[{
    "handle": 3,
    "properties": 10,
    "uuid": "00002a00-0000-1000-8000-00805f9b34fb"
}, {
    "handle": 5,
    "properties": 2,
    "uuid": "00002a01-0000-1000-8000-00805f9b34fb"
}, {
    "handle": 7,
    "properties": 2,
    "uuid": "00002a04-0000-1000-8000-00805f9b34fb"
}, {
    "handle": 11,
    "properties": 16,
    "uuid": "0000fd09-0000-1000-8000-00805f9b34fb"
}, {
    "handle": 14,
    "properties": 4,
    "uuid": "0000fd0a-0000-1000-8000-00805f9b34fb"
}]
```

</details>
<br />

#### Discover all characteristics in one service:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services/<service_uuid>/characteristics?mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

[{
    "handle": 48,
    "properties": 10,
    "uuid": "00002a06-0000-1000-8000-00805f9b34fb"
}]
```

</details>
<br />

#### Discover all descriptors in one characteristic:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/characteristics/<characteristic_uuid>/descriptors?mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

[{
    "handle": 48,
    "properties": 10,
    "uuid": "00002a06-0000-1000-8000-00805f9b34fb"
}]
```

</details>
<br />

#### Discover a specific service by service UUID:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services?mac=<hubmac>&uuid=<uuid>
```
<details><summary>Response Example</summary>

```
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "uuid": "00001801-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "handle": 8
}
```

</details>
<br />

#### Discover a specific characteristic by characteristics UUID:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/characteristics?mac=<hubmac>&uuid=<uuid>
```

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "handle": 45,
    "properties": 4,
    "uuid": "00002a06-0000-1000-8000-00805f9b34fb"
}
```

</details>
<br />

#### Discover all services, characteristics, and descriptors all at once:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services/characteristics/descriptors?mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

[{
    "uuid": "00001800-0000-1000-8000-00805f9b34fb",
    "primary": true,
    "characteristics": [{
        "descriptors": [{
            "handle": 3,
            "uuid": "00002a00-0000-1000-8000-00805f9b34fb"
        }],
        "handle": 3,
        "properties": 10,
        "uuid": "00002a00-0000-1000-8000-00805f9b34fb"
    }, {
        "descriptors": [{
            "handle": 5,
            "uuid": "00002a01-0000-1000-8000-00805f9b34fb"
        }],
        "handle": 5,
        "properties": 2,
        "uuid": "00002a01-0000-1000-8000-00805f9b34fb"
    }, {
        "descriptors": [{
            "handle": 7,
            "uuid": "00002a04-0000-1000-8000-00805f9b34fb"
        }],
        "handle": 7,
        "properties": 2,
        "uuid": "00002a04-0000-1000-8000-00805f9b34fb"
    }],
    "handle": 1
}]
```

</details>
<br />

### Read/Write the Value of a Specific Characteristic
The read/write operations are based on the handle (found in the discover result) of a specific characteristic.
To read by the handle:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/handle/<handle>/value?mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json

{
    "handle": "36",
    "value": "56312e362e31"
}
```

</details>
<br />

To write by the handle:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/handle/<handle>/value/<value>?mac=<hubmac>
```

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain

OK
```

</details>
<br />

Below is an example for opening and closing a specific characteristic’s notification and indication by Write API.

First, you need to call the Discover API to find the corresponding descriptors of the
specified characteristic. Then, open the descriptors, find the UUID and its corresponding
handle, e.g. “37”. Now, you can use this handle in the Write API. To open the notification,
set the value to "0100"; to open the indication, set the value to "0200"; to close the
notification/indication, set the value to "0000" (37, 0100, 0200 and 0000 are examples).

```
GET http://{your AC domain}/api/gatt/nodes/<node>/handle/37/value/0100?mac=<hubmac>
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain

OK
```
</details>
<br />

### Get Advertise Data
Begin advertising data:
```
GET http://{your AC domain}/api/advertise/start?mac=<mac>&interval=<interval>&ad_data=<ad_data>&resp_data=<resp_data>
```
Here are the parameters:

| Parameter | Description |
|-----------|-------------|
| `interval` | (Mandatory): advertise interval in ms. |
| `ad_data` | (Mandatory): advertise package, the data type is string. |
| `resp_data` | (Mandatory): advertise response package, the data type is string. |

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```

</details>
<br />

Stop advertise data:
```
GET http://{your AC domain}/api/advertise/stop?mac=<mac>
```

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```

</details>
<br />

### Get Device Connection Status
SSE API to get the connection status of all the devices that have connected to a router:
```
GET http://{your AC domain}/api/management/nodes/connectionstate?mac=<hubmac>
```
When a device status is changed from disconnected to connected, or from connected to
disconnected, you will get a response. For example:
```json
data: {"handle":"CC:1B:E0:E8:0D:F2","connectionState":"connected"}
data: {"handle":"88:C6:26:92:58:77","connectionState":"disconnected"}
```
<br />

### Receive Notification and Indication
SSE API to continues receive notification and indication:
```
GET http://{your AC domain}/api/gatt/nodes?event=1&mac=<hubmac>
```

<br />

## Positioning API
Cassia supports room-based Bluetooth location tracking. Below are the related APIs.

**NOTE**: Before calling any positioning APIs, please call scan API for the related routers.
Positioning APIs are only available through Cassia AC.

To identify the closest router a BLE device is located:
```
GET http://{your AC domain}/api/middleware/position/by-device/<device_mac>
```
It will return {“hubMac”:”hubMac1”}, e.g. {“hubMac”:”CC:1B:E0:E0:01:47”}.

To obtain the closest router list for all the BLE devices that the AC can detect:
```
GET http://{your AC domain}/api/middleware/position/by-device/*
```
It will return a list:
```
{
    "device1": {
        "hubMac": "hubMac1"
    },
    "device2": {
        "hubMac": "hubMac2"
    },
    ...
}
```

<details><summary>Example</summary>

```
{
    "11: 22: 33: 44: 55: 66": {
        "hubMac": "CC: 1 B: E0: E0: 01: 47"
    },
    "11: 22: 33: 44: 55: 77": {
        "hubMac": "CC: 1 B: E0: E0: 01: 48"
    },
}
```

</details>
<br />

To get the list of BLE devices around a Cassia router:
```
GET http://{your AC domain}/api/middleware/position/by-ap/<hub_mac>
```
It will return:
```
["device1", "device2","device3", ...]
```

<details><summary>Example</summary>

```
["11:22:33:44:55:66","11:22:33:44:55:AA",...].
```

</details>
<br />

To get the list of BLE devices for all the routers within the AC:
```
GET http://{your AC domain}/api/middleware/position/by-ap/*
```
It will return:
```
{
    "hubMac1": ["device1", "device2", "device3"…],
    "hubMac2": ["device1", "device2", "device3"…],
    ...
}
```

<details><summary>Example</summary>

```
{
    "CC:1B:E0:E0:11:22": ["11:22:33:44:55:66", "11:22:33:44:55:AA"…],
    …
}
```

</details>
<br />

## Secure Pairing API
Starting from 1.2 release, Cassia supports Bluetooth 4.1 Secure Simple Pairing, namely
Just Works, Passkey Entry and Legacy OOB.

Here are the mapping between pair modes, APIs and typical responses:

| Pair Mode | Step 1: API Pair Request | Step 2: API Pair-input Request |
| --- | --- | --- |
| Just Works | Return 0 for pairing failed or 1 for successful. | N/A |
| Passkey Entry | Return 5 for using passkey entry (initiator inputs). | Return 0 for pairing failed or 1 for successful. |
| Legacy OOB | Return 3 for using legacy OOB. | Return 0 for pairing failed or 1 for successful. |

### Pair Request

```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<hubmac>
```
Body parameters:

| Parameter | Description |
| ---- | ---- |
| `bond` | Bond to the node. Default value is 1. |
| `legacy-oob` | (Optional): Default value is 0, which means not using Legacy OOB. If Legacy OOB is needed, please set it to 1. |
| `io-capability` | (Optional): See below table. Default value is KeyboardDisplay. |

IO Capability:

| Value | Comments |
| ---- | ---- |
| DisplayOnly | Check BLE specification version 4.2 |
| DisplayYesNo | Check BLE specification version 4.2 |
| KeyboardOnly | Check BLE specification version 4.2 |
| NoInputNoOutput | Check BLE specification version 4.2 |
| KeyboardDisplay | Default value |

Response Parameters:

| Name | Optional/Mandatory | Description |
| -- | -- | -- |
| HTTP 500 error | Optional | Please check the [Error Messages](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Error-Messages) section. |
| `pairingStatusCode` | Optional | See below table |
| `pairingStatus` | Optional | Description of pairing status code |
| `display` | Optional | Display for pairing status code 6 |

Pairing Status Codes:

| Status Code | Status Description |
| -- | -- |
| 0 | Pairing Failed |
| 1 | Pairing Successful |
| 2 | Pairing Aborted |
| 3 | LE Legacy Pairing OOB Expected |
| 4 | LE Secure Connections Pairing OOB Expected |
| 5 | Passkey Input Expected |
| 6 | Passkey Display Expected |
| 7 | Numeric Comparison Expected (LE Secure Connections Pairing only) |

<br />

### Pair-Input Request
**NOTE**: This API is not needed for Just Works.
```
POST http://<your AC domain>/api/management/nodes/<node>/pairinput?mac=<hubmac>
```

Body example for Passkey Entry (application/json):
```json
{ "passkey": "123456" }
```

Body example for Legacy OOB (application/json):
```json
{ "tk": "0x0123456789ABCDEF0123456789ABCDEF" }
```

The response format is same as pair request API.

<br />

### Unpair Request
```
DELETE http://<your AC domain>/api/management/nodes/<node>/bond?mac=<hubmac>
```
<details><summary>Response Example</summary>

```
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```

</details>
<br />

### Just Works Example
```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<hubmac>
```
Body example (application/json):
```json
{ "bond": 1, "io-capability": "NoInputNoOutput"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "pairingStatusCode": 1, "pairingStatus": "Pairing Successful" }
```

</details>

### Passkey Entry Example: Initiator Inputs
Step #1
```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<hubmac>
```
Body example (application/json):
```json
{ "bond": 1, "io-capability": "KeyboardDisplay" }
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "pairingStatusCode": 5, "pairingStatus": "Passkey Input Expected" }
```

</details>

Step #2
```
POST http://<your AC domain>/api/management/nodes/<node>/pairinput?mac=<hubmac>
```
Body example (application/json):
```json
{ "passkey": "123456" }
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "pairingStatusCode": 1, "pairingStatus": "Pairing Successful" }
```

</details>

### LE Legacy Pairing OOB Example
Step #1
```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<hubmac>
```
Body example (application/json):
```json
{ "bond": 1, "legacy-oob": 1 }
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "pairingStatusCode": 3, "pairingStatus": "LE Legacy Pairing OOB Expected" }
```

</details>

Step #2
```
POST http://<your AC domain>/api/management/nodes/<node>/pairinput?mac=<hubmac>
```
Body example (application/json):
```json
{ "tk": "0x0123456789ABCDEF0123456789ABCDEF" }
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "pairingStatusCode": 1, "pairingStatus": "Pairing Successful" }
```

</details>
<br />


## Router Auto-Selection API
From firmware 1.3, Cassia AC can select one router automatically from a list of
candidates, and then connect the BLE device by using this router. The selection is based
on RSSI, router load, and router capabilities.
If a customer wants to connect a BLE device with a specific router, or he wants to use a
customized router selection algorithm, he should use the APIs in chapter 5.3.3.

**NOTE**: these APIs are only available through Cassia AC.

### Router Auto-Selection
This API will enable/disable router auto-selection function. If the flag is 1, the router
auto-selection function will be enabled. If the flag is 0, the router auto-selection function
will be disabled.

**NOTE**: This API should be called before using any other router auto-selection APIs. The
user can also switch on/off router auto-selection function in Cassia AC settings, like below
snapshot.

**Figure 6: Router auto-selection configuration in AC**

```POST http://{your AC domain}/api/aps/ap-select-switch```

Body example (application/json):
```json
{"flag": 1}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "status": "success", "flag": 1 }
```
</details>

### Connect a Device
This API will automatically select one router from a list of candidates, and use it to
connected the device.
```POST http://{your AC domain}/api/aps/connections/connect```

Parameters for JSON body:

| Parameter | Description |
| ----      | ----        |
| `aps` |  The list of routers which will be used for this auto-select connect request. The user can use one or multiple router’s MAC or * for “aps”. If the user uses *, it means all the online routers that controlled by the AC should be included. |
| `devices` | Only one device MAC address can be added in "devices". |

Body example (application/json):
```json
{ "aps": ["CC:1B:E0:E7:FE:F8","CC:1B:E0:E7:FE:F9","CC:1B:E0:E7:FE:FA"], "devices": ["F7:1
8:BC:18:F0:3A"] }
```

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```
</details>

### Disconnect a Device
This API will disconnect a device. In json Body, only one device’ MAC address can be
added in “devices”.

```POST http://{your AC domain}/api/aps/connections/disconnect```

Body example (application/json):
```json
{ "devices": ["F7:18:BC:18:F0:3A"] }
```

<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```
</details>

## SSE Combination API
These APIs simplify the handling of multiple routers. They also improve the scalability of
AC in terms of the number of routers supported in given hardware resources.

**NOTE**: these APIs are only available through Cassia AC.

Before firmware 1.3, if an application wants to control routers with RESTful APIs through
AC, the application has to create three SSE tunnels for each router: one for scan data, one
for notification/indication data, and one for connected device status.

From firmware 1.3, the application only needs to create one SSE tunnel with AC. This SSE
tunnel can receive scan data, notification/indication data, and connected device status
for all routers controlled by this AC.

### Create Combined SSE
This API will create one combined SSE connection with AC. This SSE connection can
receive scan data, notification/indication data, and connected device status for all the
routers controlled by this AC.

```GET http://{your AC domain}/api/aps/events```

When invoke this API, AC will return a message immediately which include all router’s
information, for example:
<details><summary>Response Example</summary>

```json
data: {
    "dataType": "state",
    "aps": {
        "CC:1B:E0:E7:FE:F8": {
            "_id": "5a93755b028e6c00519ce1dc",
            "id": "CC:1B:E0:E7:FE:F8",
            "mac": "CC:1B:E0:E7:FE:F8",
            "name": "Cassia Router",
            "group": "",
            "status": "online",
            "model": "X1000",
            "version": "1.2.0.1803131043",
            "position": "",
            "time": 1519613275655,
            "ip": "192.168.1.202",
            "localip": "192.168.1.202",
            "uptime": 14873,
            "offline_time": 0,
            "online_time": 1522052125,
            "update_status": "update_ok",
            "update_reason": "",
            "update_version": "",
            "update_progress": 0,
            "notify": true,
            "connectionstate": true
        },
        "CA:79:F5:B6:1F:04": {
            "devices": {
                "CA:79:F5:B6:1F:04": {
                    "type": "random",
                    "bdaddrs": {
                        "bdaddr": "CA:79:F5:B6:1F:04",
                        "bdaddrType": "random"
                    },
                    "chipId": 0,
                    "handle": "",
                    "name": "",
                    "connectionState": "connected",
                    "id": "CA:79:F5:B6:1F:04"
                }
            }
        }
    }
}
```
</details>

One keep-alive message will be returned every 30 seconds to make sure the SSE link is up and running.

If scanning is open, this SSE tunnel will send scanning data to user application through AC. For example:

<details><summary>Scanning Data Example</summary>

```json
data:
{
  "dataType": "scan",
  "ap": "CC:1B:E0:E7:FE:F8",
  "bdaddrs": [
    {
      "bdad": "CC:1B:E0:E0:98:16",
      "bdaddrType": "public"
    }
  ],
  "adData": "0201060D084361737369615F5331303030",
  "name": "Cassia_S1000",
  "rssi": -34,
  "evt_type": 3
}
```

</details>

If notification is open (default configuration), this SSE tunnel will return the notification messages to user application when any router has notification messages to AC.

<details><summary>Notification Message Example</summary>

```json
data:
{
    "dataType": "notification",
    "ap": "CC:1B:E0:E7:FE:F8",
    "value": "FF000C000205100101010126",
    "device": "CA:79:F5:B6:1F:04",
    "handle": 16
}
```

</details>

If connection-state is open (default configuration), this SSE tunnel will return the device’s connection status to user application when any device’s status changes. For example:

<details><summary>Connection Status Example</summary>

```json
data: {"handle":"CA:79:F5:B6:1F:04","connectionState":"disconnected","dataType":"connection_state","ap":"CC:1B:E0:E7:FE:F8"}
data: {"handle":"CA:79:F5:B6:1F:04","connectionState":"connected","dataType":"connection_state","ap":"CC:1B:E0:E7:FE:F8"}
```

</details>

If ap-state is open (default configuration), this SSE tunnel will return the ap-state information when router’s status is changed between online and offline. For example:

<details><summary>AP-State Information Example</summary>

```json
data:
{
    "dataType": "ap_state",
    "ap": "CC:1B:E0:E7:FE:F8",
    "mac": "CC:1B:E0:E7:FE:F8",
    "status": "offline",
    "offline_time": 1522067273296
}
```

</details>
