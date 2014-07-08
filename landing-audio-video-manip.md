Audio and Video Manipulation
============================

The beauty of the web is that you can combine technologies to create new forms. Having native audio and video in the browser means we can use these data streams with technologies such as Canvas or WebGL.

The Web Audio API also provides a myriad of possibilities for manipulationg audio. Let's take a look at what's possible.


Video Manipulation
------------------

The ability to read the pixel values of each frame of video can be very useful.

### Video and Canvas

Canvas is a new way of drawing into web pages, it is very powerful and can be coupled tightly with video.

The general technique is to :

- Write a frame from the video element to an intermediary canvas element
- Read the data from the intermediary canvas element and manipulate it
- Write the manipulated data to your 'display' canvas
- Pause and repeat.

We can set up our video player and canvas element like this:

`````html
<video id="my-video" controls="true" width="480" height="270">
  <source src="http://jplayer.org/video/webm/Big_Buck_Bunny_Trailer.webm" type="video/webm">
  <source src="http://jplayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v" type="video/mp4">
</video>

<canvas id="my-canvas" width="480" height="270"></canvas>
`````

and manipulate it like this (in this case we are making a black and white version):

`````javascript
var processor = {  
  timerCallback: function() {  
    if (this.video.paused || this.video.ended) {  
      return;  
    }  
    this.computeFrame();  
    var self = this;  
    setTimeout(function () {  
      self.timerCallback();  
    }, 16); // roughly 60 frames per second  
  }, 

  doLoad: function() { 
    this.video = document.getElementById("my-video"); 
    this.c1 = document.getElementById("my-canvas"); 
    this.ctx1 = this.c1.getContext("2d"); 
    var self = this;  

    this.video.addEventListener("play", function() { 
      self.width = self.video.width;  
      self.height = self.video.height;  
      self.timerCallback(); 
    }, false); 
  },  

  computeFrame: function() { 
    this.ctx1.drawImage(this.video, 0, 0, this.width, this.height); 
    var frame = this.ctx1.getImageData(0, 0, this.width, this.height); 
    var l = frame.data.length / 4;  

    for (var i = 0; i < l; i++) {
      var grey = (frame.data[i * 4 + 0] + frame.data[i * 4 + 1] + frame.data[i * 4 + 2]) / 3; 

      frame.data[i * 4 + 0] = grey; 
      frame.data[i * 4 + 1] = grey; 
      frame.data[i * 4 + 2] = grey; 
    } 
    this.ctx1.putImageData(frame, 0, 0); 

    return; 
  } 
};  
`````

Once the page has loaded you can call 
`````javascript
processor.doLoad()
`````

####Note 1

Due to potential security issues if your video is on a different domain to your code, you'll need to enable (CORS)[http://en.wikipedia.org/wiki/Cross-origin_resource_sharing] on your video server.

####Note 2

The above is a minimal example of how to manipulate video with canvas, for effciency you may consider using requestAnimationFrame instead of setTimeout for browsers that support it. 


