Most of the Bluetooth GAP/GATT operations are exposed in RESTful APIs. The signatures of
those APIs are fully compliant with Bluetooth SIG’s Internet Working Group RESTful API
specification.

**NOTE**: If you are accessing the RESTful API from the container, use the container static address: 10.10.10.254

**NOTE**: When using the /cassia/* APIs in Standalone Router mode, you may need to log in to the router first. To do so, send a POST request with the following instructions:
1. Make sure the request header contains:
```
Content-Type: application/x-www-form-urlencoded
```

2. The form data should contain the username and password of the router login. For example:
```
username=admin&password=admin2
```

3. Send the POST request to the login API. For example:
```
http://{router ip}/cassia/login
```

After the POST request returns a 200 OK response, you should be able to call the /cassia/* APIs without getting a Login page HTML response.

<br>

## Common Parameters
Here are common parameters for the RESTful API:

| Parameter | Description |
|-----------|-------------|
| `mac` |  The mac address of a Cassia router (e.g. CC:1B:E0:E0:24:B4). |
| `node` |  The mac address of a BLE device (e.g. EF:F3:CF:F0:8B:81). |
| `handle` | After you find the device services, based on the device’s Bluetooth profile, you can identify its corresponding handle index in the UUID (e.g. 37). |
| `value` | the hex value written into the handle (e.g. FF000C00). |
| `chip` | (Optional) 0 or 1, indicates which chip of the Cassia router is used for scan and connect. By default, the router will pick up the chip automatically based on an internal algorithm. S Series routers only support chip 0, X1000/E1000/C1000 supports 0 and 1. |
<br />

## Management API
### Obtain Cassia Router’s Configuration
You can use the API below to obtain the configuration of a router, including its IP address,
model, version, etc.

**NOTE**: Since v2.0.2, the container status has been removed from the default API output to avoid the problem of oversized UDP packets. To get the container information, add the parameter fields=container to the API URL like (for AC API):

```
http://{your AC domain}/api/cassia/info?mac=<hubmac>&fields=container
```

<br>

AC Managed:
```
GET http://{your AC domain}/api/cassia/info?mac=<hubmac>
```
Local:
```
GET http://{router ip}/cassia/info
```
Container:
```
GET http://10.10.10.254/cassia/info
```

| Parameter | Description |
|-----------|-------------|
| `fields`    | (Optional): type of field to return from the router's configuration information. The field `container` returns the container status information. |

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
Container:
```
GET http://10.10.10.254/cassia/reboot
```

<br />

## Traffic Related API
### Scan Bluetooth Devices
To use the router to scan Bluetooth Low Energy (BLE) devices through your AC:

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>
```
Local:
```
GET http://{router ip}/gap/nodes?event=1
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1
```

This API is a Server-Sent Events (SSE) which will be running continuously. Please check
figure 5 for response example.
Here are more optional parameters:

| Parameter | Description |
|-----------|-------------|
| `active`  | (Optional): 0 or 1, 0 indicates passive scanning and 1 active scanning. If you don't specify, by default Cassia routers will perform passive scanning. |
| `filter_duplicates` | (Optional): 0 or 1, turn on/off to filter duplicated records. Default is 0. |


<br>

#### Filter Scanned Data based on Device MAC, RSSI, Name, and UUID
This API can significantly reduce the amount of packets sent from the router to the server.

**NOTE**: Multiple filters can be used at the same time. Scanned data is returned if all
conditions are met. The wildcard is not supported.

**NOTE**: Multiple filter_name and filter_mac filters are allowed at the same time. The adData and scanData packets that meet one of the filters will be sent to the application.

**NOTE**: Different filter types are allowed at the same time (e.g. filter_value, filter_name and filter_mac). Only the adData and scanData packets that meet all the filters will be sent to the application.

Users can filter the devices based on its MAC address.

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_mac=<mac1>,<mac2>, … , <macX>
```
Local:
```
GET http://{router ip}/gap/nodes?event=1&filter_mac=<mac1>,<mac2>, … , <macX>
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1&filter_mac=<mac1>,<mac2>, … , <macX>
```

Users can filter out devices based on its RSSI level, e.g. filter out devices who’s RSSI value is weaker than a certain value.

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_rssi=<rssi>
```
Local:
```
GET http://{router ip}/gap/nodes?event=1&filter_rssi=<rssi>
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1&filter_rssi=<rssi>
```

In addition, users can filter out devices based on service UUID and name inside the scanned packets. The service UUID may be only part of the UUID in BLE profile. What is more, filter_uuid should not include “-”.

**NOTE**: filter_uuid can only filter the adData and scanData which includes BLE EIR type 0x02 to 0x07. Please search EIR_UUID16_SOME in this wiki for more information.

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_uuid=<uuid1>,<uuid2>, … , <uuidX>
```
```
GET http://{your AC domain}/api/gap/nodes?event=1&mac=<hubmac>&filter_name=<name1>,<name2>, … , <nameX>
```
Local:
```
GET http://{router ip}/gap/nodes?event=1&filter_uuid=<uuid1>,<uuid2>, … , <uuidX>
```
```
GET http://{router ip}/gap/nodes?event=1&filter_name=<name1>,<name2>, … , <nameX>
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1&filter_uuid=<uuid1>,<uuid2>, … , <uuidX>
```
```
GET http://10.10.10.254/gap/nodes?event=1&filter_name=<name1>,<name2>, … , <nameX>
```

**NOTE**: The structure of BLE advertise packets and scan response packets is [1 Byte Length (type + data) + 1 Byte Type + Data] x n. In order to filter by UUID or name, the corresponding type should be included in advertise packets (adData) or scan response packets (scanData). Below are the types:

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

Below is an example which includes name in scan response:
<br />

![Figure 7](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/Screen%20Shot%202019-09-23%20at%202.57.33%20PM.png)


Below is an example which includes UUID in advertise packet. The uuid in this advertise
packets is F0FF. Please move the last byte (FF) forward and add the rest of the bytes(F0),
here comes the filter_uuid= FFF0:

<br />

![Figure 8](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/Screen%20Shot%202019-09-23%20at%202.57.53%20PM.png)

<br>

#### Enhanced Scan Filter (v2.0 and above)
In order to improve the flexibility of scan filter, Cassia enhanced name & MAC filter and added value filter from v2.0.

**filter_name**
* Full name: same as legacy filter_name.
* Filter out unknown name: format is *. Filter out the adv packets without name in both ad_data and scan_data.
* Prefix: format is Cassia*. Filter prefix in either ad_data or scan_data.
* Suffix: format is *Cassia. Filter suffix in either ad_data or scan_data.

Examples:

cURL (Local):
```
curl -v 'http://172.16.10.99/gap/nodes?event=1&filter_name=Cassia*,36NOTES,*aaa
```

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&filter_name=Cassia*,36NOTES,*aaa
```
Local:
```
GET http://{router ip}/gap/nodes?event=1&filter_name=Cassia*,36NOTES,*aaa
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1&filter_name=Cassia*,36NOTES,*aaa
```
<br>
<br>

**filter_mac**
* Full MAC: same as legacy filter_mac.
* Prefix: format is CC:DD:EE*. Filter the adv packets by MAC prefix.

Examples:

cURL (Local):
```
curl -v 'http://172.16.10.99/gap/nodes?event=1&filter_mac=CC:DD:EE*,CC:1B:E0:E8:0B:4B'
```

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&filter_mac=CC:DD:EE*,CC:1B:E0:E8:0B:4B
```
Local:
```
GET http://{router ip}/gap/nodes?event=1&filter_mac=CC:DD:EE*,CC:1B:E0:E8:0B:4B
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1&filter_mac=CC:DD:EE*,CC:1B:E0:E8:0B:4B
```

<br>
<br>

**filter_value**
* filter value with data xx from offset yy

**NOTE**: Currently, filter_value only supports one value. This API does not accept multiple filter_value values at the same time.

Examples:

cURL (Local):
```
curl -v 'http://172.16.10.99/gap/nodes?event=1&filter_value=\{"offset":"7","data":"0302E9"\}'
```

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?event=1&filter_value=\{"offset":"7","data":"0302E9"\}
```
Local:
```
GET http://{router ip}/gap/nodes?event=1&filter_value=\{"offset":"7","data":"0302E9"\}
```
Container:
```
GET http://10.10.10.254/gap/nodes?event=1&filter_value=\{"offset":"7","data":"0302E9"\}
```

Output example

```
data: {"name":"(unknown)","evtType":0,"rssi":-67,"adData":"0201060302E9FE08FFEC82 0418000363","bdaddrs":[{"bdaddr":"CC:1B:E0:E8:11:6A","bdaddrType":"public"}]}
```

Note: When using Chrome to call scan filter API, please encode "{" and "}", for example:

```
http://172.16.10.99/gap/nodes?event=1&filter_value=%7B"offset":"7","data":"0302E9"%7D
```

<br>
<br>

### Connect/Disconnect to a Target Device
To use the router to connect to specific BLE devices using Cassia AC:

AC Managed:
```
POST http://{your AC domain}/api/gap/nodes/<node>/connection?mac=<hubmac>
```
Local:
```
POST http://{router ip}/gap/nodes/<node>/connection
```
Container:
```
POST http://10.10.10.254/gap/nodes/<node>/connection
```

**NOTE**: Multiple connecting requests cannot be handled simultaneously by one router.
User needs to handle requests in serial, which is to wait for the response and then invoke
the next connecting request.


| Parameter | Description |
|-----------|-------------|
| `type`    | (Mandatory): the BLE device’s address type, either public or random. Default is public if not specified. |
| `timeout` | (Optional): in ms, the connection request will timeout if it can’t be finished within this time. The default timeout is 5,000ms. The range of value is 200ms – 20000ms. |
| `auto`    | (Optional): 0 or 1, indicates whether or not the BLE device will be automatically reconnected after it is disconnected unexpectedly. Return value: 200 for success, 500 for error. The default value is 0 (don't reconnect). (After the BLE connection is reconnected, the user application needs to reconnect the up-layer connections. For example, resubscribe the BLE notifications.) **This parameter is disabled for firmware v1.4.3 and above!** |
| `discovergatt` | (Optional): 0 or 1 (default) ❖ Value 1 indicates the router should use the cached GATT database which was discovered during previous connection. It will save time for service discover API, but maybe the information is not updated. ❖ Value 0 indicates the router should not use the cached GATT database. When a user calls the service discover API, the router should read the GATT services & characteristics from the BLE device. |

Here is an example for accessing the router from a local network (no "/api" and "mac=<mac>"):
```
curl -X POST -H "content-type: application/json" -d '{"timeout":"1000","type":"public"}' 'http://172.16.10.6/gap/nodes/CC:1B:E0:E8:09:2B/connection'
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

AC Managed:
```
DELETE http://{your AC domain}/api/gap/nodes/<node>/connection?mac=<hubmac>
```
Local:
```
DELETE http://{router ip}/gap/nodes/<node>/connection
```
Container:
```
DELETE http://10.10.10.254/gap/nodes/<node>/connection
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
AC Managed:
```
GET http://{your AC domain}/api/gap/nodes?connection_state=connected&mac=<hubmac>
```
Local:
```
GET http://{router ip}/gap/nodes?connection_state=connected
```
Container:
```
GET http://10.10.10.254/gap/nodes?connection_state=connected
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

### Batch Connect/Disconnect to a Target Device (v2.0 and above)
To use the router to connect to specific BLE devices using Cassia AC:

AC Managed:
```
POST http://{your AC domain}/api/gap/batch-connect
```
Local:
```
POST http://{router ip}/gap/batch-connect
```
Container:
```
POST http://10.10.10.254/gap/batch-connect
```

**NOTE**: Parameters are provided via JSON (application/json) through the POST request.

| Parameter | Description |
|-----------|-------------|
| `type`    | (Mandatory): the BLE device’s address type, either public or random. |
| `timeout` | (Optional): timeout value for each individual connection of one device. Default value: 5000 (ms). |
| `per_dev_timeout `    | (Optional): timeout value for single device, including retry duration. Default value: 10000 (ms).  ‘per_dev_timeout’ should be greater than ‘timeout’ |
| `list` | (Optional): connection MAC list, format is JSON array. Example: [{"type":"public","addr":"C0:00:5B:D1:B7:25"},{"type":"public","addr":"C0:00:5B:D1:AF:F0"}]，could be one or multiple MACs, could add into existing multi-connect list. <br> Sample ：<br> ```curl -v -X POST -H "content-type: application/json" -d '{"list":[{"type":"public","addr":"C0:00:5B:D1:B7:25"},{"type":"public","addr":"C0:00:5B:D1:AF:F0"}], "timeout":"5000"}' 'http://172.16.10.99/gap/batch-connect' ``` |

Here is an example using the Local (Standalone Router Mode) API (no "/api" and "mac=<mac>"):
```
              curl -v -X POST -H "content-type: application/json" -d '{"list":[{"type":"public","addr":"C0:00:5B:D1:B7:25"},{"type":"public","addr":"C0:00:5B:D1:AF:F0"}], "timeout":"5000"}' 'http://172.16.10.99/gap/multi-connect'
```

<br>
<br>

__Connection-State SSE returns additional connection timeout status:__

Get the connection timeout status through an SSE stream.

SSE cURL example:
```
curl –v 'http://172.16.10.99/management/nodes/connection-state'
```

Stream Output Data format：
``` JSON
data: {"handle":"C0:00:5B:D1:B7:26","chipId":0,"connectionState":"connect timeout"}
```

<br>
<br>

__Remove batch-connect interface and empty connection queue:__

This API removes the batch-connection activity and empties the queue of connections.

cURL:
```
curl -v -X DELETE 'http://172.16.10.99/gap/batch-connect'
```

AC Managed:
```
DELETE http://{your AC domain}/api/gap/multi-connect
```
Local:
```
DELETE http://{router ip}/gap/multi-connect
```
Container:
```
DELETE http://10.10.10.254/gap/multi-connect
```
**NOTE**: No parameters are accepted for this API.

<br>
<br>
<br>

### Discover GATT Services and Characteristics
#### Discover all services:

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/services
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/services
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/characteristics?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/characteristics
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/characteristics
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services/<service_uuid>/characteristics?mac=<hubmac>
```
Local:
```
GET http://{your AC domain}/gatt/nodes/<node>/services/<service_uuid>/characteristics
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/services/<service_uuid>/characteristics
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/characteristics/<characteristic_uuid>/descriptors?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/characteristics/<characteristic_uuid>/descriptors
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/characteristics/<characteristic_uuid>/descriptors
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services?mac=<hubmac>&uuid=<uuid>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/services?uuid=<uuid>
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/services?uuid=<uuid>
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/characteristics?mac=<hubmac>&uuid=<uuid>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/characteristics?uuid=<uuid>
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/characteristics?uuid=<uuid>
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/services/characteristics/descriptors?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/services/characteristics/descriptors
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/services/characteristics/descriptors
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/handle/<handle>/value?mac=<hubmac>
```
Local:
```
GET http://{your AC domain}/gatt/nodes/<node>/handle/<handle>/value
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/handle/<handle>/value
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/handle/<handle>/value/<value>?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/handle/<handle>/value/<value>
```
Container:
```
GET http://{router ip}/gatt/nodes/<node>/handle/<handle>/value/<value>
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes/<node>/handle/37/value/0100?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes/<node>/handle/37/value/0100
```
Container:
```
GET http://10.10.10.254/gatt/nodes/<node>/handle/37/value/0100
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

### Send Advertise Data
Begin advertising data:

AC Managed:
```
GET http://{your AC domain}/api/advertise/start?mac=<mac>&interval=<interval>&ad_data=<ad_data>&resp_data=<resp_data>
```
Local:
```
GET http://{router ip}/advertise/start?interval=<interval>&ad_data=<ad_data>&resp_data=<resp_data>
```
Container:
```
GET http://10.10.10.254/advertise/start?interval=<interval>&ad_data=<ad_data>&resp_data=<resp_data>
```

Here are the parameters:

| Parameter | Description |
|-----------|-------------|
| `interval` | (Mandatory): advertising interval in ms. Default value 500ms. |
| `ad_type` | (Optional): advertising type (see below table). Default value 3. |
| `ad_data` | (Mandatory): advertise package, the data type is string. |
| `resp_data` | (Mandatory): scan response package. The data type is string. When you want to send resp_data, please set ad_type=0. |

| Value	| ad_type | Comments |
| -- | -- | -- |
| 0 | ADV_IND | Connectable undirected advertising |
| 1 | ADV_DIRECT_IND | Connectable directed advertising |
| 2 | ADV_SCAN_IND | Scannable undirected advertising |
| 3 | ADV_NONCONN_IND | Non connectable undirected advertising |
|4 | SCAN_RSP | Scan Response |


<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: text/plain
OK
```

</details>
<br />

Stop sending advertise data:

AC Managed:
```
GET http://{your AC domain}/api/advertise/stop?mac=<mac>
```
Local:
```
GET http://{router ip}/advertise/stop
```
Container:
```
GET http://10.10.10.254/advertise/stop
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

AC Managed:
```
GET http://{your AC domain}/api/management/nodes/connectionstate?mac=<hubmac>
```
Local:
```
GET http://{router ip}/management/nodes/connectionstate
```
Container:
```
GET http://10.10.10.254/management/nodes/connectionstate
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

AC Managed:
```
GET http://{your AC domain}/api/gatt/nodes?event=1&mac=<hubmac>
```
Local:
```
GET http://{router ip}/gatt/nodes?event=1
```
Container:
```
GET http://10.10.10.254/gatt/nodes?event=1
```

<br />

### Get RSSI for BLE Connection (v2.0 and above)
Users can get the current RSSI of a BLE connection with this API.

AC Managed:
```
GET http://{your AC domain}/api/gap/nodes/<node>/rssi?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gap/nodes/<node>/rssi?mac=<hubmac>
```
Container:
```
GET http://10.10.10.254/gap/nodes/<node>/rssi?mac=<hubmac>
```

<details><summary>Response Example</summary>

```json
{"id":"C0:00:5B:D1:A9:20","rssi":-33}
```

</details>
<br />

If users want to get a continuous RSSI report for all the BLE connections of a router, they can use below SSE API.

AC Managed:
```
GET http://{your AC domain}/api/gap/rssi?mac=<hubmac>
```
Local:
```
GET http://{router ip}/gap/rssi?mac=<hubmac>
```
Container:
```
GET http://10.10.10.254/gap/rssi?mac=<hubmac>
```

Here are the parameters:

| Parameter | Description |
|-----------|-------------|
| `rssi` | (Optional): Only report RSSI if the RSSI is lower than this threshold. Default is 127. |
| `rssi_interval` | (Optional): RSSI report interval. The unit is ms. The default value is 1000. |
| `filter_mac` | (Optional): If users only want to get a RSSI report of a particular BLE devices, they can use this parameter. The format is same as the parameter filter_mac in [Filter Scanned Data based on Device MAC, RSSI, Name, and UUID](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#filter-scanned-data-based-on-device-mac-rssi-name-and-uuid) |

<details><summary>Response Example</summary>

```json
{"id":"C0:00:5B:D1:A9:20","rssi":-34}
```

</details>
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
Since the v1.2 release, Cassia supports Bluetooth 4.1 Secure Simple Pairing, namely Just Works, Passkey Entry and Legacy OOB.

**(v2.0 and above)**: Starting from the v2.0 release, Cassia supports Bluetooth 4.2 Pairing, namely Just Works, Passkey Entry, Security OOB and Numeric Comparison.

**NOTE**: Before the v2.0 release, users must call the connect API before calling the pair API. From the v2.0 release, the API Pair Request will set up the BLE connection automatically if the connection has not been set up.

Here is the mapping between pair modes, APIs, and typical responses.

**NOTE**: The 4.1 and 4.2 in the table refers to the Bluetooth versions. The "Y" and "N" means "Yes" and "No" to whether the Pair Mode is supported in a Bluetooth version.

| Pair Mode | 4.1 | 4.2 | Step 1: API Pair Request | Step 2: API Pair-input Request |
| --- | --- | --- | --- | --- |
| Just Works | Y | Y | Returns 0 for pairing failed or 1 for successful. | N/A |
| Passkey Entry | Y | Y | Returns 5 for using passkey entry (initiator inputs). | Returns 0 for pairing failed or 1 for successful. |
| Legacy OOB | Y | N | Returns 3 for using legacy OOB. | Returns 0 for pairing failed or 1 for successful. |
| Security OOB | N | Y | Returns 0 for pairing failed or 1 for successful | N/A |
| Numeric Comparison | N | Y | Returns 7 for using numeric comparison. | Returns 0 for pairing failed or 1 for successful. |

### Pair Request

AC Managed:
```
POST http://{your AC domain}/api/management/nodes/<node>/pair?mac=<hubmac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair
```

Body parameters:

| Parameter | Description |
| ---- | ---- |
| `oob` | (Optional): 0 means no OOB, 1 means Legacy OOB, 2 means Security OOB. Default is 0. Setting oob=0 or 1 has the same function as the old parameter `legacy-oob` (still valid for backward compatibility). |
| `legacy-oob` | (Deprecated): Default value is 0, which means not using Legacy OOB. If a user wants to use Legacy OOB, please set it to 1. **This parameter is replaced by the `oob` parameter, but this parameter is still valid for backward compatibility.** |
| `iocapability` | (Mandatory): See the table below. Same as the old parameter `io-capability` (still valid for backward compatibility). |
| `io-capability` | (Deprecated): See below table. Default value is KeyboardDisplay. |
| `rand` | (Mandatory for Security OOB): Random value for pair. Provided by the device. |
| `confirm` | (Mandatory for Security OOB): Confirm value for pair. Provided by the device. |
| `timeout` | (Optional): Pair connection attempt timeout in ms. The default value is 5 seconds (5000). |
| `type` | (Optional): ): The type of device (public or random). The default value is public. |
| `bond` | (Optional): Saves the pair key in the router and device, 0 or 1. The default value is 1 (save). |


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
| `display` | Optional | Display for pairing status code 6 and 7 |

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

<br>

### Pair-Input Request
**NOTE**: This API is only needed for Passkey Entry, Legacy OOB, and Numeric Comparison.

AC Managed:
```
POST http://{your AC domain}/api/management/nodes/<node>/pair-input?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair-input
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair-input
```
<br>

Body example for Passkey Entry (application/json):
```json
{ "passkey": "123456" }
```
<br>

Body example for Numeric Comparison (application/json):

Success:
```json
{ "passkey": "1" }
```

Fail:
```json
{ "passkey": "0" }
```
<br>

Body example for Legacy OOB (application/json):
```json
{ "tk": "0x0123456789ABCDEF0123456789ABCDEF" }
```

The response format is same as the pair request API.

<br />

### Unpair Request
AC Managed:
```
DELETE http://{your AC domain}/api/management/nodes/<node>/bond?mac=<router-mac>
```
Local:
```
DELETE http://{router ip}/management/nodes/<node>/bond
```
Container:
```
DELETE http://10.10.10.254/management/nodes/<node>/bond
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
AC Managed:
```
POST http://{your AC domain}/api/management/nodes/<node>/pair?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair
```

Body example (application/json):
```json
{"iocapability": "NoInputNoOutput"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"pairingStatusCode": 1, "pairingStatus": "Pairing Successful"}
```

</details>

<br>

### Passkey Entry Example: Initiator Inputs
Step #1

AC Managed:
```
POST http://{your AC domain}/api/management/nodes/<node>/pair?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair
```

Body example (application/json):
```json
{"iocapability": "KeyboardDisplay"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"pairingStatusCode": 5, "pairingStatus": "Passkey Input Expected"}
```

</details>

Step #2

AC Managed:
```
POST http://{your AC domain}/api/management/nodes/<node>/pair-input?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair-input
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair-input
```

Body example (application/json):
```json
{"passkey": "123456"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"pairingStatusCode": 1, "pairingStatus": "Pairing Successful"}
```

</details>

<br>

### Legacy Pairing OOB Example
Step #1

AC Managed:
```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair
```

Body example (application/json):
```json
{"oob": 1, "iocapability": "KeyboardDisplay"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"pairingStatusCode": 3, "pairingStatus": "LE Legacy Pairing OOB Expected"}
```

</details>

Step #2

AC Managed:
```
POST http://<your AC domain>/api/management/nodes/<node>/pair-input?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair-input
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair-input
```

Body example (application/json):
```json
{"tk": "0x0123456789ABCDEF0123456789ABCDEF"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"pairingStatusCode": 1, "pairingStatus": "Pairing Successful"}
```

</details>

<br>

### Numeric Comparison Example
Step #1

AC Managed:
```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair
```

Body example (application/json):
```json
{"iocapability":"KeyboardDisplay"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"display":"425472","pairingStatus":"Numeric Comparison Expected","pairingStatusCode":7}
```

</details>

Step #2

AC Managed:
```
POST http://<your AC domain>/api/management/nodes/<node>/pair- input?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair-input
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair-input
```

Body example (application/json):
```json
{"passkey":"1"}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{ "pairingStatusCode": 1, "pairingStatus": "Pairing Successful" }
```

</details>

<br>

### Security OOB Example
Step #1

AC Managed:
```
POST http://<your AC domain>/api/management/nodes/<node>/pair?mac=<router-mac>
```
Local:
```
POST http://{router ip}/management/nodes/<node>/pair
```
Container:
```
POST http://10.10.10.254/management/nodes/<node>/pair
```

Body example (application/json):
```json
{
  "oob":2,
  "timeout":20000,
  "type":"random",
  "rand":"e18df48c5ad68f0ee3d541e4d60f9ae5",
  "confirm":"dbc116a241c8894a67bcbd6841a79473",
  "bond":0,
  "iocapability":"KeyboardDisplay"
}
```
<details><summary>Response Example</summary>

```json
Status-Line : HTTP/1.1 200 OK/r/n
Header : (general-header)
Message-body: application/json
{"pairingStatusCode":1,"pairingStatus":"Pairing Successful"}
```

</details>

<br />


## Router Auto-Selection API
From firmware 1.3, Cassia AC can select one router automatically from a list of
candidates, and then connect the BLE device by using this router. The selection is based
on RSSI, router load, and router capabilities.
<br>
If users want to connect a BLE device with a specific router, or they want to use a customized router selection algorithm, they should use the APIs in [Connect/Disconnect to a Target Device](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/RESTful-API#connectdisconnect-to-a-target-device).

**NOTE**: these APIs are only available through Cassia AC.

### Router Auto-Selection
This API will enable/disable router auto-selection function. If the flag is 1, the router
auto-selection function will be enabled. If the flag is 0, the router auto-selection function
will be disabled.

**NOTE**: This API should be called before using any other router auto-selection APIs. The
user can also switch on/off router auto-selection function in Cassia AC settings, like below
snapshot.

<br />

![Figure 6](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f6.png)

**Figure 6: Router auto-selection configuration in AC**

<br />

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

### Open Scan
This API will open router scanning for all the routers in the router list. The SSE tunnel will receive scan data.

```POST http://{your AC domain}/api/aps/scan/open```

Body example (application/json):

```json
{
  "aps": [
    "CC:1B:E0:E7:FE:F8",
    "CC:1B:E0:E7:FE:F8",
    "CC:1B:E0:E7:FE:F8",
    "CC:1B:E0:E7:FE:F8"
  ],
  "chip": 0,
  "active": 0,
  "filter_name": "cassia"
}
```

Parameters for JSON Body:

| Parameter | Description |
|--|--|
| `aps` | (Mandatory): one or multiple router’s MAC address |
| `chip` | (Optional): 0 or 1. It means which chip to scan |
| `active` | (Optional): 0 or 1. 0 means enable passive scanning; 1 means enable active scanning |
| `filter_name` | (Optional): filter for device name |
| `filter_mac` | (Optional): filter for device MAC |
| `filter_uuid` | (Optional): filter for device UUID |

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```

### Close Scan
This API will close the router scanning for all the routers in the router list. The user application will not receive scan data anymore.

```POST http://{your AC domain}/api/aps/scan/close```

Parameters for JSON Body:

| Parameter | Description |
| -- | -- |
| `aps` | One or multiple router’s MAC address |

Body example (application/json):

```json
{
    "aps": [
        "CC:1B:E0:E7:FE:F8",
        "CC:1B:E0:E7:FE:F8"
    ]
}
```

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
```

### Open Notify
This API will open the notification messages on SSE tunnel. The notification data will be sent to the user application on this SSE tunnel.

```POST http://{your AC domain}/api/aps/notify/open```

Parameters for JSON Body:

| Parameter | Description |
| -- | -- |
| `aps` | One or multiple router’s MAC address |

Body example (application/json):

```json
{
  "aps": [
    "CC:1B:E0:E7:FE:F8",
    "CC:1B:E0:E7:FE:F8"
  ]
}
```

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```

### Close Notify
This API will close the notification messages on SSE tunnel. The notification data will not be sent to the user application on this SSE tunnel anymore.

```POST http://{your AC domain}/api/aps/notify/close```

Parameters for JSON Body:

| Parameter | Description |
| -- | -- |
| `aps` | One or multiple router’s MAC address |

Body example (application/json):

```json
{
    "aps": [
        "CC:1B:E0:E7:FE:F8",
        "CC:1B:E0:E7:FE:F8"
    ]
}
```

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```

### Open Connection-State Report
This API will open the connection-state monitoring on SSE tunnel. The connection-state data will be sent to the user application on this SSE tunnel when the state of the connected device changed.

```POST http://{your AC domain}/api/aps/connection-state/open```

Parameters for JSON Body:

| Parameter | Description |
| -- | -- |
| `aps` | One or multiple router’s MAC address |

Body example (application/json):

```json
{
    "aps": [
        "CC:1B:E0:E7:FE:F8",
        "CC:1B:E0:E7:FE:F8"
    ]
}
```

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```

### Close Connection-State Report
This API will close the connection-state monitoring on SSE tunnel. The connection-state data will not be sent to the user application on this SSE tunnel anymore. 

 ```POST http://{your AC domain}/api/aps/connection-state/close```

Parameters for JSON Body:

| Parameter | Description |
| -- | -- |
| aps | One or multiple router’s MAC address |

Body example (application/json):
```json
{
    "aps": [
        "CC:1B:E0:E7:FE:F8",
        "CC:1B:E0:E7:FE:F8"
    ]
}
```

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```

### Open AP-State Report
This API will open the ap-state monitoring for all routers on SSE tunnel. The data of ap-state will be sent to the user application on this SSE tunnel when the router state changed between online and offline.

```GET http://{your AC domain}/api/aps/ap-state/open```

Response example:

```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```

### Close AP-State Report
This API will close the ap-state monitoring for all routers on SSE tunnel. The data of ap-state will not be sent to the user application on this SSE tunnel anymore.

```GET http://{your AC domain}/api/aps/ap-state/close```

Response example:
```json
Status-Line  : HTTP/1.1 200 OK/r/n
Header       : (general-header)
Message-body : text/plain
OK
```
