Please setup the Cassia router, connect the router to Cassia AC or run the router in
standalone mode.

For more information, please check Router Quick Start Guide, Private AC
Server Installation Guide, and Cassia User Manual at:\
[https://www.cassianetworks.com/knowledge-base/general-documents](https://www.cassianetworks.com/knowledge-base/general-documents)

Now you can operate your router to do certain tasks with your Bluetooth devices through the RESTful API.

### [Access Local Router](#access-local-router)
Your application can access local Cassia routers directly (usually in the same network), instead of through Cassia AC. Below is an example of running RESTful API in a web browser to access Cassia router in local network (for debug purpose).

<br />

![Figure 5](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f5.jpg)

<p style="text-align:center">

**Figure 5: Access local Cassia router**

</p>

<br />

### [Access Cassia Router through the Cassia AC](#access-cassia-router-through-the-cassia-ac)
Before starting to use RESTful API’s through the Cassia AC, you will need developer
credentials (a Developer Key and a Developer Secret). It is not needed for a RESTful API on a
local router. These developer credentials authorize the remote control of the Cassia
Bluetooth router. To request your developer credentials, please contact the Cassia support
team at support@cassianetworks.com or contact your sales representative.

Here is a sample:
```
client_id:tester, secret:198c776539c41234
```

Then, you need to follow below steps:
  * Do an OAuth2.0 authentication with the AC using developer credentials granted. For
example: you have a developer ID: tester, secret: 10b83f9a2e823c47, use base64 to
encode string "tester:10b83f9a2e823c47" and get
"dGVzdGVyOjEwYjgzZjlhMmU4MjNjNDc="
  * Authenticate the user identity using the following HTTP request, taking
demo.cassia.pro as your AC server as an example.
```
POST api/oauth2/token HTTP/1.1
Host: demo.cassia.pro
Headers: {Authorization: Basic dGVzdGVyOjEwYjgzZjlhMmU4MjNjNDc=
Content-Type: application/x-www-form-urlencoded}
Body:
{grant_type=client_credentials}
```
  * If everything goes well, you will get a response like this, which includes access_token:
```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{ token_type: 'bearer',
access_token:
'2b6ced831413685ec33204abc2a9a476310a852f53a763b72c854fd7708499f1bc0b362
6bfcfef2a2cfe0519356c9d7cb1b514243cb29f60e76b92d4a64ea8bd',
expires_in: 3600 }
```
  * Now you can use access_token to access the other RESTful APIs by appending an
access_token parameter. For example:<br/>
```http://demo.cassia.pro/api/gap/nodes?event=1&mac=<routermac>&access_token=xxx```<br/>
Or, you can add {Authorization : 'Bearer ' + access_token } in HTTP headers.

**NOTE**: Make sure to append “/api” after {your AC domain} and add “mac=<mac>” to
identify which router is used.