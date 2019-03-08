#### JavaScript
```javascript
var credentials = {
  id: 'tester',
  secret: '816213f8b5c2877d'
};
var access_token = '';
var request = require('request');
var options = {
  url : 'http://demo.cassia.pro/api/oauth2/token',
  method : 'POST',
  form : {'grant_type' : 'client_credentials'},
  headers : {
    Authorization : 'Basic ' + new Buffer(credentials.id + ':' + credentials.secret, 'ascii').toString('base64'),
  }
};

request(options, function(error, req, body) {
 if (error) {
   console.log(error);
   return;
 }
 var data = JSON.parse(body);
 access_token = data.access_token;
 console.log(data);
 var options = {
   url : 'http://demo.cassia.pro/api/client', //you can change this to the IP address and port your Router is using.
   method : 'GET',
   // form : {'grant_type' : 'client_credentials'},
   headers : {
     Authorization : 'Bearer ' + access_token,
   }
 };
});

request(options, function(error, request, body) {
 console.log(body);
});
```