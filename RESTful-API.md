Most of the Bluetooth GAP/GATT operations are exposed in RESTful APIs. The signatures of
those APIs are fully compliant with Bluetooth SIG’s Internet Working Group RESTful API
specification.

## Common Parameters
Here are common parameters for RESTful API:
  * mac: the mac address of a Cassia router (e.g. CC:1B:E0:E0:24:B4)
  * node: the mac address of a BLE device (e.g. EF:F3:CF:F0:8B:81)
  * handle: after you find the device services, based on the device’s Bluetooth profile, you
can identify its corresponding handle index in the UUID (e.g. 37)
  * value: the value written into the handle
  * chip (optional): 0 or 1, indicates which chip of the Cassia router is used for scan and
connect. By default, the router will pick up the chip automatically based on an internal
algorithm. S Series routers only support chip 0, X1000/E1000/C1000 supports 0 and 1.

## Management API
### Obtain Cassia Router’s Configuration
You can use below API to obtain the configuration of a router, including its IP address,
model, version, etc.
```
GET http://{your AC domain}/api/cassia/info?mac=<hubmac>
```
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


### Obtain Cassia Router’s Status (Through AC)
You might use below API to obtain the status of a router, either online or offline. The return result is a JSON object.

**NOTE**: This API is only available through Cassia AC.
```
GET http://{your AC domain}/api/cassia/hubs/<hubmac>
```
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


#### Monitor Cassia Router’s Status (Through AC)
You can use this API to monitor the status of a router continuously.

**NOTE**: This API is a Server-Sent Events (SSE) API and is only available through Cassia AC.
```
GET http://{your AC domain}/api/cassia/hubStatus
```
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


### Reboot a Router Remotely

