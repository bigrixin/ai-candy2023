# Video stream - JsMpeg

```
// Some code
JSMpeg 

 https://github.com/phoboslab/jsmpeg

====== prepare ===========================================

	1. Install ffmpeg (https://ffmpeg.org/  download and path)
	   -- 

	2. Install Node.js and npm

	3. Install http-server (Raspberry Pi Live Webcam)
	   -- npm -g install http-server

	4. Install git and clone JsMpeg
	   -- git clone https://github.com/phoboslab/jsmpeg.git
	   
	5. cd jsmpeg/

	6. Install the Node.js Websocket Library
	   -- npm install ws

====== run  ===========================================


	7. Start the Websocket relay in the jsmpeg/
	   
		 node websocket-relay.js supersecret 8081 8082
	   

	8. Start ffmpeg to capture the webcam video 

		 ffmpeg -i "rtsp://admin:password@192.168.0.35:554/cam/realmonitor?channel=1&subtype=1" -f mpegts -codec:v mpeg1video -s 640x480 -b:v 1000k -bf 0 http://localhost:8081/supersecret

		 ffmpeg -i out.ts -f mpegts -codec:v mpeg1video -s 640x480 -b:v 1000k -bf 500 http://localhost:8081/supersecret


	9. See a live webcam in browser


=================================================
   view-stream.html
=================================================
	<!DOCTYPE html>
	<html>
	<head>
		<title>JSMpeg Stream Client</title>
		<style type="text/css">
			html, body {
				background-color: #111;
				text-align: center;
			}
		</style>
		
	</head>
	<body>
		<canvas id="video-canvas"></canvas>
		<script type="text/javascript" src="jsmpeg.min.js"></script>
		<script type="text/javascript">
			var canvas = document.getElementById('video-canvas');
			var url = 'ws://192.168.0.168:8082/';
			var player = new JSMpeg.Player(url, {canvas: canvas});
		</script>
	</body>
	</html>

```
