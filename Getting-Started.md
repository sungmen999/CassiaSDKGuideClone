Please setup the Cassia router, connect the router to Cassia AC or run the router in
standalone mode.

For more information, please check Router Quick Start Guide, Private AC
Server Installation Guide, and Cassia User Manual at:\
[https://www.cassianetworks.com/knowledge-base/general-documents](https://www.cassianetworks.com/knowledge-base/general-documents)

Now you can operate your router to do certain tasks with your Bluetooth devices through the RESTful API.

### [Access Local Router](#access-local-router)
Your application can access local Cassia routers directly (usually in the same network), instead of through Cassia AC. Please turn on Local RESTful API in advance according to [Two Sets of RESTful APIs](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Cassia-Router-Overview#two-sets-of-restful-apis).

Below is an example of running the RESTful API in a web browser to access the Cassia router in a local network for debugging purpose.

<br />

![Figure 5](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/master/images/f5.jpg)

<p style="text-align:center">

**Figure 5: Access local Cassia router**

</p>

<br />

### [Access Cassia Router through the Cassia AC](#access-cassia-router-through-the-cassia-ac)
**NOTE**:
  * The steps below are mandatory before using any AC RESTful API.
  * The access_token will expire in 3600 seconds. Please update it periodically.

Before starting to use RESTful API’s through the Cassia AC, you will need developer
credentials (a Developer Key and a Developer Secret). It is not needed for a RESTful API on a
local router. These developer credentials authorize the remote control of the Cassia
Bluetooth router. To request your developer credentials, please contact the Cassia support
team at support@cassianetworks.com or contact your sales representative.

Here is a sample:
```
key:tester, secret:198c776539c41234
```

Then, you need to follow the steps below to get the access_token:
  * Do an OAuth 2.0 authentication with the AC using developer credentials granted. For example: you have a developer key: tester, secret: 10b83f9a2e823c47, use base64 to encode string "tester:10b83f9a2e823c47" and get "dGVzdGVyOjEwYjgzZjlhMmU4MjNjNDc="
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
```http://demo.cassia.pro/api/gap/nodes?event=1&mac=<router-mac>&access_token=xxx```<br/>
Or, you can add {Authorization : 'Bearer ' + access_token } in the HTTP headers.

  * Please update access_token periodically before it expires (3600 seconds).<br>
    Please check [Sample Code to Update Access Token](https://github.com/CassiaNetworks/CassiaSDKGuide/wiki/Sample-Code-to-Update-Access-Token) for an example of updating the token every 30 minutes. The "refreshCycle = 30 * 60" can be changed to another cycle period.

  * When the access_token expires or the access_token is invald, the AC RESTful API calls using that token will return an HTTP 403 Forbidden status code. There should be a JSON response like the following if the token is expired or invalid:

```json
// 20200423141128
// http://demo.cassia.pro/api/gap/nodes?event=1&mac=<router-mac>&access_token=<access_token>

{
  "error": "forbidden",
  "error_description": "Token not found or expired"
}
```

**NOTE**: Make sure to append “/api” after {your AC domain} and add “mac=<mac>” to
identify which router is used.

### [Enable OAuth Token for Local Router](#local-router-oauth)
You can turn on OAuth2 token Authentication on Router's Web Page.

It's almost the same as oauth2 token used on AC except the url to get token is "oauth2/token"(without api part)
* How to obtain oauth token:
```
POST oauth2/token HTTP/1.1
Host: demo.cassia.pro
Authorization: Basic dGVzdGVyOjEwYjgzZjlhMmU4MjNjNDc=
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
```

* you can use "curl" command for test(use "--user" option, it will encode Authorization Header for you):
```
curl --user '{username}:{password}' -d 'grant_type=client_credentials' http://{router's ip}/oauth2/token
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
access_token parameter in url query(access_token=xxx) or add Header in request (Authorization : Bearer xxx)