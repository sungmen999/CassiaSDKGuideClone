We integrated Cassia the RESTful API into a Bluetooth debug tool with a visual interface. After a
RESTful API call, the debug tool will show the response messages. It will help developers to
integrate business applications and Bluetooth devices with the Cassia Bluetooth router and
AC. This tool is available here: [Cassia Bluetooth Debug Tool v1](http://www.bluetooth.tech/debugger)

There is a new beta version of the Bluetooth Debug Tool (v2) available here:
[Cassia Bluetooth Debug Tool v2](http://www.bluetooth.tech/debugger2/dist)

- **WARNING:** Starting with the v2.0.3 release, CORS is disabled by default on AC and Router. When using this Bluetooth Debug Tool, please set ‘Access Control Allow Origin’ in the console setting. Please refer to [Debugger2-Troubleshooting](http://www.bluetooth.tech/debugger2/dist/Debugger2-Troubleshooting.pdf) for detailed instructions. If using Chrome version>=94, please copy this link and open it in Chrome chrome://flags/#block-insecure-private-network-requests is set to Disabled. -

<br />

![Figure 1](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/debug_toolv1.png)

**Figure 1: Cassia Bluetooth Debug Tool v1**

<br />

![Figure 2](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/debug_toolv2.png)

**Figure 2: Cassia Bluetooth Debug Tool v2**

<br />

**API Info**: This area contains the user's most commonly used commands. When you click on
the buttons, the parameters, and descriptions related to that command will pop up.

**Scan List**: When turning on this option, the Cassia router will start scanning for all BLE devices within its range. The BLE devices need to be in broadcast mode. You can connect to
one or multiple devices.

**Device and Services List**: Turn on this option to see the connected devices. Based on the
device’s Bluetooth profile, you can write a value or turn on the notifications.

**Notify List**: If you have turned on the notification of the correct handle, you will see a stream
of raw data flowing in the Notify List window.

<br />

(Example image is still being worked on.)

**Figure 3: Cassia Bluetooth Debug Tool Example**

<br />

From firmware 1.4, Cassia AC integrates with Bluetooth debug tool. For more information, please
check Cassia AC Bluetooth Debug Tool User Guide at: <br />
[https://www.cassianetworks.com/knowledge-base/general-documents](https://www.cassianetworks.com/knowledge-base/general-documents/)