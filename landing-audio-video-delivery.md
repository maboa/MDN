Audio and Video Delivery
========================


HTML
----

Audio for the web can be specified:

`````html
<audio controls>
  <source src="audiofile.mp3" type="audio/mpeg">
  
  <!-- fallback for browsers that don't suppport mp3 -->
  <source src="audiofile.ogg" type="audio/ogg">
  
  <!-- fallback for browsers that don't support audio tag -->
  <a href="audiofile.mp3">download audio</a>
</audio>
`````

Video for the web can be specified:

`````html
<video controls width="640" height="480">
  <source src="videofile.mp4" type="video/mp4">
  
  <!-- fallback for browsers that don't suppport mp4 -->
  <source src="videofile.webm" type="video/webm">
  
  <!-- fallback for browsers that don't support video tag -->
  <a href="videofile.mp4">download video</a>
</video>
`````

JavaScript
----------

You can create audio on the fly like this:

`````javascript
var myAudio = document.createElement('audio');

if (myAudio.canPlayType('audio/mpeg')) {
  myAudio.setAttribute('src','audiofile.mp3');
}

if (myAudio.canPlayType('audio/ogg')) {
  myAudio.setAttribute('src','audiofile.ogg');
}
`````

Video similarly:

`````javascript
var myVideo = document.createElement('video');

if (myVideo.canPlayType('video/mp4')) {
  myVideo.setAttribute('src','videofile.mp4');
}

if (myVideo.canPlayType('video/webm')) {
  myVideo.setAttribute('src','videofile.webm');
}
`````
