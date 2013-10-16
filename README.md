# alldata-peer-server-http

_Stability: 1 - [Experimental](https://github.com/tristanls/stability-index#stability-1---experimental)_

[![NPM version](https://badge.fury.io/js/alldata-peer-server-http.png)](http://npmjs.org/package/alldata-peer-server-http)

Peer Server HTTP module for [AllData](https://github.com/tristanls/alldata), a distributed master-less write-once immutable event store database implementing "All Data" part of [Lambda Architecture](http://www.slideshare.net/nathanmarz/runaway-complexity-in-big-data-and-a-plan-to-stop-it).

## Usage

```javascript
var AllDataPeerServer = require('alldata-peer-server-http');
var allDataPeerServer = new AllDataPeerServer({
    hostname: 'localhost',
    port: 8080
});

allDataPeerServer.on('_put', function (key, event, callback) {
    console.log('received request to _put: ' + key + ' ' + event);
    // process the _put
    callback(); // indicate success
});

allDataPeerServer.listen(function () {
    console.log('server listening...'); 
});
```

## Test

    npm test

## Overview

AllDataPeerServer will listen to HTTP `POST` requests containing the `_put` request key and event encoded as a JSON string in the POST body.

## Documentation

### AllDataServer

**Public API**

  * [AllDataPeerServer.listen(options, \[callback\])](#alldatapeerserverlistenoptions-callback)
  * [new AllDataPeerServer(options)](#new-alldatapeerserveroptions)
  * [allDataPeerServer.close(\[callback\])](#alldatapeerserverclosecallback)
  * [allDataPeerServer.listen(\[callback\])](#alldatapeerserverlistencallback)
  * [Event '_put'](#event-_put)

### AllDataPeerServer.listen(options, [callback])

  * `options`: See `new AllDataPeerServer(options)` `options`.
  * `callback`: See `allDataPeerServer.listen(callback)` `callback`.
  * Return: _Object_ An instance of AllDataPeerServer with server running.

Creates new AllDataPeerServer and starts the server.

### new AllDataPeerServer(options)

  * `options`: _Object_
    * `hostname`: _String_ _(Default: undefined)_ Hostname for the server to listen on. If not specified, the server will accept connections directed to any IPv4 address (`INADDR_ANY`).
    * `port`: _Integer_ _(Default: 8080)_ Port number for the server to listen on.

Creates a new AllDataPeerServer instance.

### allDataPeerServer.close([callback])

  * `callback`: _Function_ _(Default: undefined)_ `function () {}` Optional callback to call once the server is stopped.

Stops the server from accepting new connections.

### allDataPeerServer.listen([callback])

  * `callback`: _Function_ _(Default: undefined)_ `function () {}` Optional callback to call once the server is up.

Starts the server to listen to new connections.

### Event `_put`

  * `function (key, event, callback) {}`
    * `key`: _String_ AllData key generated for the `event`.
    * `event`: _Object_ JavaScript object representation of the event to `_put`.
    * `callback`: _Function_ `function (error) {}` The callback to call with an error or success of the `_put` operation.

Emitted when the server receives a new `_put` request from a peer client.

Signal error via `callback(true)` and success via `callback()`.

```javascript
allDataPeerServer.on('_put', function (key, event, callback) {
    console.log('received request to put: ' + key + ' ' + event); 
    // ... process the put
    callback();
});
```