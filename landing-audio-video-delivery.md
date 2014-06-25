Audio and Video Delivery
========================


###HTML Audio

`````html
<audio controls preload="auto">
  <source src="audiofile.mp3" type="audio/mpeg">
  
  <!-- fallback for browsers that don't suppport mp3 -->
  <source src="audiofile.ogg" type="audio/ogg">
  
  <!-- fallback for browsers that don't support audio tag -->
  <a href="audiofile.mp3">download audio</a>
</audio>
`````

The code above will create an audio player that attempts to preload as much audio as possible for smooth playback.

> Note: preload may be ignored by some mobile browsers.


###HTML Video

`````html
<video controls width="640" height="480" poster="initialimage.png" autoplay muted>
  <source src="videofile.mp4" type="video/mp4">
  
  <!-- fallback for browsers that don't suppport mp4 -->
  <source src="videofile.webm" type="video/webm">
  
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English">
  <track src="subtitles_no.vtt" kind="subtitles" srclang="no" label="Norwegian">
  
  <!-- fallback for browsers that don't support video tag -->
  <a href="videofile.mp4">download video</a>
</video>
`````

The code above creates a video player of dimensions 640x480 pixels, displaying a poster image until the video is played. We instruct the video to autoplay but to be muted by default.

> Note: autoplay may be ignored by some mobile browsers.

###JavaScript Audio

`````javascript
var myAudio = document.createElement('audio');

if (myAudio.canPlayType('audio/mpeg')) {
  myAudio.setAttribute('src','audiofile.mp3');
}

if (myAudio.canPlayType('audio/ogg')) {
  myAudio.setAttribute('src','audiofile.ogg');
}
`````

###JavaScript Video

`````javascript
var myVideo = document.createElement('video');

if (myVideo.canPlayType('video/mp4')) {
  myVideo.setAttribute('src','videofile.mp4');
}

if (myVideo.canPlayType('video/webm')) {
  myVideo.setAttribute('src','videofile.webm');
}
`````
