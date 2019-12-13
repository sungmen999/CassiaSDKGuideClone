## Setting Up Postman for AC-Managed Cassia RESTful API
### Download Postman
[https://www.getpostman.com/downloads](https://www.getpostman.com/downloads/)

For this guide, we are using: Postman Version 7.13.0

### Postman Initial Setup
Once Postman is finished installing, open the application.
A screen asking to create an account or sign in will appear. (It is optional to sign in or not.)

![Postman Initial Screen](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p1.png)

<br>
This is an example of a plain Postman workspace after you open the application for the first time:

![Plain Postman Workspace](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p2.png)
<br>

### Request a Cassia AC RESTful API Access Token
At the top-left corner, there should be a "New" button.
Click on it and click on "Request".

![New Request Button](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p3.png)

<br>
A pop-up window may ask for you to save this request into a collection. Create a collection and save the request into it. 

This is an example of saving a Postman request into a collection called "Authorization":

![Save Request into Authorization Collection](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p4.png)

<br>

A new tab should appear in the workspace. Change the default "GET" request type to "POST".

![Request Type from GET to POST](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p5.png)

<br>

Then, fill in the settings like in the following screenshots:

![p6](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p6.png)

![p7](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p7.png)

![p8](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p8.png)

<br>

In Postman, use Basic Auth to get an OAuth 2.0 bearer token. 

This will set the HTTP Authorization header.

Just enter the Developer Key and Secret in the Postman Basic Auth fields.

![p9](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p9.png)

<br>

Setting the Authorization header manually is also fine. 

To do this, use a base64 encoding tool to generate an encoding based on the the Developer Key and Secret (key:secret).

Here is an example Python script to generate the base64 encoding for Basic Auth:

```python

import base64

id_secret = 'cassia:cassia'
encoded_id_secret = base64.b64encode(id_secret.encode())

print(encoded_id_secret.decode())

```

![p10](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p10.png)

<br>

Now, set the rest of the settings like the following screenshots:

![p11](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p11.png)

![p12](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p12.png)

![p13](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p13.png)

![p14](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p14.png)

![p15](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p15.png)

<br>

Alternatively, you can set the Authorization header like so to access the AC RESTful API:

![p16](https://github.com/CassiaNetworks/CassiaSDKGuideResources/blob/develop/images/postman_guide/p16.png)

<br>

<br>

## Setting Up Postman for Standalone (Local) Cassia RESTful API
This section is coming soon...