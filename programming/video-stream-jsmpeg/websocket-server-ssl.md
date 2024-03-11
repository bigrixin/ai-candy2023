# Websocket-Server (SSL)

```
// Websocket-relay.js

// Use the websocket-relay to serve a raw MPEG-TS over WebSockets. You can use
// ffmpeg to feed the relay. ffmpeg -> websocket-relay -> browser
// Example:
// node websocket-relay yoursecret 8081 8082
// ffmpeg -i <some input> -f mpegts https://localhost:8081/yoursecret
var http = require('http');
var fs = require('fs'),
    https = require('https'),
    WebSocket = require('ws');
	
//var httpProxy = require('http-proxy');
var express = require('express');

//var proxy = httpProxy.createProxyServer({ ws: true });
var app = express();

var dir = 'recordings';
var recordFileName = dir;
recordFileName += '/';
recordFileName += Date.now();
recordFileName += '.ts'


if (process.argv.length < 3) {
    console.log(
        'Usage: \n' +
        'node websocket-relay-s.js <secret> [<stream-port> <websocket-port>]'
    );
    process.exit();
}

var STREAM_SECRET = process.argv[2],
    STREAM_PORT = process.argv[3] || 8081,
    WEBSOCKET_PORT = process.argv[4] || 8082,
    RECORD_STREAM = false;

var privateKey = fs.readFileSync('key-rsa.pem', 'utf8');
var certificate = fs.readFileSync('cert.pem', 'utf8');

if (privateKey != null && certificate != null) {
    credentials = {
        key: privateKey,
        cert: certificate
    };
    console.log('Private key and Certificate found! \n');
} else {
    console.log('Private key and/or Certificate not found!');
    process.exit();
}


// test server  https://192.168.0.168:8082/ 
app.get('/', (req, res) => {
    res.send('This is an secure server');
}); 

//------out put------------------------------------------------------------------------

//  Set up routes (and middlewares if we had any)
var router = express.Router();
router.all('/', (req, res) => res.send('Hi there!'));
app.use('/', router);


// Websocket Server
var socketServer = null;

if (credentials != null)
{
	
	var httpsServer = https.createServer(credentials, app);
	
//	httpsServer.on('upgrade', function (req, socket, head) {
//		 console.log(' work...???');
//        proxy.ws(req, socket, head);
//    });
	
	httpsServer.listen(WEBSOCKET_PORT, err => {
	   if (err) {
	    	console.log('Well, this didn\'t work...');
		    process.exit();
	   }
	   console.log('WebSocket Server is listening on port: ' + WEBSOCKET_PORT);
	});
	 
 
	socketServer = new WebSocket.Server({
        server: httpsServer  // , perMessageDeflate: false
     });
}
else
{
	var httpServer = http.createServer(app);
	httpServer.listen(WEBSOCKET_PORT);	
    socketServer = new WebSocket.Server({
		port: WEBSOCKET_PORT,
		perMessageDeflate: false
	 });
}

 

socketServer.connectionCount = 0;

socketServer.on('connection', function(socket, upgradeReq) {
    console.log('---------Client connected------------- \n');
	
    socketServer.connectionCount++;
	
    console.log(
        'New WebSocket Connection: ',
        (upgradeReq || socket.upgradeReq).socket.remoteAddress,
        (upgradeReq || socket.upgradeReq).headers['user-agent'],
        '(' + socketServer.connectionCount + ' total)'
    );

    socket.on('close', function(code, message) {
        socketServer.connectionCount--;
        console.log(
            'Disconnected WebSocket (' + socketServer.connectionCount + ' total)'
        );
    });
	
	socket.on('error', function(e) {
        console.log('Error, client disconnected ' + e);
   });
});

socketServer.on('message', msg => {
  console.log('Client said: ' + msg.toString()); 
});

socketServer.broadcast = function(data) {
    socketServer.clients.forEach(function each(client) {
        if (client.readyState === WebSocket.OPEN) {
            client.send(data, {binary: true});
        }
    });
};




//------in come-----------------------------------------------------------------------

// HTTPS Server to accept incomming MPEG-TS Stream from ffmpeg
var streamServer = https.createServer(credentials, function(request, response) {
    // check param
    var params = request.url.substr(1).split('/');

    if (params[0] !== STREAM_SECRET) {
        console.log(
            'Failed Stream Connection: ' + request.socket.remoteAddress + ':' +
            request.socket.remotePort + ' - wrong secret.'
        );
        response.end();
    }

    response.connection.setTimeout(0);

    console.log(
        'Stream Connected: ' + request.socket.remoteAddress + ':' + request.socket.remotePort
    );

    // Broadcast data
    request.on('data', function(data) {
        socketServer.broadcast(data);
        if (request.socket.recording) {
            request.socket.recording.write(data);
        }
    });

    request.on('end', function() {
        console.log('close');
        if (request.socket.recording) {
            request.socket.recording.close();
        }
    });

    // Record the stream to a local file?
    if (RECORD_STREAM) {
        console.log('recording data ......\n');
        if (!fs.existsSync(dir)) {
            fs.mkdirSync(dir);
        }

        request.socket.recording = fs.createWriteStream(recordFileName);
    }
}) 

// Keep the socket open for streaming
streamServer.headersTimeout = 0;
streamServer.listen(STREAM_PORT);
//------------------------------------------------------------------------------


console.log('Listening for incomming MPEG-TS Stream on https://127.0.0.1:' + STREAM_PORT + '/<secret>');
console.log('Awaiting WebSocket connections on wss://127.0.0.1:' + WEBSOCKET_PORT + '/');



```
