For users who have migrated from C1000-2B version to X1000, you need to make two
changes: change host and add “/api” to the beginning of the URL. See below for an example.

Example code on C1000-2B
#### JavaScript
```javascript
var host = "http://api.cassianetworks.com";
// <- Get token here.
$.ajax({url: host+"/oauth2/token", headers: headers, type:"post", success: function(data){
// ...
}});
```

In X1000/E1000/C1000/S Series, you use it this way:
#### JavaScript
```javascript
var host = "http://demo.cassia.pro/api";
// <- Get token here.
$.ajax({url: host+"/oauth2/token", headers: headers, type:"post", success: function(data){
// ...
}});
```

or

```
POST api/oauth2/token HTTP/1.1
Host: demo.cassia.pro
Headers: {Authorization: Basic dGVzdGVyOjEwYjgzZjlhMmU4MjNjNDc=
Content-Type: application/x-www-form-urlencoded}
Body:
{grant_type=client_credentials}
```