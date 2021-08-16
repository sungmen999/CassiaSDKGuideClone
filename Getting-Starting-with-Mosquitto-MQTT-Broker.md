# Setting up the MQTT Broker Server
## MacOS
### Installing via Homebrew

Update homebrew. (The --verbose flag just makes it easier to know the progress of the update process.)

```
brew update --verbose
```

Install Mosquitto.

```
brew install mosquitto
```

After installing Mosquitto, there should be a mosquitto.conf file located in the /usr/local/etc/mosquitto folder. Open it with a text editor.

```
vim /usr/local/etc/mosquitto/mosquitto.conf
```

Add the following lines to mosquitto.conf:

```
listener 1883
allow_anonymous true
```

NOTE: The `allow_anonymous true` setting is just used for evaluating the MQTT Bypass feature. Without this, you might struggle with `Client <unknown> disconnected, not authorised.` errors. It's better to define the IP address of the gateways and machines that you want data from.

NOTE: Port 8883 is reserved for TLS encrypted MQTT connections. The client will load the system CA certificates and use TLS mode if port 8883 is used without specifying your own certificates. It is recommended to either use port 1883 or add TLS encryption to the port 8883 listener.

Save the file.

Run Mosquitto using the following command:

```
/usr/local/sbin/mosquitto -c /usr/local/etc/mosquitto/mosquitto.conf
```

The terminal should show something like this:
```
1629147983: mosquitto version 2.0.10 starting
1629147983: Config loaded from /usr/local/etc/mosquitto/mosquitto.conf.
1629147983: Opening ipv6 listen socket on port 1883.
1629147983: Opening ipv4 listen socket on port 1883.
1629147983: mosquitto version 2.0.10 running
```


# Setting up the MQTT Client
## MacOS localhost
For an easy to evaluate the Cassia Gateways MQTT Bypass feature, you can set up the MQTT client locally.
From the terminal, just run the following command:

```
mosquitto_sub -t scanTopic -p 1883 -h localhost
```

NOTE: It is always recommended to have a more secure connection by adding a username and password to the MQTT communication, but for the purpose of evaluating the MQTT Bypass feature quickly, we recommend this command.

After running the mosquitto_sub command, in the terminal running the MQTT broker server, you should see something like this:
```
1629148763: New connection from ::1:61446 on port 1883.
1629148763: New client connected from ::1:61446 as auto-56DBE474-A6B8-16FC-770A-24FD6F34B7D7 (p2, c1, k60).
```

To test the `scanTopic` topic, you can run the mosquitto_pub command:

```
mosquitto_pub -h localhost -m "testing" -t scanTopic
```

After running this command, in the MQTT broker server terminal window, you should see something like this:

```
1629149048: New connection from ::1:63050 on port 1883.
1629149048: New client connected from ::1:63050 as auto-AAF2A6B3-A9C9-E218-8C88-31F6DC319D84 (p2, c1, k60).
1629149048: Client auto-AAF2A6B3-A9C9-E218-8C88-31F6DC319D84 disconnected.
```

In the MQTT client terminal window, you should see the word "testing" being outputted to the screen like this:

```
testing
```


# Configuring the Cassia Gateway to use the Mosquitto MQTT Broker
## Configuring the Cassia Gateway MQTT Bypass Feature
### Standalone Gateway Mode
![Standalone MQTT Bypass 1](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/mqtt_bypass_local_1.png)
![Standalone MQTT Bypass 2](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/mqtt_bypass_local_2.png)
![Standalone MQTT Bypass 3](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/mqtt_bypass_local_3.png)
### AC Managed Gateway Mode
![AC MQTT Bypass 1](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/mqtt_bypass_ac_1.png)
![AC MQTT Bypass 2](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/mqtt_bypass_ac_2.png)
![AC MQTT Bypass 3](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/mqtt_bypass_ac_3.png)

Click on Apply (Standalone Gateway Mode) or Save (AC Managed Gateway Mode) to apply the MQTT Bypass settings.

After a while, in the MQTT Broker terminal window, you should see something like this:
```
1629150774: New connection from 192.168.4.26:54490 on port 1883.
1629150774: New client connected from 192.168.4.26:54490 as cc:1b:e0:e0:fc:70-0 (p1, c1, k60).
```

In the MQTT Client terminal window, there should be a continuous stream of data from the `scanTopic` topic.

# Closing Notes
This guide is just to get you started with the Mosquitto MQTT broker.
To learn more about what you can do with the Mosquitto MQTT broker, please visit this website: [https://mosquitto.org](https://mosquitto.org)

Please feel free to reach out to [Cassia Support](https://www.cassianetworks.com/support) for any questions.

