---
description: 'Stream in:'
---

# Client (SSL)

ffmpeg -thread\_queue\_size 4096 -i "rtsp://cam/realmonitor?channel=1\&subtype=1" -f mpegts -codec:v mpeg1video -s 640x480 -b:v 1000k -bf 0 https://127.0.0.1:8081/supersecret

Stream display:

```
// View stream html
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
	<canvas id="video-canvas" width="640" height="266"></canvas>
 	<script type="text/javascript" src="https://jsmpeg.com/jsmpeg.min.js"></script>
	<script type="text/javascript">
		var canvas = document.getElementById('video-canvas');
		var url = 'wss://192.168.0.168:8082/';
		var player = new JSMpeg.Player(url, {canvas: canvas});
	</script>
	
 
</body>
</html>

```
