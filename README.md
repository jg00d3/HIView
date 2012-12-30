mjpeg-consumer
==================
  
A node.js stream implementation that consumes http mjpeg streams and emits jpegs.
  
###Install###

	npm install mjpeg-consumer

----------------------  

###Objects###
Requiring the `mjpeg-consumer` module returns a readable/writable stream implementation that takes an http stream of mjpegs and emits jpegs.

**MjpegConsumer()**

	var MjpegConsumer = require("mjpeg-consumer");
	var consumer = new MjpegConsumer();

###Sample Usage###
The `mjpeg-consumer` isn't very useful without a writable pipe to pipe jpegs to. I've built the [file-on-write](https://github.com/mmaelzer/file-on-write) stream to write a file every time `write` is called on it. The below example opens a stream to an IP camera, pipes the results to the `mjpeg-consumer` which processes the stream and emits parsed jpegs to the `file-on-write` writer.

	var request = require("request");
	var MjpegConsumer = require("../lib/mjpeg-consumer");
	var FileOnWrite = require("file-on-write");

	var writer = new FileOnWrite({ 
		path: './video',
		ext: '.jpg'
	});
	var consumer = new MjpegConsumer();

	request("http://192.168.1.2/videostream.cgi").pipe(consumer).pipe(shedder).pipe(writer);