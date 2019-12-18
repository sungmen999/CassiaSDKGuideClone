```javascript
const request = require('superagent');

const requestTimeout = 5000;
const refreshCycle = 30 * 60;

// ac configuration
const acConf = {
  host: 'http://192.168.0.226',
  developer: 'cassia',
  secret: 'cassia'
};

let token = null;

const apiRouters = {
  TOKEN: `${acConf.host}/api/oauth2/token`,
  APS_STATUS: `${acConf.host}/api/cassia/hubs`,
};

// get auth header
function _getAuthHeader() {
  let auth = `${acConf.developer}:${acConf.secret}`;
  auth = new Buffer(auth).toString('base64');
  return {Authorization: `Basic ${auth}`};
}

// get token header
function _getTokenHeader() {
  return  {Authorization : `Bearer ${token}`};
}

// get token
function _apiGetAcToken(callback) {
  request.post(apiRouters.TOKEN)
    .timeout(requestTimeout) // time out
    .retry(3) // try 3 times
    .set(_getAuthHeader())
    .send({grant_type: 'client_credentials'})
    .end(function(err, res) {
      return callback(err, res.body.access_token);
    });
}

// get token in every 30 minutes
setInterval(function() {
  _apiGetAcToken(function(err, ret) {
    if (err) return console.log(err);
    token = ret;
  })
}, refreshCycle * 1000);


// get the status of all router
function apiGetApsStatus(callback) {
  return request.get(apiRouters.APS_STATUS)
    .timeout(requestTimeout)
    .set(_getTokenHeader())
    .end(function(err, res) {
      return callback(err, res.body);
    });
}

// example
_apiGetAcToken(function(err, ret) {
  if (err) return console.log(err);
  token = ret;
  apiGetApsStatus(function(err, res) {
    console.log(err, res);
  });
})
```