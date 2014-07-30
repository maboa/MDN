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

>Note 1 : Due to potential security issues if your video is on a different domain to your code, you'll need to enable [CORS (Cross Origin Resource Sharing)](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) on your video server.

>Note 2 : The above is a minimal example of how to manipulate video with canvas, for efficiency you may consider using requestAnimationFrame instead of setTimeout for browsers that support it. 

See Also: [Manipulating Video Using Canvas](https://developer.mozilla.org/en-US/docs/Web/HTML/Manipulating_video_using_canvas)

###Video and WebGL

WebGL is a powerful API that uses canvas to (typically) render three-dimensional scenes. You can combine WebGL and the video element to create video textures, which means you can put video inside 3D scenes.

See Also: [Using the Video frames as a Texture](https://developer.mozilla.org/en-US/docs/Web/WebGL/Animating_textures_in_WebGL#Using_the_video_frames_as_a_texture)

You can also use a JavaScript library built upon WebGL - [THREE.js](http://threejs.org) to [achieve this effect](http://stemkoski.github.io/Three.js/Video.html) 

###Playback Rate

We can also adjust the rate that audio and video plays back using an attribute of the audio and video element called playBackRate. PlaybackRate is number that represents a multiple to be applied to rate of playback - 0.5 represents half-speed while 2 represents double speed.

HTML:
`````html
<video id="my-video" controls src="http://jplayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v"></video>
`````

JavaScript:
`````javascript
var myVideo = document.getElementById('my-video');
myVideo.playbackRate = 2;
`````

[Try it](http://jsbin.com/qomuvefu/2/edit)

More info [HTML5 playbackRate explained](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/HTML5_playbackRate_explained)

>Note : this works with the audio element too but in both cases while the rate changes the pitch doesn't. To manipulate the audio's pitch you need to use the Web Audio API.


Audio Manipulation
------------------

PlaybackRate aside, to manipulate audio you'll typically use the Web Audio API. We can use the audio track of an audio or video element.

###Audio Filters

HTML:
`````html
<video id="my-video" controls src="myvideo.mp4" type="video/mp4"></video>
`````

JavaScript:
`````javascript
var audioSource = context.createMediaElementSource(document.getElementById("my-video"));
var filter = context.createBiquadFilter();
audioSource.connect(filter);
filter.connect(context.destination);
`````

> Note: unless you have CORS enabled, to avoid security issues your video should be on the same domain as your code.

Common filters can be applied to nodes:

- Low Pass - allows frequencies below the cutoff frequency to pass through and attenuates frequencies above the cutoff
- High Pass - allows frequencies above the cutoff frequency to pass through and attenuates frequencies below the cutoff
- Band Pass - allows a range of frequencies to pass through and attenuates the frequencies below and above this frequency range
- Low Shelf - allows all frequencies through, but adds a boost (or attenuation) to the lower frequencies
- High Shelf - allows all frequencies through, but adds a boost (or attenuation) to the higher frequencies
- Peaking - allows all frequencies through, but adds a boost (or attenuation) to a range of frequencies
- Notch - allows all frequencies through, except for a set of frequencies
- Allpass - allows all frequencies through, but changes the phase relationship between the various frequencies

Example:

JavaScript:
`````javascript
var filter = context.createBiquadFilter();
filter.type = "lowshelf";
`````
See [BiquadFilterNode](https://developer.mozilla.org/en-US/docs/Web/API/BiquadFilterNode)

###Convolutions and Impulses

It's also possible to apply impulse responses to audio using the convolver node. An impulse response is the sound created after a brief impulse of sound (like a hand clap). An impulse response will signify the environment in which the impulse was created (echo etc).

Example:

`````Javascript
var convolver = context.createConvolver();
convolver.buffer = this.impulseResponseBuffer;
// Connect the graph.
source.connect(convolver);
convolver.connect(context.destination);
`````

See [Developing Game Audio with the Web Audio API (Room effects and filters)](http://www.html5rocks.com/en/tutorials/webaudio/games/#toc-room)

Try out some [Convolution Effects in Real-Time](http://chromium.googlecode.com/svn/trunk/samples/audio/convolution-effects.html)


###Spatial Audio

We can also position audio using a panner node. A panner node allows us to define a source cone as well as postional and directional elements - all in 3D space.  

Example:

`````Javascript
var panner = context.createPanner();
panner.coneOuterGain = 0.2;
panner.coneOuterAngle = 120;
panner.coneInnerAngle = 0;

panner.connect(context.destination);
source.connect(panner);
source.start(0);
  
// Position the listener at the origin.
context.listener.setPosition(0, 0, 0);
`````

###JavaScript Codecs

It's also possible to manipulate audio at a low level using JavaScript. This can be useful should you want to create audio codecs. 

Libraries currently exist for the following formats :

- AAC - ([AAC.js](https://github.com/audiocogs/aac.js))
- ALAC - ([alac.js](https://github.com/audiocogs/alac.js))
- FLAC - ([flac.js](https://github.com/audiocogs/flac.js))
- MP3 - ([mp3.js](https://github.com/audiocogs/mp3.js))
- Opus - ([Opus.js](https://github.com/audiocogs/opus.js))
- Vorbis - ([vorbis.js](https://github.com/audiocogs/vorbis.js))

[Try out a few demos](http://audiocogs.org/codecs/). Audiocogs also provide a JavaScript Framework - [Aurora.js](http://audiocogs.org/codecs/) intended to help you author your own codecs.


See Also
--------

- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [AudioContext](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext)
- [Various Web Audio API Examples](https://github.com/mdn/)
