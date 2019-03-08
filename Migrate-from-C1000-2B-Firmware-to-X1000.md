For users who have migrated from C1000-2B version to X1000, you need to make two
changes: change host and add “/api” to the beginning of the URL. See below for an example.

Example code on C1000-2B
```javascript
var host = "http://api.cassianetworks.com";
// <- Get token here.
$.ajax({url: host+"/oauth2/token", headers: headers, type:"post", success: function(data){
// ...
}});
```