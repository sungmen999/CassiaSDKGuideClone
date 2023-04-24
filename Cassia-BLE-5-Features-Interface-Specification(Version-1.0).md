# 1.1 Update PHY for BLE connection

After BLE link is established, PHY can be updated to 1M, 2M, CODED for transmitting and/or receiving with the following API. 

## URL：'/gap/nodes/<node>/phy'

## Input Parameters:

| Parameter | Description |
|-----------|-------------|
|Tx| 			Requested PHY type for transmitting, can be set to 1M, 2M, CODED, or any combination of comma separated values.|
|Rx| 			Requested PHY type for receiving, can be set to 1M, 2M, CODED, or any combination of comma separated values.|
|coded_option*|	When requested PHY type is CODED, can be set to 0, 1, 2 for Tx.(0: auto negotiation; 1: S2,  2: S8) |
* coded_option for Rx is decided by the device.
Return Value: 
Actual PHY type decided by device

## Example:
1.  After successfully establish BLE link by calling connection API with the default 1M PHY, 
    (POST http://{router ip}/gap/nodes/<node>/connection), 
    update PHY to long range mode with S=8
```shell
curl -v -X POST -H "content-type: application/json" -d '{"tx":"CODED","coded_option":"2"}' 'http://127.0.0.1/gap/nodes/00:12:6F:5B:17:2F/phy'
{"id":"00:12:6F:5B:17:2F","rx_phy":"CODED","tx_phy":"CODED"}
```
2. Updating PHY to 2M for transmitting.
```shell
curl -v -X POST -H "content-type: application/json" -d '{"tx":"2M,1M","rx":"1M"}' 'http://127.0.0.1/gap/nodes/00:12:6F:5B:17:2F/phy'
{"id":"00:12:6F:5B:17:2F","rx_phy":"1M","tx_phy":"2M"}
```

3. Get PHY and code option information.
```shell
curl -v  'http://127.0.0.1/gap/nodes/00:12:6F:5B:17:2F/phy' 
```
example 1：
{""tx_op"":""S2"",""tx_phy"":""CODED"",""id"":""CC:1B:E0:E2:34:83"",""rx_phy"":""CODED"",""rx_op"":""S2""}

example 2：
{""tx_op"":""S2"",""tx_phy"":""CODED"",""id"":""CC:1B:E0:E2:32:B7"",""rx_phy"":""CODED"",""rx_op"":""S2""}

example 3：
{""tx_op"":""S2"",""tx_phy"":""CODED"",""id"":""CC:1B:E0:E2:34:83"",""rx_phy"":""CODED"",""rx_op"":""S8""}

example 4：
{""tx_op"":""S8"",""tx_phy"":""CODED"",""id"":""CC:1B:E0:E2:32:B7"",""rx_phy"":""CODED"",""rx_op"":""S2""}

# 2.1 Extended Advertising Feature - Scan

## URL: /gap/nodes/?active=1&event=1&chip=0&phy=1M,2M,CODED

## Input Parameters:
| Parameter | Description |
|-----------|-------------|
|phy| Requested PHY type, can be set to 1M, 2M or CODED, or any combination of comma separated values.|

## Data received:

1. For legacy BLE 4 advertising, data format is not changed. No primaryPhy info in the packet. 
Event Type (evtType) is 0 ~ 4

| Value | Parameter Description |
|-----------|-------------|
|0x00|Connectable undirected advertising (ADV_IND)|
|0x01|Connectable directed advertising (ADV_DIRECT_IND)|
|0x02|Scannable undirected advertising (ADV_SCAN_IND)|
|0x03|Non connectable undirected advertising (ADV_NONCONN_IND)|
|0x04|Scan Response (SCAN_RSP)|

2. For extended advertising, the following new format with evtType=5 will be used.

``` data: {"bdaddrs":[{"bdaddr":"50:31:AD:03:0F:ED","bdaddrType":"public"}],"adData":"020106030399FD06FFBD06011111100943323030303030383145000000","name":"xxxxxxxxx","rssi":-19,"evtType":5,"primaryPhy":"1M","secondryPhy":"2M","sid":0,"txPower":127,"interval":0,"prop":"Connectable","dataStatus":"Complete"} ```

# 2.2 Extended Advertising Feature - Advertising
 Start advertising: 
## URL: /advertise/ext/start 
## Method: POST 
## Input Parameters:
| Parameter | Description |
|-----------|-------------|
|chip| BLE chip, optional, default value 0 |
|instance_id|instance id, optional, range 0-1, to use existing id,default value is 0xff，to create new id.
|properties|advertising type flag，(Connectable、Scannable、Directed、High_Duty、Legacy、Omit_address、TxPower)，default value is 0 (no scannable no connectable)|
|channelmap|advertising channel, default value is 7 (channel 37,38,39)|
|interval|advertising interval, default value is 1000ms |
|ad_data|adv data, maximum length 31 bytes in 2.2.0 (251 bytes in future), optional.  |     		|scan_data|scan response data, maximum length 31 bytes in 2.2.0 (251 bytes in future), optional |	|primary_phy|1M or CODED|
|secondary_phy|1M or CODED, default is 1M.|
|random_address|mandatory, random MAC address|
|tx_power|optional, default is 127. Range from -127 ! 127.|

##  Return Value：
200 + OK or 500 + error description  
## Example:
```shell
curl -v -X POST -H "content-type: application/json" -d '{"ad_data":"0201060C094341535349412D42454143", "instance_id":"0", "chip":"1", "properties":"Connectable", "primary_phy":"1M", "secondary_phy":"2M", "interval":"100", "random_address":"07:6A:80:BD:31:37"}' 'http://127.0.0.1/advertise/ext/start'
```

Stop advertising: 
## URL: /advertise/ext/stop 
## Method: GET 
## Input Parameter：
| Parameter | Description |
|-----------|-------------|
|chip|BLE chip, default is 0|
|instance_id|instance ID (0-7), 0xFF is to stop all. mandatory|

##  Return Value：
200 + OK or 500 + error description

# 3.1 Establish Connection with 2M or CODED PHY

## URL: /gap/nodes/:node/connection
## Method: POST
## Input Parameters:
| Parameter | Description |
|-----------|-------------|
|phy|Requested PHY type, can be set to 1M or CODED, or any combination of comma separated values. Default is 1M.|
|secondary_phy(optional)| make connection with PHY type only.|

## Data received:
No change on receiving data.

## Example:
```shell
curl -v -X POST -H "content-type: application/json" -d '{"phy":"CODED","type":"random"}' 'http://127.0.0.1/gap/nodes/07:6A:80:BD:31:37/connection?chip=1'
```

A new parameter 'secondary_phy' is added for connection API to make connection with specific PHY only. 1. scan extended adv packet with primary phy: 1M and secondary phy: 2M, and make connection on 2M only. 
```curl -v -X POST -H "content-type: application/json" -d '{"phy":"1M","type":"random","secondary_phy":"2M"}' 'http://127.0.0.1/gap/nodes/07:6A:80:BD:31:37/connection?chip=0' ```
2. scan extended adv packet with primary phy: CODED and secondary phy: 1M, and make connection on 1M only.
 ```curl -v -X POST -H "content-type: application/json" -d '{"phy":"CODED","type":"random","secondary_phy":"1M"}' 'http://127.0.0.1/gap/nodes/07:6A:80:BD:31:37/connection?chip=0' ```
3. scan extended adv packet with primary phy: CODED and secondary phy: CODED, and make connection on CODED only. 
` curl -v -X POST -H "content-type: application/json" -d '{"phy":"CODED","type":"random","secondary_phy":"CODED"}' 'http://127.0.0.1/gap/nodes/07:6A:80:BD:31:37/connection?chip=0'`

# 3.2 Connection List

## URL: /gap/nodes
## Method: GET
## Data received:
add tx_phy and rx_phy on receiving data.

## Example:
```shell
curl 'http://127.0.0.1/gap/nodes'
{"nodes":[{"type":"random","chipId":0,"tx_phy":"1M","id":"64:45:35:36:0D:EA","pairStatus":"none","bdaddrs":{"bdaddr":"64:45:35:36:0D:EA","bdaddrType":"random"},"handle":"","rx_phy":"1M","connectionState":"connected","name":""} 
```

# 3.3 Examples to establish Connection with 2M or CODED PHY

## Scenario 1: Device supports both 1M and 2M PHY 

1. Device send legacy advertising packet only

Container APP sends connection request for 1M phy
Container APP sends PHY update request to 2M 

Question 1: is it more battery friendly to advertising on 1M only and update to 2M later? In this case, in which way container app/gateway knows the sensor supports both 1M and 2M and can be updated to 2M?
Question 2: 2M phy has smaller range than 1M, in case there is connection problem after update to 2M, container app/gateway needs to avoid updating to 2M continuously.

2. Device sends 2 kinds of advertising packet simultaneously with the same MAC address, 
Legacy (No primary and secondary PHY info)
primary PHY : 1M,  secondary PHY : 2M

When container APP sends connection request without setting phy, Gateway will make connection on 1M.
When container APP send connection request with phy setting to value of ‘primary PHY’, Gateway will scan both legacy and extended advertising and establish connection on 1M or 2M PHY.

Note 1: In release 2.1.1, the selection of 1M and 2M for connection depends on advertising packets received by gateway. In later release, algorithm will be implemented to optimize selection of PHY according to conditions such as sensor’s signal strength, environment noise level, retransmit times, etc. 


## Scenario 2: Device supports both 1M and CODED PHY 

Device sends 2 kinds of advertising packet simultaneously with the same MAC address,
legacy adv packet without primary and secondary PHY info
primary PHY : CODED,  secondary PHY : CODED
           
Container APP sends connection request with phy setting to any of the following values,  i) “1M”, ii) “CODED”, iii) “1M,CODED”

In case of i) and ii), gateway will establish corresponding BLE connection based on input “1M” or “CODED”.

In case of iii) “1M, CODED”, gateway will scan both legacy and extended advertising and establish connection on 1M or CODED.
Note 1: In release 2.1.1, the selection of 1M or CODED for connection depends on advertising packets received by gateway. In later release, algorithm will be implemented to optimize selection of PHY according to conditions such as sensor’s signal strength, environment noise level, retransmit times, etc. 
