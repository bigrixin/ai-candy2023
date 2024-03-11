# Play RTSP video stream



RTSP video stream

```
// Some code

RTSP video stream include Server and Client
  -- Server side 
     Node / Javascript 
	 
  -- Client side
     HTML / Javascript 
	 Angular
	 
	 
=================================================================================
 RTSP Server 
=================================================================================
	1. installation 

	   -- FFMPEG
		1. download FFmpeg
		   https://ffmpeg.org/download.html

		2. \ffmpeg\bin\ffmpeg  add rule in firewall and path

	   -- npm i node-rtsp-stream-jsmpeg  or    
		   -- npm install node-rtsp-stream   v0.0.9  (only use this)

			server.js

			  var Stream = require('node-rtsp-stream')
			  var stream = new Stream({
			    name: 'socket',
			    streamUrl: 'rtsp://admin:*****@192.168.0.35:554/cam/realmonitor?channel=1&subtype=1',
			    wsPort: 8080,
			    ffmpegOptions:{
			        '-stats': '',
			        '-r': 20,
			        '-s': '800 600'
			    }
			  })  

	2. run RTSP server:

		 node server.js

=================================================================================
 Client - html version
=================================================================================
    Index.html

	<!DOCTYPE html>
	<head>
	  <meta charset="utf-8">
	  <meta name="viewport" content="width=device-width">
	  <title>DEMO node-rtsp-stream-jsmpeg</title>
	  <script src="https://jsmpeg.com/jsmpeg.min.js"></script>
	</head>
	<body>
	  <div>
		<canvas id="video-canvas">
		</canvas>
	  </div>

	  <script type="text/javascript">
	  var url = "ws://localhost:8080";
	  var canvas = document.getElementById('video-canvas');
	  var player = new JSMpeg.Player(url, {canvas: canvas});
	  </script>
	</body>

=================================================================================
 Client - Angular version - @cycjimmy/jsmpeg-playe V6.05 
=================================================================================

	1. npm install @cycjimmy/jsmpeg-player --save

	2. component.ts

	   import JSMpeg from '@cycjimmy/jsmpeg-player';

	   ngOnInit(): void {
		 new JSMpeg.VideoElement("#video-canvas", 'ws://localhost:8080', {
		   autoplay: true,
		   loop: true
		 });
	   }

	3.  declarations.d.ts

		declare module '@cycjimmy/jsmpeg-player';

	4. component.html

	   <div id="video-canvas" style="height: 480px; width: 640px"></div>

=================================================================================
reference:

	https://github.com/cycjimmy/jsmpeg-player/pkgs/npm/jsmpeg-player
	https://github.com/Zveroslav/node-rtsp-stream-jsmpeg#readme
```

{% file src="../../.gitbook/assets/rtsp-ws.zip" %}
